# Speakr — Référence complète des variables d'environnement

> Compilé depuis la documentation, les fichiers `.example` et les release notes.
> Sources : `env.transcription.example`, `env.sso.example`, docs officielles, release notes v0.5→v0.8.20

---

## 🔧 TRANSCRIPTION — Sélection du connecteur

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `TRANSCRIPTION_CONNECTOR` | `openai_whisper` `openai_transcribe` `asr_endpoint` `mistral` `vibevoice` `azure_openai_transcribe` | auto-détecté | Laisser vide = détection automatique |
| `TRANSCRIPTION_API_KEY` | `sk-xxx` | — | Clé OpenAI, Mistral, Azure selon connecteur |
| `TRANSCRIPTION_BASE_URL` | `https://api.openai.com/v1` | OpenAI | URL base du provider |
| `TRANSCRIPTION_MODEL` | `gpt-4o-transcribe-diarize` `whisper-1` `voxtral-mini-latest` | `whisper-1` | Détermine aussi le connecteur si non forcé |
| `WHISPER_MODEL` | `whisper-1` | — | ⚠️ Déprécié → utiliser `TRANSCRIPTION_MODEL` |
| `USE_ASR_ENDPOINT` | `true` | `false` | ⚠️ Déprécié → poser `ASR_BASE_URL` suffit |
| `USE_NEW_TRANSCRIPTION_ARCHITECTURE` | `true` `false` | `true` | Mettre à `false` pour déboguer avec l'ancien code |

---

## 🎤 ASR ENDPOINT — Service auto-hébergé

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ASR_BASE_URL` | `http://whisperx-asr:9000` | — | Active automatiquement le connecteur ASR |
| `ASR_DIARIZE` | `true` `false` | `true` | Activer la diarisation |
| `ASR_MIN_SPEAKERS` | `1` | — | Aide la précision de la diarisation |
| `ASR_MAX_SPEAKERS` | `5` | — | Aide la précision de la diarisation |
| `ASR_RETURN_SPEAKER_EMBEDDINGS` | `true` `false` | `false` | **WhisperX uniquement** — active les profils vocaux |
| `ASR_TIMEOUT` | `1800` | `1800` | Timeout en secondes (30 min) |

---

## 🧠 LLM — Modèle de texte (résumés, titres, chat)

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `TEXT_MODEL_BASE_URL` | `https://openrouter.ai/api/v1` | OpenRouter | URL endpoint compatible OpenAI |
| `TEXT_MODEL_API_KEY` | `sk-or-v1-xxx` | — | Clé API du provider LLM |
| `TEXT_MODEL_NAME` | `openai/gpt-4o-mini` `llama3.2` | — | Nom du modèle |
| `CHAT_MODEL_BASE_URL` | `http://ollama:11434/v1` | = TEXT_MODEL | Modèle séparé pour le chat interactif |
| `CHAT_MODEL_API_KEY` | `ollama` | = TEXT_MODEL | Clé pour le modèle de chat |
| `CHAT_MODEL_NAME` | `llama3.2:latest` | = TEXT_MODEL | Si non défini, utilise TEXT_MODEL |
| `LLM_REQUEST_TIMEOUT` | `120` | — | Timeout requêtes LLM en secondes (pour modèles lents) |
| `GPT5_REASONING_EFFORT` | `minimal` `low` `medium` `high` | `medium` | Uniquement GPT-5 via OpenAI direct |
| `GPT5_VERBOSITY` | `low` `medium` `high` | `medium` | Uniquement GPT-5 via OpenAI direct |

---

## 🔍 EMBEDDINGS — Mode Enquête (Inquire)

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ENABLE_INQUIRE_MODE` | `true` `false` | `false` | Active la recherche sémantique |
| `EMBEDDING_MODEL` | `all-MiniLM-L6-v2` | `all-MiniLM-L6-v2` | Modèle sentence-transformers local |
| `EMBEDDING_BASE_URL` | `https://api.openai.com/v1` | — | Provider externe pour les embeddings (API mode) |
| `EMBEDDING_API_KEY` | `sk-xxx` | — | Clé si provider externe |
| `EMBEDDING_DIMENSIONS` | `384` `1536` | `384` | Dimensions du modèle d'embedding |
| `EMBEDDING_API_MAX_RETRIES` | `3` | — | Retries sur erreurs transitoires (rate limits, 5xx) |
| `EMBEDDING_API_BACKOFF_SECONDS` | `2` | — | Délai initial backoff exponentiel |

