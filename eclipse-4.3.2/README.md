## Eclipse 4.3.2

* [Eclipse 4.3.2](https://www.eclipse.org/downloads/packages/release/kepler/sr2)

Tested with:

* [Eclipse IDE for Java Developers](https://www.eclipse.org/downloads/packages/eclipse-ide-java-developers/keplersr2)
* [Eclipse IDE for Java EE Developers](https://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/keplersr2)

### Installing workaround

Note: The environment variable ECLIPSE_HOME should be set to the installation directory of your local Eclipse installation.

	$ id -un
	root

	$ export BASE_URL=https://github.com/vakuum/fvwm-eclipse/raw/master

	$ export ECLIPSE_HOME=/opt/eclipse-java-4.3.2
	# $ export ECLIPSE_HOME=/opt/eclipse-jee-4.3.2

	$ export JAR=org.eclipse.swt.gtk.linux.x86_64_3.102.1.v20140206-1358.jar

	$ mv "${ECLIPSE_HOME}/plugins/${JAR}" "${ECLIPSE_HOME}/plugins/${JAR}-disabled"

	$ wget "${BASE_URL}/eclipse-4.3.2/org.eclipse.swt.gtk.linux.x86_64-3.102.1-SNAPSHOT.jar" -O "${ECLIPSE_HOME}/plugins/${JAR}"

	$ chmod 644 "${ECLIPSE_HOME}/plugins/${JAR}"

### Building workaround

Note: The following steps are only necessary if you want to build the workaround on your own. Please see the requirements below for more details.

	$ mkdir ~/fvwm-eclipse

	$ cd ~/fvwm-eclipse

	$ git clone https://github.com/vakuum/fvwm-eclipse.git

eclipse.platform.swt:

	$ cd ~/fvwm-eclipse

	$ git clone https://git.eclipse.org/gitroot/platform/eclipse.platform.swt.git

	$ cd eclipse.platform.swt

	$ git checkout R4_3_maintenance

	$ patch -p1 < ../fvwm-eclipse/eclipse-4.3.2/eclipse.platform.swt-R4_3_maintenance.patch

	$ cd bundles/org.eclipse.swt

	$ mvn clean install

	$ ls -1 target/*.jar
	target/org.eclipse.swt-3.102.1-SNAPSHOT.jar

eclipse.platform.swt.binaries:

	$ cd ~/fvwm-eclipse

	$ git clone https://git.eclipse.org/gitroot/platform/eclipse.platform.swt.binaries.git

	$ cd eclipse.platform.swt.binaries

	$ git checkout R4_3_maintenance

	$ patch -p1 < ../fvwm-eclipse/eclipse-4.3.2/eclipse.platform.swt.binaries-R4_3_maintenance.patch

	$ cd bundles/org.eclipse.swt.gtk.linux.x86_64

	$ mvn clean package

	$ ls -1 target/*.jar
	target/org.eclipse.swt.gtk.linux.x86_64-3.102.1-SNAPSHOT-sources.jar
	target/org.eclipse.swt.gtk.linux.x86_64-3.102.1-SNAPSHOT.jar

	$ cd target

	$ unzip org.eclipse.swt.gtk.linux.x86_64-3.102.1-SNAPSHOT.jar META-INF/MANIFEST.MF

	$ grep '^Bundle-Version' META-INF/MANIFEST.MF
	Bundle-Version: 3.102.1.v20151112-1349

	$ sed -i'' 's/^Bundle-Version: 3.102.1.v20151112-1349/Bundle-Version: 3.102.1.v20140206-1358/' META-INF/MANIFEST.MF

	$ grep '^Bundle-Version' META-INF/MANIFEST.MF
	Bundle-Version: 3.102.1.v20140206-1358

	$ zip -u org.eclipse.swt.gtk.linux.x86_64-3.102.1-SNAPSHOT.jar META-INF/MANIFEST.MF

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
