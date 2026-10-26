# Privacy: GovBot Site & Community Risk plugin

Effective 2026-10-26. Publisher: Ideation Lab, Corp., https://govbot.ai, hello@govbot.ai.

## What the plugin itself does

The plugin consists of skill instructions and references to nine remote MCP servers. It runs no code on the user's computer, stores no data, uses no account or credential, and sets no cookies.

## What is sent, and where

When a skill runs, Claude sends tool arguments (coordinates, place or county names, census identifiers, dates, filters and, for a site screening, a street address) to the GovBot servers listed below. No other destination receives data from the plugin. Each server retrieves public records from the federal publishers named in `README.md`, and may pass the query parameters needed for that request to the publisher. Each server's own privacy policy describes the request metadata it processes and how long it is kept:

| Server | Privacy policy |
| - | - |
| counties.govbot.ai | https://counties.govbot.ai/privacy |
| water.govbot.ai | https://water.govbot.ai/privacy |
| tides.govbot.ai | https://tides.govbot.ai/privacy |
| earthquakes.govbot.ai | https://earthquakes.govbot.ai/privacy |
| lands.govbot.ai | https://lands.govbot.ai/privacy |
| structures.govbot.ai | https://structures.govbot.ai/privacy |
| environment.govbot.ai | https://environment.govbot.ai/privacy |
| designations.govbot.ai | https://designations.govbot.ai/privacy |
| disasters.govbot.ai | https://disasters.govbot.ai/privacy |

## Personal data

The plugin does not ask for personal data. A street address typed for a site screening is sent to designations.govbot.ai, which matches it with the Census Bureau Geocoder to obtain a point; the plugin does not store it. The servers keep operational request logs for 30 days, as their privacy policies state. Tool arguments are not to include confidential information or sensitive personal data. The plugin is not intended for people under 18.

## Contact

hello@govbot.ai
