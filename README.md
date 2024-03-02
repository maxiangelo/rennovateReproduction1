# Problem Statement:

The <java.version> property in the pom.xml file is not being suggested for a major update by Renovate. Despite following the recommended configuration mentioned [here](https://docs.renovatebot.com/java/#maven), it appears that the Maven manager lacks support for Java version updates. This limitation is confirmed by the community on [Stack Overflow](https://stackoverflow.com/questions/78067595/how-to-get-renovate-to-update-the-java-version-in-my-pom-xml/78076990).

Example Scenario
To illustrate the issue, consider the provided demo POM:

Demo POM:

```xml
<!-- snippet -->
<properties>
    <java.version>17</java.version>
</properties>
<!-- ... -->
```

# Expected Outcome  

In the example scenario, Renovate should recognize the <java.version> property with the value 17 and propose a major update to the latest Java version, such as 21. However, the current behavior suggests otherwise.
