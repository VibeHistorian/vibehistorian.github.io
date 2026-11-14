TinySynth 1.1.3 defines `internalcontext` but its constructor does not read that
option. The patch honors it so the adapter can bind an OfflineAudioContext without
opening a realtime audio device. It also exposes the existing sequencer timer as
`timerId` so disposing an offline engine clears it. The MIDI send path only
auto-resumes realtime contexts: offline contexts start via `startRendering()`.
This prevents rejected resume promises from the two initialization MIDI messages.
Synthesis is unchanged.

The `smplr@1.1.0` patch adds per-region pan and scale tuning, volume-envelope
delay/attack/hold/decay/sustain, and release from the envelope's computed level.
It also filters undefined preset defaults so they cannot overwrite numeric
defaults and produce NaN pitch parameters. Both ESM/CommonJS builds and both
declaration files are patched. SF2 parsing and bank/program resolution stay in
`src/app/core/audio/sf2-resolver.ts`; the package patch is only renderer support.

Install with the pnpm version pinned in ../package.json:
  pnpm install --frozen-lockfile
The ../pnpm-workspace.yaml file registers both patches; ../pnpm-lock.yaml
records their hashes. Do not apply them by hand or install with npm.
When updating a patch, use pnpm patch / pnpm patch-commit and commit the
patch, workspace configuration and regenerated lockfile together.
