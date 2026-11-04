# Notes for Android

A small, offline notes app built with Kotlin and Jetpack Compose. Notes live in memory only and are intentionally not saved to a database or device storage.

## Features

- Browse, create, edit, and delete notes.
- Search note titles and bodies (case-insensitive).
- Sort notes by last-updated date, ascending or descending.
- Save changes explicitly; blank notes cannot be saved.
- Body-only notes are supported and receive the title `Untitled`.

## Architecture and approach

The app uses a lightweight MVVM structure. `MainActivity` hosts the Compose UI, `NotesViewModel` owns screen state and user actions, and `NoteOperations` provides small, deterministic search/sort functions. The in-memory list is the single source of truth; configuration changes are handled by the ViewModel, but process death clears all notes by design.

No network, database, or persistence dependencies are used. The UI is intentionally straightforward so the focus stays on functional behavior and edge cases.

## Project structure

```text
.
├── app/
│   ├── build.gradle.kts
│   └── src/
│       ├── main/
│       │   ├── AndroidManifest.xml
│       │   └── java/com/example/notes/
│       │       ├── MainActivity.kt       # Compose screens and navigation state
│       │       ├── Note.kt               # Note model and search/sort operations
│       │       └── NotesViewModel.kt      # In-memory state and note actions
│       └── test/java/com/example/notes/
│           └── NoteOperationsTest.kt     # Search and sort edge cases
├── build.gradle.kts
└── settings.gradle.kts
```

## Build and run

Open this directory in Android Studio (with JDK 17 and Android SDK 35 installed), allow Gradle sync, then run the `app` configuration on an emulator or device. Alternatively, run `./gradlew test` and `./gradlew assembleDebug` if a Gradle wrapper is available in your environment.

## Behavior notes

- The list is ordered by the note's last-updated timestamp. A stable ID tie-breaker keeps results deterministic when timestamps match.
- Search ignores leading/trailing whitespace and matches both title and body without regard to case.
- Deletion is immediate after confirmation. Editing an existing note preserves its creation date and updates its modification date when saved.