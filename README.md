[<img src="./images/logo.png" width="400" height="200"/>](./images/logo.png)

# redis-jar
[![Build](https://github.com/eclipse-ecsp/redis-jar/actions/workflows/maven-publish.yml/badge.svg)](https://github.com/eclipse-ecsp/redis-jar/actions/workflows/maven-publish.yml)

The `redis-jar` project is a utility component for providing Redis JARs to run integration test cases.
The following JARs are provided with support for respective operating systems :
```
redis-server-4.0.8.app | MAC OS X (x86 and x64)
redis-server-4.0.8     | UNIX (x86 and x64)
redis-server.exe       | Windows (x86 and x64)
```

# Table of Contents
* [Getting Started](#getting-started)
* [Usage](#usage)
* [How to contribute](#how-to-contribute)
* [Built with Dependencies](#built-with-dependencies)
* [Code of Conduct](#code-of-conduct)
* [Authors](#authors)
* [Security Contact Information](#security-contact-information)
* [Support](#support)
* [Troubleshooting](#troubleshooting)
* [License](#license)
* [Announcements](#announcements)


## Getting Started

To build the project in the local working directory after the project has been cloned/forked, run:

```mvn clean install```

from the command line interface.

### Prerequisites

1. Maven
2. Java 17

### Installation

[How to set up maven](https://maven.apache.org/install.html)

[Install Java](https://stackoverflow.com/questions/52511778/how-to-install-openjdk-11-on-windows)

### Running the tests

```mvn test```

Or run a specific test

```mvn test -Dtest="TheFirstUnitTest"```

To run a method from within a test

```mvn test -Dtest="TheSecondUnitTest#whenTestCase2_thenPrintTest2_1"```

### Deployment

`redis-jar` project serves as an utility library for the services. It is not meant to be deployed as a service in any cloud environment.

## Usage
Add the following dependency in the target project
```
<dependency>
  <groupId>com.eclipse.ecsp.platform</groupId>
  <artifactId>redis-jar</artifactId>
  <version>1.X.X</version>
</dependency>

```

### Using a redis-jar

Redis-jar provides embedded-redis version 0.6, along with JARs for starting up Redis on a machine for running integration test cases. 
These JARs can be used to start embedded Redis server on a given port.

Example:

```java
import redis.embedded.RedisExecProvider;
import redis.embedded.RedisServer408;
import redis.embedded.util.Architecture;
import redis.embedded.util.OS;

public class EmbeddedRedisServer extends ExternalResource {
    @Override
    protected void before() throws Throwable {
        RedisExecProvider igniteProvider = RedisExecProvider.defaultProvider();
        igniteProvider.override(OS.MAC_OS_X, Architecture.x86,
                "redis-server-4.0.8.app");
        igniteProvider.override(OS.MAC_OS_X, Architecture.x86_64,
                "redis-server-4.0.8.app");
        igniteProvider.override(OS.UNIX, Architecture.x86,
                "redis-server-4.0.8");
        igniteProvider.override(OS.UNIX, Architecture.x86_64,
                "redis-server-4.0.8");
        igniteProvider.override(OS.WINDOWS, Architecture.x86, "redis-server.exe");
        igniteProvider.override(OS.WINDOWS, Architecture.x86_64, "redis-server.exe");
        port = new PortScanner().getAvailablePort(6379);
        redis = new RedisServer408(igniteProvider, port);
        redis.start();
        RedisConfig.overridingPort = port;
    }

    @Override
    protected void after() {
        redis.stop();
    }

    public int getPort() {
        return port;
    }
}
```

## Built With Dependencies

|                                                 Dependency                                                 | Purpose                                            |
|:----------------------------------------------------------------------------------------------------------:|:---------------------------------------------------|
|                                     [Maven](https://maven.apache.org/)                                     | Dependency Management                              |
|                                     [Junit](https://junit.org/junit5/)                                     | Testing framework                                  |


## How to contribute

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for details on our contribution guidelines, and the process for submitting pull requests to us.

## Code of Conduct

Please read [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md) for details on our code of conduct.

## Authors

* **Kaushal Arora**

See also the list of [contributors](https://github.com/eclipse-ecsp/redis-jar/contributors) who participated in this project.

## Security Contact Information

Please read [SECURITY.md](./SECURITY.md) to raise any security related issues.

## Support
Contact the project developers via the project's "dev" list - https://accounts.eclipse.org/mailing-list/ecsp-dev


## Troubleshooting

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for details on how to raise an issue and submit a pull request to us.

## License

This project is licensed under the XXX License - see the [LICENSE.md](./LICENSE.md) file for details.

## Announcements

All updates to this library are documented in our [Release Notes](./release_notes.txt) and [releases](https://github.com/eclipse-ecsp/redis-jar/releases).
For the versions available, see the [tags on this repository](https://github.com/eclipse-ecsp/redis-jar/tags).

[license]: ./LICENSE.md
[license img]: https://img.shields.io/badge/license-GNU%20LGPL%20v2.1-blue.svg

[artifactory]: https://github.com/orgs/eclipse-ecsp/packages/com/eclipse/ecsp/platform/redis-jar/
