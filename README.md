# Kokoro Web Text-to-Speech (TTS) API Documentation

## Introduction
The Kokoro Web TTS API provides a powerful and flexible interface for generating high-quality speech from text. Deployed via Docker, this API supports multiple languages, voices, and models, making it suitable for applications ranging from voice assistants to animation syncing. This documentation covers all available endpoints, their parameters, example requests and responses, available voices and models, and integration with OpenAI-compatible SDKs.

**Base URL**: `http://host.docker.internal:3000/api/v1`

> **Note**: Ensure the Docker container is running and accessible at the specified host URL. Replace `host.docker.internal` with the appropriate host if accessing from a different environment.

## Endpoints

### 1. List Available Languages
- **Endpoint**: `GET /api/v1/audio/langs`
- **Purpose**: Retrieve a list of supported language codes for text-to-speech generation.
- **Response**: A JSON object containing an array of language codes (e.g., `en`, `es`, `fr`).
- **Use Case**: Use these language codes to ensure proper pronunciation in speech generation requests.

**Example Request**:
```bash
curl -X GET http://host.docker.internal:3000/api/v1/audio/langs
```

**Example Response**:
```json
{
  "langs": ["en-us", "en-gb", "es-419", "ja", "cmn", "hi", "it", "pt-br"]
}
```

### 2. List Available Models
- **Endpoint**: `GET /api/v1/audio/models`
- **Purpose**: Retrieve a list of available TTS models (e.g., quantized versions like `model_q8f16` or `model_q4_0`).
- **Response**: A JSON object containing an array of model names.
- **Use Case**: Select a model for use in the `/audio/speech` endpoint.

**Example Request**:
```bash
curl -X GET http://host.docker.internal:3000/api/v1/audio/models
```

**Example Response**:
```json
{
  "models": ["model_fp32", "model_q4", "model_uint8", "model_fp16", "model_q4f16", "model_uint8f16", "model_quantized", "model_q8f16"]
}
```

### 3. Generate Audio from Text
- **Endpoint**: `POST /api/v1/audio/speech`
- **Purpose**: Convert input text into audio using a specified model and voice.
- **Parameters**:
  - `model` (string, required): The TTS model to use (e.g., `model_q8f16`). See `/audio/models` for available options.
  - `voice` (string, required): The voice style to use (e.g., `af_heart`). See `/audio/voices` for available options.
  - `input` (string, required): The text to convert to speech.
  - `response_format` (string, optional): The output audio format (e.g., `mp3`, `pcm`, `wav`). Default is `mp3`.
  - `speed` (float, optional): Speed multiplier for the audio (e.g., `1.0` for normal speed).
- **Response**: Binary audio file in the specified format.
- **Use Case**: Generate audio for narration, voiceovers, or interactive applications.

**Example Request**:
```bash
curl -X POST http://host.docker.internal:3000/api/v1/audio/speech \
  -H "Content-Type: application/json" \
  -d '{
        "model": "model_q8f16",
        "voice": "af_heart",
        "input": "Hello world! This is a test.",
        "response_format": "mp3",
        "speed": 1.0
      }' \
  --output output.mp3
```

**Output**: A binary MP3 file (`output.mp3`) saved to the local directory.

### 4. List Available Voices
- **Endpoint**: `GET /api/v1/audio/voices`
- **Purpose**: Retrieve a list of available voice styles for speech generation.
- **Response**: A JSON object containing an array of voice identifiers.
- **Use Case**: Choose a voice style for the `/audio/speech` endpoint.

**Example Request**:
```bash
curl -X GET http://host.docker.internal:3000/api/v1/audio/voices
```

**Example Response**:
```json
{
  "voices": ["af_heart", "af_alloy", "af_aoede", "af_bella", "af_jessica", "af_kore", "af_nicole", "af_nova", "af_river", "af_sarah", "af_sky", "am_adam", "am_echo", "am_eric", "am_fenrir", "am_liam", "am_michael", "am_onyx", "am_puck", "am_santa", "bf_emma", "bf_isabella", "bm_george", "bm_lewis", "bf_alice", "bf_lily", "bm_daniel", "bm_fable", "ef_dora", "em_alex", "em_santa", "jf_alpha", "jf_gongitsune", "jf_nezumi", "jf_tebukuro", "jm_kumo", "zf_xiaobei", "zf_xiaoni", "zf_xiaoxiao", "zf_xiaoyi", "zm_yunjian", "zm_yunxi", "zm_yunxia", "zm_yunyang", "hf_alpha", "hf_beta", "hm_omega", "hm_psi", "if_sara", "im_nicola", "pf_dora", "pm_alex", "pm_santa"]
}
```

