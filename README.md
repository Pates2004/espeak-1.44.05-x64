# eSpeak 1.44.05 for Windows x64

[![Windows x64 build](https://github.com/Pates2004/espeak-1.44.05-x64/actions/workflows/windows-x64.yml/badge.svg)](https://github.com/Pates2004/espeak-1.44.05-x64/actions/workflows/windows-x64.yml)

This repository is a native 64-bit Windows port of the classic eSpeak 1.44.05
text-to-speech synthesizer.

The Windows runtime contains:

- `espeak.exe`, the command-line synthesizer;
- `espeak_lib.dll`, the public eSpeak C API;
- `espeak_sapi.dll`, a native 64-bit SAPI 5 engine;
- `TTSApp.exe`, a small SAPI voice test application;
- `Vario.exe`, an accessible SAPI language and voice-variant manager;
- the voices, compiled language dictionaries, dictionary sources and
  documentation from eSpeak 1.44.05.

The voice-variant collection is synchronized with the 104 variants shipped by
eSpeak NG 1.52.0. The `fast` variant keeps its equivalent classic-eSpeak syntax
so the complete collection loads without parser errors on this engine.

The ready-to-use Windows installer is available from the
[GitHub Releases page](https://github.com/Pates2004/espeak-1.44.05-x64/releases/latest).

Release r27 restores native Up/Down navigation in Vario and exposes every
language node as one standard checkable tree item, including its selection and
expanded/collapsed state, for NVDA and other UI Automation clients.

Release r28 corrects the Polish `ci` pronunciation in inflected words such as
*druciana*, *bociana*, *starcia* and *tarcia*. It removes overly broad rules
that turned `ci` into `si`, and includes a focused regression check. The x86
edition uses the same compiled Polish dictionary.

Release r29 lets Vario select between its original upper-range Sonic boost
and NVDA-style threefold speed across the entire SAPI rate scale. The NVDA
mode is selected by default; the separate boost checkbox stays off until
enabled. The 32-bit and 64-bit editions offer the same modes.

All installed eSpeak executables and libraries are AMD64 binaries. The old
32-bit PortAudio library has been replaced with a native WinMM compatibility
layer, the SAPI server no longer requires the legacy ATL project, and the
programs use the static MSVC runtime.

## Local r33 update

User instructions in English and Polish are in
[`platforms/windows/Readme.txt`](platforms/windows/Readme.txt) and
[`Vario/README.md`](platforms/windows/Vario/README.md).

Vario now has an Alt-accessible Settings menu with light/dark themes, an
interface language override (system, English, Polish) and optional usage hints.
System language selects Polish only when the primary Windows UI language is
Polish. Preferences are stored in `vario.ini` beside `Vario.exe`; existing Sonic
settings are migrated from the architecture-specific registry without deleting
that recovery source. SAPI voice registration remains normal Windows integration.

The new smooth speed mode spans 80-1350 WPM. It uses the native engine up to
300 WPM and then holds that articulation while Sonic applies the remaining
compression. Legacy upper-range and NVDA-style modes remain available. Fresh
settings default to smooth; previously saved speed settings are preserved.

The r33 dictionary update corrects `kwadratowy` in Polish square-bracket names
and improves word separation in existing multiword character and symbol names,
including `u zamknięte`. This is a dictionary-only change: ordinary text spacing,
the synthesis engine, Sonic and Vario behavior are not changed.

The earlier r32 dictionary update restored the user-selected pronunciations from
the supplied `BOY` reference for numeric 30, 40 and 200, including those components
in larger numbers. Their written-word equivalents keep the existing Polish
affricate: digits and words intentionally differ. Numeric 300 and `trzysta`
are unchanged. No primary stress markers are moved.

Standard reductions in `pięćdziesiąt`, `sześćdziesiąt`, `dziewięćdziesiąt` and
their derived forms remain unchanged, as in `BOY`. The careful `pierwsz-`
pronunciation retains its `f`, while the fuller `sześćset`/600 articulation with
`ć` remains an explicit user preference, not a claim of standard pronunciation.
The existing `Tarzan` and `kolaż` pronunciation is preserved. This release does
not change engine, Sonic or Vario functionality; the existing features above
remain available.

Vario requires the matching **.NET Desktop Runtime 10** (x64 or x86). It is
framework-dependent and does not include a private runtime. Final local
installers are in `installfiles`; previous builds are kept in `snapshots`.

## Building

Requirements:

- Visual Studio Build Tools 2022 with the MSVC x64 toolchain and Windows SDK;
- .NET 10 SDK for Vario;
- Inno Setup 6 when building the installer.

Run from the repository root:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File platforms\windows\build-x64.ps1
```

The script builds all x64 components, performs WAV and COM/SAPI smoke tests,
stages the complete package and creates the installer under `installfiles`.
Use `-SkipInstaller` when only the binaries are required.

Vario is a framework-dependent single-file application and requires .NET
Desktop Runtime 10 x64 on the user's computer. The installer does not bundle
the runtime. If it is missing, launching Vario opens the standard Windows
.NET app-host download prompt. The native eSpeak engine works without .NET.

The same build and tests run on GitHub Actions for every push and pull request.
Successful runs publish the Windows installer as a downloadable workflow
artifact.

## Windows x64 port

The modern project files and Windows-specific implementation are in
[`platforms/windows/x64`](platforms/windows/x64). The original legacy project
files remain in the source tree as historical reference but are not used by
the x64 build.

The old eSpeakEdit project is not part of the runtime build. The original
archive contains its legacy project metadata and headers, but not the complete
editor implementation or its wxWidgets 2.8 dependencies.

## License

eSpeak is distributed under the GNU General Public License version 3. See
[`License.txt`](License.txt).
