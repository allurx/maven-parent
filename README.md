# maven-parent

公共 Maven 父 POM，坐标为 `io.allurx:maven-parent`，统一 Java 构建和发布配置。依赖管理、模块和 annotation processor 由使用方声明。

- 默认编译目标为 Java 25，使用 UTF-8 编码和固定的插件版本。
- 显式启用 `release` profile 后，为 Java 模块附加 sources、Javadoc，并启用 GPG 签名和 Maven Central 发布配置。
- 本工程采用 `pom` packaging，只发布 POM 及其签名、校验文件。完整配置见 [pom.xml](pom.xml)。

## 使用

在项目的 `pom.xml` 中声明父 POM。点击下方版本链接查看 Maven Central，使用时将 `LATEST_VERSION` 替换为具体的已发布版本号：

<pre><code>&lt;parent&gt;
    &lt;groupId&gt;io.allurx&lt;/groupId&gt;
    &lt;artifactId&gt;maven-parent&lt;/artifactId&gt;
    &lt;version&gt;<a href="https://central.sonatype.com/artifact/io.allurx/maven-parent">LATEST_VERSION</a>&lt;/version&gt;
    &lt;relativePath/&gt;
&lt;/parent&gt;</code></pre>

使用方应声明自己的项目坐标及 `name`、`description`、`url`、`scm`、`issueManagement`，避免继承父 POM 的项目元数据。编译目标可通过 `maven.compiler.release` 覆盖，插件版本可通过对应的 `*.version` 属性覆盖。

## 本地验证

安装 JDK 25 和 Maven 后执行：

```shell
mvn -B -ntp clean verify
mvn -B -ntp -Prelease -Dgpg.skip=true clean verify
```

上述命令仅验证父 POM 的构建生命周期，不上传制品；第二条启用 `release` profile 并跳过签名。使用方的 Java 编译、测试和打包行为需在对应项目中验证。

## CI/CD

[CI](.github/workflows/ci.yml) 执行构建验证；[Release](.github/workflows/release.yml) 在推送 `v*` 标签时发布，并自动生成 GitHub Release 说明。两者均复用 [allurx-build](https://github.com/allurx/allurx-build) 的共享工作流。

维护约定见 [AGENTS.md](AGENTS.md)。采用 Apache License 2.0，见 [LICENSE.txt](LICENSE.txt)。
