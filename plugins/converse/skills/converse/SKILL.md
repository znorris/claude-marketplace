---
name: converse
description: This skill should be used when the user wants spoken replies through the host system's text-to-speech engine, for example when invoking /converse or asking to "talk to me", "speak with me", "say it out loud", "read it to me", or otherwise requesting a spoken back-and-forth. Keeps a conversational cadence with short turns that leave room for the user to reply. Do not auto-trigger on routine status updates.
license: MIT
---

# Purpose

Hold a spoken back-and-forth with the user using whatever TTS engine the host system provides. Speech replaces the prose response for that turn, it does not duplicate it.

# Activation

Activate only when:

- The user runs the `/converse` slash command, or
- The user explicitly asks for speech ("talk to me about X", "speak with me about it", "say that out loud", "read it to me").

Do not activate on generic status updates, tool narration, or because output happens to be short. Once active, stay active for the rest of the conversation unless the user asks to stop ("stop talking", "text only", "quiet mode").

# Detecting an Available Engine

On first activation, detect what is installed. Run cheap probes via the Bash tool and use the first one that succeeds:

- macOS: `command -v say`
- Linux: `command -v spd-say` then `command -v espeak` then `command -v espeak-ng` then `command -v festival`
- Windows / PowerShell: `powershell -c "Add-Type -AssemblyName System.Speech"` (then use `System.Speech.Synthesis.SpeechSynthesizer`)
- WSL: prefer the Linux options above; fall back to `powershell.exe` if none are present.

If nothing is available, tell the user once in plain text and fall back to normal text replies. Do not re-probe every turn, remember the engine for the rest of the session.

# Speaking

Invoke the engine via Bash. Quote carefully so shell metacharacters in the spoken text do not break the command. Examples:

- macOS: `say "..."`
- Linux (speech-dispatcher): `spd-say -w "..."` (the `-w` flag waits until speech finishes; without it the daemon speaks asynchronously)
- Linux (espeak / espeak-ng): `espeak "..."` or `espeak-ng "..."`
- Linux (festival): `echo "..." | festival --tts` (cold start is slow, several seconds to load voices)
- PowerShell: `powershell -c "Add-Type -AssemblyName System.Speech; (New-Object System.Speech.Synthesis.SpeechSynthesizer).Speak('...')"` (`.Speak()` is synchronous; escape apostrophes in the spoken text by doubling them)

Prefer a blocking invocation so the next turn does not start while audio is still playing.

# Cadence

Speech is for conversation, not narration. Keep each spoken turn short enough that the user can jump in:

- One or two sentences per turn is the target. Three is the ceiling.
- Never speak multiple paragraphs, lists, code, file paths, long identifiers, or command output.
- End on something the user can respond to: a question, a checkpoint, a short status with an implicit "ok to continue?".
- If a thought is too long to say briefly, say the headline and offer to expand: "Found three issues, want me to walk through them?"
- Do not read tool results aloud. Summarize.

# Text Output Alongside Speech

When speaking, do not also write the spoken sentence as prose in the reply. The user sees the Bash tool call and can expand it with Ctrl+O. Acceptable text output for a spoken turn:

- Nothing at all, or
- A different kind of artifact (a diff, a file path, a code block) that is not a transcript of the speech.

Do not write "I said: ..." or repeat the spoken line. Do not add a written summary of what was just spoken.

# Constraints

- Do not speak secrets, tokens, long URLs, or full file contents.
- Do not chain multiple `say` calls in one turn to work around the length guidance, that defeats the cadence rule.
- Do not adjust voice, rate, or language unless the user asks.
- Do not install a TTS engine. If none is present, fall back to text and tell the user once.
