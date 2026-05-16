---
title: "Qt HTTPS 部署依赖库说明"
date: 2026-05-16
draft: false
categories: ["qt"]
tags: ["Qt", "HTTPS", "部署"]
---

## 使用 Qt 进行 HTTPS 通信需要哪些库？

当使用 Qt 的 `QSslSocket` 进行 HTTPS 通信时，需要拷贝以下 DLL 文件到程序目录：

### 必须拷贝的 DLL 文件

```
your-app/
├── ssleay32.dll      # OpenSSL SSL 库
├── libeay32.dll      # OpenSSL 基础库
├── msvcr100.dll      # VC++ 2010 C 运行时
└── msvcp100.dll      # VC++ 2010 C++ 运行时
```

### 版本要求

- **OpenSSL 版本**：OpenSSL 1.0.2j（使用 `ssleay32.dll` 和 `libeay32.dll`）
- **VC++ 运行时**：Visual C++ 2010 Redistributable (x86)

### 重要注意事项

1. **位数必须匹配**：32 位程序用 32 位 DLL，64 位程序用 64 位 DLL
2. **版本必须匹配**：OpenSSL DLL 版本需与 Qt 编译时使用的版本一致

### 部署检查

运行前确保程序目录下包含上述 4 个 DLL 文件即可。