### 5. Generate Phonemes from Text
- **Endpoint**: `POST /api/v1/text/phonemize`
- **Purpose**: Convert input text into phonemes for advanced pronunciation control or animation syncing.
- **Parameters**:
  - `text` (string, required): The text to convert into phonemes.
  - `lang` (string, required): The language code (e.g., `en-us`). See `/audio/langs` for available options.
- **Response**: A JSON object containing an array of phonemes.
- **Use Case**: Useful for lip-syncing in animations or fine-tuning pronunciation.

**Example Request**:
```bash
curl -X POST http://host.docker.internal:3000/api/v1/text/phonemize \
  -H "Content-Type: application/json" \
  -d '{
        "text": "Hello world!",
        "lang": "en-us"
      }'
```

**Example Response**:
```json
{
  "phonemes": ["HH", "EH", "L", "OW", " ", "W", "ER", "L", "D"]
}
```

## Available Voices
The following table lists all available voices, including their IDs, languages, genders, target quality, and overall grades. Voices are grouped by language for clarity.

| ID            | Name          | Language         | Gender | Target Quality | Overall Grade |
|---------------|---------------|------------------|--------|----------------|---------------|
| **English (US)** | | | | | |
| af_heart      | Heart         | English (US)     | Female | A              | A             |
| af_alloy      | Alloy         | English (US)     | Female | B              | C             |
| af_aoede      | Aoede         | English (US)     | Female | B              | C+            |
| af_bella      | Bella         | English (US)     | Female | A              | A-            |
| af_jessica    | Jessica       | English (US)     | Female | C              | D             |
| af_kore       | Kore          | English (US)     | Female | B              | C+            |
| af_nicole     | Nicole        | English (US)     | Female | B              | B-            |
| af_nova       | Nova          | English (US)     | Female | B              | C             |
| af_river      | River         | English (US)     | Female | C              | D             |
| af_sarah      | Sarah         | English (US)     | Female | B              | C+            |
| af_sky        | Sky           | English (US)     | Female | B              | C-            |
| am_adam       | Adam          | English (US)     | Male   | D              | F+            |
| am_echo       | Echo          | English (US)     | Male   | C              | D             |
| am_eric       | Eric          | English (US)     | Male   | C              | D             |
| am_fenrir     | Fenrir        | English (US)     | Male   | B              | C+            |
| am_liam       | Liam          | English (US)     | Male   | C              | D             |
| am_michael    | Michael       | English (US)     | Male   | B              | C+            |
| am_onyx       | Onyx          | English (US)     | Male   | C              | D             |
| am_puck       | Puck          | English (US)     | Male   | B              | C+            |
| am_santa      | Santa         | English (US)     | Male   | C              | D-            |
| **English (UK)** | | | | | |
| bf_emma       | Emma          | English (UK)     | Female | B              | B-            |
| bf_isabella   | Isabella      | English (UK)     | Female | B              | C             |
| bm_george     | George        | English (UK)     | Male   | B              | C             |
| bm_lewis      | Lewis         | English (UK)     | Male   | C              | D+            |
| bf_alice      | Alice         | English (UK)     | Female | C              | D             |
| bf_lily       | Lily          | English (UK)     | Female | C              | D             |
| bm_daniel     | Daniel        | English (UK)     | Male   | C              | D             |
| bm_fable      | Fable         | English (UK)     | Male   | B              | C             |
| **Spanish** | | | | | |
| ef_dora       | Dora          | Spanish          | Female | C              | D             |
| em_alex       | Alex          | Spanish          | Male   | C              | D             |
| em_santa      | Santa         | Spanish          | Male   | C              | D             |
| **Japanese** | | | | | |
| jf_alpha      | Alpha         | Japanese         | Female | B              | C+            |
| jf_gongitsune | Gongitsune    | Japanese         | Female | B              | C             |
| jf_nezumi     | Nezumi        | Japanese         | Female | B              | C-            |
| jf_tebukuro   | Tebukuro      | Japanese         | Female | B              | C             |
| jm_kumo       | Kumo          | Japanese         | Male   | B              | C-            |
| **Chinese** | | | | | |
| zf_xiaobei    | Xiaobei       | Chinese          | Female | C              | D             |
| zf_xiaoni     | Xiaoni        | Chinese          | Female | C              | D             |
| zf_xiaoxiao   | Xiaoxiao      | Chinese          | Female | C              | D             |
| zf_xiaoyi     | Xiaoyi        | Chinese          | Female | C              | D             |
| zm_yunjian    | Yunjian       | Chinese          | Male   | C              | D             |
| zm_yunxi      | Yunxi         | Chinese          | Male   | C              | D             |
| zm_yunxia     | Yunxia        | Chinese          | Male   | C              | D             |
| zm_yunyang    | Yunyang       | Chinese          | Male   | C              | D             |
| **Hindi** | | | | | |
| hf_alpha      | Alpha         | Hindi            | Female | B              | C             |
| hf_beta       | Beta          | Hindi            | Female | B              | C             |
| hm_omega      | Omega         | Hindi            | Male   | B              | C             |
| hm_psi        | Psi           | Hindi            | Male   | B              | C             |
| **Italian** | | | | | |
| if_sara       | Sara          | Italian          | Female | B              | C             |
| im_nicola     | Nicola        | Italian          | Male   | B              | C             |
| **Portuguese (Brazil)** | | | | | |
| pf_dora       | Dora          | Portuguese (Brazil) | Female | C              | D             |
| pm_alex       | Alex          | Portuguese (Brazil) | Male   | C              | D             |
| pm_santa      | Santa         | Portuguese (Brazil) | Male   | C              | D             |

