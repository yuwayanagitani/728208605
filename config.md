# AI Card Divider — Configuration Guide (Numbered Keys)

This add-on reads settings from `config.json`.

To keep `config.json` **neatly ordered**, this version uses **numbered keys** like `01_...`, `02_...`, etc.
The add-on is also **backward compatible** with older unnumbered keys (e.g., `question_field`) if you still have them.

<span style="color:#d35400">Important:</span> API keys are **not** stored in `config.json`.  
Set them as environment variables and reference the variable names via the settings below.

---

## Recommended default `config.json` (matches the current defaults)

```json
{
  "01_question_field": "Front",
  "02_answer_field": "Back",
  "03_max_answer_chars": 220,
  "04_output_language": "English",
  "05_max_cards": 5,
  "06_provider": "openai",
  "07_openai_model": "gpt-4o-mini",
  "08_openai_api_key_env": "OPENAI_API_KEY",
  "09_openai_api_base": "https://api.openai.com/v1/chat/completions",
  "10_gemini_model": "gemini-2.5-flash-lite",
  "11_gemini_api_key_env": "GEMINI_API_KEY",
  "12_gemini_api_base": "https://generativelanguage.googleapis.com/v1beta",
  "13_temperature": 0.2,
  "14_max_output_tokens": 1200,
  "15_tag_for_new": "Split",
  "16_tag_for_original": "LongAnswer"
}
```

---

## Settings reference

### 01_question_field
The field name used as the **question** (prompt).  
Default: `Front`

### 02_answer_field
The field name used as the **answer**.  
Default: `Back`

### 03_max_answer_chars
Only notes whose answer length exceeds this number of characters will be processed.  
Default: `220`

### 04_output_language
Language used for both generated questions and answers.  
Default: `English`

### 05_max_cards
Maximum number of split cards created per original note.  
Default: `5`

---

## Provider selection

### 06_provider
Choose which AI provider to use.

- `openai` — OpenAI Chat Completions endpoint
- `gemini` — Google Gemini `generateContent` endpoint

Default: `openai`

---

## OpenAI settings (used when 06_provider = "openai")

### 07_openai_model
OpenAI model name.  
Default: `gpt-4o-mini`

### 08_openai_api_key_env
Environment variable name that stores your OpenAI API key.  
Default: `OPENAI_API_KEY`

### 09_openai_api_base
OpenAI endpoint URL (Chat Completions).  
Default: `https://api.openai.com/v1/chat/completions`

---

## Gemini settings (used when 06_provider = "gemini")

### 10_gemini_model
Gemini model name.  
Default: `gemini-2.5-flash-lite`

### 11_gemini_api_key_env
Environment variable name that stores your Gemini API key.  
Default: `GEMINI_API_KEY`

### 12_gemini_api_base
Gemini API base URL.  
Default: `https://generativelanguage.googleapis.com/v1beta`

The add-on will call:
- `https://generativelanguage.googleapis.com/v1beta/models/{model}:generateContent`

---

## Generation controls

### 13_temperature
Controls randomness. Lower values are more deterministic and usually produce cleaner JSON.  
Default: `0.2`

### 14_max_output_tokens
Maximum output tokens the model may generate.  
Default: `1200`

<span style="color:#2980b9">Tip:</span> If Gemini responses are truncated or JSON parsing fails, increasing this value is the first thing to try.

---

## Tagging behavior

### 15_tag_for_new
Tag added to **newly created split notes**.  
Default: `Split`

### 16_tag_for_original
Tag added to the **original source note** after splitting.  
Default: `LongAnswer`

This helps prevent accidental re-processing on subsequent runs.

---

## API key setup (environment variables)

Set your API keys in environment variables (names configured by `08_openai_api_key_env` and `11_gemini_api_key_env`).

### macOS / Linux (zsh/bash)
```bash
export OPENAI_API_KEY="your_openai_key_here"
export GEMINI_API_KEY="your_gemini_key_here"
```

### Windows (PowerShell)
```powershell
setx OPENAI_API_KEY "your_openai_key_here"
setx GEMINI_API_KEY "your_gemini_key_here"
```

<span style="color:#d35400">Important:</span> Restart Anki after setting environment variables.

---

## Troubleshooting

### “API key is missing…”
The configured environment variable is not set. Set it and restart Anki.

### “Failed to parse JSON…”
The model returned non-JSON or truncated JSON.

Try:
- Increase `14_max_output_tokens` (especially for Gemini)
- Reduce `05_max_cards`
- Keep `13_temperature` low (e.g., `0.0`–`0.2`)

---

## Summary

> Use numbered keys (`01_...` to `16_...`) to keep `config.json` tidy.  
> Choose a provider with `06_provider`, set your API key via environment variables, and increase `14_max_output_tokens` if Gemini outputs get truncated.
