# HANDOFF (updated): paste this whole file into a new Claude/ChatGPT chat and attach evergreen.zip

## Instruction to the new assistant
Continue helping me finish and SUBMIT this hackathon project. I am in a hurry. Be decisive, do the work, give copy-paste outputs.

## Context
Hackathon: "Build, Ship, Shape: Amazon Developer Hackathon" on Devpost. Track: Alexa+. Mini challenges: AWS Builder + Open Source.
DEADLINE: Fri Oct 23 2026, 12:00 pm PT = 12:30 am IST Sat Oct 24 (about 19:00 UTC). My location: Chennai. Submit a draft early.
Project: **Evergreen**, an Alexa+ MCP server for elder care + life-story reminiscence. Elder (Paati) talks to Alexa+: mood + medicine check-ins, gentle story questions saved as memory cards. Family gets weekly summary, alerts, and a dashboard.

## Files in evergreen.zip
- server.py: Python MCP server (mcp>=1.23,<2 official SDK, FastMCP, Streamable HTTP at /mcp, spec 2025-11-25, stateless + JSON responses). Binds 127.0.0.1 by default (env HOST to change), PORT env (default 8000).
- Tools: set_profile, log_checkin, suggest_story_prompt, save_memory, weekly_summary, notify_family (writes outbox.json; publishes to Amazon SNS if ALERT_SNS_TOPIC_ARN is set)
- Extra routes: /dashboard?name=paati, /api/summary/{name}, /api/memories/{name}
- dashboard.html: family dashboard (alert card, stats, life-story cards)
- seed.py: demo data for "Paati" (declining mood, missed doses, 3 stories)
- chat.py: Claude on Amazon Bedrock (boto3 Converse API with tool use) + MCP client; loads the Agent Skill as the system prompt. `python chat.py elder` / `family`
- skills/evergreen-care/SKILL.md: the Agent Skill
- README.md, LICENSE (MIT), requirements.txt (mcp<2, uvicorn, boto3)

## What is TESTED vs NOT
TESTED: server starts; MCP initialize with 2025-11-25 works; all 6 tools listed; weekly_summary and notify_family work via an MCP client; dashboard loads (user confirmed on localhost:8000).
NOT TESTED: chat.py against real Bedrock (no AWS credentials in my sandbox; syntax checked only). SNS publishing untested.
To run chat.py: AWS credentials configured, Bedrock model access enabled, then
  export AWS_REGION=us-east-1 BEDROCK_MODEL_ID=<Claude model or inference profile ID enabled in the account>
If Bedrock fails or is not set up in time: DO NOT tick AWS Builder; use MCP Inspector (npx @modelcontextprotocol/inspector, connect to http://127.0.0.1:8000/mcp, Streamable HTTP) for the demo instead.

## Remaining TODO (priority order)
1. [ ] Run locally: pip install -r requirements.txt; python seed.py; python server.py
2. [ ] Push to PUBLIC GitHub repo; confirm MIT license shows in About. (Do not commit data.json/outbox.json/.venv)
3. [ ] Start the Devpost draft: track Alexa+, description, repo URL
4. [ ] Try chat.py with Bedrock (see above). If it works, tick AWS Builder and describe the Bedrock use in Product Feedback
5. [ ] Record demo < 3 min, upload PUBLIC to YouTube/Vimeo, no copyrighted music
6. [ ] Open Source mini-challenge: the new public repo counts. Provide contribution URL, repo URL, GitHub username, description
7. [ ] Paste product feedback + friction log, submit well before the deadline
8. [ ] Stretch only if hours remain: MCP App (inline dashboard). Docs: apps.extensions.modelcontextprotocol.io/api (Agent Skills: create-mcp-app, add-app-to-server). Official SDK is Node/TypeScript; our server is Python.

## Rules recap
- Repo: public with visible open-source license (or private shared with testing@devpost.com + knmeiss, giolaq, anishamalde, mosesroth, emersonsklar). Clear setup/run instructions. Must import and call the MCP SDK at runtime (done).
- Alexa+ requirement: self-hosted MCP server (spec 2025-11-25+, Streamable HTTP) or Agent Skill, or a simulated Alexa+ web app.
- Video < 3 min showing the project working. Text description. Product Feedback per tool used. Pick track + mini challenges.
- Judging (equal weight): Tech Implementation, Design, Potential Impact, Quality of the Idea. Friction log gives up to +10%.
- Creative signals judges list: agentic multi-service workflows, state across sessions, cards/carousels, MCP Apps, Agent Skills, purchasing.

## Demo script (2:30)
0:00 Problem: families can't see how elders are really doing; stories get lost.
0:20 Elder: "I took my vitamin but forgot my BP tablet, feeling a bit dizzy" -> log_checkin
0:50 Evergreen asks a story question -> elder answers -> save_memory
1:20 Family: "How is Paati this week?" -> weekly_summary -> alert -> notify_family
1:50 Dashboard: alert card + life-story memory cards
2:15 Architecture: MCP Streamable HTTP + Agent Skill + Bedrock (+ SNS)
2:30 Impact: caretaking on any Alexa+ device. Say "not medical advice".

## Friction log (copy into Devpost)
1. Task: install MCP Python SDK. Steps: pip install mcp, then `from mcp.server.fastmcp import FastMCP`. Expected: works. Actual: ModuleNotFoundError on mcp 2.x (FastMCP renamed MCPServer). Severity: medium. Workaround: pin mcp<2. Suggestion: pin SDK versions in starter docs and show the v2 migration notice in getting-started.
2. Task: follow the Streamable HTTP security guidance. Expected: one clear default. Actual: spec requires Origin validation and recommends binding to localhost, but quickstarts bind to 0.0.0.0. Severity: low-medium. Workaround: default HOST=127.0.0.1. Suggestion: show secure defaults in examples.
3. Task: find how to build MCP Apps in Python. Expected: Python guide. Actual: the official ext-apps SDK docs are Node/TypeScript-first. Severity: medium. Suggestion: add a Python MCP Apps quickstart.

## Product feedback draft
Tools used: MCP Python SDK (server, Streamable HTTP); Agent Skills (SKILL.md as system prompt at runtime); Amazon Bedrock Converse API with tool use (chat client); Amazon SNS (optional family alerts).
Worked well: FastMCP decorators made tools quick to define; stateless_http + json_response made curl and client testing simple; the spec is clear.
Needs work: v2 rename not flagged in intro docs; unclear how to test against real Alexa+ vs a simulator; Python MCP Apps guidance missing.
Onboarding: zero to hello world in under an hour. Build again: Yes, because MCP tools make voice agents composable and the Skill format keeps behavior reusable.

## Idea assessment (honest)
~7.5/10. Strong: emotional stakes, two user types, persistent state, life-story book. Weak: analysis is rule-based, only one real action (notify_family). Ways to raise score: real alert delivery (SNS/email) in the demo, Bedrock in the demo, MCP App dashboard. Keep a "not medical advice" disclaimer.
