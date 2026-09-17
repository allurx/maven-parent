# maven-parent

公共 Maven 父 POM `io.allurx:maven-parent`，统一 Java 构建和发布配置。

- 默认编译目标为 Java 25，使用 UTF-8 编码和固定插件版本。
- 显式启用 `release` profile 后，为 Java 模块附加 sources、Javadoc，并启用 GPG 签名和 Maven Central 发布配置。
- 本工程采用 `pom` packaging，只发布 POM 及其签名、校验文件。完整配置见 [pom.xml](pom.xml)。

## 使用

在项目的 `pom.xml` 中声明父 POM，将 `LATEST_VERSION` 替换为链接中的已发布版本：

<pre><code>&lt;parent&gt;
    &lt;groupId&gt;io.allurx&lt;/groupId&gt;
    &lt;artifactId&gt;maven-parent&lt;/artifactId&gt;
    &lt;version&gt;<a href="https://central.sonatype.com/artifact/io.allurx/maven-parent">LATEST_VERSION</a>&lt;/version&gt;
    &lt;relativePath/&gt;
&lt;/parent&gt;</code></pre>

依赖管理、模块和 annotation processor 由使用方声明。设置自己的项目坐标及 `name`、`description`、`url`、`scm`、`issueManagement`，避免继承父 POM 的项目元数据。

通过 `maven.compiler.release` 覆盖编译目标，通过对应的 `*.version` 属性覆盖插件版本。

## 本地验证

安装 JDK 25 和 Maven 后执行：

```shell
mvn -B -ntp clean verify
mvn -B -ntp -Prelease -Dgpg.skip=true clean verify
```

两条命令均不上传制品；第二条启用 `release` profile 并跳过签名。验证仅覆盖父 POM 生命周期，Java 模块的编译、测试和打包需在使用方验证。

CI 与发布使用 [allurx-build](https://github.com/allurx/allurx-build)。

## 许可证

[Apache License 2.0](LICENSE.txt)。
