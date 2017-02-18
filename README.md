Eclipse JDT Language Server
===========================

The Eclipse JDT Language Server is a Java language specific implementation of the [Language Server Protocol](https://github.com/Microsoft/language-server-protocol)
and can be used with any editor that supports the protocol, to offer good support for the Java Language. The server is based on:

* [Eclipse LSP4J](https://github.com/eclipse/lsp4j), the Java binding for the Language Server Protocol,
* [Eclipse JDT](http://www.eclipse.org/jdt/), which provides Java support (code completion, references, diagnostics...), 
* [M2Eclipse](http://www.eclipse.org/m2e/) which provides Maven support,
* [Buildship](https://github.com/eclipse/buildship) which provides Gradle support.

Features
--------------
* As you type reporting of parsing and compilation errors
* Code completion
* Javadoc hovers
* Code outline
* Code navigation
* Code lens (references)
* Highlights
* Code formatting
* Maven pom.xml project support
* Limited Gradle support (Android projects are not supported)


First Time Setup
--------------
0. Fork and clone the repository
1. Install Eclipse [Neon Java EE](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/neonr)
that will have most needed already installed. Alternately,
you can get the [Eclipse IDE for Java developers](http://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/neonr)
and just instal Eclipse PDE from marketplace.

2. Once installed use `File > Open Projects from File System...` and
point it `java-language-server` and Eclipse should automatically
detect the projects and import it properly.

3. If, after import, you see an error on `pom.xml` about Tycho, you can use Quick Fix
(Ctrl+1) to install the Tycho maven integration.


Building from command line
----------------------------

1. Install [Apache Maven](https://maven.apache.org/)

2. This command will build the server into `/org.eclipse.jdt.ls.product/target/repository` folder:
```bash    
    $ mvn clean verify
````

Running from the command line
------------------------------
1. Choose a connection type from "Managing connection types" section below, and then set those environment variables in your terminal prior to continuing

2. Make sure to build the server using the steps above in the "Building from command line" section

3. `cd` into the build directory of the project: `/org.eclipse.jdt.ls.product/target/repository`

4. Prior to starting the server, make sure that your socket (TCP or sock file) server is running for both the IN and OUT sockets. You will get an error if the JDT server cannot connect on your ports/files specified in the environment variables

5. To start the server in the active terminal, run:
```bash
java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=1044 -Declipse.application=org.eclipse.jdt.ls.core.id1 -Dosgi.bundles.defaultStartLevel=4 -Declipse.product=org.eclipse.jdt.ls.core.product -Dlog.protocol=true -Dlog.level=ALL -noverify -Xmx1G -jar ./plugins/org.eclipse.equinox.launcher_1.4.0.v20161219-1356.jar -configuration ./config_linux -data /path/to/data
```

6. Choosing a value for `-configuration`: this is the path to your platform's configuration directory. For linux, use `./config_linux`. For windows, use `./config_win`. For mac/OS X, use `./config_mac`.

7. Choosing a value for `-data`: the value for your data directory, should be the directory where your active workspace is, and you wish for the java langserver to add in its default files. Should also be the absolute path to this directory, ie., /home/username/workspace

8. Notes about debugging: the `-agentlib:` is for connecting a java debugger agent to the process, and if you wish to debug the server from the start of execution, set `suspend=y` so that the JVM will wait for your debugger prior to starting the server

9. Notes on jar versions: the full name of the build jar file above, `org.eclipse.equinox.launcher_1.4.0.v20161219-1356.jar`, may change incrementally as the project version changes. If java complains about jar not found, then look for the latest version of the `org.eclipse.equinox.launcher_*` jar in the `/org.eclipse.jdt.ls.product/target/repository/plugins` directory and replace it in the command after the `-jar`

Managing connection types
-------------------------
Java Language server supports sockets, named pipes, and standard streams of the server process
to communicate with the client. Client can communicate its preferred connection methods 
by setting up environment variables. 

* To use **name pipes**  set the following environment variables before starting
the server.
```
STDIN_PIPE_NAME --> where client reads from
STDOUT_PIPE_NAME --> where client writes to
```
* To use **plain sockets** set the following environment variables before starting the
server.
```
STDIN_PORT --> client reads
STDOUT_PORT --> client writes to
```
optionally you can set host values for socket connections
```
STDIN_HOST
STDOUT_HOST
```
* To use standard streams(stdin, stdout) of the server process do not set any 
of the above environment variables and the server will fall back to standard streams. 

For socket and named pipes the client is expected to create the connections
and wait for server the connect.


Feedback
---------

* File a bug in [GitHub Issues](https://github.com/eclipse/eclipse.jdt.ls/issues).
* [Tweet](https://twitter.com/GorkemErcan) [us](https://twitter.com/fbricon) with other feedback.

Clients
-------
This repository contains only the server implementation. Here are some known clients consuming this server:

* [vscode-java](https://github.com/redhat-developer/vscode-java) : an extension for Visual Studio Code

Continuous Integration Builds
-----------------------------
Our [CI server](https://ci.eclipse.org/ls/) publishes builds to http://download.eclipse.org/jdtls/snapshots/


License
-------
EPL 1.0, See [LICENSE](LICENSE) file.