---

## ✂️ CHUNKING — Découpage des fichiers audio

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ENABLE_CHUNKING` | `true` `false` | `true` | Désactiver pour `openai_whisper` seulement |
| `CHUNK_LIMIT` | `20MB` `600s` `10m` | `20MB` | Limite par chunk : taille ou durée |
| `CHUNK_OVERLAP_SECONDS` | `3` | `3` | Chevauchement entre chunks (précision aux bords) |
| `MISTRAL_ENABLE_CHUNKING` | `true` `false` | `false` | Chunking côté app pour Mistral Voxtral |
| `MISTRAL_MAX_DURATION_SECONDS` | `3600` | — | Durée max par chunk Mistral |

---

## 🗣️ MISTRAL VOXTRAL

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `TRANSCRIPTION_CONNECTOR` | `mistral` | — | Forcer le connecteur Mistral |
| `TRANSCRIPTION_API_KEY` | `your-mistral-key` | — | Clé API Mistral |
| `TRANSCRIPTION_MODEL` | `voxtral-mini-latest` | — | Modèle Voxtral |
| `TRANSCRIPTION_BASE_URL` | `https://api.mistral.ai` | — | Endpoint Mistral custom |

---

## 🤖 VIBEVOICE (vLLM auto-hébergé)

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `TRANSCRIPTION_CONNECTOR` | `vibevoice` | — | Forcer le connecteur VibeVoice |
| `TRANSCRIPTION_BASE_URL` | `http://your-vllm:8000` | — | URL du serveur vLLM |
| `TRANSCRIPTION_MODEL` | `vibevoice` | — | Nom du modèle chargé |
| `TRANSCRIPTION_API_KEY` | `your-key` | — | Si vLLM nécessite une auth |

---

## 🔑 SSO / OIDC

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ENABLE_SSO` | `true` `false` | `false` | Active l'authentification SSO |
| `SSO_PROVIDER_NAME` | `Keycloak` `Azure AD` `Google` | — | Nom affiché sur le bouton login |
| `SSO_CLIENT_ID` | `speakr-client` | — | Client ID OIDC |
| `SSO_CLIENT_SECRET` | `xxx` | — | Secret OIDC |
| `SSO_DISCOVERY_URL` | `https://keycloak/.well-known/openid-configuration` | — | URL de découverte OIDC |
| `SSO_REDIRECT_URI` | `https://speakr.company.com/auth/sso/callback` | — | URI de callback après auth |
| `SSO_AUTO_REGISTER` | `true` `false` | `false` | Créer un compte auto si 1ère connexion SSO |
| `SSO_ALLOWED_DOMAINS` | `company.com,subsidiary.org` | vide (tous) | Restreindre l'inscription aux domaines |
| `SSO_DISABLE_PASSWORD_LOGIN` | `true` `false` | `false` | Forcer SSO-only (sauf admin en fallback) |

---

## 👤 ADMIN — Utilisateur initial

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ADMIN_USERNAME` | `admin` | `admin` | Créé au 1er démarrage |
| `ADMIN_EMAIL` | `admin@company.com` | — | Email de l'admin |
| `ADMIN_PASSWORD` | `changeme123` | — | Min 8 caractères — **à changer immédiatement** |
| `ALLOW_REGISTRATION` | `true` `false` | `false` | Permettre l'auto-inscription |
| `REGISTRATION_ALLOWED_DOMAINS` | `company.com` | vide (tous) | Filtrer l'inscription par domaine |

---

## 💬 TOKENS & BUDGETS LLM

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `SUMMARY_MAX_TOKENS` | `8000` | `8000` | Max tokens pour les résumés |
| `CHAT_MAX_TOKENS` | `5000` | `5000` | Max tokens pour le chat |
| `TITLE_MAX_TOKENS` | `500` | — | Max tokens pour la génération de titre |
| `EVENT_MAX_TOKENS` | `1000` | — | Max tokens pour l'extraction d'événements |

---

## 🗄️ BASE DE DONNÉES & STOCKAGE

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `SQLALCHEMY_DATABASE_URI` | `sqlite:////data/instance/transcriptions.db` | SQLite | Ou `postgresql://user:pass@host:5432/db` |
| `UPLOAD_FOLDER` | `/data/uploads` | `/data/uploads` | Chemin de stockage des fichiers audio |

---

