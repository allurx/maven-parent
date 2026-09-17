# maven-parent 工作指引

## POM 维护

- 依赖管理、模块、annotation processor 和专属测试参数由使用方声明；不加入业务依赖或全局测试跳过规则。
- 使用已安装的 JDK 和 Maven，不引入 Maven Wrapper。
- XML 使用四空格缩进；`properties` 按属性名字母顺序排列，内部不留空行。
- 插件版本在 properties 中定义，由 `pluginManagement` 引用；仅管理版本时不额外绑定目标。使用固定稳定版本，不使用 SNAPSHOT、预发布版、`LATEST` 或开放版本范围。
- 签名和发布配置仅放在显式启用的 `release` profile 中。
- 仅发布 POM 及其签名、校验文件，不生成占位 JAR，也不设置会被 Java 模块继承的全局 sources/Javadoc 跳过选项。

## 验证

- POM 变更后执行 [README 的本地验证](README.md#本地验证)。
- 审计插件版本时，通过 effective POM 检查继承和生命周期默认带入的插件。
- 修改可继承配置时，检查使用方的 effective POM；涉及 Java 编译、JPMS、测试或 sources/Javadoc 行为时，使用相应 Java 模块验证。
