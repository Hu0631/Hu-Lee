
#  Husang’s Vocabulary & Study App  
**An AI + Community-Driven Personalized Vocabulary Learning Platform**

---

##  Purpose  
> “A fast and lightweight English learning app  
> where users build their own vocabulary sets  
> and study situation-based English in real time —  
> with or without AI.”

---

##  Main Features

###  Vocabulary Management
- Create, edit, and delete word cards  
- Auto-add pronunciation symbols and example sentences (from DB or simple AI)  
- Group multiple cards into sets

###  Pronunciation & Listening
- Listen via Text-to-Speech (TTS)  
- Compare pronunciation via Speech-to-Text (STT) feedback using local free APIs

###  Chatbot-Based Learning
- Practice English conversations using your vocabulary and examples  
- Optional lightweight AI chatbot (or database-based logic)

###  Example Sentence Generation
- Combine selected words to generate new sentences  
- Cache results to prevent redundant generation and API costs-

###  Context-Based Learning
- Categories such as *Travel, Business, Daily Life, Academic*  
- Recommends words according to questionnaire results

###  Onboarding Questionnaire
- Collects user data such as language, level, goals, daily/weekly frequency  
- Saves profile and preferences to Firestore

###  Learning Missions (Tasks)
- Auto-generate daily/weekly tasks based on user goals  
  e.g., “Learn 5 Business English words,” “Practice 3 Travel Conversation Sentences”

###  Study Group Feature
- Create and join groups  
- Share words, images, and notes  
- Real-time feed and chat powered by Firebase Realtime Database

###  Random Chat & Community
- Share vocabulary, examples, and motivational quotes  
- Simple random chat for language exchange

###  Motivation Feed
- “Quote of the Day by username + timestamp” format  
- Encourages consistency and peer motivation

###  Data Management & Automation
- User-submitted word validation and auto-approval system  
- Includes profanity filter, duplication check, and approval rules  
- Controlled via Firestore Security Rules

---

## ⚙️ System Architecture

[Flutter App]  
├─ Word input / view / pronunciation / chatbot UI  
├─ Questionnaire (level, context, goal)  
│  
▼  
[Firebase Firestore]  
├─ users (user profiles and learning progress)  
├─ base_vocab (official vocabulary and examples)  
├─ user_vocab (user-submitted words)  
├─ tasks (personalized learning missions)  
├─ groups (study groups, chat, community feed)  
└─ quotes (motivation and quotes feed)  
│  
▼  
[Firebase Functions / Node.js]  
├─ Word validation (profanity, duplication)  
├─ Caching and analytics management  
├─ Example sentence generation (AI or DB-based)  
└─ Chatbot API integration (optional)  
│  
▼  
[Local Device]  
├─ Speech-to-Text (pronunciation feedback)  
├─ Text-to-Speech (listen to pronunciation)  
└─ SQLite offline cache  

---

##  Data Flow Example

1. App launch → display questionnaire  
2. Save user preferences to Firestore  
3. Fetch vocabulary list from **base_vocab** filtered by (level, context)  
4. Show learning cards / chatbot / example generation screen  
5. On completion → update progress in Firestore  
6. Reflect results in the motivation & quotes feed  

---

##  Database Design (Firestore)

| Collection | Description | Key Fields |
|-------------|-------------|-------------|
| **users** | User profile & settings | name, email, level, context, daily_goal, weekly_goal |
| **base_vocab** | Official verified vocabulary | word, level, context, definition, example, approved:true |
| **user_vocab** | User-submitted vocabulary | word, context, example, approved:false, submitted_by |
| **tasks** | Personalized missions | user_id, word_list, goal, deadline |
| **groups** | Study groups | group_name, members[], posts[], chat[] |
| **quotes** | Motivational feed | content, author, user_id, timestamp |

---

##  Security & Automation

