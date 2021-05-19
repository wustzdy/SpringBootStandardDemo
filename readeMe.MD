https://www.jianshu.com/p/4a0b67c06787

Jib 是 Google 开发的可以直接构建 Java 应用的 Docker 和 OCI 镜像的类库，以 Maven 和 Gradle 插件形式提供。

通过 Jib，Java 开发者可以使用他们熟悉的 Java 工具来构建容器。Jib 是一个快速而简单的容器镜像构建工具，它负责处理将应用程序打包到容器镜像中所需的所有步骤。它不需要你编写 Dockerfile 或安装 Docker，而且可以直接集成到 Maven 和 Gradle中 —— 只需要将插件添加到构建中，就可以立即将 Java 应用程序容器化。

mvn 添加构建依赖

<build>
        <plugins>
            <!--mvn build 插件-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <!--jib 构建插件-->
            <plugin>
                <groupId>com.google.cloud.tools</groupId>
                <artifactId>jib-maven-plugin</artifactId>
                <version>0.9.8</version>
                <!--加了这段代码，构建命令 使用mvn package 否则 使用mvn compile jib:dockerBuild -->
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>dockerBuild</goal>
                        </goals>
                    </execution>
                </executions>
                <!--加了这段代码，构建命令 使用mvn package 否则 使用mvn compile jib:dockerBuild -->
                <configuration>
                    <!--
                    from 如果使用aliyun的registry.cn-hangzhou.aliyuncs.com进行加速，需要在mvn配置文件setting.xml添加下面配置
                    <servers>
                        <server>
                          <id>registry.cn-hangzhou.aliyuncs.com</id>
                          <username>你的阿里云账号</username>
                          <password>你的阿里云密码</password>
                        </server>
                    </servers>
                    -->
                    <from>
                        <image>openjdk:8-jre-alpine</image>
                    </from>
                    <!--构建的images 名字 和 tag(自定义)-->
                    <to>
                        <image>athzcc/${project.artifactId}:1.0.0</image>
                    </to>
                </configuration>
            </plugin>
        </plugins>
    </build>

使用此插件要先在本机安装docker,构建进入构建项目的根目录，
执行构建命令mvn compile jib:dockerBuild,
构建完成在查看docker images

运行镜像 docker run imageName:tag