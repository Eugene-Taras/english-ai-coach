# ENG‑Coach — AI tutor in Telegram
ENG‑Coach is an AI-powered English tutor in Telegram. Practice translation, get feedback with explanations, access grammar theory, and hear native-like audio. Covers levels A1–B2 with multilingual UI. Built solo in 3 months with tests and architecture. Suitable for EdTech pilots or personal use.
- A smart English trainer in Telegram: clear error explanations, contextual theory, and TTS — all in one dialogue.
- Fast learning cycle: do → make a mistake → understand ↔ fix → reinforce.

**Try the demo:** [@YourEnglishAICoachBot](https://t.me/YourEnglishAICoachBot)  
**Looking for EdTech pilots/collaboration →** aiengcoach@gmail.com


> Solo project built in ~3 months. No prior IT background; AI used as an engineering assistant.  
> A working Telegram bot with multiple explanation styles, error analysis, theory, and audio.

---

## How it works
### Step-by-step: Try it in 30 seconds
1. Press **Start** in the bot.
2. Choose the explanation language, your level (A1–B2), and mode. There are two modes:
   **Live feedback** — a human-like explanation in natural language; **Analysis + Theory** — structured analysis with error topics, a **Second chance**, and **Theory** / **Detailed analysis** buttons.
3. The bot will send a phrase in the selected language to translate into English. Task language and explanation language can differ, so you can practice translation while receiving tips in your native language.
4. Send your answer.
5. If the answer is correct — great! Tap **Listen** to hear the reference pronunciation and move on to the next task.
6. If there’s a mistake:
   * In **Analysis + Theory**, the bot shows the error topics, gives a **Second chance**, and provides **Theory** and **Detailed analysis** buttons.
   * In **Live feedback**, you get a human-like explanation in natural language.
7. After the analysis or correction, proceed to the next task.

### Video demos
- Correct answer → TTS: [watch](https://github.com/user-attachments/assets/55180dcb-49b5-4309-9084-bb7cb7f1d309)
- Second chance (hint → retry): [watch](https://github.com/user-attachments/assets/50ff4d49-a19c-4bcd-ad43-6960c9094481)
- Multiple errors (RU): [watch](https://github.com/user-attachments/assets/40c266a1-0ea6-4f2b-a815-a591442edc34)
- Multilingual UI: [watch](https://github.com/user-attachments/assets/62f2b85b-ef1e-4590-b145-aa84f0e55c20)

### Feature highlights with screenshots

<table>
  <tr>
    <td>
      <a href="https://github.com/user-attachments/assets/d1ae45b4-b5c9-4bb5-b06e-1bd9d4a2f1e5">
        <img src="https://github.com/user-attachments/assets/d1ae45b4-b5c9-4bb5-b06e-1bd9d4a2f1e5" width="400" alt="Error analysis + second chance" />
      </a>
      <div align="center"><sub>Error analysis + second chance</sub></div>
    </td>
    <td>
      <a href="https://github.com/user-attachments/assets/a2e9a785-edc4-4be3-a772-367505104a10">
        <img src="https://github.com/user-attachments/assets/a2e9a785-edc4-4be3-a772-367505104a10" width="400" alt="Reference audio (TTS)" />
      </a>
      <div align="center"><sub>Reference audio (TTS)</sub></div>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/user-attachments/assets/6e5863e2-2d0e-4ce0-b9c7-069b43a23522">
        <img src="https://github.com/user-attachments/assets/6e5863e2-2d0e-4ce0-b9c7-069b43a23522" width="400" alt="Multilingual UI" />
      </a>
      <div align="center"><sub>Multilingual UI</sub></div>
    </td>
    <td>
      <a href="https://github.com/user-attachments/assets/a268b06e-a6e7-4771-b843-3bd058683abe">
        <img src="https://github.com/user-attachments/assets/a268b06e-a6e7-4771-b843-3bd058683abe" width="400" alt="Theory — Present Simple" />
      </a>
      <div align="center"><sub>Theory — Present Simple</sub></div>
    </td>
  </tr>
</table>

---

## What’s implemented

- Dialog practice: prompts, mistakes, explanations
- Flexible analysis — not tied to a single “correct” answer (accepts valid paraphrases)
- “Second chance” system: gentle hint with retry option
- AI explanations: “Classic” (plain) and “Technical” (structured)
- Theory on demand — right below the specific error
- TTS for correct variants (yours or corrected)
- Levels A1–B2; multilingual UI
- Inappropriate/toxic message filter

---

## Roadmap
- Full‑fledged tutor personas tailored to specific language groups
- “Quest” mode with narrative voice‑over (story + test): live episodes, branching answers, inline explanations, a final mini‑test per chapter
- Characters that persist across modes
- Audio skills: “Audio dictation” (listen → type)
- Peer practice & co-op quests: 1-on-1 auto-match by level, timed rooms, simple moderation
- Progress: personal stats and difficulty adaptation; task selection based on error history
- Cross‑platform expansion: native iOS/Android apps and a web client; feature parity with Telegram, synced progress and settings
- Shared API layer: expose core as HTTP API; account linking (Telegram ↔ email), push notifications; privacy/export controls

---

## Who it’s for
- Self‑learners and busy people — short daily practice
- Anyone who needs understandable feedback, not just a “verdict”
- Teachers — as a supplementary practice tool or homework trainer

---

## Architecture

```mermaid
graph TD

    subgraph DEV["Development processes (offline)"]
        direction LR
        YML_SOURCES["exercise_sources/*.yml"]
        BUILD_SCRIPT["build_exercises.py"]
        UTILS_SCRIPTS["utils/*.py"]

        YML_SOURCES -->|Read by| BUILD_SCRIPT
        UTILS_SCRIPTS -- "Generation / Translation" --> YML_SOURCES
        UTILS_SCRIPTS -- "Generation / Translation" --> THEORY_SOURCES
    end

    subgraph APP["Telegram bot application"]
        direction TB

        subgraph UI_LAYER["UI and handlers layer"]
            BOT_PY["bot.py - entry point"]
            HANDLERS["handlers/*.py"]
            KEYBOARDS["keyboards.py"]
        end

        subgraph CORE["Core business logic"]
            PRACTICE_ENGINE["practice_engine.py"]
            THEORY_ENGINE["theory_engine.py"]
            ERROR_TRACKING["error_tracking.py"]
            GEMINI_CLIENT["gemini_client.py"]
            PROFILE_ENGINE["profile_engine.py"]
            LOCALIZATION["localization.py"]
            TTS_ENGINE["tts_engine.py"]
        end

        subgraph DATA["Data layer (shared)"]
            USER_PROFILES_DB["user_profiles.db"]
            EXERCISES_JSON["core/exercises.json"]
            THEORY_SOURCES["theory_sources"]
            LOCALES["locales"]
            AUDIO_CACHE["audio_cache"]
        end
    end

    BUILD_SCRIPT -->|Generates| EXERCISES_JSON

    BOT_PY -- "Registers" --> HANDLERS
    HANDLERS -->|Requests| PRACTICE_ENGINE
    HANDLERS -->|Requests| THEORY_ENGINE
    HANDLERS -->|Requests| PROFILE_ENGINE
    HANDLERS -->|User reply| ERROR_TRACKING
    HANDLERS -->|UI composition| KEYBOARDS
    HANDLERS -->|TTS requests| TTS_ENGINE
    KEYBOARDS -->|Button texts| LOCALIZATION

    ERROR_TRACKING -->|AI request| GEMINI_CLIENT
    PRACTICE_ENGINE -->|Reads| EXERCISES_JSON
    THEORY_ENGINE -->|Reads| THEORY_SOURCES
    PROFILE_ENGINE -->|DB access| USER_PROFILES_DB
    LOCALIZATION -->|Reads| LOCALES
    TTS_ENGINE -->|Cache access| AUDIO_CACHE
```

---

## Quality approach
Quality and stability are key priorities. A multi‑layer automated testing setup includes:
- **Unit tests:** validate each module in isolation (from TTS engine to the profile system)
- **Integration tests:** ensure correct interaction of core components
- **E2E tests:** emulate full user scenarios (answer → error analysis → theory request)
- **Data completeness tests:** automatically verify the presence of theory and localization files for all topics

---

## Development process
Development happens in an isolated environment (`telegram_test`) to add features without risking real users. The public demo (`telegram_demo`) is updated only after full testing and stabilization, ensuring reliability and quality for end users.

---

## Technologies

**Runtime & concurrency**
- Python 3.11+, asyncio, logging

**Interface**
- python‑telegram‑bot 22.x (ApplicationBuilder, Command/Message/Callback handlers, Reply/Inline keyboards)

**Data**
- SQLite (user_profiles.db) + aiosqlite (async DB access)

**AI & integrations**
- OpenAI SDK, Google Generative AI
- python‑dotenv — load tokens/config from `.env`

**Speech (TTS)**
- gTTS — audio generation
- File cache (name derived from text/mode/level hash, directory `audio_cache/{lang}/{mode}/{hash}.mp3`)

**Configuration & content**
- PyYAML — build exercises from `exercise_sources/*.yml` into `core/exercises.json`
- JSON (localization/taxonomy), Markdown (theory)

**Testing**
- pytest (unit, integration, E2E) + pytest‑asyncio/anyio for async scenarios

---

## What I learned
- Running a product from scratch with AI as a technical partner
- Designing live learning scenarios (contextual theory)
- Building robust Telegram bots with high‑quality feedback

---

## Demo
> Telegram: [@YourEnglishAICoachBot](https://t.me/YourEnglishAICoachBot)

---

## Contact
Creator: Eugene Taras  
<a href="mailto:aiengcoach@gmail.com">aiengcoach@gmail.com</a>