| Layer | Description | Access Permission |
|--------|--------------|-------------------|
| **base_vocab** | Verified official vocabulary | Only admin can modify |
| **user_vocab** | User-submitted words | Auto-filter + approval required |
| **my_vocab** | Personal vocabulary | Owner-only access |
| **Security Rules** | Controlled by Firestore Rules | Authenticated users only |
| **Auto Filtering** | Profanity, duplication, spam detection | Processed via Cloud Functions |

---

##  Tech Stack & Estimated Monthly Cost

| Component | Technology | Reason | Cost (AUD) |
|------------|-------------|---------|-------------|
| **DB/Auth** | Firebase Firestore + Auth | Realtime, serverless | Free – 10 |
| **AI Chatbot (optional)** | GPT-4o-mini API | Low-cost example/chat generation | 10 – 30 |
| **Speech Features** | Google STT / Flutter TTS | Local, free | Free |
| **Backend** | Firebase Functions | No dedicated server required | Free – 5 |
| **Total** | — | MVP stage estimate | **≈ 20 – 40 AUD/month** |

---

##  Development Roadmap (Full-time Estimate)

| Phase | Goal | Duration |
|--------|------|-----------|
| **1** | Vocabulary + Pronunciation + Example (MVP) | 2 – 3 weeks |
| **2** | Questionnaire + Personalized Recommendation | 2 weeks |
| **3** | Group/Feed + Auto-Approval System | 3 – 4 weeks |
| **4** | UI Enhancement + Chatbot (optional) | 3 weeks |
| **Total** | — | **≈ 2 – 3 months** |

---

##  Core Concept Summary

> **“Learn only the English that fits your real-life context —  
> A community-driven, personalized vocabulary app.”**

- AI is optional (acts as a helper)  
- Database-centric, lightweight architecture  
- Community participation expands content quality and reach  

---

##  Future Expansion Plans
- **Supabase/PostgreSQL** integration for large-scale data management  
- **User reputation system** for trusted word approval  
- **PWA Web Version** using React + Firebase  
- **Premium Plan** offering unlimited examples/chatbot usage  
- **Offline Learning Mode** with full SQLite support  

---

##  One-Line Summary  
> “A fast, lightweight Firebase-based English learning app —  
> evolving with its users, not with costly AI.”


#  Husang’s Vocabulary & Study App — v2  
**Added Feature: External Word Sharing (Text Selection → Add to Voca)**  

---

##  Feature Summary

> "Add new vocabulary directly from any app!"

Users can **select a word or phrase in any mobile app** (browser, SNS, reader, etc.),  
tap **Share → Vocabulary App**, and the selected text is automatically sent  
to the app’s “Add Word” screen with the word prefilled.

---

#  Provider Architecture — Husang’s Vocabulary App

**Document:** PROVIDER_STRUCTURE.md  
**Last Updated:** 2025-11-07  

---

##  Purpose

Use Flutter’s **Provider** package to manage shared app state efficiently, separating UI presentation from data logic.  
This helps the app stay simple, reactive, and easy to scale.

---

##  Overview

The app consists of multiple screens sharing the same data:

- **Home** – progress and user info  
- **Word List** – vocabulary management  
- **Add Word** – create new entry  
- **Tasks** – weekly progress tracking  
- **Settings** – user preferences and goals  

To keep everything synchronized and modular, the architecture uses **three Providers**:

1. **UserProvider** — manages user profile and learning goals  
2. **WordProvider** — manages vocabulary data  
3. **TaskProvider** — manages weekly tasks and progress  

---

##  Provider Structure

### 1️ UserProvider

**Purpose:**  
Manage user profile, level, and learning goals.

**Used In:**  
HomeScreen, SettingsScreen, TaskScreen

**Why:**  
User data is shared across many screens, so centralizing it in a Provider avoids redundant Firestore reads and simplifies goal updates.

**Firestore Link:**  
Reads and writes from `users/{userId}` collection.

---

### 2️ WordProvider

**Purpose:**  
Handle all vocabulary data, including both system and user-added words.

**Used In:**  
WordListScreen, AddWordScreen, HomeScreen

