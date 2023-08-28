# MySQL Module

See [Database containers](./index.md) for documentation and usage that is common to all relational database container types.

## Overriding MySQL my.cnf settings

For MySQL databases, it is possible to override configuration settings using resources on the classpath. Assuming `somepath/mysql_conf_override`
is a directory on the classpath containing .cnf files, the following URL can be used:

  `jdbc:tc:mysql:8.0.36://hostname/databasename?TC_MY_CNF=somepath/mysql_conf_override`

Any .cnf files in this classpath directory will be mapped into the database container's /etc/mysql/conf.d directory,
and will be able to override server settings when the container starts.

## MySQL `root` user password

If no custom password is specified, the container will use the default user password `test` for the `root` user as well.
When you specify a custom password for the database user,
this will also act as the password of the MySQL `root` user automatically. 

## Adding this module to your project dependencies

Add the following dependency to your `pom.xml`/`build.gradle` file:

=== "Gradle"
    ```groovy
    testImplementation "org.testcontainers:mysql:{{latest_version}}"
    ```
=== "Maven"
    ```xml
    <dependency>
        <groupId>org.testcontainers</groupId>
        <artifactId>mysql</artifactId>
        <version>{{latest_version}}</version>
        <scope>test</scope>
    </dependency>
    ```

!!! hint
    Adding this Testcontainers library JAR will not automatically add a database driver JAR to your project. You should ensure that your project also has a suitable database driver as a dependency.

## Start Using MySQL Container in Integration Test

```groovy
import org.testcontainers.containers.MySQLContainer
import org.testcontainers.spock.Testcontainers

import spock.lang.Shared
import spock.lang.Specification

@Testcontainers
class MyITSpec extends Specification {

    @Shared
    final MySQLContainer MYSQL = new MySQLContainer("mysql:5.7.43").withDatabaseName("testDbName")

    def setupSpec() {
        System.setProperty(
                "DB_URL",
                String.format("jdbc:mysql://localhost:%s/elide?serverTimezone=UTC", MYSQL.firstMappedPort)
        )
    }
}
```

!!! hint
    If _withDatabaseName_ is invoked, DB container will create a database as well in the container. This is very useful for IT tests that requires a pre-configured DB to be ready 
