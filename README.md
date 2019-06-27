Presented by
====

* [Romain Pelisse](https://github.com/rpelisse)

Requirement: this lab assume the attendee is familliar with Java/JEE application development.

Connecting to the system
===

Go request your lab environnment on [GUID Grabber](https://www.opentlc.com/gg/gg.cgi?profile=generic_pc)
Lab Activation Key: quarkus

Use the browser on your sytem to allocate one instance of this lab for you. Once this is done, you'll get a DNS hostname (or an IP) to get to the remote system provided for you. Simply use the SSH command to connect to the system with the user 'quarkus':

``` $ ssh quarkus@<DNS-name>```

If you are not familliar with SSH, please let me (the instructor) know!

Lab 0 - Setting Up Developement Env (15')
===

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

** Please do use the Quarkus version 0.15.0 and not a newer version, thanks! **

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
$ mvn clean compile quarkus:dev
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
===

This lab contains a series of requirements for you to implement by changing the code of the application we build during the first lab. Each requirement will allow you to explore the API and services provided by the Quarkus framework. Use this lab to explore some pretty cool feature of Quarkus, like the hot replacement of code!

* Implements a log file (using [java.nio.files](http://roufid.com/append-content-to-file-in-java/#files)) for all business request, using @PostConstruct to open the logfile and @PreDestroy to ensure the resource is properly closed
* Refactor your code to move the logic into a separate object, use [CDI injection](https://quarkus.io/guides/cdi-reference) facility to inject as a dependency (using @Inject)
* Do a quick benchmark to evaluate the performance of your implementation
* Replace your logfile using the [logging facility offered by Quarkus](https://quarkus.io/guides/logging-guide), rerun your benchmark to compare the performance of this new implementation
* Ensure this logger is invoked during startup and shutdown using the [lifecycle events](https://quarkus.io/guides/application-lifecycle-events-guide)

Lab 2 - ReST
===

* Modify the service to return response as JSON [JSON](https://quarkus.io/guides/rest-json-guide)
	* Create a class called Answer, with two fields 'name' and 'description'
	* Implements all required getter/setter
	* Add hashCode() and equals() methods
	* Add quarkus-resteasy-jsonb to the pom.xml
	* Modify the service to return JSON using the usual annotation
	* Modify the method to return a list of Answer

* Use the [Hibernate Validator](https://quarkus.io/guides/validation-guide) facility to validate input data.
    * Add new method in the service trigger by a PUT call and expectation a string as input parameter
    * turns the list of Answer into a static field, that is emptied by the call to the PUT method (before adding the new value
    * Adding a new value for Answer should use the following URL path template: /quarkus/<name>/<description>
    * Modify the previous GET accordingly
    * Test that everything is working as expected
    * Add the quarkus-hibernate-validator dependency to the pom.xml
    * Inject the validator service (using @Inject) into the service class and use the appropriate annotation to ensure that the name is at 5 characters long

At this point, we are going to change the nature of our service. Up until now, it was simply returning an precomputed answer. We are now going to turn it into a caching proxy to the online ReST service 'https://restcountries.eu/'. Everytime the information on a country will be requested, the service will return and cache it locally. Only info on requested country will be cached. For simplicity sake, the cache will be implemented using a static final instance of a simple Map as a property of the service class.

* Use the Rest Client feature of Quarkus to implement the caching proxy for the 'Rest Countries' API
    * Add the dependency to 'quarkus-smallrye-rest-client' to the pom.xml of the project
	* Replace the Answer class by the Country class (provided on [Quarkus guide pages](https://quarkus.io/guides/rest-client-guide#setting-up-the-model) )
	* Create an interface for the ReST Service (provided on [Quarkus guide pages](https://quarkus.io/guides/rest-client-guide#create-the-interface) )
    * Add the URL to the ReST Service in the application configuration (as describedon [Quarkus guide pages](https://quarkus.io/guides/rest-client-guide#create-the-configuration) )
    * Validate that the service is properly invoked and implement the cache using a simple Map, implements two endpoints, one returning the value of the country, the other listing the content of the cache
    * Advanced: Implements a DELETE method that allow to remove country from the cache and a PUT method that allow to add country (that are potentialy missing from the remote service)

Lab 3 - Hibernate (with Panache) - TODO
====


We will now modify our cache, implemented in the previous lab, to use HypersonicSQL database as a storage instead of the Map. To do so, we'll follow the Quarkus tutorial called ["Simplified Hibernate ORM with Panache"](https://quarkus.io/guides/hibernate-orm-panache-guide).

Part 1 - Configure the datasource

* Test connection to the database
    * First add the required dependency for the HSQL JDBC driver using the add extentions mechanism provided by Quarkus:
```
$ mvn quarkus:add-extension -Dextensions="jdbc-h2"
```
    * Configure the datasource into the application.properties, following the example provided in [the Quarkus guide](https://quarkus.io/guides/datasource-guide#configuring-the-datasource)
        * Note that we will use a in-memory database for simplicity sake, so here is the JDBC URL to use:
```
quarkus.datasource.url=jdbc:h2:mem:mymemdb
```
		* Don't forget that HSQL expect the username SA
		* Tweak the configuration to log the SQL, this help debugging later one
		* Also indicate that the base generation should "drop and create"
		* Don't do anything more than that for now
    * For the next step, you'll need an extensions called Agroal, use the list-extensions feature to find the exact name of the extensions and install it:
```
$ mvn quarkus:list-extensions
```
    * Once the extension installed, inject (@Inject) an AgroalDataSource in the service class
    * Implement a GET (/ds-check) to check that the dependency is properly injected (just return the result of toString() from the injected value)

		* At last, for testing purpose, add a import.sql file, next to the application.properties, which will load a few values into your database:
```
NSERT INTO country(id,name,alpha2Code,capital) VALUES (nextval('hibernate_sequence'), 'France', 'fr', 'Paris')
INSERT INTO country(id, name,alpha2Code,capital) VALUES (nextval('hibernate_sequence'), 'Deustschland','de', 'Berlin')

```
Part 2 - Using Panache to manipulate our entity

* Using Panache (see [Quarkus guide](https://quarkus.io/guides/hibernate-orm-panache-guide)
    * Search for the extensions associated with Panache and install it
    * Transform the Country class into a Panache Entity (see [the example on the Quarkus guide](https://quarkus.io/guides/hibernate-orm-panache-guide#first-an-example) )
		* Remove the field Currency and Capital of the Entity, as we won't need them
	* Add a import.sql file, next to the application.properties, which will load a few values into your database:
```
INSERT INTO country(id,name,alpha2Code) VALUES (nextval('hibernate_sequence'), 'France', 'fr')
INSERT INTO country(id, name,alpha2Code) VALUES (nextval('hibernate_sequence'), 'Deustschland','de')
```
	* Modify the /ds-check endpoint to return the content of the database using Panache built-in method: Country.listAll(Sort.by("name"));
        * Return only the list of alpha2code (fr, de).

Part 3 - Implementing the local database

* Implement a method getCountry to search for the country, based on its name, and returns the alpha2code
    * You'll need to implement a method findByname in Country (see [Quarkus guide](https://quarkus.io/guides/hibernate-orm-panache-guide#first-an-example)
* If the country is not in the database, the service will fallback to the ReST service
* Once the info returned by the remote service, the country needs to be added to the database
    * Remember that a write operation on a database has more constraint than a read operation ...

Lab 4 - Security - TODO
===

In this section of the lab, we are going to secure the two endpoints we have previoulsy implemented. The endpont /ds-check, currently the content of the database will be secured to be only accessible to user associated to the group 'admin'. The endpoint /country will be only available to user authentificated as 'user'. Obviously, users belonging to the 'admin' needs to also belong to the 'user' group. To implement such authentification and authorization, we will follow the [Quarkus guide on security](https://quarkus.io/guides/security-guide).

* Add the required dependency to the project's pom file: quarkus-elytron-security
* Configure Quarkus for security:
```
# Let's secure our stuff
quarkus.security.file.enabled=true
quarkus.security.file.users=users.properties
quarkus.security.file.roles=roles.properties
quarkus.security.file.auth-mechanism=BASIC
quarkus.security.file.realm-name=MyRealm
 ```
 * Add the user.properties file:
 ```
 yourusername=yourpassword
 admin=admin
 ```
 * Add the roles.properties file:
 ```
 yourusername=user
 admin=user, admin
 ```
* Finally, using the @RolesAllowed annotation configure the security as described above
* Check that everything has been configured properly using the following queries:
```
$ curl  http://localhost:8080/quarkus/dscheck
Not authorized
$ curl  http://<yourusername>:<yourpassword>@localhost:8080/quarkus/dscheck
Access forbidden: role not allowed
$ curl  http://<yourusername>:<yourpassword>@localhost:8080/quarkus/country/France
fr
$ curl  http://localhost:9091/quarkus/dscheck
Not authorized
$ curl http://admin:theotherguy@localhost:9091/quarkus/dscheck
[de,fr]
```

Lab 5 - Native image with GraalVM
===

OK, so now we have a very lightweight ReST Service that starts in barely a few milliseonds. It's already incredibly fast compare to regular Java application server, but let's go even further, by generating a ... native image!

Download GraalVM:
```
wget https://github.com/oracle/graal/releases/download/vm-1.0.0-rc16/graalvm-ce-1.0.0-rc16-linux-amd64.tar.gz
```

**Please, download and use GraalVM v1.0.0-rc16 and not a more recent version!**

Decompress the archive and set the GRAALVM_HOME to match resulting directory:

```
$ export GRAALVM_HOME=/home/quarkus/graalvm
```

Build the application using the following commnad line:

```
$  mvn package -Pnative
```

Once the build is finished, you should have an executable file in the target folder of your project:

```
$ file target/*-runner
target/quarkus-intro-lab-1.0-SNAPSHOT-runner: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=40559638e695105e1014a11b14ff0dfce5b2fb6b, with debug_info, not stripped
```

You can now start your application and see IF it works ;)

```
$ ./target/*-runner
```

If the application does not work, try to see what you can remove or change your application to make it work. If you don't want to get stuck here, simply revert app to the content of the first lab and move on from there.

Advanced Lab: modify the build to allow the standalone executable to [support HTTPS connection](https://quarkus.io/guides/native-and-ssl-guide).

Once you have a native image, you can even go further than that, and even generate a docker image to run your service. Do so, following the [instruction](https://quarkus.io/guides/building-native-image-guide#producing-a-native-executable) available on the Quarkus website. (but you won't be able during the lab, as Docker is not installed on the system).