**Why:**  
Keeps vocabulary lists synchronized across screens and allows instant updates when new words are added, without extra Firestore calls.

**Firestore Link:**  
- Reads from `base_vocab` (official words)  
- Writes to `user_vocab` (user-submitted or shared words)

---

### 3️ TaskProvider

**Purpose:**  
Manage weekly tasks, completion status, and progress updates.

**Used In:**  
TaskScreen, HomeScreen

**Why:**  
Ensures progress bars and checklists stay in sync across screens when a user completes or updates a task.

**Firestore Link:**  
Maps to documents in `tasks/{userId_week}` and updates fields like `completed_words` and `progress`.

---

##  Integration Flow

**Main Integration:**  
All Providers are registered globally in `main.dart` through `MultiProvider`.  
Each screen accesses data via context, listening for changes or triggering updates when necessary.

**Usage Example (conceptually):**

- **Listening:** Home screen reacts to progress or goal changes automatically.  
- **Updating:** AddWord screen adds a new word to WordProvider, which refreshes WordList automatically.

---

##  Firestore + Provider Sync

| Action | Provider | Firestore Collection | Trigger |
|--------|-----------|----------------------|----------|
| Load user settings | UserProvider | `users` | On app start |
| Update goals | UserProvider | `users` | When user saves settings |
| Add new word | WordProvider | `user_vocab` | When user adds word |
| Receive shared word | WordProvider | `user_vocab` | When shared via intent |
| Generate weekly task | TaskProvider | `tasks` | Weekly or manual creation |
| Update progress | TaskProvider | `tasks` | On task completion toggle |

---

##  Why This Structure Works

| Principle | Description |
|------------|-------------|
| **Simplicity** | Only three Providers handle all major data flows |
| **Speed** | Reactive updates using `notifyListeners()` ensure smooth UI |
| **Efficiency** | Providers act as in-memory caches, reducing Firestore reads |
| **Safety** | Each Provider controls one collection and limited logic |
| **Scalability** | Easy to add new Providers later (e.g., ChatbotProvider or GroupProvider) |

---

##  Summary Table

| Provider | Role | Firestore Collection | Used In Screens |
|-----------|------|----------------------|-----------------|
| **UserProvider** | Manages user info and learning goals | `users` | Home, Settings, Task |
| **WordProvider** | Handles vocabulary data | `base_vocab`, `user_vocab` | WordList, AddWord, Home |
| **TaskProvider** | Tracks weekly tasks and progress | `tasks` | Task, Home |

---

##  Development Notes

- Start UI design using **dummy Provider data** (without Firestore).  
- Once the interface is stable, connect each Provider to Firestore streams.  
- Avoid deep `Consumer` nesting — use it only where state actually changes.  
- Keep Provider files **logic-only**, not UI code.  
- Firestore handles persistence, while Provider manages in-memory state.

---

##  Summary

The Provider architecture keeps Husang’s app clean, fast, and scalable.  
It enables:  

- Unified state across all screens  
- Simple synchronization with Firestore  
- Clear separation between UI and logic  

**Result:**  
A lightweight, reactive vocabulary app that stays simple but powerful.  

# Husang Vocabulary & Study App

A lightweight Flutter vocabulary learning app focused on fast capture, local-first study flows, and optional cloud sync.

## Product Purpose
The app helps users collect English words quickly, organize them into folders, and study through word/notes workflows with minimal friction.

Primary design goals:
- Fast capture from camera/manual/share intent
- Guest mode that works mostly from local cache
- Optional Firebase-backed sync for signed-in users
- Simple provider-based state sharing across screens

## Current Feature Scope (from implemented code)
- Vocabulary management (add, memo, star, check, move folder, delete)
- Notes management (text, star/check, folder move, image attachment)
- Guest mode with local persistence via Hive
- Login mode with Firestore + Firebase Storage integration
- External text sharing on Android (Share/Process Text -> app)
- Dictionary-backed enrichment via local SQLite lookup
- Camera OCR-related capture flow and scrap pipeline
- Theme/font preferences persisted locally

