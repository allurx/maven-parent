# maven-parent

公共 Maven 父 POM，坐标为 `io.allurx:maven-parent`，统一 Java 构建和发布配置，不管理业务依赖。

默认使用 Java 25、UTF-8 和固定的插件版本。`release` profile 提供 sources、Javadoc、GPG 签名和 Maven Central 发布配置；具体设置见 [pom.xml](pom.xml)。

## 使用

以下版本发布到 Maven Central 后，可通过 parent 继承：

```xml
<parent>
    <groupId>io.allurx</groupId>
    <artifactId>maven-parent</artifactId>
    <version>1.0.0</version>
    <relativePath/>
</parent>
```

使用方应声明自己的项目坐标及 `name`、`description`、`url`、`scm`、`issueManagement`，避免误用父 POM 的元数据。插件版本可通过同名属性覆盖。

## 本地验证

安装 JDK 25 和 Maven 后执行：

```shell
mvn -B -ntp clean verify
mvn -B -ntp -Prelease -Dgpg.skip=true clean verify
```

本工程是纯父 POM，上述命令只验证其构建生命周期；第二条跳过签名且不上传制品。使用方的 Java 编译、测试和打包行为需单独验证。

## CI/CD

[CI](.github/workflows/ci.yml) 执行构建验证；[Release](.github/workflows/release.yml) 在推送 `v*` 标签时发布。本工程只发布 POM 及其签名、校验文件，GitHub Release 说明自动生成。

维护约定见 [AGENTS.md](AGENTS.md)。采用 Apache License 2.0，见 [LICENSE.txt](LICENSE.txt)。
