# eSpeak 1.44.05 for Windows x64

This repository is a native 64-bit Windows port of the classic eSpeak 1.44.05
text-to-speech synthesizer.

The Windows runtime contains:

- `espeak.exe`, the command-line synthesizer;
- `espeak_lib.dll`, the public eSpeak C API;
- `espeak_sapi.dll`, a native 64-bit SAPI 5 engine;
- `TTSApp.exe`, a small SAPI voice test application;
- the voices, compiled language dictionaries, dictionary sources and
  documentation from eSpeak 1.44.05.

All installed eSpeak executables and libraries are AMD64 binaries. The old
32-bit PortAudio library has been replaced with a native WinMM compatibility
layer, the SAPI server no longer requires the legacy ATL project, and the
programs use the static MSVC runtime.

## Building

Requirements:

- Visual Studio Build Tools 2022 with the MSVC x64 toolchain and Windows SDK;
- Inno Setup 6 when building the installer.

Run from the repository root:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File platforms\windows\build-x64.ps1
```

The script builds all x64 components, performs WAV and COM/SAPI smoke tests,
stages the complete package and creates the installer under `build\installer`.
Use `-SkipInstaller` when only the binaries are required.

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
