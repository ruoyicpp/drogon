# Drogon 预编译库

为 [RuoYi-Cpp](https://gitee.com/ruoyicpp/ruoyi-cpp) 项目提供的 Drogon 框架预编译静态库。

## 📦 包含内容

- **Windows** (MSYS2 UCRT64) - 13 MB
  - libdrogon.a, libtrantor.a, libjsoncpp.a
  - 完整头文件

- **Linux** (Ubuntu/Debian) - 13 MB
  - libdrogon.a, libtrantor.a
  - 完整头文件

## 🚀 快速使用

### 1. 克隆仓库

```bash
git clone https://gitee.com/ruoyicpp/drogon.git
```

### 2. 放置到项目中

将 `windows/` 和 `linux/` 目录放到 RuoYi-Cpp 项目的 `drogon/` 目录。

### 3. 修改 CMakeLists.txt

```cmake
if(WIN32)
    set(DROGON_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/drogon/windows")
else()
    set(DROGON_INSTALL_PREFIX "${CMAKE_SOURCE_DIR}/drogon/linux")
endif()
```

### 4. 安装系统依赖

**Windows (MSYS2 UCRT64):**
```bash
pacman -S mingw-w64-ucrt-x86_64-gcc mingw-w64-ucrt-x86_64-cmake \
          mingw-w64-ucrt-x86_64-openssl mingw-w64-ucrt-x86_64-postgresql
```

**Linux:**
```bash
sudo apt-get install -y build-essential cmake libssl-dev libpq-dev libjsoncpp-dev
```

### 5. 编译项目

```bash
cmake -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j$(nproc)
```

## 📋 版本信息

- **Drogon**: 最新稳定版
- **编译日期**: 2026-05-13
- **C++ 标准**: C++17

## 🔗 相关链接

- [RuoYi-Cpp 主项目](https://gitee.com/ruoyicpp/ruoyi-cpp)
- [Drogon 官网](https://drogon.org/)
- [Drogon GitHub](https://github.com/drogonframework/drogon)

## 📄 许可证

Drogon 框架采用 MIT 许可证。
