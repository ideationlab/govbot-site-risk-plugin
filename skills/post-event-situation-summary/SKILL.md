---
name: post-event-situation-summary
description: Write a dated, cited situation summary for one place after one event (flood, storm, earthquake or wildfire) from federal public data - observations, alerts and warnings, storm reports, FEMA disaster declarations, and earthquake records or fire perimeters as relevant - using the GovBot connectors. Use when asked what happened, what the records show, or for a situation summary after an event at a place.
---

# Post-event situation summary

Produce a situation summary for one place and one event from the GovBot connectors in this plugin. The summary reports the federal records for the event, with publisher, time and date. It separates observed records from forecasts and makes no determination.

Tools are named below by server key and tool name. If the same GovBot connectors are already added to the account, Claude uses those and the tool names are the same; call the tool of that name from whichever GovBot server provides it.

## 1. Fix the place and the event

- Place: a point, a street address, a city or a county. For a city or county name, call `govbot-counties` `resolve_place` with the name and state; for an address, call `govbot-designations` `get_designations_for_address` and use the matched point. Note the state, county FIPS and a representative point (the given point, the matched address, or a point the user confirms).
- Event: its type and dates. If no dates are given, ask for them, or for a current event use the last 3 days. Use UTC windows in tool calls and state local times where the records give them.

## 2. Collect the records

Run independent calls in parallel. Use only the groups that apply to the event type. A tool that fails or returns nothing is recorded as such; continue with the others.

All events
- `govbot-water` `get_nws_active_alerts_at_point`: alerts active now (state the query time).
- `govbot-water` `search_ipaws_alert_archive` for the event window (at most 31 days): alerts sent.
- `govbot-disasters` `list_disaster_declarations` for the state and county, declared from the event start: any FEMA declaration and the programs declared.

Flood or storm
- `govbot-water` `list_flood_warning_polygons` for the event window (at most 3 days per call): warnings issued near the place.
- `govbot-water` `search_flood_storm_reports` for the state and window (at most 14 days): Local Storm Reports near the place.
- `govbot-water` `get_daily_rainfall_estimate` for the point on each event day (MRMS, past days only), and `get_atlas14_rainfall_depth` for context.
- `govbot-water` `list_usgs_stream_sites` for a box of about 0.2 degrees around the point, and `get_river_gauge_metadata` for a named NWS gauge if the user gives one.
- Coastal place: `govbot-tides` `find_tide_stations_near_point`, then `get_observed_water_levels` and `get_tide_predictions` for the nearest station over the event window; report observed minus predicted only as the two published series side by side.
- `govbot-disasters` `search_flood_events` for the county and dates (Storm Events, 1996-2025 snapshot; recent events may not be held yet).

Earthquake
- `govbot-earthquakes` `search_earthquakes_near_point` (radius 100 km, event window) and `get_earthquake_details` for the main event: origin time, magnitude, depth, ShakeMap values where published.
- `govbot-earthquakes` `search_landslides_near_point` (radius 25 km) for recorded landslides in the inventory.

Wildfire
- `govbot-lands` `list_active_fire_perimeters` for the state and `get_fire_perimeter` for the fire named or nearest: acres, containment and update time as published.
- `govbot-lands` `get_land_manager_at_point` for the point.

Exposure (any event)
- `govbot-structures` `summarize_structures_near_point` (radius up to 2 km) for structure counts near the point. Report it as the inventory near the point, not as damaged structures.

## 3. Write the summary

Use this structure, in plain professional language:

1. **Heading.** "Post-event situation summary", the place, the event and its dates, and the date and time the records were retrieved.
2. **Summary.** Three to six sentences stating what the records show, each with its source and time in parentheses.
3. **Timeline.** Records in time order, each with time (and time zone), source, record type (observation, alert, warning, report, declaration, perimeter update) and value as published.
4. **Sections** for the groups used: Alerts and warnings; Observations (rainfall, water levels, stream sites, storm reports); Earthquake records; Fire perimeter; Declarations; Structures near the point. Label forecasts and predictions as such and keep them apart from observations. Keep sources separate; do not merge or reconcile values from different publishers.
5. **Not returned.** Each tool that returned no records or failed, stated as "no record returned by <source> for <query>".
6. **Qualifications.** State: this summary is drawn from federal public records at the retrieval time; it is not a damage assessment and not a regulatory, insurance, engineering or property determination; records are revised after an event and can lag it; an empty result is not evidence that an event or its effects did not occur.
7. **Sources.** One line per source: publisher, dataset or service, record time or vintage, and the GovBot server that returned it.

## Wording rules

- State facts and their sources. No dramatic, promotional or reassuring language.
- Do not state whether a place is safe, whether to stay or leave, or whether a road, building or area is passable or habitable.
- For warnings, evacuations and protective actions, name the issuing authority (the National Weather Service office, local emergency management or the land management agency) as the source to follow, and give no advice of your own.
