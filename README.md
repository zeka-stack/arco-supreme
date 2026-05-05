---
published: 2022.01.01
---

# 全局基础父项目

## 📖 作用

`arco-supreme` 是 Zeka.Stack 框架的**最顶层父项目**，作为所有项目的基础父级 POM。它定义了全局的构建规范、依赖版本、插件版本等基础配置，是整个
Zeka.Stack 生态的根基。

## 🎯 为什么这么设计

### 1. 统一管理的基础

在大型项目中，统一管理是至关重要的：

- **版本一致性**：所有项目使用相同的依赖版本，避免版本冲突
- **构建规范**：统一的编码规范、构建流程、质量标准
- **技术栈统一**：确保整个组织使用统一的技术栈

### 2. 简化子项目配置

通过集中管理公共配置，子项目可以：

- **零配置使用**：继承父项目配置，无需重复定义
- **自动获得功能**：自动获得代码质量检查、测试覆盖率等功能
- **减少维护成本**：配置集中管理，升级时只需修改一处

### 3. 分层架构设计

`arco-supreme` 作为最顶层，定义了：

```
arco-supreme (全局基础) ← 当前模块
└── arco-builder (构建总控)
    └── arco-dependencies-parent (依赖管理)
        └── arco-project-dependencies (插件管理)
            └── arco-project-builder (构建逻辑)
                ├── arco-business-parent (业务型项目)
                └── arco-component-parent (组件型项目)
```

## 🚀 如何使用

### 1. 作为父项目继承

所有 Zeka.Stack 项目都应该继承 `arco-supreme` 或其子项目：

```xml
<parent>
    <groupId>dev.dong4j</groupId>
    <artifactId>arco-supreme</artifactId>
    <version>2.0.0-SNAPSHOT</version>
    <relativePath/>
</parent>
```

### 2. 自动获得的配置

继承后自动获得：

- ✅ 统一的 JDK 版本（Java 17）
- ✅ 统一的编码规范（UTF-8）
- ✅ 统一的依赖版本管理
- ✅ 统一的插件版本管理
- ✅ 统一的构建规范

### 3. 版本管理

框架使用 `flatten-maven-plugin` 进行版本管理：

```xml
<properties>
    <revision>2.0.0-SNAPSHOT</revision>
</properties>
```

子项目可以使用 `${revision}` 引用版本，实现版本统一管理。

## 📦 主要功能

### 1. 组件部署地址定义

统一管理各个组件的部署地址配置，支持：

- Maven Central 发布
- 私有 Maven 仓库发布
- 快照和发布版本的分离管理

### 2. 全局 Maven 插件管理

#### flatten-maven-plugin

处理版本字段的扁平化管理，支持 `${revision}` 变量：

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>flatten-maven-plugin</artifactId>
    <configuration>
        <flattenMode>resolveCiFriendliesOnly</flattenMode>
    </configuration>
</plugin>
```

#### maven-compiler-plugin

定义全局 JDK 版本要求：

```xml
<properties>
    <java.version>17</java.version>
</properties>
```

### 3. 依赖管理

管理核心依赖库的版本：

- **JUnit BOM**：测试框架版本管理
- **Lombok**：代码简化工具
- **Annotations**：JetBrains 注解库（编译时 null 检查）
- **Log4j2 BOM**：日志框架版本管理
- **Hutool BOM**：工具库版本管理
- **Jackson BOM**：JSON 处理版本管理

### 4. 公共依赖和插件版本管理

统一管理所有子项目共享的：

- **依赖库版本**：Spring Boot、Spring Cloud、MyBatis Plus 等
- **插件版本**：Checkstyle、PMD、JaCoCo、Javadoc 等

### 5. 项目标准化配置

定义统一的：

- **编码规范**：UTF-8
- **行结束符**：LF（Unix 风格）
- **构建参数**：默认跳过测试和 Javadoc（提高构建速度）

### 6. 构建流程控制

配置全局的：

- **distributionManagement**：统一管理部署仓库地址
- **可重现构建**：支持 `outputTimestamp` 配置
- **源码打包**：自动生成源码包（发布到 Maven Central 时）

## 🔧 配置说明

### 版本管理

```xml
<properties>
    <!-- 全局版本号 -->
    <global.version>2.0.0-SNAPSHOT</global.version>

    <!-- 可重现构建时间戳 -->
    <outputTimestamp.1.0.0>2025-05-12T00:00:00Z</outputTimestamp.1.0.0>
    <outputTimestamp.2.0.0-SNAPSHOT>2025-08-30T00:00:00Z</outputTimestamp.2.0.0-SNAPSHOT>
</properties>
```

### 构建优化

```xml
<properties>
    <!-- 默认跳过测试（提高构建速度） -->
    <maven.test.skip>true</maven.test.skip>

    <!-- 默认跳过 Javadoc（提高构建速度） -->
    <maven.javadoc.skip>true</maven.javadoc.skip>
</properties>
```

### 仓库配置

框架支持两种发布方式：

#### Maven Central 发布

```bash
mvn clean deploy -P central
```

需要配置：

- GPG 签名
- Sonatype 账号
- 源码和 Javadoc 包

#### 私有仓库发布

```bash
mvn clean deploy -P private
```

需要配置环境变量：

- `MVN_PRIVATE_USERNAME`
- `MVN_PRIVATE_PASSWORD`
- `MVN_PRIVATE_SNAPSHOTS_URL`
- `MVN_PRIVATE_RELEASE_URL`

## 📝 最佳实践

1. **版本管理**：
    - 使用 `${revision}` 变量统一管理版本
    - 每次发布新版本时更新 `outputTimestamp` 配置

2. **依赖升级**：
    - 在 `arco-supreme` 统一升级依赖版本
    - 所有子项目自动受益

3. **构建优化**：
    - 开发时跳过测试和 Javadoc，提高构建速度
    - CI/CD 时启用所有检查，确保质量

## 🔗 相关链接

- [[arco-meta/arco-builder/index|构建框架总览]]
- [[arco-meta/arco-maven-plugin/index|Maven 插件总览]]
- [[arco-meta/arco-processor/index|注解处理器总览]]