## 🔊 AUDIO — Compression & traitement

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `AUDIO_COMPRESS_UPLOADS` | `true` `false` | `true` | Compresser WAV/AIFF à l'upload |
| `AUDIO_CODEC` | `mp3` `flac` `opus` | `mp3` | Codec cible pour la compression |
| `AUDIO_BITRATE` | `128k` `192k` | `128k` | Bitrate (ignoré pour FLAC) |
| `AUDIO_UNSUPPORTED_CODECS` | `opus,vorbis` | — | Codecs à convertir avant envoi à l'ASR |
| `MAX_CONCURRENT_UPLOADS` | `3` | `3` | Uploads simultanés max |

---

## 🎬 VIDÉO

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `VIDEO_RETENTION` | `true` `false` | `false` | Conserver le flux vidéo pour la lecture in-browser |
| `VIDEO_PASSTHROUGH_ASR` | `true` `false` | `false` | Envoyer la vidéo brute à l'ASR (multi-pistes) |

---

## 🤝 COLLABORATION & PARTAGE

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ENABLE_INTERNAL_SHARING` | `true` `false` | `false` | Activer le partage entre utilisateurs |
| `ENABLE_PUBLIC_SHARING` | `true` `false` | `false` | Activer les liens publics partageables |
| `SHOW_USERNAMES_IN_UI` | `true` `false` | `false` | Afficher les noms d'utilisateurs dans l'UI |
| `READABLE_PUBLIC_LINKS` | `true` `false` | `false` | Rendu SSR des pages publiques (pour LLM/scrapers) |
| `USERS_CAN_DELETE` | `true` `false` | `true` | Autoriser les users à supprimer leurs enregistrements |

---

## 🗑️ RÉTENTION & AUTO-SUPPRESSION

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ENABLE_AUTO_DELETION` | `true` `false` | `false` | Activer la suppression automatique |
| `DEFAULT_RETENTION_DAYS` | `90` `30` `0` | `0` (désactivé) | Rétention globale en jours |
| `DELETION_MODE` | `audio_only` `full_recording` | `audio_only` | Supprimer seulement l'audio ou tout |
| `DELETE_ORPHANED_SPEAKERS` | `true` `false` | `false` | Supprimer les profils vocaux sans enregistrements |

---

## 📁 DOSSIERS

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ENABLE_FOLDERS` | `true` `false` | `false` | Activer l'organisation en dossiers |

---

## ⚙️ TRAITEMENT EN ARRIÈRE-PLAN

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `JOB_QUEUE_WORKERS` | `2` `4` | `2` | Workers pour la transcription (tâches longues) |
| `SUMMARY_QUEUE_WORKERS` | `2` `4` | `2` | Workers pour les résumés LLM (tâches rapides) |
| `JOB_MAX_RETRIES` | `3` | `3` | Tentatives avant échec définitif |

---

## 📂 AUTO-PROCESSING (dossier surveillé)

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ENABLE_AUTO_PROCESSING` | `true` `false` | `false` | Surveiller un dossier pour auto-traitement |
| `AUTO_PROCESS_MODE` | `admin_only` | `admin_only` | Qui peut déposer des fichiers |
| `AUTO_PROCESS_WATCH_DIR` | `/data/auto-process` | — | Chemin du dossier surveillé |

---

## 📤 AUTO-EXPORT

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `ENABLE_AUTO_EXPORT` | `true` `false` | `false` | Exporter automatiquement les transcriptions |
| `AUTO_EXPORT_DIR` | `/data/exports` | — | Dossier de destination des exports |

---

## 🌍 APPLICATION GÉNÉRALE

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `TIMEZONE` | `Europe/Paris` `UTC` `America/New_York` | `UTC` | TZ database name |
| `LOG_LEVEL` | `INFO` `DEBUG` `WARNING` `ERROR` | `INFO` | Niveau de log |

---

## 🐛 VARIABLES DE DÉBOGAGE / WORKAROUNDS

| Variable | Valeurs / Exemple | Par défaut | Notes |
|---|---|---|---|
| `TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD` | `true` | — | Fix erreur PyTorch 2.6 dans le conteneur WhisperX |

---

## ⚠️ VARIABLES DÉPRÉCIÉES (encore fonctionnelles)

| Variable dépréciée | Remplacée par |
|---|---|
| `USE_ASR_ENDPOINT=true` | Poser `ASR_BASE_URL` suffit |
| `WHISPER_MODEL` | `TRANSCRIPTION_MODEL` |

---

*Sources : `config/env.transcription.example`, `config/env.sso.example`, documentation officielle, release notes v0.5–v0.8.20*
