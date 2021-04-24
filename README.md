# Nexus私服

## 1. nexus是什么？
Nexus 是一个强大的仓库管理器，极大地简化了内部仓库的维护和外部仓库的访问。
## 2. 为什么使用nexus？
```
    2.1 节省外网带宽;
    2.2 加速项目构建;
    2.3 部署第三方构件;
    2.4 提高稳定性，增强控制;
    2.5 降低中央仓库的负荷等。
```
## 3. 如何使用nexus?
温馨提示: 使用该私服构建服务前请系好安全带,因为构建速度太快乐。
### 4.1 私服地址
地址：http://nexus-domain

### 4.2 maven中使用settings.xml实例
```javascript
<?xml version="1.0" encoding="UTF-8"?>

<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!--
    这里改为自己的本地仓库
  -->

  <localRepository>/Users/xh/Documents/server/apache-maven-3.6.3/repository</localRepository>

  <pluginGroups></pluginGroups>
  <proxies></proxies>

  <!-- servers
   | This is a list of authentication profiles, keyed by the server-id used within the system.
   | Authentication profiles can be used whenever maven must make a connection to a remote server.
   |-->
  <servers>
    <!--这里配置我们刚才创建的user用户所对应的releases-->
    <server>
        <id>nexus-releases</id>
        <username>username</username>
        <password>password</password>
    </server>
    <!--这里配置我们刚才创建的user用户所对应的snapshots-->
    <server>
        <id>nexus-snapshots</id>
        <username>username</username>
        <password>password</password>
    </server>
  </servers>

  <mirrors>
    <mirror>
        <id>nexus-releases</id>
        <mirrorOf>*</mirrorOf>
        <url>http://nexus-domain/repository/maven-public/</url>
    </mirror>
    <mirror>
        <id>nexus-snapshots</id>
        <mirrorOf>*</mirrorOf>
        <url>http://nexus-domain/repository/maven-snapshots/</url>
    </mirror>
  </mirrors>

  <profiles>
    <profile>  
      <id>nexus</id>
      <repositories>
        <repository>
          <id>nexus-releases</id>
          <url>http://nexus-domain/repository/maven-public/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled></snapshots>
        </repository>
        <repository>
          <id>nexus-snapshots</id>
          <url>http://nexus-domain/repository/maven-snapshots/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots>
        </repository>
      </repositories>
      <pluginRepositories>  
        <pluginRepository>
          <id>nexus-releases</id>  
          <url>http://nexus-domain/repository/maven-public/</url>  
          <releases><enabled>true</enabled></releases>  
          <snapshots><enabled>true</enabled></snapshots> 
        </pluginRepository>
        <pluginRepository>
          <id>nexus-snapshots</id>  
          <url>http://nexus-domain/repository/maven-snapshots/</url>  
          <releases><enabled>true</enabled></releases>  
          <snapshots><enabled>true</enabled><updatePolicy>always</updatePolicy></snapshots> 
        </pluginRepository>
      </pluginRepositories>
      <activation>
        <activeByDefault>true</activeByDefault>      
        <jdk>1.8</jdk>
      </activation>
      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
      </properties>
    </profile>
  </profiles>

  <activeProfiles>
    <activeProfile>nexus</activeProfile>
  </activeProfiles>

</settings>
```

### 4.3 gradle中使用实例
```javascript
repositories {
	maven {
		url 'http://nexus-domain/repository/maven-public/'
		credentials {
			username = 'username'
			password = 'password'
		}
	}
}
```

### 4.4 上传Jar
上传jar有多种方式，可以通过命令行、nexus图形界面、IDE工具上传。这几种方式网上的资料都有很多，这里用Maven项目作为演示介绍一种我最常用也是最简单的一种，Gradle和其他操作形式请参考网络资料。

Maven、Gradle项目实例：
```java
http://gitlab.com/service-public/nexus-maven.git
http://gitlab.com/service-public/nexus-gradle.git
```

#### 4.5 Maven-POM配置
1. POM配置
```javascript
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.5</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.xh.nexus.maven</groupId>
    <artifactId>nexus-maven</artifactId>
    <version>0.0.1-RELEASES</version>
    <name>nexus-maven</name>
    <description>Demo project for Spring Boot</description>
    <properties>
        <java.version>1.8</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

    <repositories>
        <repository>
            <id>nexus-releases</id>
            <url>http://nexus-domain/repository/maven-releases/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>nexus-snapshots</id>
            <url>http://nexus-domain/repository/maven-snapshots/</url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
            </snapshots>
        </repository>
    </repositories>

    <distributionManagement>
        <snapshotRepository>
            <id>nexus-snapshots</id>
            <url>http://nexus-domain/repository/maven-snapshots/</url>
            <uniqueVersion>true</uniqueVersion>
        </snapshotRepository>
        <repository>
            <id>nexus-releases</id>
            <url>http://nexus-domain/repository/maven-releases/</url>
        </repository>
    </distributionManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

2. 有idea的在里面点击部署也可以，执行命令部署也可以。命令如下：
```javascript
mvn clean package deploy
```
