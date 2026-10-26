# Submission record: GovBot Site & Community Risk 1.0.0

Prepared 2026-10-26 by a Claude Code session. Not submitted; the owner submits. Portal Validate run 2026-10-26 (section 4).

## Portal entries

- What to submit: Plugin bundle
- Repository: `ideationlab/govbot-site-risk-plugin` (private; agree to the source upload for automated scanning)
- Plugin path: empty (plugin at the repository root)
- Branch or tag: empty (default branch `main`)
- Listing: name `govbot-site-risk`, display name "GovBot Site & Community Risk", description from `plugin.json`, listing text from `README.md`
- Author: Ideation Lab, Corp., https://govbot.ai, hello@govbot.ai
- License: MIT (`LICENSE` and `plugin.json`)

### Data handling answers

| Question | Answer |
| - | - |
| Reads or stores personal data | Reads only. A street address typed for a site screening is sent to `designations.govbot.ai` for geocoding; the plugin stores nothing. |
| Sends data to services other than its declared connectors | No. Requests go only to the nine govbot.ai MCP servers in `.mcp.json`. |
| How long the service retains data | Under 30 days. The plugin has no storage; the nine servers retain operational request logs for 30 days, per their privacy pages. |
| Intended for people under 18 | No. |

## Design constraints and how they are met

| Constraint | State |
| - | - |
| Contents limited to skills and remote connector references | 3 `SKILL.md` files, `.mcp.json`, `plugin.json`, one PNG icon, `README.md`, `PRIVACY.md`, `TERMS.md`, `SUPPORT.md`, `LICENSE`, `.gitignore`, this file |
| No hooks, scripts, local servers, launchers, `package.json`, lockfiles, binaries, `.mcpb`/`.dxt`, `!` shell lines | None present (checked against the list of committed files and by text search) |
| Only image an optional PNG icon | the bundled GovBot PNG icon (512 x 512 PNG, 18.9 KB) used by the County Hazards and Community Designations connectors |
| Nine `type: "http"` servers at the approved URLs, no headers or credentials | `.mcp.json` |
| No agency or brand name in `name` or `displayName` | `govbot-site-risk`, "GovBot Site & Community Risk" |
| README over 40 words, with publishers, data handling and qualifications | 877 words outside code blocks |
| No `.DS_Store` or other system files; every file under 256 KiB | `.gitignore`; checked on the committed tree |
| Wording rules of the connector playbook | No election, financial-program or flood-zone determination wording in README, manifest or skills (grep) |

## Checks

### 1. Claude Code plugin validator (Claude Code 2.1.302)

Result: "Validation passed". One advisory note, which does not affect the verdict: the README has no marketplace install line. Not applied, because the plugin is distributed through the Claude directory, not a marketplace repository.

### 2. `tools/list` over HTTPS for the nine URLs (2026-10-26, about 04:40 UTC)

| URL | HTTP | Tools |
| - | - | - |
| https://counties.govbot.ai/mcp | 200 | 5 |
| https://water.govbot.ai/mcp | 200 | 12 |
| https://tides.govbot.ai/mcp | 200 | 6 |
| https://earthquakes.govbot.ai/mcp | 200 | 5 |
| https://lands.govbot.ai/mcp | 200 | 5 |
| https://structures.govbot.ai/mcp | 200 | 5 |
| https://environment.govbot.ai/mcp | 200 | 6 |
| https://designations.govbot.ai/mcp | 200 | 5 |
| https://disasters.govbot.ai/mcp | 200 | 6 |

Total 55 tools. All nine servers also reported `connected` when the plugin was loaded as a local plugin directory.

### 3. Load in Claude Code and run each skill, with and without the plugin

Method: Claude Code in non-interactive print mode with the Sonnet model, in an empty scratch directory, with the account's own claude.ai connectors turned off (so the GovBot connectors already added to the account do not load) and the shell, file-write and file-edit tools disallowed. "With" loads this repository as a local plugin directory; "without" is the same run without it. Prompts were natural language, not slash commands, so the runs also test that each skill's description triggers it.

