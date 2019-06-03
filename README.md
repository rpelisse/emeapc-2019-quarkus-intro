Use the browser on your sytem to allocate one instance of this lab for you. Once this is done, you'll get a DNS hostname (or an IP) to get to the remote system provided for you. Simply use the SSH command to connect to the system with the user 'quarkus':

``` $ ssh quarkus@<DNS-name>```

The password is: re3dh4t1!

If you are not familliar with SSH, please let me (the instructor) know!

Lab 0 - Setting Up Developement Env (15')

First of all, double check that the system you've been allocated for this lab has all the required software available:

* Check that Java 9 or later is installed:
```
$ java -version
openjdk version "11.0.2" 2019-01-15 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.2+7-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.2+7-LTS, mixed mode, sharing)
```

Also check that Maven 3.5 (or beyond) is available:

```
$ mvn -version
Apache Maven 3.5.4 (Red Hat 3.5.4-5)
```

Now, use Maven to set up a Quarkus project. Don't worry, an archetype has been provided, most of it is automated. Just run the following Maven command:

```
mvn io.quarkus:quarkus-maven-plugin:0.15.0:create \
    -DprojectGroupId=org.redhat.emeapc \
    -DprojectArtifactId=quarkus-intro-lab \
    -DclassName="org.redhat.emeapc.GreetingResource" \
    -Dpath="/quarkus"
```

Now, change your default directory to the directory created by the previous command (./quarkus-intro-lab):

```
$ cd ./quarkus-intro-lab
```

If you look at the content of the directory, you'll see that all the file you need to start a project has been generated:

```
$ tree .
.
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
    ├── main
    │   ├── docker
    │   │   ├── Dockerfile.jvm
    │   │   └── Dockerfile.native
    │   ├── java
    │   │   └── org
    │   │       └── redhat
    │   │           └── emeapc
    │   │               └── GreetingResource.java
    │   └── resources
    │       ├── application.properties
    │       └── META-INF
    │           └── resources
    │               └── index.html
    └── test
        └── java
            └── org
                └── redhat
                    └── emeapc
                        ├── GreetingResourceTest.java
...
15 directories, 10 files
```

For now, you just need to focus on the pom.xml and the GreetingResource.java. First open the pom.xml, review its content and ensure you understand everything in it. If not, now is the time to ask question to the instructor!

Once you finished review the pom.xml, take a quick look at the GreetingResource.java and let the instructor to know if you have any question regarding its content. Once this is done, it's time to build!

```
mvn clean compile
```

This compilation should happen without any problem, so you can now fire the app!

```
$ mvn clean compile quarks:dev
[quarkus@control getting-started]$ mvn clean compile quarkus:dev
[INFO] Scanning for projects...
[INFO]
[INFO] ----------------------< org.acme:getting-started >----------------------
[INFO] Building getting-started 1.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- maven-clean-plugin:2.5:clean (default-clean) @ getting-started ---
[INFO]
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ getting-started ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 2 resources
[INFO]
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ getting-started ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /home/quarkus/getting-started/target/classes
[INFO]
[INFO] --- quarkus-maven-plugin:0.15.0:dev (default-cli) @ getting-started ---
Listening for transport dt_socket at address: 5005
2019-06-03 08:04:08,543 INFO  [io.qua.dep.QuarkusAugmentor] (main) Beginning quarkus augmentation
2019-06-03 08:04:09,977 INFO  [io.qua.dep.QuarkusAugmentor] (main) Quarkus augmentation completed in 1434ms
2019-06-03 08:04:10,825 INFO  [io.quarkus] (main) Quarkus 0.15.0 started in 2.683s. Listening on: http://[::]:8080
2019-06-03 08:04:10,830 INFO  [io.quarkus] (main) Installed features: [cdi, resteasy]
```

Now, in other terminal, connect again to the system and test the service using curl:

```
$ curl -v -X GET http://localhost:8080/quarkus
Note: Unnecessary use of -X or --request, GET is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 8080 (#0)
> GET /quarkus HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.61.1
> Accept: */*
>
< HTTP/1.1 404 Not Found
< Connection: keep-alive
< Content-Type: text/html;charset=UTF-8
< Content-Length: 84
< Date: Mon, 03 Jun 2019 12:07:28 GMT
<
* Connection #0 to host localhost left intact
```

Before moving to the next lab, do ask any question about this part to the instructor. Also note that from now on, you are free to do or skip any part of the lab. Focus on what you wish to explore. The following labs are, as much as much as possible, independant from each other. Also that it's unlikely you will have time to do all of them during the time allowed by this lab.

Lab 1 - Quarkus Core Features (CDI, Logging,...)

This lab contains a series of requirements for you to implement by changing the code of the application we build during the first lab. Each requirement will allow you to explore the API and services provided by the Quarkus framework.

* Implements a log file for all business request, using @PostConstruct to open the logfile and @PreDestroy to ensure the resource is properly closed
* Refactor your code to move the logic into a separate object, use [CDI injection](https://quarkus.io/guides/cdi-reference) facility to inject as a dependency (using @Inject)
* Do a quick benchmark to evaluate the performance of your implementation
* Replace your logfile using the [logging facility offered by Quarkus](https://quarkus.io/guides/logging-guide), rerun your benchmark to compare the performance of this new implementation
* Ensure this logger is invoked during startup and shutdown using the [lifecycle events](https://quarkus.io/guides/application-lifecycle-events-guide)

Lab 3 - ReST

* Modify the service to support [JSON](https://quarkus.io/guides/rest-json-guide)
* Use the [Hibernate Validator](https://quarkus.io/guides/validation-guide) facility to validate input data.
* Use the [ReST Client](https://quarkus.io/guides/rest-client-guide) to implement a proxy to TODO

Lab 4 - Hibernate (with Panache) - TODO

* Set up Hibernate
* Simplify using Panache: https://quarkus.io/guides/hibernate-orm-panache-guide

Lab 5 - Security - TODO

* https://quarkus.io/guides/security-guide

Lab 6 - Native image with GraalVM

OK, so now we have a very lightweight ReST Service that starts in barely a few milliseonds. It's already incredibly fast compare to regular Java application server, but let's go even further, by generating a ... native image!

Download GraalVM:
```
wget https://github.com/oracle/graal/releases/download/vm-19.0.0/graalvm-ce-linux-amd64-19.0.0.tar.gz
```

Decompress the archive and set the GRAALVM_HOME to match resulting directory:

```
$ export GRAALVM_HOME=/home/quarkus/graalvm
```

Build the application using the following commnad line:

```
$  mvn package -Pnative
```

TODO: Add the info on starting the app

Once this is working, add a new endpoint to your service called '/toClass'. This end point will take a Java classname as a paramter, such as 'java.lang.String', instantiate the class using reflection , cll


You can even go further than that, and even generate a docker image to run your service. Do so, following the [instruction](https://quarkus.io/guides/building-native-image-guide#producing-a-native-executable) available on the Quarkus website. (but you won't be able during the lab, as Docker is not installed on the system).

Advanced Lab: modify the build to allow the standalone executable to [support HTTPS connection](https://quarkus.io/guides/native-and-ssl-guide).
