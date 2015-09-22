# Database Migrations



## Why database migrations

![Simple View](database-migrations/SimpleView.png)


![Enviroments](database-migrations/Environments.png)


We have been pretty good at solving them on the code side.

- Version control is now universal with better tools everyday.
- We have reproducible builds and continuous integration.
- We have well defined release and deployment processes.

![Soft Green](database-migrations/SoftGreen.png)


But what about the database?

![DB Red](database-migrations/DbRed.png)


- What state is the database in on this machine?
- Has this script already been applied or not?
- Has the quick fix in production been applied in test afterwards?
- How do you set up a new database instance?


> Database migrations are a great way to regain control of this mess.


- Recreate a database from scratch
- Make it clear at all times what state a database is in
- Migrate in a deterministic way from your current version of the database to a newer one



## How Flyway works

<!-- .slide: data-background="white" -->

![Flyway](database-migrations/flyway-logo-tm.png)


### Empty Database

![EmptyDb](database-migrations/EmptyDb.png)


![EmptySchemaVersion](database-migrations/EmptySchemaVersion.png)


![Migration-1-2.png](database-migrations/Migration-1-2.png)


![SchemaVersion-1-2](database-migrations/SchemaVersion-1-2.png)


### Migrating to Newer Versions

![PendingMigration](database-migrations/PendingMigration.png)


![Migration21](database-migrations/Migration21.png)


![SchemaVersion21](database-migrations/SchemaVersion21.png)



## Use Flyway


### Gradle 2.1 and newer

- build.gradle

```groovy
plugins {
    id "org.flywaydb.flyway" version "3.2"
}
```


- gradle.properties

```
flyway.url=jdbc:hsqldb:file:/db/flyway_sample;shutdown=true
flyway.user=myUser
flyway.password=mySecretPwd
```


![SqlMigrationBaseDir](database-migrations/SqlMigrationBaseDir.png)


`$ gradle flywayMigrate -i`


### Spring Boot integration

Execute Flyway database migrations on startup

- add the org.flywaydb:flyway-core to your classpath



## 参考资料

- [Flyway](http://flywaydb.org/)
- [Execute Flyway database migrations on startup](http://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-execute-flyway-database-migrations-on-startup)