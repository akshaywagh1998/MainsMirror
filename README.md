# MainsMirror

A prototype AI examiner for UPSC/IFoS Mains practice. The intended workflow accepts photos of handwritten answers and returns rubric-based feedback, a model answer, an exam-style diagram, and a spaced-repetition drill.

## Run locally

    export MODEL_BASE_URL=https://openrouter.ai/api/v1
    export MODEL_NAME=meta-llama/llama-4-scout:free
    export MODEL_API_KEY=<your-provider-key>
    python3 app/server.py

The code expects an OpenAI-compatible chat-completions endpoint. Availability, price, rate limits, and image support depend on the provider and must be checked before use. This repository has not been verified against a live Meta model route. Do not treat local mock results as model output.

## Local checks

    python3 -m unittest discover -s tests -v

The tests call local route handlers with stubbed model replies. They do not call an external API, prove grading quality, or constitute a browser security audit. The mock rail is for local testing only.

## Safety

Model-provided text is rendered as text nodes. Diagram SVG is parsed on the server and reduced to an allowlist of simple SVG elements and attributes; the browser imports the parsed SVG DOM. Keep the application private until a real browser/security review and a live inference test are complete.

## API

- `POST /api/evaluate`: multipart image field `images` (up to 4 files, 8 MiB each) or JSON `{"images":["data:image/jpeg;base64,..."]}`
- `POST /api/diagram`: JSON `{"evaluation": ...}`
- `POST /api/drill`: JSON `{"evaluation": ...}`
- `GET /health`
