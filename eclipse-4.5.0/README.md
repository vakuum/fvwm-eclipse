## Eclipse 4.5.0

* [Eclipse 4.5.0](https://www.eclipse.org/downloads/packages/release/mars/r)

Tested with:

* [Eclipse IDE for Java Developers](https://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/marsr)
* [Eclipse IDE for Java EE Developers](https://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/marsr)

### Installing workaround

Note: The environment variable ECLIPSE_HOME should be set to the installation directory of your local Eclipse installation.

	$ id -un
	root

	$ export BASE_URL=https://github.com/vakuum/fvwm-eclipse/raw/master

	$ export ECLIPSE_HOME=/opt/eclipse-java-4.5.0
	# $ export ECLIPSE_HOME=/opt/eclipse-jee-4.5.0

	$ export JAR=org.eclipse.swt.gtk.linux.x86_64_3.104.0.v20150528-0211.jar

	$ mv "${ECLIPSE_HOME}/plugins/${JAR}" "${ECLIPSE_HOME}/plugins/${JAR}-disabled"

	$ wget "${BASE_URL}/eclipse-4.5.0/org.eclipse.swt.gtk.linux.x86_64-3.104.0-SNAPSHOT.jar" -O "${ECLIPSE_HOME}/plugins/${JAR}"

	$ chmod 644 "${ECLIPSE_HOME}/plugins/${JAR}"

### Building workaround

Note: The following steps are only necessary if you want to build the workaround on your own. Please see the requirements below for more details.

	$ mkdir ~/fvwm-eclipse

	$ cd ~/fvwm-eclipse

	$ git clone https://github.com/vakuum/fvwm-eclipse.git

eclipse.platform.releng.aggregator:

	$ cd ~/fvwm-eclipse

	$ git clone https://git.eclipse.org/gitroot/platform/eclipse.platform.releng.aggregator.git

	$ cd eclipse.platform.releng.aggregator

	$ git checkout R4_5

	# Please see https://bugs.eclipse.org/bugs/show_bug.cgi?id=441990 for details.
	$ git cherry-pick 4bf6cec23dc70ae4cc8284de33e8422b2f978c43

	# Please see https://bugs.eclipse.org/bugs/show_bug.cgi?id=474640 for details.
	$ git cherry-pick 3e2456df8606bdf6f550ee558303094687584807

	# Please see https://bugs.eclipse.org/bugs/show_bug.cgi?id=481904 for details.
	$ git cherry-pick cead9b4ed75bb79cd5dd9a226ba99ec38e430918

	# Please see https://bugs.eclipse.org/bugs/show_bug.cgi?id=482488 for details.
	$ git cherry-pick 62d4e0082f88da34986671f5cb9cf7cd2b8ea254

	$ patch -p1 < ../fvwm-eclipse/eclipse-4.5.0/eclipse-platform-parent-R4_5.patch

	$ cd eclipse.platform.releng.prereqs.sdk

	$ mvn clean install

	$ cd ~/fvwm-eclipse

	$ ln -s eclipse.platform.releng.aggregator/eclipse-platform-parent .

eclipse.platform.swt:

	$ cd ~/fvwm-eclipse

	$ git clone https://git.eclipse.org/gitroot/platform/eclipse.platform.swt.git

	$ cd eclipse.platform.swt

	$ git checkout R4_5

	$ patch -p1 < ../fvwm-eclipse/eclipse-4.5.0/eclipse.platform.swt-R4_5.patch

	$ cd bundles/org.eclipse.swt

	$ mvn clean install

	$ ls -1 target/*.jar
	target/org.eclipse.swt-3.104.0-SNAPSHOT.jar

eclipse.platform.swt.binaries:

	$ cd ~/fvwm-eclipse

	$ git clone https://git.eclipse.org/gitroot/platform/eclipse.platform.swt.binaries.git

	$ cd eclipse.platform.swt.binaries

	$ git checkout R4_5

	$ patch -p1 < ../fvwm-eclipse/eclipse-4.5.0/eclipse.platform.swt.binaries-R4_5.patch

	$ cd bundles/org.eclipse.swt.gtk.linux.x86_64

	$ mvn clean package

	$ ls -1 target/*.jar
	target/org.eclipse.swt.gtk.linux.x86_64-3.104.0-SNAPSHOT-sources.jar
	target/org.eclipse.swt.gtk.linux.x86_64-3.104.0-SNAPSHOT.jar

## Requirements

### Java 1.8.0

	$ export JAVA_HOME=/opt/jdk-1.8.0
	$ export PATH=$JAVA_HOME/bin:$PATH

	$ java -version
	java version "1.8.0_45"
	...

* [Java](http://www.oracle.com/technetwork/java/)
* [Java Download](http://www.oracle.com/technetwork/java/javase/downloads/)

### Maven 3.1

	$ export M2_HOME=/opt/maven-3.1
	$ export PATH=$M2_HOME/bin:$PATH

	$ mvn -version
	Apache Maven 3.1.1 (0728685237757ffbf44136acec0402957f723d9a; 2013-09-17 17:22:22+0200)
	...

* [Maven](https://maven.apache.org/)
* [Maven Download](https://maven.apache.org/download.cgi)

## License

Copyright (c) 2015-2016 Clemens Fuchslocher, released under the [EPL version 1.0](../LICENSE).
