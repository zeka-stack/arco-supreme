## 1. 项目定位

- 作为所有项目的最顶层父项目
- 作为基础框架的核心管理模块

## 2. 主要功能

### 2.1 组件部署地址定义

- 统一管理各个组件的部署地址配置

### 2.2 全局 Maven 插件管理

- 配置 flatten-maven-plugin 插件：处理版本(version)字段的扁平化管理
- 配置 maven-compiler-plugin 插件：定义 JDK 版本要求（Java 1.8）

### 2.3 依赖管理

- 管理核心依赖库：
    - `junit-bom`：JUnit 测试框架的 BOM（Bill of Materials）管理
    - `lombok`：代码简化工具库
    - `annotations`：JetBrains 提供的注解库，用于编译时检查 null 值

### 2.4 公共依赖和插件版本管理

- 统一管理所有子项目共享的依赖库版本号
- 统一管理 Maven 插件的版本号，包括但不限于：
    - maven-checkstyle-plugin
    - maven-pmd-plugin
    - maven-javadoc-plugin
    - jacoco-maven-plugin 等质量检测插件

### 2.5 项目标准化配置

- 定义了统一的编码规范（UTF-8）、行结束符（LF）、缩进等格式要求
- 设置默认跳过测试和Javadoc生成，提高构建效率
- 统一配置了源码仓库信息和开发者信息

### 2.6 构建流程控制

- 配置了 distributionManagement，统一管理快照和发布版本的部署仓库地址
- 设置了全局的构建参数和策略

这个模块通过集中管理这些公共配置和依赖，确保了整个组织内所有项目的构建配置一致性，简化了子项目的配置复杂度，并保证了技术栈的统一性。
