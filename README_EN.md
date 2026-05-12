<div align="center">

English | [中文](README.md)

# Drogon Prebuilt Static Libraries

**Official Drogon Framework Prebuilt Static Libraries**

Ready-to-use Drogon static libraries for [RuoYi-Cpp](https://gitee.com/ruoyicpp/ruoyi-cpp) project

<p>
<img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License">
<img src="https://img.shields.io/badge/C++-17-blue.svg" alt="C++17">
<img src="https://img.shields.io/badge/Drogon-latest-green.svg" alt="Drogon">
<img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux-lightgrey.svg" alt="Platform">
</p>

<p>
<img src="https://img.shields.io/badge/Windows-MSYS2%20UCRT64-blue.svg" alt="Windows">
<img src="https://img.shields.io/badge/Linux-Ubuntu%20%7C%20Debian-orange.svg" alt="Linux">
<img src="https://img.shields.io/badge/size-26%20MB-green.svg" alt="Size">
</p>

**Reference Project**: [RuoYi-Cpp Framework](https://gitee.com/ruoyicpp/ruoyi-cpp) | [GitHub](https://github.com/ruoyicpp/ruoyi)

[Quick Start](#-quick-start) • [Usage](#-usage) • [FAQ](#-faq)

</div>

---

## 📌 About Drogon

**Drogon** is a high-performance HTTP application framework based on C++17/20, developed and maintained by the official Drogon team.

- 🌐 **Official Website**: https://drogon.org/
- 💻 **Official GitHub**: https://github.com/drogonframework/drogon
- 📖 **Official Documentation**: https://drogon.docsforge.com/

> **Note**: This repository provides static libraries compiled from **official Drogon source code** without any modifications, ensuring complete consistency with the official version.

---

## 📦 Contents

### Windows Platform (MSYS2 UCRT64)
```
windows/
├── include/          # Headers (1.1 MB)
│   ├── drogon/      # Drogon framework headers
│   ├── trantor/     # Trantor network library headers
│   └── json/        # JsonCpp headers
└── lib/             # Static libraries (12 MB)
    ├── libdrogon.a   (9.6 MB)  - Drogon main library
    ├── libtrantor.a  (1.4 MB)  - Network library
    └── libjsoncpp.a  (380 KB)  - JSON library
```

### Linux Platform (Ubuntu/Debian)
```
linux/
├── include/          # Headers (1.5 MB)
│   ├── drogon/      # Drogon framework headers
│   └── trantor/     # Trantor network library headers
└── lib/             # Static libraries (12 MB)
    ├── libdrogon.a   (11 MB)   - Drogon main library
    └── libtrantor.a  (1.4 MB)  - Network library
```

**Total Size**: ~26 MB (Windows 13 MB + Linux 13 MB)

---

## 🚀 Quick Start

### Method 1: Use Prebuilt Libraries (Recommended)

**Advantages**: 
- ✅ No need to compile Drogon, save time
- ✅ Avoid compilation errors and dependency issues
- ✅ Ready to use, start developing quickly

**Steps**:

1. **Clone this repository**
   ```bash
   # Gitee (Recommended for China)
   git clone https://gitee.com/ruoyicpp/drogon.git
   
   # GitHub
   git clone https://github.com/ruoyicpp/drogon.git
   ```

2. **Place in your project**
   
   Put the cloned `drogon` directory in your project root:
   ```
   your-project/
   ├── drogon/           ← Place here
   │   ├── windows/
   │   └── linux/
   ├── src/
   ├── CMakeLists.txt
   └── ...
   ```

3. **Configure CMakeLists.txt**
   
   Add to your `CMakeLists.txt`:
   ```cmake
   # Auto-detect platform
   if(WIN32)
       set(DROGON_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/drogon/windows")
   else()
       set(DROGON_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/drogon/linux")
   endif()
   
   # Set include path
   include_directories(${DROGON_INSTALL_PREFIX}/include)
   
   # Set library path
   link_directories(${DROGON_INSTALL_PREFIX}/lib)
   
   # Link libraries
   target_link_libraries(your_target
       drogon
       trantor
       # Windows also needs jsoncpp
       $<$<PLATFORM_ID:Windows>:jsoncpp>
   )
   ```

4. **Install system dependencies**

   **Windows (MSYS2)**:
   
   **Recommended: UCRT64 Toolchain** (Full ABI compatibility with prebuilt libraries)
   ```bash
   # Open MSYS2 UCRT64 terminal
   pacman -S mingw-w64-ucrt-x86_64-gcc \
             mingw-w64-ucrt-x86_64-cmake \
             mingw-w64-ucrt-x86_64-openssl \
             mingw-w64-ucrt-x86_64-postgresql \
             mingw-w64-ucrt-x86_64-hiredis
   ```
   
   **Optional: MinGW64 Toolchain** (Requires plaintext mode compilation)
   ```bash
   # Open MSYS2 MinGW64 terminal
   pacman -S mingw-w64-x86_64-gcc \
             mingw-w64-x86_64-cmake \
             mingw-w64-x86_64-openssl \
             mingw-w64-x86_64-postgresql \
             mingw-w64-x86_64-hiredis
   ```
   
   > **Note**: 
   > - **UCRT64**: Recommended, fully compatible with prebuilt library ABI
   > - **MinGW64**: Can be used, but requires adding `-DRUOYI_DEV_BYPASS_LICENSE=ON` in CMake to skip license encryption

   **Linux (Ubuntu/Debian)**:
   ```bash
   sudo apt-get update
   sudo apt-get install -y \
       build-essential \
       cmake \
       libssl-dev \
       libpq-dev \
       libjsoncpp-dev \
       libhiredis-dev \
       uuid-dev \
       zlib1g-dev
   ```

5. **Build your project**
   ```bash
   cmake -B build -DCMAKE_BUILD_TYPE=Release
   cmake --build build -j$(nproc)
   ```

---

## 📚 Usage

### Why Use Static Libraries?

| Feature | Static Library (.a) | Dynamic Library (.so/.dll) |
|---------|---------------------|----------------------------|
| Deployment | ✅ Single file, no extra dependencies | ❌ Need to carry .so/.dll files |
| Performance | ✅ Better compile-time optimization | ⚠️ Runtime loading overhead |
| Compatibility | ✅ Not affected by system library versions | ❌ May have version conflicts |
| File Size | ⚠️ Larger executable | ✅ Smaller executable |

**Recommended Scenarios**: 
- ✅ Production deployment
- ✅ Cross-platform distribution
- ✅ Avoid dependency hell

### Linking Order

Static library linking order is important, the correct order is:
```cmake
target_link_libraries(your_target
    drogon          # 1. Drogon main library
    trantor         # 2. Trantor network library
    jsoncpp         # 3. JSON library (Windows)
    ssl crypto      # 4. OpenSSL
    pthread dl      # 5. System libraries (Linux)
)
```

### Complete Example

Create a simple HTTP server:

**main.cpp**:
```cpp
#include <drogon/drogon.h>

int main() {
    drogon::app()
        .setLogPath("./")
        .setLogLevel(trantor::Logger::kWarn)
        .addListener("0.0.0.0", 8080)
        .setThreadNum(4)
        .run();
    return 0;
}
```

**CMakeLists.txt**:
```cmake
cmake_minimum_required(VERSION 3.15)
project(MyApp)

set(CMAKE_CXX_STANDARD 17)

# Configure Drogon path
if(WIN32)
    set(DROGON_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/drogon/windows")
else()
    set(DROGON_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/drogon/linux")
endif()

include_directories(${DROGON_INSTALL_PREFIX}/include)
link_directories(${DROGON_INSTALL_PREFIX}/lib)

add_executable(myapp main.cpp)

target_link_libraries(myapp
    drogon
    trantor
    $<$<PLATFORM_ID:Windows>:jsoncpp>
)
```

**Build and Run**:
```bash
cmake -B build
cmake --build build
./build/myapp
```

Visit http://localhost:8080 and you're done!

---

## ❓ FAQ

### Q1: Windows compilation error "ENTRYPOINT_NOT_FOUND"

**Cause**: Using wrong toolchain (MinGW64 instead of UCRT64)

**Solution**: 
1. Make sure to use **MSYS2 UCRT64** terminal (not MinGW64)
2. Check PATH environment variable, ensure UCRT64 path is first
3. Delete `build` directory and recompile

### Q2: Linux compilation cannot find headers

**Cause**: System dependencies not installed

**Solution**:
```bash
sudo apt-get install -y libssl-dev libpq-dev libjsoncpp-dev
```

### Q3: Linking error "undefined reference"

**Cause**: Wrong static library linking order or missing dependencies

**Solution**: 
1. Check linking order (drogon → trantor → jsoncpp)
2. Ensure all system dependencies are installed
3. Add `-lpthread -ldl` (Linux)

### Q4: How to verify successful compilation?

After running the program, if you see the following output, it's successful:
```
[INFO] Server running on 0.0.0.0:8080
```

### Q5: Want to recompile Drogon?

If the prebuilt libraries don't fit your environment, you can compile from official source:

**Compile from official GitHub**:
```bash
git clone https://github.com/drogonframework/drogon.git
cd drogon
git submodule update --init
mkdir build && cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=OFF
make -j$(nproc)
sudo make install
```

---

## 📋 Version Info

| Item | Version |
|------|---------|
| Drogon | Latest stable |
| Build Date | 2026-05-13 |
| C++ Standard | C++17 |
| Windows Compiler | GCC 13.x (MSYS2 UCRT64) |
| Linux Compiler | GCC 13.3.0 |
| Build Options | Release, static linking |

---

## 🔗 Related Links

- 📦 [RuoYi-Cpp Main Project (Gitee)](https://gitee.com/ruoyicpp/ruoyi-cpp) - Complete project using this library
- 📦 [RuoYi-Cpp Main Project (GitHub)](https://github.com/ruoyicpp/ruoyi) - GitHub mirror
- 🌐 [Drogon Official Website](https://drogon.org/) - Official homepage
- 💻 [Drogon GitHub](https://github.com/drogonframework/drogon) - Official source code
- 📖 [Drogon Documentation](https://drogon.docsforge.com/) - Official docs
- 🛠️ [MSYS2 Official](https://www.msys2.org/) - Windows build environment

---

## 📄 License

Drogon framework uses **MIT License**.

This prebuilt library repository also uses MIT License.

---

## 🤝 Contributing

Issues and Pull Requests are welcome!

If this prebuilt library helps you, please give it a ⭐ Star!

---

<div align="center">

**Made with ❤️ for RuoYi-Cpp**

</div>