## Architecture

### 1) Client Application Layer (Flutter)
Core screens and flows are organized under `lib/screens` and use Provider for state updates.

Main boot sequence:
1. Initialize camera
2. Initialize Firebase
3. Initialize Hive
4. Create singleton-like providers
5. Register external share listener
6. Launch app with `MultiProvider`

### 2) State Management Layer (Provider)
The app currently registers these providers globally:
- `AppStateProvider`: guest/login mode flag and shared-text staging state
- `GuestProvider`: guest-only folders/words/notes/theme/font from Hive
- `WordsProvider`: logged-in word list and search logic
- `NotesProvider`: logged-in notes and media operations
- `FolderProvider`, `FolderUiState`: folder data + UI state coordination
- `ScrapProvider`: token->word pipeline using SQLite dictionary + Hive save
- `UserProvider`: profile/settings from Firestore
- `TabProvider`, `ThemeManager`: UI-level app state

### 3) Data Layer
The app uses a hybrid storage strategy:
- Firestore: user cloud data (profile, words, folders, notes)
- Firebase Storage: note image files (logged-in mode)
- Hive: local cache/source of truth in guest mode and offline support
- SQLite dictionary DB: lexical lookup (`vocab`, `posBlocks`, `relations`, `morphology`)

## Workflow

### A. App Startup Workflow
1. App initializes platform services (camera/Firebase/Hive).
2. Providers are created and injected via `MultiProvider`.
3. External share channel is attached for cold/warm start shared text.
4. Splash/auth gate determines guest or signed-in path.

### B. Guest Mode Vocabulary Workflow
1. Guest data loads from Hive (`folders`, `words`, `notes`).
2. Default folders (`All`, `Important`) are ensured.
3. User actions (add/edit/star/check/search/move/delete) mutate in-memory lists.
4. Each mutation is persisted to Hive and reflected through `notifyListeners()`.

### C. Logged-in Vocabulary Workflow
1. Word operations run through `WordsProvider`.
2. Provider updates Firestore via `WordsRepository`.
3. Same objects are also stored in Hive for local continuity.
4. UI updates immediately from provider state.

### D. Notes + Image Workflow
1. Note metadata is saved in provider + Hive.
2. Logged-in image attach uploads optimized original + thumbnail to Firebase Storage.
3. Download URL is stored in Firestore note document and provider list.
4. Guest image attach stores optimized files in app documents directory.

### E. External Share Workflow (Android)
1. User selects text in another app and taps Share.
2. `ShareReceiverActivity` captures text and forwards it via MethodChannel.
3. Flutter `ExternalShareListener` parses shared text into tokens.
4. `ScrapProvider` normalizes, quality-filters, deduplicates, and saves words.
5. Guest or logged-in word provider reloads from Hive and UI reflects changes.

## Database Schema

### Firestore (implemented paths)

#### `users/{uid}`
User profile/settings fields used by `UserModel`:
- `username` (string)
- `email` (string)
- `created_at` (timestamp)
- `theme` (string)
- `words_per_day` (number)
- `study_days` (number)
- `selected_folders` (array<string>)
- `font_size` (number)
- `last_login_at` (timestamp, optional)
- `app_version` (string, optional)

#### `users/{uid}/words/{wordId}`
Vocabulary fields from `Vocab.toJson()`:
- `word`, `ipa`, `soundUrl`
- `freq`, `relationCount`
- `memo`, `isStarred`, `isChecked`
- `folderName`, `folderId`
- `addedAt`, `updatedAt`
- `status` (`scraped` | `confirmed`)
- `isCustom` (bool)
- `source` (`camera` | `external` | `manual`)

#### `users/{uid}/folders/{folderId}`
Folder fields from `FolderModel`:
- `id`, `name`, `type` (`word` | `note`)
- `totalWords`
- `createdAt`
- `updatedAt` (when renamed/merged)

