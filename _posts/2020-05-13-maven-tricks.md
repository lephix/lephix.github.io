
## Parameters for maven command
+ Specific profile: `-P ${profile}`
+ Force update repository: `-U`

## Add git info into a static file
In SprintBoot, you can directly visit it by url http://host/git.json. Build time and commit id are included.

```
<plugin>
    <groupId>pl.project13.maven</groupId>
    <artifactId>git-commit-id-plugin</artifactId>
    <version>2.2.4</version>
    <executions>
        <execution>
            <id>get-the-git-infos</id>
            <goals>
                <goal>revision</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <dotGitDirectory>${project.basedir}/.git</dotGitDirectory>
        <prefix>git</prefix>
        <verbose>false</verbose>
        <generateGitPropertiesFile>true</generateGitPropertiesFile>
        <generateGitPropertiesFilename>${project.build.outputDirectory}/static/git.json</generateGitPropertiesFilename>
        <format>json</format>
        <gitDescribe>
            <skip>false</skip>
            <always>false</always>
            <dirty>-dirty</dirty>
        </gitDescribe>
    </configuration>
</plugin>
```

## Custom SpringBoot artifact classifier
This will change the executable artifact name with a given classifier.

```
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <executions>
        <execution>
            <id>repackage</id>
            <configuration>
                <classifier>comic</classifier>
            </configuration>
        </execution>
    </executions>
</plugin>
```

## Delete a file from the output and artifact package.
Use `<resource>` tag.
```
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <excludes>
                <exclude>comic-*.yml</exclude>
            </excludes>
        </resource>
    </resources>
</build>
```

Use ant plugin
```
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-antrun-plugin</artifactId>
    <version>1.8</version>
    <executions>
        <execution>
            <phase>compile</phase>
            <goals>
                <goal>run</goal>
            </goals>
            <configuration>
                <tasks>
                    <delete file="${project.build.outputDirectory}/application-prod.yml"/>
                </tasks>
            </configuration>
        </execution>
    </executions>
</plugin>
```

## Fix SpringBoot start timeout problem
```
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <wait>1000</wait>
        <maxAttempts>180</maxAttempts>
    </configuration>
</plugin>
```
