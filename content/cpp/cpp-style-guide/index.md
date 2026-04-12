+++
date = '2026-04-12T16:30:00+08:00'
title = 'C++ 编程规范'
slug = 'cpp-style-guide'
categories = ['教程']
+++

# C++ 编程规范

这是我的个人 C++ 编程规范，基于现代 C++ 标准（C++11 及以上）。在编写 C++ 代码时，Claude 应严格遵循这些规则。

## 命名规范

### 变量命名

- **局部变量**: 小写字母 + 下划线后缀
  ```cpp
  int count_ = 0;
  std::string name_ = "example";
  double temperature_ = 25.5;
  ```

- **成员变量**: 小写字母 + 下划线后缀
  ```cpp
  class MyClass {
  private:
      int value_;
      std::string data_;
  };
  ```

- **全局变量**: 避免使用，如必须使用则加 `g_` 前缀
  ```cpp
  int g_global_counter = 0;
  ```

### 函数命名

- **普通函数**: 小写字母，单词间用下划线分隔
  ```cpp
  void calculate_sum();
  int get_value();
  ```

- **类成员函数**: 小写字母，单词间用下划线分隔
  ```cpp
  class MyClass {
  public:
      void process_data();
      int get_count() const;
  };
  ```

### 类型命名

- **类名**: 大驼峰命名法（PascalCase）
  ```cpp
  class DataProcessor {};
  class HttpClient {};
  ```

- **结构体**: 大驼峰命名法
  ```cpp
  struct Point {
      double x_;
      double y_;
  };
  ```

- **枚举**: 大驼峰命名法，枚举值全大写
  ```cpp
  enum class Color {
      RED,
      GREEN,
      BLUE
  };
  ```

- **类型别名**: 大驼峰命名法
  ```cpp
  using StringList = std::vector<std::string>;
  using IntPtr = std::unique_ptr<int>;
  ```

### 常量命名

- **常量**: 全大写，单词间用下划线分隔
  ```cpp
  const int MAX_BUFFER_SIZE = 1024;
  constexpr double PI = 3.14159265359;
  ```

### 命名空间

- **命名空间**: 全小写，单词间用下划线分隔
  ```cpp
  namespace my_project {
  namespace utils {
      // ...
  }
  }
  ```

## 初始化规范

### 统一初始化（Uniform Initialization）

**强制使用花括号 `{}` 进行变量初始化**，避免使用圆括号 `()` 或赋值 `=`。

```cpp
// ✅ 正确：使用花括号初始化
int value_{42};
double pi_{3.14};
std::string name_{"Alice"};
std::vector<int> numbers_{1, 2, 3, 4, 5};

// ❌ 错误：不使用圆括号或赋值
int value(42);
double pi = 3.14;
```

### 类成员初始化

优先使用成员初始化列表，并使用花括号：

```cpp
class MyClass {
public:
    MyClass(int value, std::string name)
        : value_{value}
        , name_{std::move(name)}
        , data_{} {
    }

private:
    int value_;
    std::string name_;
    std::vector<int> data_;
};
```

### 默认成员初始化

在类定义中直接初始化成员变量：

```cpp
class Config {
private:
    int timeout_{30};
    bool enabled_{true};
    std::string host_{"localhost"};
};
```

## 设计模式

### 单例模式

**强制使用静态局部变量实现单例**（Meyers' Singleton），线程安全且简洁。

```cpp
class Singleton {
public:
    // 禁止拷贝和赋值
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    // 获取单例实例
    static Singleton& get_instance() {
        static Singleton instance_{};
        return instance_;
    }

    void do_something() {
        // ...
    }

private:
    // 私有构造函数
    Singleton() = default;
    ~Singleton() = default;
};

// 使用方式
Singleton::get_instance().do_something();
```

**禁止使用以下单例实现方式**：
- ❌ 全局静态指针 + 懒加载（非线程安全）
- ❌ 双重检查锁定（复杂且易错）
- ❌ 饿汉式静态成员（初始化顺序问题）

## 现代 C++ 特性使用

### 智能指针

- **优先使用智能指针**，避免裸指针和手动内存管理
- 独占所有权使用 `std::unique_ptr`
- 共享所有权使用 `std::shared_ptr`
- 避免使用 `std::auto_ptr`（已废弃）

```cpp
// ✅ 正确
auto ptr_ = std::make_unique<MyClass>(arg1, arg2);
auto shared_ = std::make_shared<Data>();

// ❌ 错误
MyClass* ptr = new MyClass(arg1, arg2);  // 需要手动 delete
```

### auto 关键字

- 复杂类型使用 `auto` 简化代码
- 简单类型可显式声明以提高可读性

```cpp
// ✅ 推荐
auto it_ = container_.begin();
auto result_ = std::make_unique<ComplexType>();

// ✅ 可接受
int count_{0};
double ratio_{1.5};
```

### 范围 for 循环

优先使用范围 for 循环：

```cpp
// ✅ 正确
for (const auto& item : container_) {
    process(item);
}

// ✅ 修改元素
for (auto& item : container_) {
    item.update();
}
```

### nullptr

使用 `nullptr` 代替 `NULL` 或 `0`：

```cpp
// ✅ 正确
int* ptr_{nullptr};

// ❌ 错误
int* ptr = NULL;
int* ptr = 0;
```