#### `users/{uid}/my_notes/{noteId}` (via Firestore service/repository)
Note fields from `NoteModel`:
- `text`, `memo`
- `isStarred`, `isChecked`
- `keywords`
- `folderName`, `folderId`
- `addedAt`, `updatedAt`
- `imagePaths`

### Firebase Storage
- `notes/{uid}/{noteId}/orig_{timestamp}.jpg`
- `notes/{uid}/{noteId}/thumb_{timestamp}.jpg`

### Hive Local Cache
Logical boxes store:
- local vocab records (word-level fields + dictionary joins + scrap metadata)
- local notes records (text/flags/folder/images)
- local folders (`word`/`note`)
- local app settings (theme/font)

### SQLite Dictionary
Read-only dictionary tables used by scrap/search detail features:
- `vocab`
- `posBlocks`
- `relations`
- `morphology`

## Why this architecture fits the app
- Keeps guest mode responsive without mandatory network access
- Allows logged-in users to sync while retaining local continuity
- Supports high-frequency capture flows (camera/share/manual)
- Separates UI from data mutation logic through providers + repositories

## Development Notes
- Keep provider classes logic-centric and avoid UI concerns inside providers.
- Preserve local-first behavior when adding new features.
- Validate guest->login transition and cache sync behavior carefully when extending flows.

---

# Dictionary Database Documentation

## 1. Overview

This project builds a **high-precision English lexical database** designed for mobile applications.

The database combines multiple linguistic resources and a custom processing pipeline to produce a structured **SQLite dictionary optimized for fast offline lookup** in Flutter apps.

### Data Sources

The database integrates the following resources:

- **WordNet 3.1**
  - word senses
  - definitions
  - example sentences
  - lexical relations

- **CMU Pronouncing Dictionary 0.7b**
  - pronunciation data used for IPA conversion

- **Wordfreq**
  - word frequency ranking

- **Custom Morphological Parser**
  - plural forms
  - verb inflections

A custom **merge pipeline** normalizes and combines these datasets while preventing data conflicts.

The final output is a **fully structured SQLite dictionary database** optimized for mobile performance.

---

# 2. Data Sources & Parsing Pipeline

## 2.1 WordNet Parsing

Parsed using custom Python scripts.

Extracted information includes:

- lemmas
- part-of-speech blocks (`posBlocks`)
- sense definitions
- usage examples

### Extracted relation categories

The parser extracts six lexical relation types:

- synonyms  
- antonyms  
- hypernyms  
- hyponyms  
- derivations  
- relatedPhrases  

Each entry also includes a calculated field: relationCount

This represents the total number of relations associated with the word and can be used for ranking complexity.

All entries are normalized into a **unified JSON structure** before database generation.

---

## 2.2 IPA Extraction (CMUdict-0.7b)

The system loads **125,770 entries** from the CMU Pronouncing Dictionary.

Processing steps:

1. Parse ARPABET phonetic notation
2. Convert ARPABET → IPA
3. Support multiple pronunciations (US/UK variants)

Rules applied during merge:

- IPA is inserted **only when the entry has no existing IPA field**
- Existing curated IPA data is never overwritten

---

## 2.3 Frequency Integration (Wordfreq)

Word frequency is calculated using: wordfreq.zipf_frequency()

Each word receives a **Zipf frequency score** stored as: freq

Merge rule:

- frequency is added only if the field was previously empty

---

## 2.4 Morphology Parser

A custom morphology parser extracts common word inflections.

### Noun

- plural form

### Verb

- third person singular
- past tense
- past participle
- present participle

Resulting JSON structure:

```json
"morphology": {
  "noun": { "plural": "..." },
  "verb": { "s": "...", "past": "...", "pp": "...", "ing": "..." }
}
```
---

## 2.5 Merge Logic

The merge pipeline follows strict precedence rules to avoid data corruption.

### Merge Priority

1. WordNet data  
2. Previously curated custom data  
3. Additional enrichments (IPA, frequency, morphology)

### Key Rule

Values are updated **only when the target field is empty**.

This guarantees:

- curated data is never overwritten
- enriched fields remain consistent
- incremental dataset updates remain safe

