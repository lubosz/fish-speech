# Voice Setup Guide

## 1. Reference Voice File Preparation

**Recommended Method (Direct File):**
```text
references/
├── test1.mp3
├── test1.lab
├── speaker2.wav
├── speaker2.lab
└── korean_voice.flac
```

**Legacy Method (Folder, Backward Compatibility):**
```text
references/
├── test1/
│   ├── sample1.wav
│   ├── sample1.lab
│   ├── sample2.wav
│   └── sample2.lab
└── speaker2/
    ├── voice1.mp3
    └── voice1.lab
```

## 2. .lab File Content Example

```text
# test1.lab
Hello, I am a test speaker.

# speaker2.lab
This is the second sample voice.
```

## 3. API Usage

### Using OpenAI Compatible API
```bash
curl -X POST "http://localhost:8080/v1/audio/speech" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "tts-1",
    "input": "Hello, this is a test voice.",
    "voice": "test1",
    "response_format": "mp3"
  }'
```

### Using Fish Speech Native API
```bash
curl -X POST "http://localhost:8080/v1/tts" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Hello, this is a test voice.",
    "reference_id": "test1",
    "format": "wav"
  }'
```

## 4. How It Works

- Converts OpenAI API's `voice: "test1"` → Fish Speech's `reference_id: "test1"`
- **Priority 1**: Uses a direct file like `references/test1.mp3` (or .wav, .flac, etc.)
- **Priority 2**: Uses all audio files in the `references/test1/` folder (backward compatibility)
- If a `.lab` file with the same name exists, it is used together as a text prompt.

## 5. File Priority

The system searches for files in the following order:
1. `references/test1.mp3`
2. `references/test1.wav`
3. `references/test1.flac`
4. ... (Other supported extensions)
5. Files inside the `references/test1/` folder (Fallback)