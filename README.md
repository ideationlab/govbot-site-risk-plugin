# GovBot Site & Community Risk

A Claude plugin from Ideation Lab, Corp. (https://govbot.ai) for screening a site or a county against federal public data. It packages three workflow skills with nine read-only GovBot connectors. Each skill names the tools it calls, keeps every source separate with its publisher and date, and writes a dated, cited document: a site screening memo, a county hazard brief or a post-event situation summary.

## What the plugin contains

- Three skills in `skills/`, written as instructions for Claude.
- `.mcp.json`, which references nine remote MCP servers over HTTPS.

The plugin contains no hooks, scripts, local servers, package installs or binary files. It runs nothing on the user's computer.

## Connectors and the publishers they read

| Server key | Connector | URL | Federal publishers read |
| - | - | - | - |
| `govbot-counties` | GovBot County Hazards | https://counties.govbot.ai/mcp | U.S. Census Bureau (place and county identifiers, American Community Survey); FEMA (registered hazard mitigation plans, National Flood Insurance Program community status and Community Rating System, flood study cross-section tables, Community Resilience Challenges Index, disaster declarations); National Weather Service (current alerts) |
| `govbot-water` | GovBot Weather, Water & Floods | https://water.govbot.ai/mcp | National Weather Service (forecasts, active alerts, flood warning polygons, Local Storm Reports); FEMA (IPAWS alert archive, flood study profiles); NOAA (MRMS rainfall estimates, Atlas 14 precipitation frequency, National Water Prediction Service gauges); USGS (stream sites); U.S. Army Corps of Engineers (National Levee Database) |
| `govbot-tides` | GovBot Tides, Elevation & Watersheds | https://tides.govbot.ai/mcp | NOAA CO-OPS (tide stations, observed water levels, predictions, datums); USGS (3DEP ground elevation, Watershed Boundary Dataset) |
| `govbot-earthquakes` | GovBot Earthquakes & Landslides | https://earthquakes.govbot.ai/mcp | USGS (Earthquake Catalog, ShakeMap, U.S. Landslide Inventory) |
| `govbot-lands` | GovBot Public Lands & Wildfire Perimeters | https://lands.govbot.ai/mcp | Bureau of Land Management (Surface Management Agency); USGS (Protected Areas Database of the United States); National Interagency Fire Center (WFIGS current fire perimeters) |
| `govbot-structures` | GovBot Structure Inventory | https://structures.govbot.ai/mcp | U.S. Army Corps of Engineers (National Structure Inventory) |
| `govbot-environment` | GovBot Environmental Site Context | https://environment.govbot.ai/mcp | U.S. Fish and Wildlife Service (National Wetlands Inventory, critical habitat); EPA (ECHO facility search and compliance reports) |
| `govbot-designations` | GovBot Community Designations | https://designations.govbot.ai/mcp | HUD (eGIS designated census tracts and areas); USDA Rural Development (designated area map service); U.S. Census Bureau (Geocoder) |
| `govbot-disasters` | GovBot Disaster History | https://disasters.govbot.ai/mcp | NOAA NCEI (Storm Events Database); FEMA (OpenFEMA disaster declarations and other OpenFEMA datasets) |

The `govbot-water` server reads NWS warning polygons, NWS Local Storm Reports and NOAA MRMS daily rainfall through the Iowa Environmental Mesonet archive of those products, and its results name that distribution.

Each connector is also listed on its own in the Claude directory. This plugin references the same URLs and changes nothing about them.

## Data handling

- All tools are read-only. No tool creates, changes or deletes anything.
- The plugin stores no data. It has no local files that record prompts or results.
- Requests go only to the nine govbot.ai servers listed above. Those servers read the federal sources named in the table.
- No account, sign-in, API key or other credential is needed. The plugin reads no environment variables and sends no headers.
- The plugin does not ask for personal data. A street address entered for a site screening is sent to the `govbot-designations` server, which matches it with the Census Bureau Geocoder to obtain a point; the plugin does not store it. The servers keep operational request logs for 30 days, as stated in their privacy policies (see `PRIVACY.md`).

## Skills and example prompts

**site-screening-memo.** Takes a point or a street address. Reads hazard context (flood study and weather records, earthquakes, landslides, fire perimeters, elevation and watershed), environmental layers, the land manager, nearby structures and federal designated areas. Writes a dated memo that cites each source.

- "Prepare a site screening memo for 39.2673, -76.7983."
- "Screen 123 Washington St, Columbus, IN 47201 and write the memo."

**county-hazard-brief.** Takes a county name or five-digit FIPS code. Reads the county brief, flood study records, Storm Events flood history, FEMA disaster declarations, designated tracts and the structure inventory summary. Writes a county brief.

- "Write a county hazard brief for Bartholomew County, Indiana."
- "County hazard brief for FIPS 24027."

**post-event-situation-summary.** Takes a place and an event (a storm, flood, earthquake or wildfire, with dates). Reads observations, alerts, warning polygons, storm reports, FEMA declarations and, where relevant, earthquake records or fire perimeters. Writes a summary that separates observed records from forecasts.

- "Summarize the situation in Asheville, NC after the flooding of 26-28 September 2024."
- "Post-event summary for the earthquake near Ridgecrest, CA in July 2019."

## Qualifications

Results are screening context drawn from federal public records. They are not a regulatory, insurance, engineering or property determination, and not a determination of status under any federal program. Each record carries its own publisher, dataset and date; snapshot sources can lag the agency's current release. An empty result means no matching record was returned, not that a hazard is absent. For warnings, evacuations and protective actions, the issuing authority (the National Weather Service, local emergency management or the land management agency) is the source to follow.

## Support

Ideation Lab, Corp. · https://govbot.ai · hello@govbot.ai

## License

MIT. See `LICENSE`.
