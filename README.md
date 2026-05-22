# ⚙️ Pipeline Automatizat de YouTube Shorts (YouTube Automation)

Acest repository conține documentația tehnică și schema de arhitectură pentru un **Sistem Automatizat și Autonom de YouTube Shorts / Reels**, proiectat conform principiilor de **Analiză și Proiectare a Sistemelor Informatice (APSI)**.

Sistemul funcționează 100% autonom după configurarea inițială, utilizând o arhitectură bazată pe microservicii și o buclă de învățare bazată pe date reale (Feedback Loop). Scopul principal este generarea de conținut video extrem de optimizat pentru algoritmul YouTube pe nișa **AI Pet Saloon**.

---

## 🗺️ Arhitectura Generală a Sistemului

Sistemul este condus de un **Orchestrator Central** scris în Python, care coordonează o serie de module specializate (locale și externe):

```mermaid
graph TD
    %% DECLANȘATOR & BAZĂ DE DATE
    CRON((🕒 CRON Job<br/>Ex: Ora 03:00)) -->|Trezire Sistem| CORE{⚙️ Orchestratorul Python<br/>Dirijorul Central}
    CORE <-->|Verifică istoric / Salvează progres| DB[(🗄️ Baza de Date SQLite)]

    %% BUCLA DE ÎNVĂȚARE
    subgraph "📊 1. Modulul de Învățare (Analiză)"
        CORE -->|Verifică clipuri vechi de 48h| YT_ANA[📈 YouTube Analytics API]
        YT_ANA -->|Extrage: Retenție, Swipe-Away| DB
    end

    %% CREAȚIE
    subgraph "🧠 2. Modulul de Creație (AI-ul Extern)"
        CORE -->|Prompt Dinamic + Date din DB| LLM[📝 OpenAI GPT-4o<br/>Aplică Curba BRUHZEN & The Loop]
        LLM -->|Script optimizat| CORE
        CORE -->|Trimite textul (cu emoție)| TTS[🗣️ ElevenLabs API]
        TTS -->|Audio .mp3| CORE
        CORE -->|Caută material vizual| STOCK[📥 Pexels API / Video Folder]
        STOCK -->|Video brut .mp4| CORE
    end

    %% ASAMBLARE LOCALĂ
    subgraph "🛠️ 3. Modulul de Asamblare (Motorul Local)"
        CORE -->|Trimite Audio| ASR[👂 Whisper ASR]
        ASR -->|Marcaje de timp milisecundă| CORE
        CORE -->|Trimite toate fișierele| FFMPEG[🎬 FFmpeg / MoviePy]
        FFMPEG -->|Video Final 100% Unic .mp4| CORE
    end

    %% PUBLICARE
    subgraph "🚀 4. Modulul de Publicare (Livrarea)"
        CORE -->|Încarcă Video + Titlu + Taguri| YT_PUB[🔴 YouTube Data API]
        YT_PUB -->|Status: PUBLICAT| DB
    end
