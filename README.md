# Claim What's Yours

**Voice-first, multilingual and accessible government-scheme assistant for India.**

Claim What's Yours is an Android companion app built for ACE Builders Hackathon 2026, Problem Statement 5: *Citizens and Public Services*. It is not a scheme directory. It is a conversational assistant that takes a citizen from "I need help" to a clear, personalised list of schemes they may be eligible for, the documents they need, and the steps to apply.

**MVP scope:** Tamil Nadu, with a focus on low-income families and senior citizens. Languages: English and Tamil.

## Principles

- **Accessibility first:** TalkBack semantics, large touch targets, voice input and spoken output, captions, and a sign-language mode fallback.
- **The LLM explains; the rule engine decides.** Eligibility is never decided by a language model. A deterministic, auditable rule engine evaluates each scheme's eligibility rules against the citizen's profile. Any LLM is used only to phrase questions and explain the engine's results in plain language.
- **Minimal data.** The app asks only for what eligibility needs. It does not collect Aadhaar, PAN, bank account, OTP or biometric data.
- **No hard-coded secrets.** API endpoints and keys are supplied through configuration, never committed to source.

## Layered architecture

| Layer | Responsibility |
|---|---|
| **Domain / rule engine** | Models for schemes, rules and profiles; deterministic eligibility evaluation; ranking; document checklists; application guides. Pure Kotlin, no Android dependencies. |
| **Conversation** | Language detection, intent parsing, profile extraction from speech or text, missing-question logic, localisation resources. Produces a profile and questions; does not decide eligibility. |
| **Voice & accessibility** | `SpeechRecognizer` input, `TextToSpeech` output with playback controls, captions, sign-language mode fallback. |
| **Persistence** | Room database for saved schemes, application tracker and offline scheme cache; WorkManager for reminders. |
| **UI** | Jetpack Compose screens with TalkBack semantics and large targets, wired with Hilt and Navigation Compose. |

Dependency direction: UI and conversation depend on the domain layer; the domain layer depends on nothing in Android.

## Project layout

```
settings.gradle.kts            Project settings and repositories
build.gradle.kts               Root plugin declarations
gradle/libs.versions.toml      Version catalog
app/build.gradle.kts           Android module configuration
app/src/main/AndroidManifest.xml
app/src/main/java/com/claimwhatsyours/app/
    ClaimApp.kt                Hilt application class
    MainActivity.kt            Single-activity Compose host (placeholder)
    di/AppModule.kt            Hilt module (placeholder)
app/src/main/assets/schemes.json   SAMPLE Tamil Nadu and central scheme data
```

## Stack

Kotlin 2.0.0, Jetpack Compose (BOM 2024.06.00), AGP 8.5.2, Hilt 2.51.1, KSP 2.0.0-1.0.23, Room, WorkManager, Retrofit and OkHttp, kotlinx.serialization. Min SDK 26, target and compile SDK 34, Java 17.

## Scheme data is SAMPLE and unverified

`app/src/main/assets/schemes.json` contains **sample data only**. Every entry is marked `isSampleData: true` and `verificationStatus: "sample_unverified"`. Scheme names, eligibility limits, benefit amounts, documents and steps are illustrative and have **not** been checked against official notifications. All official source and application links point to https://www.myscheme.gov.in. Before any real use, the data must be verified against the official sources and the verification fields updated.

## Status

Initial scaffold only. The app currently shows a placeholder screen. Domain, conversation, voice, persistence and UI layers are not yet implemented.
