# cv-rs

[![Build Status][travis-image]][travis-url]
[![Build status][appveyor-image]][appveyor-url]
[![standard-readme compliant][standard-readme-image]][standard-readme-url]

> This library primarily provides bindings (somewhat idiomatic) and APIs for
> OpenCV 3.x.

[Documentation](https://nebgnahz.github.io/cv-rs/cv/)

## Table of Contents

- [Background](#background)
- [Install](#install)
- [Usage](#usage)
- [Contribute](#contribute)
- [License](#license)

## Background

OpenCV (Open Source Computer Vision Library: http://opencv.org) is an
open-source BSD-licensed library that includes several hundreds of computer
vision algorithms. It's mainly developed in C++. This library provides Rust
bindings to access OpenCV functionalities. First, C bindings are created
(in [native](native) folder); then [Rust APIs](src/lib.rs) are constructed
atop. Although this manual process seems an inefficient process, it has served
me well as a learning experience to both OpenCV and Rust. In terms of OpenCV API
coverage, modules and functions are implemented as needed.

Please check out the [documentation](https://nebgnahz.github.io/cv-rs/cv/) to
see what has been ported. If you have demand for porting specific features,
please open an issue, or better create a PR.

Attempts to use [rust-bindgen](https://github.com/servo/rust-bindgen)
or [cpp_to_rust](https://github.com/rust-qt/cpp_to_rust) haven't been very
successful (I probably haven't tried hard enough). There is another
port [opencv-rust](https://github.com/kali/opencv-rust/) which generates OpenCV
bindings using a Python script (more automated).

## Install

Before anything, make sure you have OpenCV 3 installed. Read this
[Introduction to OpenCV][opencv-intro] to get started.

Then in any Rust project, add this to your `Cargo.toml`:

```
[dependencies]
cv = { git = "https://github.com/nebgnahz/cv-rs.git" }
```

And add this to your crate:

```
extern crate cv;
use cv::*;
```

And then, enjoy the power of OpenCV.

If you'd like to use OpenCV GPU functions, it's inside `cv::cuda`. Enable it
with the following code in `Cargo.toml`:

```
[dependencies.cv]
git = "https://github.com/nebgnahz/cv-rs"
features = [ "gpu" ]
```

### Windows


Depending on your install, you have to set `%OPENCV_DIR%` environment variable `%OPENCV_DIR%\include` points to the OpenCV includes diretory (for example, it could be `C:\opencv\build`). You will also need to add `%OPENCV_DIR%\bin` to the `%PATH%`. 

##### If you are using MSVC toolchain:
- Set  environment variable `%OPENCV_LIB%` to `%OPENCV_DIR%\x64\<vc14 or vc15>\lib`, which has `opencv_world<version_number>.lib`

##### If you are using GNU toolchain:

In this case, configuration process is a bit more complicated, because you have to build OpenCV from sources. Follow these steps:
- Add `MinGW` to the `PATH`. This path could look like `C:\Program Files\mingw-w64\x86_64-7.2.0-posix-seh-rt_v5-rev1\mingw64\bin`. If you haven't installed it yet, download it from [here](https://sourceforge.net/projects/mingw-w64/files/latest/download). Choose architecture `x86_64` during installation.
- Download and install [CMake](https://cmake.org/download/). Caution, don't download the source code, you need the binary.
- Run ‘cmake-gui’ in `<cmake-binary>\bin`. For example, `C:\Program Files\CMake\bin\cmake-gui.exe`
- Enter `%OPENCV_DIR%\..\sources` path in the Windows Explorer and copy result in ‘Where is the source code:’ text box (_CMake doesn't understand path traversal syntax while explorer does_). For example, it could be `C:\opencv\sources`.
- Do the same thing with `%OPENCV_DIR%\x64\mingw` and paste it in ‘Where to build the binaries:’ text box. For example, it could be `C:\opencv\build\x64\mingw`.
- Click ‘Configure’. In the popping window, select MinGW option under ‘Specify the generator for this project’ and click ‘Finish’.
- Search for `WITH_IPP` and **disable** it (otherwise ‘cannot find -lRunTmChk’ error is expected when building OpenCV).
- Search for `BUILD_opencv_world` and **enable** it (otherwise `cv-rs` won't work).
- Click ‘Generate’. Select 'MinGW makefiles' from the dropdown and click ok. Now you have properly configured OpenCV.
- Open bash shell in `OPENCV_DIR%\x64\mingw64` directory and run `mingw32-make`. You should get following result:
![MinGW make](https://preview.ibb.co/fuTz1b/2018_01_07.png)
- Set environment variable `%OPENCV_LIB%` to `%OPENCV_DIR%\x64\mingw64\lib`, which has `opencv_world<version_number>.dll.a`.


## Usage

See available examples on how this library might be used.

- [Display Image](examples/display_image.rs)
- [Video Capture](examples/video_capture.rs), optional GPU code
- [Face Detection](examples/face_detect.rs)
- [Camshift](examples/camshift.rs)
- [HOG Detection](examples/hog.rs), optional GPU code

## Contribute

See [the contribute file](contribute.md)! PRs highly welcome.

You may also simply open up an issue for feature/porting request.

Small note: If editing the README, please conform to the
[standard-readme](https://github.com/RichardLitt/standard-readme) specification.

## License

MIT © Ben Zhang

<!-- links -->
[travis-image]: https://travis-ci.org/nebgnahz/cv-rs.svg?branch=master
[travis-url]: https://travis-ci.org/nebgnahz/cv-rs
[appveyor-image]: https://ci.appveyor.com/api/projects/status/dutogjshst3oyra2?svg=true
[appveyor-url]: https://ci.appveyor.com/project/nebgnahz/cv-rs
[standard-readme-image]: https://img.shields.io/badge/standard--readme-OK-green.svg?style=flat-square
[standard-readme-url]: https://github.com/RichardLitt/standard-readme
[opencv-intro]: http://docs.opencv.org/3.1.0/df/d65/tutorial_table_of_content_introduction.html