### Recommended Voices
Based on the `targetQuality` and `overallGrade` metrics, the best voices are those with `targetQuality` of A and high `overallGrade` (A or A-). The top recommended voices are:

| ID         | Name  | Language     | Gender | Target Quality | Overall Grade |
|------------|-------|--------------|--------|----------------|---------------|
| af_heart   | Heart | English (US) | Female | A              | A             |
| af_bella   | Bella | English (US) | Female | A              | A-            |

**Recommendation**: Use `af_heart` for the highest quality and natural-sounding English (US) female voice, followed by `af_bella` for slightly less but still excellent quality.

### Available Models
The following table lists all available TTS models, including their quantization type and size. Models are ordered from best to worst based on implied quality (higher precision and larger size typically indicate better quality, though smaller models may be faster).

| ID                | Quantization                     | Size    |
|-------------------|----------------------------------|---------|
| model_fp32        | fp32                             | 326 MB  |
| model_q4          | 4-bit matmul                    | 305 MB  |
| model_uint8       | 8-bit & mixed precision          | 177 MB  |
| model_fp16        | fp16                             | 163 MB  |
| model_q4f16       | 4-bit matmul & fp16 weights      | 154 MB  |
| model_uint8f16    | Mixed precision                  | 114 MB  |
| model_quantized   | 8-bit                            | 92.4 MB |
| model_q8f16       | Mixed precision                  | 86 MB   |

### Recommended Models
Based on the quantization and size, the best models for high-quality output are:

1. **model_fp32** (326 MB): Full 32-bit floating-point precision, offering the highest quality but largest size and potentially slower processing.
2. **model_q4** (305 MB): 4-bit matrix multiplication, balancing high quality with a slightly smaller size.
3. **model_fp16** (163 MB): 16-bit floating-point precision, providing good quality with reduced size compared to `model_fp32`.

**Recommendation**: Use `model_fp32` for the highest quality audio output, especially for production environments where quality is critical. For faster processing with minimal quality loss, consider `model_fp16` or `model_q4`.

## Summary Table
| Endpoint                     | Method | Description                          |
|------------------------------|--------|--------------------------------------|
| `/api/v1/audio/langs`        | GET    | List supported languages            |
| `/api/v1/audio/models`       | GET    | List available TTS models           |
| `/api/v1/audio/voices`       | GET    | List available voice styles         |
| `/api/v1/audio/speech`       | POST   | Generate audio from text            |
| `/api/v1/text/phonemize`     | POST   | Convert text to phonemes            |

## Using with OpenAI-Compatible SDKs
The Kokoro Web TTS API is compatible with OpenAI SDKs, allowing seamless integration with existing workflows. Below is an example using the JavaScript (Node.js) OpenAI SDK to generate speech with a recommended voice and model.

**Example JavaScript Code**:
<xaiArtifact artifact_id="597d2db1-fed7-494a-91b9-328945aac53f" artifact_version_id="b5fb3811-f3bc-46c4-ad02-aa619320f810" title="tts_example.js" contentType="text/javascript">
import OpenAI from "openai";
import fs from "fs/promises";

const openai = new OpenAI({
  baseURL: "http://host.docker.internal:3000/api/v1",
  apiKey: "not-needed-if-no-auth",
});

async function generateSpeech() {
  try {
    const mp3 = await openai.audio.speech.create({
      model: "model_fp32",
      voice: "af_heart",
      input: "Today is a wonderful day to build something people love!",
      response_format: "mp3",
    });
    const buffer = Buffer.from(await mp3.arrayBuffer());
    await fs.writeFile("output.mp3", buffer);
    console.log("Audio file saved as output.mp3");
  } catch (error) {
    console.error("Error generating speech:", error);
  }
}

generateSpeech();
