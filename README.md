# ffmpeg

A CMake wrapper that builds ffmpeg from source as an `ExternalProject`, so a
project can depend on ffmpeg without asking the person building it to install
ffmpeg first.

Two pieces:

| File | |
|---|---|
| `CMakeLists.txt` | Configures, builds and installs ffmpeg as an external project. |
| `Findffmpeg.cmake` | A find module, so `find_package(ffmpeg)` works against the result. |

The build type is carried through to ffmpeg's own configure flags rather than
ignored. A `MinSizeRel` build passes `--enable-small`, which is the case that
motivated writing this: a release build that ships ffmpeg wants the small build,
and the default wrapper approach gives you the large one.

## Use

Add it as a subdirectory or fetch it:

```cmake
include(FetchContent)
FetchContent_Declare(ffmpeg GIT_REPOSITORY https://github.com/godofecht/ffmpeg.git)
FetchContent_MakeAvailable(ffmpeg)
```

Tests live under `test/`, including a subproject case that checks it still works
when consumed rather than built at top level.

Known gaps, both marked in the source: version checking in `Findffmpeg.cmake`,
and macOS universal binaries.

## Licensing

This wrapper is mine, under [LICENSE](LICENSE). It builds ffmpeg but does not
contain it. ffmpeg itself is LGPL-2.1 or GPL-2.0 depending on the configure
flags you choose, and those terms apply to whatever you build with it.
