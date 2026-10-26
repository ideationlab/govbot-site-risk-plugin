---
name: county-hazard-brief
description: Write a dated, cited hazard brief for one U.S. county from federal public data - hazard history, FEMA flood study records, NOAA Storm Events flood records, FEMA disaster declarations, federal designated tracts and the structure inventory - using the GovBot connectors. Use when asked for a county hazard brief, county hazard profile or county disaster history.
---

# County hazard brief

Produce a brief for one county from the GovBot connectors in this plugin. The brief reports what each federal source holds for the county, with its publisher and date. It makes no determination.

Tools are named below by server key and tool name. If the same GovBot connectors are already added to the account, Claude uses those and the tool names are the same; call the tool of that name from whichever GovBot server provides it.

## 1. Fix the county

- Five-digit FIPS code given: use it.
- County name given: call `govbot-counties` `resolve_place` with the name and state. Use the county GEOID it returns. If more than one county matches, ask which one; do not choose.
- Note the two-letter state code and the county name for later steps.

## 2. Collect the records

Run independent calls in parallel. A tool that fails or returns nothing is recorded as such in the brief; continue with the others.

- `govbot-counties` `get_county_brief` for the GEOID, all groups including `current`: identity, planning, flood insurance community status, flood studies, resilience, disasters and current alerts.
- `govbot-counties` `list_county_flood_studies` for the FIPS: study status, effective or issue date, and the streams studied (first page).
- `govbot-counties` `get_nfip_community_status` for the GEOID: participating communities and Community Rating System class as published.
- `govbot-disasters` `search_flood_events` for the state and county FIPS, sorted by property damage, then again sorted by date: the largest and the most recent flood-related Storm Events records, 1996-2025.
- `govbot-disasters` `list_disaster_declarations` for the state and county FIPS: the 25 most recent declarations.
- `govbot-earthquakes` `summarize_earthquakes_for_area` for the state over the last 50 years, magnitude 3 and above. Report it as state-level context.
- `govbot-designations` `list_designated_tracts` for the state and county, for each tract-based designation that `describe_designations` lists, one designation at a time rather than in parallel: count of designated tracts per designation.
- `govbot-structures` `summarize_structures_in_census_geography` for the county GEOID: structure counts by occupancy type and other summaries returned.

## 3. Write the brief

Use this structure, in plain professional language:

1. **Heading.** "County hazard brief", the county name, state and FIPS code, and the date the records were retrieved.
2. **Summary.** Four to eight sentences stating the main facts returned (population, plan coverage, flood study status, declaration count, largest recorded flood events, structure count), each with its source in parentheses. No ranking or judgement of risk.
3. **Sections:** Identity; Hazard mitigation plans; Flood insurance program community status; FEMA flood study record; Flood events (Storm Events); Disaster declarations; Earthquake context (state); Federal designated tracts; Structure inventory; Current alerts.
   In each section, list values as returned with units, then the publisher, dataset, vintage or date range, and retrieval time. Keep sources separate: Storm Events damage estimates, FEMA declarations and structure counts are different measures and are not combined or compared as one figure. Where two sources differ, report both.
4. **Not returned.** Each tool that returned no records or failed, stated as "no record returned by <source> for <query>".
5. **Qualifications.** State: this brief is screening context from federal public records; it is not a regulatory, insurance, engineering or property determination, and not a determination of status under any federal program; Storm Events damages are National Weather Service estimates in nominal dollars, not verified or insured losses; snapshot sources can lag the agency's current release; an empty result is not evidence that a hazard or event is absent.
6. **Sources.** One line per source: publisher, dataset or service, vintage, and the GovBot server that returned it.

## Wording rules

- State facts and their sources. No promotional, rhetorical or reassuring language.
- Do not rate the county's risk, rank it against other counties or state what the county should do.
- Report designation and program status exactly as the agency's record states it, and make no determination of status under any program.
- For a current alert, name the issuing National Weather Service office or local emergency management as the source to follow; give no protective advice of your own.
