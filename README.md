## About

Eclipse 4 doesn't work well under FVWM because of a problem described in [#442210](https://bugs.eclipse.org/bugs/show_bug.cgi?id=442210).

This repository contains the [workaround](https://bugs.eclipse.org/bugs/show_bug.cgi?id=442210#c4) from the [above bug report](https://bugs.eclipse.org/bugs/show_bug.cgi?id=442210).

## Eclipse 4.4.2

* [Eclipse 4.4.2](https://www.eclipse.org/downloads/packages/release/luna/sr2)

Tested with:

* [Eclipse IDE for Java Developers](https://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/lunasr2)
* [Eclipse IDE for Java EE Developers](https://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr2)

### Installing workaround

Note: The environment variable ECLIPSE_HOME should be set to the installation directory of your local Eclipse installation.

	$ id -un
	root

	$ export BASE_URL=https://github.com/vakuum/fvwm-eclipse/raw/master

	$ export ECLIPSE_HOME=/opt/eclipse-java-4.4.2
	# $ export ECLIPSE_HOME=/opt/eclipse-jee-4.4.2

	$ export JAR=org.eclipse.swt.gtk.linux.x86_64_3.103.2.v20150203-1351.jar

	$ mv "${ECLIPSE_HOME}/plugins/${JAR}" "${ECLIPSE_HOME}/plugins/${JAR}-disabled"

	$ wget "${BASE_URL}/eclipse-4.4.2/org.eclipse.swt.gtk.linux.x86_64-3.103.2-SNAPSHOT.jar" -O "${ECLIPSE_HOME}/plugins/${JAR}"

	$ chmod 644 "${ECLIPSE_HOME}/plugins/${JAR}"

### Building workaround

Note: The following steps are only necessary if you want to build the workaround on your own.

	$ mkdir ~/fvwm-eclipse

	$ cd ~/fvwm-eclipse

	$ git clone https://github.com/vakuum/fvwm-eclipse.git

eclipse.platform.swt:

	$ cd ~/fvwm-eclipse

	$ git clone https://git.eclipse.org/gitroot/platform/eclipse.platform.swt.git

	$ cd eclipse.platform.swt

	$ git checkout R4_4_maintenance

	$ patch -p1 < ../fvwm-eclipse/eclipse-4.4.2/eclipse.platform.swt-R4_4_maintenance.patch

	$ cd bundles/org.eclipse.swt

	$ mvn clean install

	$ ls -1 target/*.jar
	target/org.eclipse.swt-3.103.2-SNAPSHOT.jar

eclipse.platform.swt.binaries:

	$ cd ~/fvwm-eclipse

	$ git clone https://git.eclipse.org/gitroot/platform/eclipse.platform.swt.binaries.git

	$ cd eclipse.platform.swt.binaries

	$ git checkout R4_4_maintenance

	$ patch -p1 < ../fvwm-eclipse/eclipse-4.4.2/eclipse.platform.swt.binaries-R4_4_maintenance.patch

	$ cd bundles/org.eclipse.swt.gtk.linux.x86_64

	$ mvn clean package

	$ ls -1 target/*.jar
	target/org.eclipse.swt.gtk.linux.x86_64-3.103.2-SNAPSHOT-sources.jar
	target/org.eclipse.swt.gtk.linux.x86_64-3.103.2-SNAPSHOT.jar

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