The pipeline processed a total of:

**153,724 words**

---

# 3. Final JSON Structure

Each dictionary entry follows the schema below.

```json
{
  "id": "string",
  "word": "string",
  "ipa": "string",
  "soundUrl": "string",
  "posBlocks": [
    {
      "pos": "string",
      "definitions": ["..."],
      "examples": ["..."]
    }
  ],
  "synonyms": [],
  "antonyms": [],
  "hypernyms": [],
  "hyponyms": [],
  "derivations": [],
  "relatedPhrases": [],
  "relationCount": 0,
  "freq": 0.0,
  "morphology": {
    "noun": { "plural": "..." },
    "verb": { "s": "...", "past": "...", "pp": "...", "ing": "..." }
  }
}
```
---

### Dataset Summary

| Metric | Value |
|------|------|
| Total entries | **153,724 words** |
| IPA coverage | **104,024 words** |
| Frequency data | **100% coverage** |
| Morphology entries | verbs and nouns parsed |

---

# 4. SQLite Database Schema

The JSON dataset is converted into a normalized SQLite schema optimized for mobile querying.

---

## 4.1 vocab table

Stores one row per vocabulary entry.

```sql
CREATE TABLE vocab (
    id TEXT PRIMARY KEY,
    word TEXT NOT NULL,
    ipa TEXT,
    soundUrl TEXT,
    freq REAL,
    relationCount INTEGER
);
```
---

## 4.2 posBlocks table

Stores definitions and examples separately to support efficient indexing and search.

```sql
CREATE TABLE posBlocks (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    vocab_id TEXT NOT NULL,
    pos TEXT,
    definition TEXT,
    example TEXT,
    FOREIGN KEY (vocab_id) REFERENCES vocab(id)
);
```
---

## 4.3 relations table

Stores lexical relations extracted from WordNet.

```sql
CREATE TABLE relations (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    vocab_id TEXT NOT NULL,
    type TEXT NOT NULL,
    value TEXT NOT NULL,
    FOREIGN KEY (vocab_id) REFERENCES vocab(id)
);
```
### Allowed relation types

- synonym  
- antonym  
- hypernym  
- hyponym  
- derivation  
- relatedPhrase  

---

## 4.4 morphology table

Stores noun and verb inflections.

```sql
CREATE TABLE morphology (
    vocab_id TEXT PRIMARY KEY,
    noun_plural TEXT,
    verb_s TEXT,
    verb_past TEXT,
    verb_pp TEXT,
    verb_ing TEXT,
    FOREIGN KEY (vocab_id) REFERENCES vocab(id)
);
```
---

# 5. Design Justification

## Why SQLite?

SQLite was chosen for several reasons:

- extremely fast random access  
- ideal for offline mobile applications  
- minimal storage overhead  
- no network dependency  
- efficient indexing support  

This makes SQLite particularly suitable for **mobile dictionary applications**.

---

## Why store relations as rows instead of JSON?

Separating relations into rows provides:

- faster filtering (synonyms, antonyms, etc.)
- efficient indexing
- easier query patterns

Example query:

```sql
SELECT value
FROM relations
WHERE vocab_id = 'word_id'
AND type = 'synonym';
```
## Why split posBlocks?

Splitting definitions and examples into separate rows allows:

- direct definition search  
- simpler UI rendering  
- faster retrieval compared to nested JSON  

---

## Why isolate morphology?

Morphological information is frequently used for:

- search normalization  
- verb conjugation assistance  
- plural matching  

Separating it into its own table allows easier updates and avoids redundancy.

---

# 6. Final Output Summary

| Metric | Value |
|------|------|
| Vocabulary entries | **153,724** |
| JSON normalization | **100%** |
| IPA coverage | **104,024 words** |
| Frequency coverage | **100%** |
| Morphology coverage | verbs and nouns |
| Database format | **SQLite** |

The result is a **high-performance offline lexical database** suitable for mobile applications requiring fast dictionary lookup and linguistic relationships.
