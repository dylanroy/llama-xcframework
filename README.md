# llama.xcframework with an iOS Simulator slice

[llama.cpp](https://github.com/ggml-org/llama.cpp) built as an XCFramework for iOS devices, the iOS Simulator and
macOS, for use as a Swift Package Manager binary target.

llama.cpp's own release XCFramework has iOS and macOS slices but no iOS Simulator slice (its CI runs
`build-xcframework.sh macos ios-device`). SwiftPM can't link a binary target on devices only, so any app that depends
on it can't build for the Simulator. These zips are the same llama.cpp release, built with `ios-sim` added.

Each [release](https://github.com/dylanroy/llama-xcframework/releases) is named after the llama.cpp tag it was built
from and holds the zip and that tag's LICENSE.

## Use

```swift
.binaryTarget(
    name: "llama",
    url: "https://github.com/dylanroy/llama-xcframework/releases/download/b12015/llama-b12015-xcframework.zip",
    checksum: "183b19169ed27bf8bf342577e0a57350950a1e025165e9899a44fae8cbf56ba3"
),
```

Each release's notes give its zip's checksum.

## How it's built

With Xcode and CMake:

```bash
git clone --depth 1 --branch b12015 https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
./build-xcframework.sh ios-sim ios-device macos
ditto -c -k --norsrc --noextattr --noacl --keepParent build-apple/llama.xcframework llama-b12015-xcframework.zip
```

Slices: `ios-arm64`, `ios-arm64_x86_64-simulator`, `macos-arm64_x86_64`, each with dSYMs.

`--norsrc --noextattr` keeps macOS's `._` AppleDouble files out of the zip. SwiftPM unzips them as plain files, and
clang then fails on `Headers/._*.h`.

## License

llama.cpp is MIT licensed; see [LICENSE](LICENSE). This repo only redistributes its build.