### constexpr

编译期常量使用 `constexpr`：

```cpp
constexpr int BUFFER_SIZE = 1024;
constexpr double calculate_area(double radius) {
    return 3.14159 * radius * radius;
}
```

### 强类型枚举

使用 `enum class` 代替传统 `enum`：

```cpp
// ✅ 正确
enum class Status {
    SUCCESS,
    FAILURE,
    PENDING
};

Status status_{Status::SUCCESS};

// ❌ 错误
enum Status {
    SUCCESS,
    FAILURE
};
```

## 函数设计

### 参数传递

- **输入参数**：
  - 基本类型：按值传递
  - 对象类型：`const` 引用传递
  
- **输出参数**：
  - 优先使用返回值
  - 多个返回值使用 `std::tuple` 或结构体
  - 必要时使用非 `const` 引用或指针

```cpp
// ✅ 正确
void process(int value, const std::string& name);
std::string get_name() const;
std::tuple<int, std::string> get_info() const;

// ❌ 避免
void get_name(std::string& out_name) const;  // 优先用返回值
```

### 返回值优化

利用 RVO（返回值优化）和移动语义：

```cpp
std::vector<int> create_vector() {
    std::vector<int> result_{};
    // ... 填充数据
    return result_;  // 自动移动，无需 std::move
}
```

### const 正确性

- 成员函数不修改对象状态时标记为 `const`
- 参数不修改时使用 `const` 引用

```cpp
class MyClass {
public:
    int get_value() const { return value_; }
    void set_value(int value) { value_ = value; }
    
private:
    int value_;
};
```

## 资源管理

### RAII 原则

所有资源获取即初始化，利用析构函数自动释放：

```cpp
class FileHandler {
public:
    explicit FileHandler(const std::string& filename)
        : file_{std::fopen(filename.c_str(), "r")} {
        if (!file_) {
            throw std::runtime_error("Failed to open file");
        }
    }
    
    ~FileHandler() {
        if (file_) {
            std::fclose(file_);
        }
    }
    
    // 禁止拷贝
    FileHandler(const FileHandler&) = delete;
    FileHandler& operator=(const FileHandler&) = delete;
    
    // 允许移动
    FileHandler(FileHandler&& other) noexcept
        : file_{other.file_} {
        other.file_ = nullptr;
    }
    
private:
    FILE* file_;
};
```

### 移动语义

- 实现移动构造和移动赋值以提高性能
- 标记为 `noexcept` 以支持容器优化

```cpp
class MyClass {
public:
    MyClass(MyClass&& other) noexcept
        : data_{std::move(other.data_)}
        , size_{other.size_} {
        other.size_ = 0;
    }
    
    MyClass& operator=(MyClass&& other) noexcept {
        if (this != &other) {
            data_ = std::move(other.data_);
            size_ = other.size_;
            other.size_ = 0;
        }
        return *this;
    }
    
private:
    std::vector<int> data_;
    size_t size_;
};
```

## 异常处理

- 构造函数失败使用异常
- 使用 RAII 保证异常安全
- 析构函数标记为 `noexcept`

```cpp
class Resource {
public:
    Resource() {
        if (!initialize()) {
            throw std::runtime_error("Initialization failed");
        }
    }
    
    ~Resource() noexcept {
        cleanup();
    }
    
private:
    bool initialize();
    void cleanup() noexcept;
};
```

## 代码组织

### 头文件保护

使用 `#pragma once`：

```cpp
#pragma once

// 头文件内容
```

### 包含顺序

1. 对应的头文件（如果是 .cpp 文件）
2. C 系统头文件
3. C++ 标准库头文件
4. 第三方库头文件
5. 项目内部头文件

```cpp
#include "my_class.h"  // 对应头文件

#include <cstdio>      // C 系统头文件
#include <cstring>

#include <iostream>    // C++ 标准库
#include <vector>
#include <memory>

#include <boost/algorithm/string.hpp>  // 第三方库

#include "utils/helper.h"  // 项目内部
```

## 禁止使用的特性

- ❌ 裸指针的 `new`/`delete`（使用智能指针）
- ❌ C 风格数组（使用 `std::array` 或 `std::vector`）
- ❌ C 风格字符串（使用 `std::string`）
- ❌ 宏定义函数（使用 `constexpr` 函数或内联函数）
- ❌ `goto` 语句
- ❌ 全局可变状态
- ❌ `using namespace std;`（污染命名空间）

## 性能优化建议

- 优先使用 `emplace_back` 而非 `push_back`
- 预留容器容量 `reserve()` 避免重新分配
- 使用 `std::move` 转移临时对象
- 避免不必要的拷贝，使用引用传递
- 小对象可以按值传递（编译器优化）

```cpp
std::vector<MyClass> vec_{};
vec_.reserve(100);  // 预留空间

vec_.emplace_back(arg1, arg2);  // 直接构造
vec_.push_back(MyClass{arg1, arg2});  // 构造后移动

auto result_ = std::move(temp_object_);  // 显式移动
```

## 总结

这些规范旨在：
- 提高代码可读性和一致性
- 利用现代 C++ 特性提升安全性和性能
- 减少常见错误和内存泄漏
- 遵循业界最佳实践

**Claude 在编写 C++ 代码时必须严格遵循以上所有规范。**