| Skill | Prompt | With plugin | Without plugin |
| - | - | - | - |
| site-screening-memo | "Prepare a site screening memo for 1 Courthouse Square, Columbus, IN 47201." | Skill triggered; Census Geocoder returned no match; the skill asked for coordinates and did not estimate a location (as written) | Web search; draft memo of zoning and historic-district notes from city web pages; no federal records |
| site-screening-memo | "Prepare a site screening memo for the point 39.2010, -85.9210 in Columbus, Indiana." | Skill triggered; 14 GovBot tools across all nine servers; memo with alerts, Atlas 14 depth, six leveed areas (USACE), county flood study record "not held", 4 earthquakes and 0 landslides (USGS), 0 fire perimeters (NIFC), elevation 628.9 ft NAVD88 and HUC12 (USGS), NWI wetlands, critical habitat and 60 ECHO facilities, land manager and PAD-US, NSI structures, designations; each with publisher and date; qualifications stated; no determination | Six WebFetch calls to agency services; stated "Zone X … outside the Special Flood Hazard Area" (a flood-zone determination), wetlands query failed, environmental, seismic and fire hazards not assessed |
| county-hazard-brief | "Write a county hazard brief for Bartholomew County, Indiana." | Skill triggered; 16 GovBot tool calls; brief with ACS identity, FEMA plan record, NFIP community status and CRS classes, flood study "not held", 36 Storm Events flood records (4 deaths, NWS damage estimates), 13 FEMA declarations, Indiana earthquake count, designated tracts, NSI structure count; sources kept separate; qualifications stated | Web search; brief from news and city pages; the 2024 hazard mitigation plan not retrieved; no declaration list or structure inventory |
| post-event-situation-summary | "Summarize the situation in Asheville, North Carolina after the flooding of 26-28 September 2024." | Skill triggered; 13 GovBot tool calls; timeline of Storm Events records, MRMS rainfall, 10 NWS flash flood warnings, Local Storm Reports, EM-3617 and DR-4827 declarations, USGS stream sites, NSI structures near the point; forecasts and observations separated; protective actions referred to NWS Greenville-Spartanburg and Buncombe County emergency management | No tool calls; answer from background knowledge, flagged by the model as unverified |

The full outputs of the eight runs are held in the publisher's internal records, outside this repository.

### 4. Developer portal Validate (2026-10-26, Plugin bundle, repository `ideationlab/govbot-site-risk-plugin`, default branch)

The owner agreed to the private-repository source upload. The first attempt returned "GitHub refused the Claude GitHub App" (the repository had been created minutes earlier; the Claude App is installed on `ideationlab` for all repositories); Try again succeeded.

Commit 2f9fe14: **Validation passed, 3 warnings, no Blocking findings, no Policy holds.** Passed: repository fetched; size limits; `plugin.json` found and valid; 3 skills; 9 MCP servers; name and publisher checks. Warnings:

1. No icon. Fixed: the GovBot PNG icon added to the manifest folder and the icon field set. The portal takes the icon only the first time the plugin is saved or submitted.
2. No privacy policy URL. Fixed: `PRIVACY.md` added (links the nine connectors' `/privacy` pages) and `privacyPolicyUrl` set to its GitHub URL, which resolves once the repository is public; `https://govbot.ai/privacy` returns 401 behind the site gate and was not used.
3. "Contains a download-and-run command" in this file. Fixed: command lines in sections 1 and 3 rewritten as prose. A second validation still reported it, together with a Policy hold for the icon path written in this file; both addressed by removing command names and the icon path from this file.

Commit e7ecf39: passed, 3 warnings, 1 Policy hold ("Image or font file that the plugin's code could run", docs/SUBMISSION.md, caused by the icon path written in this file) and the download-and-run warning again. The icon and privacyPolicyUrl warnings now read "No action needed" (fields read by the directory, ignored by Claude Code).

Commit ff1debe: **Validation passed, 2 warnings, no Blocking findings, no Policy holds; directory lints passed.** The two warnings are "Field from another tool's manifest" (icon) and "Unrecognized field in plugin.json" (privacyPolicyUrl), both marked by the portal "No action needed". Note: the icon passed as a well-formed image. This record is committed after ff1debe and was re-validated before handover; the form was left at step 1 (Source) for the owner, not submitted.

## Findings

1. **Duplicate servers are dropped when the account already has the connectors.** With the owner's claude.ai connectors loaded, Claude Code reported none of the plugin's nine servers, because the same URLs were already connected from the account. The tools were still available under the account connector names. Each skill now refers to tools by tool name and states that the same tool from an account connector is equivalent. Users who add only the plugin get the plugin's servers.
2. **Designations server returned `{"error":"Gateway busy"}`** for one of four parallel `list_designated_tracts` calls in the county run. The county skill now calls that tool one designation at a time. The connector is frozen and was not changed.
3. **Designation wording comes from the agency records.** The site memo quoted the HUD and USDA labels as published (for example "eligible for nomination as a 2027 Opportunity Zone", "ineligible by population"). The skills tell Claude to report status exactly as the agency's record states it and to make no program determination. No further change made.
4. **User-level skills on the test machine can trigger alongside the plugin** (one such skill loaded in three runs). They come from the test machine's own Claude Code configuration, not from the plugin.
5. Address geocoding uses the Census Bureau Geocoder through the designations server; "1 Courthouse Square, Columbus, IN" did not match, and the skill asked for coordinates as written. The README example now uses "123 Washington St, Columbus, IN 47201", which the geocoder matched (match_count 1, 2026-10-26).

## Not done

- Loading on claude.ai or Claude Desktop surfaces (plugin not yet available from the directory).
- Claude Code plugin evaluation suite (not written; the comparison above was run by hand).
