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
    checksum: "6db499c80fa4d29972f4df31d595647fe164e16bd1c4a523224d7babf5bd4a7e"
),
```

Each release's notes give its zip's checksum.

## How it's built

With Xcode and CMake:

```bash
git clone --depth 1 --branch b12015 https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
./build-xcframework.sh ios-sim ios-device macos
ditto -c -k --keepParent build-apple/llama.xcframework llama-b12015-xcframework.zip
```

Slices: `ios-arm64`, `ios-arm64_x86_64-simulator`, `macos-arm64_x86_64`, each with dSYMs.

## License

llama.cpp is MIT licensed; see [LICENSE](LICENSE). This repo only redistributes its build.
