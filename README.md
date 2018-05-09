# usb4java for Eclipse

This project allows to generate a P2 repository that contains usb4java libraries
that can be consumed and used by Eclipse plug-ins.

## Description

This project has been created after countless hours of trying to get usb4java
to work with an Eclipse plug-in.

The easiest way would have been to just dump all usb4java dependencies in a `lib` directory inside the Eclipse plug-in. To avoid such a mess, multiple attempts were
made to have those dependencies as external Eclipse compatible bundles.

Unfortunately class loading works differently with normal JARs and with Eclipse
plug-ins/bundles. This leads to problems, for example when trying to specify the
package name of the classes that implement the `javax.usb.api` API in `javax.usb.properties`. This does not seem to work across bundles without
hacking the class loading mechanisms. Instead a shortcut was taken by combining `usb4java-javax` and `javax.usb.api` in one bundle and also placing the `javax.usb.properties` file in there.

Eventually this should be done properly, but for now it seems to be the best
compromise between messiness, hacking effort and lack of time.

## Contents

* org.usb4java
  * Git submodule
    * [fredg02/usb4java](https://github.com/fredg02/usb4java) forked from [usb4java/usb4java](https://github.com/usb4java/usb4java)
  * Branch [usb4java-1.2.0_bundle](https://github.com/fredg02/usb4java/tree/usb4java-1.2.0_bundle)
  * Changes:
      * Converted usb4java 1.2.0 release to an Eclipse compatible bundle
* usb4java-javax
  * Git submodule
    * [fredg02/usb4java-javax](https://github.com/fredg02/usb4java-javax) forked from [usb4java/usb4java-javax](https://github.com/usb4java/usb4java-javax)
    * Branch [usb4java-javax-1.2.0-bundle_plus_javax.usb.api](https://github.com/fredg02/usb4java-javax/tree/usb4java-javax-1.2.0-bundle_plus_javax.usb.api)
    * Changes:
      * Converted usb4java-javax 1.2.0 release to an Eclipse compatible bundle
      * Added `javax.usb.api` classes to this bundle
      * Added java.usb.properties file
* usb4java-parent
  * Maven/Tycho style parent project
* usb4java-repository
  * Maven/Tycho style P2 repository project
* usb4java-target
  * Maven/Tycho style target platform

## Build

#### Instructions

1. `git submodule update --init`
2. `mvn clean verify -f usb4java-target/pom.xml`
3. `ant -f usb4java-target/fixTargetDefinition.xml`
4. `mvn clean verify -f usb4java-parent/pom.xml`

This will create a P2 repository in `usb4java-repository/target/repository` or
as a Zip file `usb4java-repository/target/usb4java-repository-1.0.0.zip`.

#####  The P2 repository should contain the following files:
* org.usb4java.bundle_1.2.0.jar
* org.usb4java.bundle.source_1.2.0.jar
* org.usb4java.libusb4java.linux-arm_1.2.0.jar
* org.usb4java.libusb4java.linux-x86_1.2.0.jar
* org.usb4java.libusb4java.linux-x86_64_1.2.0.jar
* org.usb4java.libusb4java.osx-x86_1.2.0.jar
* org.usb4java.libusb4java.osx-x86_64_1.2.0.jar
* org.usb4java.libusb4java.windows-x86_1.2.0.jar
* org.usb4java.libusb4java.windows-x86_64_1.2.0.jar
* usb4java-javax_javax.usb.api_1.2.0.jar
* usb4java-javax_javax.usb.api.source_1.2.0.jar
