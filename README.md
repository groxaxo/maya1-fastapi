# Maya1 - Text-to-Speech


## Quick Start

### 1. Install
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 2. Configure
```bash
# Create .env file
echo "MAYA1_MODEL_PATH=maya-research/maya1" > .env
echo "HF_TOKEN=your_token_here" >> .env

# Login to HuggingFace
huggingface-cli login
```

### 3. Start Server
```bash
./server.sh start
# Server runs on http://localhost:8000
```

### 4. Generate Speech
```bash
curl -X POST "http://localhost:8000/v1/tts/generate" \
  -H "Content-Type: application/json" \
  -d '{
    "description": "Male voice in their 30s with american accent",
    "text": "Hello world <excited> this is amazing!",
    "stream": false
  }' \
  --output output.wav
```

## API

### Native API

**Endpoint:** `POST /v1/tts/generate`

**Request:**
```json
{
  "description": "Voice description",
  "text": "Text with <emotion> tags",
  "temperature": 0.4,
  "max_tokens": 500,
  "stream": false
}
```

**Response:** WAV audio file (24kHz, 16-bit mono)

### OpenAI-Compatible API (Open WebUI Integration)

This server is fully compatible with Open WebUI's TTS integration using OpenAI-compatible endpoints.

**List Models:** `GET /v1/models`

**List Voices:** `GET /v1/audio/voices`

**Generate Speech:** `POST /v1/audio/speech`

**Request:**
```json
{
  "model": "maya1-tts",
  "input": "Hello world",
  "voice": "default",
  "response_format": "wav",
  "speed": 1.0
}
```

**Available voices:** `default`, `male`, `female`

**Response formats:** `wav`, `pcm` (Note: `speed` parameter is accepted for compatibility but not currently implemented)

#### Open WebUI Configuration

1. In Open WebUI, go to **Admin Panel** → **Settings** → **Audio**
2. Set **Text-to-Speech Engine** to **"Custom TTS"**
3. Enter the base URL: `http://localhost:8000/v1` (or your server URL)
4. Save and test the TTS functionality

## Emotion Tags

`<angry>`, `<chuckle>`, `<cry>`, `<curious>`, `<disappointed>`, `<excited>`, `<exhale>`, `<gasp>`, `<giggle>`, `<gulp>`, `<laugh>`, `<laugh_harder>`, `<mischievous>`, `<sarcastic>`, `<scream>`, `<sigh>`, `<sing>`, `<snort>`, `<whisper>`

## Commands

```bash
./server.sh start    # Start server
./server.sh stop     # Stop server
./server.sh restart  # Restart server
./server.sh status   # Check status
```

## License

MIT