# manage docker with maven
In this example I'm going to demonstrate how easily amd quick postgres database can be started in docker by using
fabric8 `docker-maven-plugin`

Table of Content

- [Install](#install)
- [`docker run -d` with `mvn docker:start`](#start)
- [`docker rm -f -v` with `mvn docker:stop`](#stop)
- [Help with `mvn docker:help`](#help)
- [Reference for reading](#read-more)

## install

assuming you have in your `pom.xml` file:

```xml
  <properties>
    <postgres.docker.name>healthcheck/postgres:alpine</postgres.docker.name>
    <postgres.docker.log.prefix>postgres</postgres.docker.log.prefix>
    <postgres.docker.ports.1>5432:5432</postgres.docker.ports.1>
    <!--<postgres.docker.ports.1>${itest.postgres.port}:5432</postgres.docker.ports.1>-->
    <postgres.docker.env.POSTGRES_DB>postgres</postgres.docker.env.POSTGRES_DB>
    <postgres.docker.envRun.POSTGRES_USER>postgres</postgres.docker.envRun.POSTGRES_USER>
    <postgres.docker.envRun.POSTGRES_PASSWORD>postgres</postgres.docker.envRun.POSTGRES_PASSWORD>
    <postgres.docker.wait.time>10000</postgres.docker.wait.time>
    <postgres.docker.wait.log>PostgreSQL init process complete</postgres.docker.wait.log>
  </properties>

  <build>
    <plugins>
      <plugin>
        <groupId>io.fabric8</groupId>
        <artifactId>docker-maven-plugin</artifactId>
        <version>0.28.0</version>
        <configuration>
          <allContainers>true</allContainers>
          <removeVolumes>true</removeVolumes>
          <watchInterval>500</watchInterval>
          <logDate>default</logDate>
          <verbose>true</verbose>
          <autoPull>always</autoPull>
          <images>
            <image>
              <external>
                <type>properties</type>
                <prefix>postgres.docker</prefix>
              </external>
            </image>
          </images>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

## start

_start postgres container with `docker run -d -p 5432:5432 ... healthcheck/postgres:apline` command_

```bash
mvn docker:start
```

## stop

_stop and remove container, like with `docker rm -f -v postgres-1` command_

```bash
mvn docker:start
```

## help
```bash
mvn docker:help
```

## cleanup if you missed to remove your other images

```bash
for i in $(docker images -q -f dangling=true); do docker rmi -f $i ; done
```

## read more

- [External Configuration](https://dmp.fabric8.io/#external-configuration)
- [docker:stop reference](https://dmp.fabric8.io/#docker:stop)
