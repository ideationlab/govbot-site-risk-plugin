---
name: site-screening-memo
description: Prepare a dated, cited site screening memo for one point (latitude and longitude) or one U.S. street address from federal public data - hazards, environmental layers, land manager, structures and federal designated areas - using the GovBot connectors. Use when asked to screen a site, parcel, address or location, or to write a site memo.
---

# Site screening memo

Produce a screening memo for one site from the GovBot connectors in this plugin. The memo reports what each federal source returned for the site, with its publisher and date. It makes no determination.

Tools are named below by server key and tool name. If the same GovBot connectors are already added to the account, Claude uses those and the tool names are the same; call the tool of that name from whichever GovBot server provides it.

## 1. Fix the site

- Latitude and longitude given: use them as given (WGS84, decimal degrees).
- Street address given: call `govbot-designations` `get_designations_for_address`. Use the matched point, state, county and census tract it returns, and quote the matched address. If no match is returned, ask for coordinates; do not estimate a location.
- Note the state, five-digit county FIPS and county name for later steps. With coordinates only, `govbot-designations` `get_designations_at_point` returns them.

## 2. Collect the records

Call these tools for the site point. Run independent calls in parallel. A tool that fails or returns nothing is recorded as such in the memo; continue with the others.

Hazards
- `govbot-water` `get_nws_active_alerts_at_point`: alerts active at query time.
- `govbot-water` `get_atlas14_rainfall_depth`: NOAA Atlas 14 24-hour, 100-year depth.
- `govbot-water` `query_levee_inventory` with a bounding box of about 0.1 degrees around the point: leveed areas nearby.
- `govbot-counties` `list_county_flood_studies` for the county FIPS: whether a FEMA flood study is held for the county, its status and date. Report it as a county-level study record; do not infer the site's position relative to any mapped flood hazard.
- `govbot-earthquakes` `search_earthquakes_near_point` (radius 100 km, magnitude 3 and above, the last 50 years unless the user states otherwise) and `search_landslides_near_point` (radius 10 km).
- `govbot-lands` `list_active_fire_perimeters` for the state; report any perimeter whose location is near the site, with its distance if the record allows it.
- `govbot-tides` `get_ground_elevation_at_point` and `get_watershed_at_point` (HUC8 and HUC12). For a coastal site, add `find_tide_stations_near_point` (radius 25 km) and `get_station_datums` for the nearest station.

Environment and land
- `govbot-environment` `get_site_environmental_summary`: wetlands within 500 m, critical habitat within 1,000 m, EPA ECHO facilities within 1 mile.
- `govbot-lands` `get_land_manager_at_point` and `get_protected_areas_at_point`.

Structures and designations
- `govbot-structures` `summarize_structures_near_point` (radius 500 m unless the user states otherwise).
- `govbot-designations` `get_designations_at_point` (skip if step 1 already returned the designations for the address).

## 3. Write the memo

Use this structure, in plain professional language:

1. **Heading.** "Site screening memo", the site (matched address and/or coordinates), state, county and census tract, and the date the records were retrieved.
2. **Summary.** Three to six sentences stating the main facts returned, each with its source in parentheses. No judgement of suitability, risk level or value.
3. **Sections**, one per topic: Weather and water; Flood study record (county); Earthquakes and landslides; Wildland fire; Elevation and watershed (and tides where used); Environmental layers; Land manager and protected areas; Structures; Federal designated areas.
   In each section, list each record with the value, units and datum as returned, then the publisher, dataset or layer, vintage or event date, and retrieval time. Keep sources separate: do not merge, average or reconcile values from different publishers. Where two sources differ, report both.
4. **Not returned.** Each tool that returned no records or failed, stated as "no record returned by <source> for <query>", not as absence of the hazard.
5. **Qualifications.** State: this memo is screening context from federal public records; it is not a regulatory, insurance, engineering or property determination, and not a determination of status under any federal program; the issuing agency's own service is authoritative; snapshot sources can lag the agency's current release; an empty result is not evidence that a hazard is absent.
6. **Sources.** One line per source: publisher, dataset or service, vintage, and the GovBot server that returned it.

## Wording rules

- State facts and their sources. No promotional, rhetorical or reassuring language.
- Do not describe a site as safe, unsafe, buildable, insurable, or in or out of a regulated area, and make no determination of status under any program. Report designation status exactly as the agency's record states it.
- For an active alert or fire, name the issuing authority (the National Weather Service office, local emergency management or the land management agency) as the source to follow; give no protective or evacuation advice of your own.
