# 🌿 Evergreen: Alexa+ companion for elder care and life stories

Elders talk to Alexa+. Evergreen logs mood and medication, spots declining trends, and gently collects life
stories into a family "life-story book". Families get a weekly digest, alerts, and memory cards.

**Track:** Alexa+ | **Mini challenges:** AWS Builder, Open Source

## Tech
- Self-hosted **MCP server** (official `mcp` Python SDK, **Streamable HTTP** at `/mcp`)
- **Agent Skill**: `skills/evergreen-care/SKILL.md`
- Persistent state across sessions (`data.json`)
- Family dashboard with memory cards at `/dashboard`

## Tools
`set_profile`, `log_checkin`, `suggest_story_prompt`, `save_memory`, `weekly_summary`

## Run
```bash
pip install -r requirements.txt
python seed.py            # demo data for "Paati"
python server.py          # MCP at http://localhost:8000/mcp
```
Open http://localhost:8000/dashboard?name=paati

Test with the MCP Inspector: `npx @modelcontextprotocol/inspector` then connect to `http://localhost:8000/mcp` (Streamable HTTP).

## License
MIT

## Bedrock chat client (AWS Builder)
`chat.py` runs Claude on **Amazon Bedrock** (Converse API with tool use) against the MCP server, loading the Agent Skill as its system prompt.
```bash
export AWS_REGION=us-east-1 BEDROCK_MODEL_ID=<a Claude model or inference profile enabled in your account>
python chat.py elder     # Paati's view
python chat.py family    # family view
```
Optional: set `ALERT_SNS_TOPIC_ARN` and `notify_family` also publishes alerts through **Amazon SNS**.
The server binds to 127.0.0.1 by default (set `HOST` to change it for deployment).
