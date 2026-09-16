# 项目协作指引

本文件适用于整个仓库。

## 修改边界

- 本工程是公共 Maven parent，产物类型为 `pom`，负责通用构建和发布配置。
- 保持依赖管理、模块列表、annotation processor 和专属测试参数由使用方声明，不加入业务依赖或全局测试跳过规则。
- 构建使用已安装的 JDK 和 Maven，不引入 Maven Wrapper。

## POM 维护

- XML 使用四个空格缩进；`properties` 按属性名的字母顺序排列，内部不留空行。
- 插件版本统一声明为 properties，由 `pluginManagement` 引用；仅为管理版本时不要额外绑定执行目标。
- 插件使用固定稳定版本，不使用 SNAPSHOT、预发布版、`LATEST` 或开放版本范围。审计版本时同时检查 effective POM 中继承和生命周期默认带入的插件。
- 签名和发布配置保留在显式启用的 `release` profile 中。
- 本工程只发布 POM 及其签名、校验文件；不要为纯 POM 生成占位 JAR，也不要设置会被 Java 模块继承的全局 sources/Javadoc 跳过选项。

## 验证

POM 配置变更后执行：

```shell
mvn -B -ntp clean verify
mvn -B -ntp -Prelease -Dgpg.skip=true clean verify
```

- 上述命令只验证父 POM 的生命周期；第二条跳过签名且不上传制品。不要将 `deploy` 用作本地验证。
- 修改可继承的配置时，检查使用方的 effective POM；涉及 Java 编译、JPMS、测试或 sources/Javadoc 的行为时，需使用相应 Java 模块验证。
- 仅文档变更时检查内容、链接和空白格式，无需运行 Maven。

## 工作流与文档

- CI/CD 入口位于 `.github/workflows/`，复用共享工作流，避免在本仓库复制其实现。
- 发布由版本标签触发，GitHub Release 说明自动生成，无需维护逐版本说明文件。
- 短期分支进入 `dev` 使用 squash；`dev` 到 `main` 的发布 PR 保留 merge commit，发布核实后同步 `main` 回 `dev`。
- README 只保留本工程的用途、使用方式和必要入口，不记录其它具体工程的适配方案；本文件只维护稳定的项目约定。
