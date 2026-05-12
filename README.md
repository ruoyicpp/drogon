<div align="center">

[English](README_EN.md) | 中文

# Drogon 预编译静态库

**Drogon 官方框架预编译静态库**

为 [RuoYi-Cpp](https://gitee.com/ruoyicpp/ruoyi) 项目提供开箱即用的 Drogon 静态库

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

**参考项目**: [RuoYi-Cpp 管理框架](https://gitee.com/ruoyicpp/ruoyi) | [GitHub](https://github.com/ruoyicpp/ruoyi)

[快速开始](#-快速开始) • [使用说明](#-使用说明) • [常见问题](#-常见问题)

</div>

---

## 📌 关于 Drogon

**Drogon** 是一个基于 C++17/20 的高性能 HTTP 应用框架，由 Drogon 官方团队开发维护。

- 🌐 **官方网站**: https://drogon.org/
- 💻 **官方 GitHub**: https://github.com/drogonframework/drogon
- 📖 **官方文档**: https://drogon.docsforge.com/

> **说明**: 本仓库提供的是 **Drogon 官方源码**编译的静态库，未做任何修改，确保与官方版本完全一致。

---

## 📦 包含内容

### Windows 平台 (MSYS2 UCRT64)
```
windows/
├── include/          # 头文件 (1.1 MB)
│   ├── drogon/      # Drogon 框架头文件
│   ├── trantor/     # Trantor 网络库头文件
│   └── json/        # JsonCpp 头文件
└── lib/             # 静态库 (12 MB)
    ├── libdrogon.a   (9.6 MB)  - Drogon 主库
    ├── libtrantor.a  (1.4 MB)  - 网络库
    └── libjsoncpp.a  (380 KB)  - JSON 库
```

### Linux 平台 (Ubuntu/Debian)
```
linux/
├── include/          # 头文件 (1.5 MB)
│   ├── drogon/      # Drogon 框架头文件
│   └── trantor/     # Trantor 网络库头文件
└── lib/             # 静态库 (12 MB)
    ├── libdrogon.a   (11 MB)   - Drogon 主库
    └── libtrantor.a  (1.4 MB)  - 网络库
```

**总大小**: 约 26 MB (Windows 13 MB + Linux 13 MB)

---

## 🚀 快速开始

### 方式一：使用预编译库（推荐）

**优点**: 
- ✅ 无需编译 Drogon，节省时间
- ✅ 避免编译错误和依赖问题
- ✅ 开箱即用，快速开始开发

**步骤**:

1. **克隆本仓库**
   ```bash
   # Gitee (国内推荐)
   git clone https://gitee.com/ruoyicpp/drogon.git
   
   # GitHub
   git clone https://github.com/ruoyicpp/drogon.git
   ```

2. **放置到项目中**
   
   将克隆的 `drogon` 目录放到你的项目根目录：
   ```
   your-project/
   ├── drogon/           ← 放在这里
   │   ├── windows/
   │   └── linux/
   ├── src/
   ├── CMakeLists.txt
   └── ...
   ```

3. **配置 CMakeLists.txt**
   
   在你的 `CMakeLists.txt` 中添加：
   ```cmake
   # 自动检测平台
   if(WIN32)
       set(DROGON_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/drogon/windows")
   else()
       set(DROGON_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/drogon/linux")
   endif()
   
   # 设置头文件路径
   include_directories(${DROGON_INSTALL_PREFIX}/include)
   
   # 设置库文件路径
   link_directories(${DROGON_INSTALL_PREFIX}/lib)
   
   # 链接库
   target_link_libraries(your_target
       drogon
       trantor
       # Windows 还需要链接 jsoncpp
       $<$<PLATFORM_ID:Windows>:jsoncpp>
   )
   ```

4. **安装系统依赖**

   **Windows (MSYS2)**:
   
   **推荐：UCRT64 工具链**（与预编译库 ABI 完全匹配）
   ```bash
   # 打开 MSYS2 UCRT64 终端
   pacman -S mingw-w64-ucrt-x86_64-gcc \
             mingw-w64-ucrt-x86_64-cmake \
             mingw-w64-ucrt-x86_64-openssl \
             mingw-w64-ucrt-x86_64-postgresql \
             mingw-w64-ucrt-x86_64-hiredis
   ```
   
   **可选：MinGW64 工具链**（需要明文模式编译）
   ```bash
   # 打开 MSYS2 MinGW64 终端
   pacman -S mingw-w64-x86_64-gcc \
             mingw-w64-x86_64-cmake \
             mingw-w64-x86_64-openssl \
             mingw-w64-x86_64-postgresql \
             mingw-w64-x86_64-hiredis
   ```
   
   > **说明**: 
   > - **UCRT64**：推荐使用，与预编译库 ABI 完全兼容
   > - **MinGW64**：可以使用，但需要在 CMake 中添加 `-DRUOYI_DEV_BYPASS_LICENSE=ON` 跳过许可证加密功能

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

5. **编译项目**
   ```bash
   cmake -B build -DCMAKE_BUILD_TYPE=Release
   cmake --build build -j$(nproc)
   ```

---

## 📚 使用说明

### 为什么使用静态库？

| 特性 | 静态库 (.a) | 动态库 (.so/.dll) |
|------|------------|-------------------|
| 部署 | ✅ 单文件，无需额外依赖 | ❌ 需要携带 .so/.dll 文件 |
| 性能 | ✅ 编译时优化更好 | ⚠️ 运行时加载有开销 |
| 兼容性 | ✅ 不受系统库版本影响 | ❌ 可能出现版本冲突 |
| 文件大小 | ⚠️ 可执行文件较大 | ✅ 可执行文件较小 |

**推荐场景**: 
- ✅ 生产环境部署
- ✅ 跨平台分发
- ✅ 避免依赖地狱

### 链接顺序说明

静态库链接顺序很重要，正确的顺序是：
```cmake
target_link_libraries(your_target
    drogon          # 1. Drogon 主库
    trantor         # 2. Trantor 网络库
    jsoncpp         # 3. JSON 库 (Windows)
    ssl crypto      # 4. OpenSSL
    pthread dl      # 5. 系统库 (Linux)
)
```

### 完整示例

创建一个简单的 HTTP 服务器：

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

# 配置 Drogon 路径
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

**编译运行**:
```bash
cmake -B build
cmake --build build
./build/myapp
```

访问 http://localhost:8080 即可！

---

## ❓ 常见问题

### Q1: Windows 编译报错 "ENTRYPOINT_NOT_FOUND"

**原因**: 使用了错误的工具链（MinGW64 而非 UCRT64）

**解决**: 
1. 确保使用 **MSYS2 UCRT64** 终端（不是 MinGW64）
2. 检查 PATH 环境变量，确保 UCRT64 路径在最前面
3. 删除 `build` 目录重新编译

### Q2: Linux 编译找不到头文件

**原因**: 系统依赖未安装

**解决**:
```bash
sudo apt-get install -y libssl-dev libpq-dev libjsoncpp-dev
```

### Q3: 链接时报错 "undefined reference"

**原因**: 静态库链接顺序错误或依赖缺失

**解决**: 
1. 检查链接顺序（drogon → trantor → jsoncpp）
2. 确保所有系统依赖已安装
3. 添加 `-lpthread -ldl` (Linux)

### Q4: 如何验证编译成功？

运行程序后，如果看到以下输出说明成功：
```
[INFO] Server running on 0.0.0.0:8080
```

### Q5: 想要重新编译 Drogon？

如果预编译库不适合你的环境，可以从官方源码编译：

**从官方 GitHub 编译**:
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

## 📋 版本信息

| 项目 | 版本 |
|------|------|
| Drogon | 最新稳定版 |
| 编译日期 | 2026-05-13 |
| C++ 标准 | C++17 |
| Windows 编译器 | GCC 13.x (MSYS2 UCRT64) |
| Linux 编译器 | GCC 13.3.0 |
| 编译选项 | Release, 静态链接 |

---

## 🔗 相关链接

- 📦 [RuoYi-Cpp 主项目 (Gitee)](https://gitee.com/ruoyicpp/ruoyi) - 使用本库的完整项目
- 📦 [RuoYi-Cpp 主项目 (GitHub)](https://github.com/ruoyicpp/ruoyi) - GitHub 镜像
- 🌐 [Drogon 官方网站](https://drogon.org/) - 官方主页
- 💻 [Drogon GitHub](https://github.com/drogonframework/drogon) - 官方源码
- 📖 [Drogon 文档](https://drogon.docsforge.com/) - 官方文档
- 🛠️ [MSYS2 官网](https://www.msys2.org/) - Windows 编译环境

---

## 📄 许可证

Drogon 框架采用 **MIT 许可证**。

本预编译库仓库同样采用 MIT 许可证。

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

如果这个预编译库对你有帮助，请给个 ⭐ Star 支持一下！

---

<div align="center">

**Made with ❤️ for RuoYi-Cpp**

</div>
