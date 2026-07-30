# HomeMadeAlexa

HomeMadeAlexa is a Python-based project providing tools and example code to build a custom Alexa skill and local/home automation integrations. This repository contains the core logic, example intent handlers, and helper scripts to test and run an Alexa skill locally or deploy to a serverless environment.

> Note: This README is intentionally generic so it can be adapted to your repository's exact architecture and filenames. Replace placeholders (e.g., `app.py`, `requirements.txt`, and environment variable names) with values used in your project.

## Table of contents
- [Features](#features)
- [Architecture](#architecture)
- [Requirements](#requirements)
- [Quick start](#quick-start)
- [Configuration](#configuration)
- [Usage examples](#usage-examples)
- [Testing](#testing)
- [Development](#development)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)

## Features
- Example Alexa skill intent handlers written in Python
- Local development tooling to test skill endpoints
- Templates for integrating with home devices or APIs (MQTT, HTTP, GPIO)
- Optional deployment-ready structure for AWS Lambda or other serverless platforms
- Unit tests and example utterances

## Architecture
This project is organized in a simple, modular way:
- `app.py` (or `server.py`): entrypoint that exposes an HTTP endpoint for Alexa skill requests (replace with your file).
- `homemadealexa/` (or `src/`): package containing handlers, utils, and business logic.
  - `handlers/`: Alexa intent handlers (LaunchRequest, IntentRequests, SessionEndedRequest)
  - `devices/`: device integration adapters (MQTT, REST, GPIO)
  - `utils/`: helper functions and common utilities
- `tests/`: unit and integration tests
- `examples/`: sample utterances, interaction model JSON, and configuration examples

Adjust these names to match your repository layout.

## Requirements
- Python 3.10+ (or appropriate supported version)
- pip
- Optional: virtualenv, ngrok (for local HTTPS tunnel), AWS CLI & SAM/Serverless framework (for deployment)

Install base dependencies:
```bash
python -m venv venv
source venv/bin/activate   # on Windows: venv\Scripts\activate
pip install -r requirements.txt
```

## Quick start (local)
1. Create and activate a virtual environment (see above).
2. Install dependencies:
   pip install -r requirements.txt
3. Run the development server (replace `app.py` with your entrypoint if different):
```bash
python app.py
# or, if a Flask app:
# export FLASK_APP=app.py
# flask run --port 5000
```
4. If you want Alexa to reach your local server during development, expose it over HTTPS (ngrok example):
```bash
ngrok http 5000
# copy the generated HTTPS URL and use it as the endpoint in the Alexa Developer Console
```

## Configuration
Store secrets and environment-specific settings either in environment variables or a `.env` file (use a library like python-dotenv to load it).

Common environment variables:
- ALEXA_SKILL_ID: your Alexa Skill ID (to validate requests)
- ALEXA_APP_ID: application identifier
- FLASK_ENV / PYTHON_ENV: `development` or `production`
- NGROK_AUTH_TOKEN: if automating ngrok during development
- DEVICE_API_KEY / MQTT_BROKER_URL: for device integrations

Example `.env`:
```
ALEXA_SKILL_ID=amzn1.ask.skill.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
DEVICE_API_KEY=replace_with_real_key
MQTT_BROKER_URL=mqtt://user:pass@broker.example.com:1883
```

## Usage examples

Example intent handler flow (high-level)
- Alexa sends an IntentRequest POST to your endpoint.
- Your server verifies the request signature and skill ID.
- The request is dispatched to an intent handler (e.g., `TurnOnLightIntent`).
- The handler calls a device adapter (MQTT/HTTP) to perform the action.
- The handler returns a JSON response Alexa understands.

Sample utterances (examples/interaction-model.json):
- "Turn on the kitchen lights"
- "Set the living room temperature to 22 degrees"
- "What's the status of the front door?"

Sample response format (simplified):
```json
{
  "version": "1.0",
  "response": {
    "outputSpeech": {
      "type": "PlainText",
      "text": "Okay, I turned on the kitchen lights."
    },
    "shouldEndSession": true
  }
}
```

## Testing
- Unit tests: use pytest or unittest.
- Run tests:
```bash
pip install -r requirements-dev.txt
pytest -q
```
- Integration: test using the Alexa Developer Console simulator or by sending signed requests (use the amazon-alexa-verifier for local testing or ngrok to forward live requests).

## Development
- Follow code style and linting rules:
```bash
flake8 .
black .
```
- Add new intent handlers under `homemadealexa/handlers/` and register them in the request dispatcher.
- When adding device integrations, include a mocked adapter implementation under `devices/mock_*.py` for tests.

## Troubleshooting
- Skill endpoint not reachable: ensure server is running and reachable via HTTPS (ngrok recommended).
- Request signature errors: confirm the request is forwarded with original headers and that skill ID validation is correct.
- Missing dependencies: double-check `requirements.txt` and your active virtual environment.

---

If you'd like, I can:
- Tailor this README to match your actual repository structure (I can read the repo and update file names and commands).
- Create and commit the README.md to your repository (I can do that if you confirm the target repo).
