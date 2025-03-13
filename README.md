# Cursor AI 助手使用指南

## 目录
1. [基础配置文件](#基础配置文件)
2. [Add Context 功能详解](#add-context-功能详解)
3. [实际应用场景](#实际应用场景)
4. [最佳实践](#最佳实践)

## 基础配置文件

### .cursorrules
项目根目录下的全局规则配置文件，用于定义 AI 助手的行为规则。

```yaml
# .cursorrules
rules:
  - when: "生成代码"
    then:
      - "使用 Swift 最新语法"
      - "添加中文注释"
      - "遵循项目规范"
      - "添加错误处理"
```

### .cursor/rules/*.mdc
模块化的规则文件，用于定义特定场景或模块的详细规则。

```markdown
# .cursor/rules/auth.mdc
## 认证模块规范

### 代码结构
- 使用 MVVM 架构
- 实现错误处理
- 添加单元测试

### API 规范
- 使用 RESTful 设计
- 实现请求重试
- 处理超时情况
```

## Add Context 功能详解

### 1. Code
用于添加代码片段作为上下文：

```swift
@Add context code:
class LoginViewController: UIViewController {
    private let viewModel: LoginViewModel
    
    func login() {
        // 实现登录逻辑
    }
}
```

#### 实践示例
1. 代码复查场景
```swift
@Add context code:
// 原始代码
func fetchUserData() {
    api.request { data in
        self.data = data!  // 潜在的强制解包问题
    }
}

// 优化后的代码
func fetchUserData() {
    api.request { [weak self] result in
        switch result {
        case .success(let data):
            self?.handleData(data)
        case .failure(let error):
            self?.handleError(error)
        }
    }
}
```

2. 性能优化场景
```swift
@Add context code:
// 性能优化前
class ImageLoader {
    func loadImages() {
        images.forEach { url in
            downloadImage(url)  // 可能导致内存峰值
        }
    }
}

// 性能优化后
class ImageLoader {
    private let queue = OperationQueue()
    
    func loadImages() {
        queue.maxConcurrentOperationCount = 3
        images.forEach { url in
            queue.addOperation {
                self.downloadImage(url)
            }
        }
    }
}
```

### 2. Docs
用于添加文档说明：

```
@Add context docs:
# 登录模块说明
- 支持账号密码登录
- 支持生物识别
- 实现记住密码
```

#### 实践示例
1. API 文档场景
```
@Add context docs:
## 用户认证 API
POST /api/v1/auth/login
请求体:
{
    "username": "string",
    "password": "string",
    "device_id": "string"
}
响应:
{
    "token": "string",
    "expires_in": 3600
}
错误码:
- 401: 认证失败
- 429: 请求过于频繁
```

2. 架构设计文档
```
@Add context docs:
# 缓存系统设计
## 架构图
[缓存架构图链接]

## 关键组件
1. 内存缓存层
   - LRU 策略
   - 最大容量: 100MB
2. 磁盘缓存层
   - LFU 策略
   - 最大容量: 1GB

## 缓存策略
1. 图片缓存: 7天
2. API 响应: 1小时
3. 用户配置: 永久
```

### 3. Git
用于添加版本控制相关信息：

#### 实践示例
1. 代码审查场景
```
@Add context git:
branch: feature/user-profile
base: develop
changes:
  - UserProfileViewController.swift (+150/-50)
  - UserProfileViewModel.swift (+80/-20)
  - UserService.swift (+30/-5)
commits:
  - hash: abc123
    message: "添加用户资料编辑功能"
    author: "张三"
  - hash: def456
    message: "实现头像上传功能"
    author: "李四"
review_comments:
  - file: UserProfileViewController.swift
    line: 25
    comment: "建议使用依赖注入"
```

2. 版本发布场景
```
@Add context git:
release: v2.1.0
changes:
  - type: feature
    description: "新增用户资料编辑"
    commits: [abc123, def456]
  - type: bugfix
    description: "修复登录闪退问题"
    commits: [ghi789]
affected_modules:
  - 用户模块
  - 认证模块
test_coverage: 85%
```

### 4. Terminals
用于添加终端命令和输出：

```
@Add context terminals:
command: pod install
output: |
  Installing Alamofire (5.8.1)
  Installing Kingfisher (7.10.1)
  Pod installation complete!
```

### 5. CodeBase
用于提供项目结构信息：

```
@Add context codebase:
structure:
  - Sources/
    - Features/
      - Login/
      - Home/
    - Common/
      - Utils/
      - Extensions/
```

### 6. Linter Errors
用于提供代码检查信息：

```
@Add context linter_errors:
file: "LoginViewController.swift"
errors:
  - line: 15
    message: "Variable name should be camelCase"
    severity: "warning"
```

### 7. Web
用于添加在线资源引用：

```
@Add context web:
api_docs:
  - url: "https://api.example.com/swagger"
    type: "Swagger API"
design:
  - url: "https://www.figma.com/file/xxx"
    type: "UI Design"
```

### 8. Recent Changes
用于提供最近代码变更信息：

#### 实践示例
1. 代码重构追踪
```
@Add context recent_changes:
refactor:
  file: "NetworkManager.swift"
  changes:
    - timestamp: "2024-01-20 10:00"
      description: "重构网络层，采用 URLSession 替代 Alamofire"
      impact:
        - "所有网络请求类"
        - "缓存管理器"
      metrics:
        - "性能提升 30%"
        - "内存占用减少 20%"
```

2. 性能优化记录
```
@Add context recent_changes:
optimization:
  file: "ImageCache.swift"
  changes:
    - timestamp: "2024-01-21 15:30"
      type: "性能优化"
      description: "优化图片缓存机制"
      details:
        - "实现 LRU 缓存策略"
        - "添加内存警告自动清理"
        - "优化图片解码流程"
      metrics:
        - "内存峰值降低 40%"
        - "加载速度提升 50%"
```

## 实际应用场景

### 1. 新功能开发
在开发新功能时，可以结合多个 Context 类型：

```
@Add context codebase:
feature: "用户认证模块重构"
structure:
  - Auth/
    - Controllers/
      - LoginViewController.swift
      - RegisterViewController.swift
    - ViewModels/
      - AuthViewModel.swift
    - Services/
      - AuthService.swift
    - Models/
      - User.swift

@Add context docs:
# 重构目标
1. 提升代码可维护性
2. 优化性能
3. 增强安全性

# 技术方案
1. 采用 MVVM 架构
2. 使用 Combine 处理数据流
3. 实现双因素认证

@Add context git:
branch: feature/auth-refactor
commits:
  - "初始化项目结构"
  - "实现基础认证流程"
  - "添加生物识别支持"
```

### 2. 性能优化
针对性能问题的优化过程：

```
@Add context linter_errors:
performance_warnings:
  - file: "ImageLoader.swift"
    issues:
      - "大量内存分配"
      - "主线程阻塞"
      - "未使用缓存机制"

@Add context code:
// 优化前
class ImageLoader {
    func loadImage(url: URL, completion: @escaping (UIImage?) -> Void) {
        URLSession.shared.dataTask(with: url) { data, _, _ in
            guard let data = data else { return }
            let image = UIImage(data: data)
            DispatchQueue.main.async {
                completion(image)
            }
        }.resume()
    }
}

// 优化后
class ImageLoader {
    private let cache = NSCache<NSURL, UIImage>()
    private let queue = OperationQueue()
    
    func loadImage(url: URL, completion: @escaping (UIImage?) -> Void) {
        if let cachedImage = cache.object(forKey: url as NSURL) {
            completion(cachedImage)
            return
        }
        
        queue.addOperation {
            URLSession.shared.dataTask(with: url) { [weak self] data, _, _ in
                guard let data = data,
                      let image = UIImage(data: data) else {
                    return
                }
                self?.cache.setObject(image, forKey: url as NSURL)
                DispatchQueue.main.async {
                    completion(image)
                }
            }.resume()
        }
    }
}

@Add context recent_changes:
optimization_results:
  - metric: "内存使用"
    improvement: "减少 40%"
  - metric: "加载时间"
    improvement: "减少 60%"
  - metric: "CPU 使用率"
    improvement: "减少 30%"
```

### 3. 安全审计
进行安全审计时的工作流程：

```
@Add context docs:
# 安全审计清单
1. 数据存储安全
2. 网络传输安全
3. 认证授权机制
4. 敏感信息处理

@Add context code:
// 安全性问题示例
class UserDefaults {
    static func savePassword(_ password: String) {
        UserDefaults.standard.set(password, forKey: "password")  // 不安全
    }
}

// 安全改进后
class SecureStorage {
    static func savePassword(_ password: String) throws {
        let data = password.data(using: .utf8)!
        let query: [String: Any] = [
            kSecClass as String: kSecClassGenericPassword,
            kSecAttrAccount as String: "userPassword",
            kSecValueData as String: data
        ]
        let status = SecItemAdd(query as CFDictionary, nil)
        guard status == errSecSuccess else {
            throw SecurityError.saveFailed
        }
    }
}

@Add context recent_changes:
security_updates:
  - timestamp: "2024-01-22"
    changes:
      - "实现 Keychain 存储"
      - "添加证书固定"
      - "实现敏感数据加密"
```

## 最佳实践

### 1. 规则文件组织
- 使用模块化的规则文件结构
- 根据功能领域划分规则
- 定期审查和更新规则

### 2. Context 使用建议
- 优先使用最相关的 Context 类型
- 保持信息简洁且有针对性
- 及时更新过时的 Context 信息

### 3. 团队协作
- 建立统一的 Context 使用规范
- 定期同步和更新文档
- 使用版本控制管理规则文件

### 4. 性能考虑
- 避免添加过多无关 Context
- 定期清理过时的 Context
- 优化 Context 的存储结构

## 总结

Cursor AI 助手的 Context 功能提供了强大的上下文管理能力，通过合理使用这些功能，可以：
1. 提高开发效率和代码质量
2. 加强团队协作和知识共享
3. 优化项目管理和维护
4. 提升代码审查和问题诊断效率

持续学习和实践这些功能，将帮助团队更好地利用 Cursor AI 助手进行开发工作。 