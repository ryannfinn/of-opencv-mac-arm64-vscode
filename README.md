# Installing OpenCV C++ on an arm64 Mac for OpenFrameworks and VSCode

This is just to document my journey for installing native OpenCV for the use of OpenFrameworks. I've used in total 8 hours for doing that and I found there's no single one tutorial that has the exact setting I'm having. Installing OpenCV is never easy and tons of issues can come up. The [tutorial written by LingDong-](https://github.com/LingDong-/fast-many-face-detection-with-cpp-or-openframeworks-on-mac-using-neural-networks) and the [official VSCode for OpenFrameworks](https://openframeworks.cc/setup/vscode/) is the base that I followed through. 

## Getting OpenCV

To get an OpenCV C++ on MacOS, I have tried getting it through Homebrew and also through compiling from source like on the [OpenCV website](https://docs.opencv.org/4.x/d0/db2/tutorial_macos_install.html).  

Both of them didn't work for me as this annoying error keeps on coming back. 
```
Undefined symbols for architecture arm64:
  "cv::Mat::Mat()", referenced from:
      ofApp::ofApp() in main.o
  "cv::Mat::~Mat()", referenced from:
      ofApp::~ofApp() in ofApp.o
ld: symbol(s) not found for architecture arm64
```

I've tried to recompile, reinstall for Debug, Release version for many times, and it still didn't work. I stumbled into a line on the verbose output of Homebrew complilation of OpenCV, saying that CMake is compiling OpenCV with architecture x86_64. Then I finally realized what the problem is. Turns out since my Mac has succeeded the Rosetta version of Homebrew back from the days when there is no native arm64 supported Homebrew, basically what I've using as Homebrew is an x86_64 version instead of an arm64 one. 

It has made CMake and Homebrew automatically selecting x86_64 installation for me. The above error results from having the two different setups for the compiler. In order for OpenCV C++ to work with OpenFrameworks code that are complied and ran in VSCode with Clang, the complier must have the same `-arch` flag setup otherwise they would conflict each other. Similar like: [this](https://stackoverflow.com/questions/76057246/why-does-cmake-insist-that-the-processor-used-is-x86-64-when-it-is-actually-arm6).

Clang Output for Openframeworks VSCode Build DEBUG:
```
-arch arm64 -platform_version macos 11.0.0 14.5
```
vs 

Clang output for OpenCV Homebrew CMake Complilation: 
```
-arch x86_64
```

If you're using an x86_64 version of Homebrew, when you install packages with it:
- The packages path would be: `/usr/local/Cellar`
- What you saw on the output when using Homebrew, you wont' see the word arm64 in the most of the bottle Homebrew downloaded for you.
- When you tried to `which brew`, the result would be somewhere also in the `/usr/local` directory.

If you're using an arm64 version of Homebrew:
- The packages path would be: `/opt/homebrew/Cellar`
- What you saw on the output when using Homebrew, you will see the word arm64 in the most of the bottle Homebrew downloaded for you.
Like this: `==> Pouring opencv--4.9.0_8.arm64_sonoma.bottle.tar.gz
🍺  /opt/homebrew/Cellar/opencv/4.9.0_8: 965 files, 138.7MB`
- When you tried to `which brew`, the result would be `/opt/homebrew/bin/brew`.

As I also installed cmake with Homebrew, it has resulted in both the failure of compliation from source and directly downloading bottles from Homebrew. This is exactly what occurred in my case. (Reference: <https://docs.brew.sh/Common-Issues#unintentional-dual-homebrew-installations>)
> Unintentional dual Homebrew installations
When using tools such as Apple’s Migration Assistant (MA), it’s possible to have two Homebrew installations unintentionally. This most commonly results in MA copying /usr/local and /Applications from an Intel-based Mac to these same paths on an Apple Silicon-based Mac. This is problematic because /Applications may contain x86_64-only apps. Using an x86_64 terminal emulator will cause the shell to use the /usr/local installation of Homebrew instead of a new installation in /opt/homebrew, which is the correct path for an arm64 Homebrew installation on macOS.

You can To rectify the installation from a setup like mine, please follow the instruction from Homebrew ([also this](<https://docs.brew.sh/Common-Issues#unintentional-dual-homebrew-installations>)) to get rid of the old x86-64 version of Homebrew to get a less chaotic installation enviornment in your MacBook lol. Or if you want to keep both arm and x86_64 version of Homebrew, at least make sure you are using the ARM one to install OpenCV and Clang for the shell/environment you're using. 


## Setting the correct flags and locations on VSCode

After having all 
as you may see from the official documentation and the git tutorial, OpenFramework does not work without setup with natively generated project on VSCode.
Gotta

https://stackoverflow.com/questions/24985713/opencv-undefined-symbols-for-architecture-x86-64-error

pkg-configs
followed the tutorial, but seems not enough


## No 
follow a commit

## Steps
- Install
- No
- config.make

