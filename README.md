# commons-pool-heritrix-1-14

Version 1.3 of commons-pool that incorporates the class changes found in heritrix 1.14.1.

## Synopsis

The heritrix-1.14.1.jar includes overrides of certain Apache commons-pool code in its jar. Specifically it makes certain methods of
`org.apache.commons.pool.impl.GenericObjectPool` protected instead of private and then subclasses `GenericObjectPool` with
`FairGenericObjectPool` (tested with `FairGenericObjectPoolTest`). These changes and subclass are included in the
`heritrix-1.14.1.jar`. Potential problems occur when the class loader loads the unmodified `GenericObjectPool` class from
`commons-pool-1.3.jar` instead of the `heritrix-1.14.1.jar` version. This project incorporates all the changes together and
produces a `commons-pool.jar` that eliminates issues with class loader variable ordering.

The whole purpose of this project to enable the Web Curator Tool (https://github.com/DIA-NZ/webcurator) to work with the Heritrix 1
agent.

## Warning

These changes only work if the dependencies have not been superseded by a more recent version. For Web Curator Tool it means that
the Heritrix 1 agent must be packaged in a war with this version of `commons-pool`.

## Motivation

Provide a non-conflicting set of dependencies so the heritrix 1.14.1 crawler can work without classpath conflicts.

## Versioning

See the `pom.xml` file for the current jar version that will be generated. Previous versions have been tagged in the
git repository. To list the tags, use:
```
git tag -l
```

### Versioned artifacts

The original source files can be found in the subfolder `original_source`.

Pre-built artifacts can be found in the subfolder `release_archive`.

### Running a maven 2.x/3.x/gradle build with a commons-pool 1.3.x dependency

When running a maven or gradle build that requires this version of commons-pool, the following commands can be used to
install the dependency in a local maven 2 repository (run these commands from the root folder of this project):
```
mkdir -pv ./target/
git clone https://github.com/WebCuratorTool/commons-pool-heritrix-1-14.git ./target/commons-pool-heritrix-1-14
mvn install:install-file -Dfile=./target/commons-pool-heritrix-1-14/commons-pool-<version>.jar
```
TODO setup the pom file
Note that the version needs to be chosen for this script to work.

## Installation

The artifacts are built using maven and will deploy to a local maven version 2 repository.

### Java version for build

We recommended that Java 5 (version 1.5) be used for the build, as this is the version that was used to create the original
jar.

Java 1.5 can be downloaded and installed from Oracle (https://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase5-419410.html).
This JVM can be run simply by setting `JAVA_HOME` to the unpacked location and including `JAVA_HOME/bin` in the shell
path.

### Maven version for build

We recommend that Maven 2.x (latest stable release 2.2.1) be used for the build (this is the version that was used to
create the jar artifacts). Maven 2.2.1 can be downloaded from
https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/2.2.1/. This version of maven can be run by setting
`MAVEN_HOME` to the path of the unpacked distribution and executing (on linux) `$MAVEN_HOME/bin/mvn clean install`.

### Missing unit tests

No unit tests were included in the source jar provided through Maven Central.

### Complete build

Simply put, execute the following on linux:
```
$MAVEN_HOME/bin/mvn [clean] install
```
or for Windows:
```
%MAVEN_HOME%/bin/mvn.bat [clean] install
```

## Contributors

The original commits are attributed to the people or organisations named in the `pom.xml` file. Issues related to the changes made
here are tracked through the github repository issue tracker. Note that there is no history prior to version 1.3 (which is when
the source was imported into this repository). See the git commits after the tagged 1.3 version for those contributors.

## License

&copy; 2019 National Library of New Zealand for all changes past version 1.3. All rights reserved.
GNU Lesser General Public License (LGPL) version 2.1 *and* Apache License Version 2.0.

For code prior up to and including version 1.3, Apache License Version 2.0. The actual code changes from the heritrix-1.14.1
codebase are LGPL and are attributed to the heritrix team.

### License incompatability

Note that the LGPL and Apache License 2.0 are incompatible. There's not really any way to resolve this. The Heritrix 1.14.1
developers made changes incompatible with the Apache license (since they released their code under the LGPL).
