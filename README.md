# rennovateReproduction1
Reproduction repo for #27645


I was wondering why my <java.version> property in my pom.xml would not get a suggested major update by Renovate.
This was a bit confusing because I saw [this documentation page about renovate skipping non LTS Java versions with the recommended config](https://docs.renovatebot.com/java/#maven). 
So I decided to ask a [question on Stack Overflow](https://stackoverflow.com/questions/78067595/how-to-get-renovate-to-update-the-java-version-in-my-pom-xml/78076990) and got an excellent answer [Gaël J](https://stackoverflow.com/users/5389127/ga%c3%abl-j) who told me that the [Maven manager](https://docs.renovatebot.com/modules/manager/maven/) currently does not support Java version updates because it does not support the [Java data source](https://docs.renovatebot.com/modules/datasource/java-version/) yet.

This seems like a big missed opportunity for me. 
Let's do an example with this demo POM:

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>demo</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.3</version>
        <relativePath/>
    </parent>
    <groupId>demo</groupId>
    <artifactId>demo-service</artifactId>
    <version>0.70.2-SNAPSHOT</version>
    <name>demo-service</name>
    <description>Demo project</description>
    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-mongodb</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>com.google.cloud.tools</groupId>
                <artifactId>jib-maven-plugin</artifactId>
                <version>${jib-maven-plugin.version}</version>
                <configuration>
                    <from>
                        <image>eclipse-temurin:${java.version}-alpine</image>  <----
                    </from>
                    ....
            </plugin>
        </plugins>
    </build>
</project>
```

and this renovate config:

```
 {
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "packageRules": [
    {
      "matchPackageNames": [
        "maven-wrapper"
      ],
      "enabled": false
    }
  ],
  "configMigration": true,
  "extends": [
    "config:recommended",
    "group:allNonMajor",
    ":approveMajorUpdates"
  ]
}
```
(I used the same as in the Stack Overflow post)

I would expect renovate to see the <java.version>17</java.version> property and search for the newest Java version and propose a major update to Java version 21.

I'm considering creating a pull request myself, but I'm not really proficient in typescript :)
So if someone wants to guide me or help me out with integrating this, please raise your hand!
