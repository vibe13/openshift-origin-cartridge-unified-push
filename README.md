# Red Hat JBoss Unified Push Server Cartridge for OpenShift

This cartridge provides the `JBoss Unified Push Server` for easy deployment to OpenShift, running on top of JBoss EAP. 

The `JBoss Unified Push Server` is a server that allows sending push notifications to different (mobile) platforms. The initial version of the server supports [Apple’s APNs](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9) and [Google Cloud Messaging](http://developer.android.com/google/gcm/index.html).

### Creation of OpenShift account
You first need to create an OpenShift account before being able to create applications with the `JBoss Unified Push Server` cartridge. For information on how to create and setup your OpenShift account, please refer to [Getting Started with OpenShift Online](https://developers.openshift.com/en/getting-started-overview.html).

### Installation
You have different options to create your application.

#### Approach 1: use the OpenShift create application page
Go to the [OpenShift create application page](https://openshift.redhat.com/app/console/application_types) and enter the cartridge URI of https://raw.githubusercontent.com/vibe13/openshift-origin-cartridge-aerogear-push/master/metadata/manifest.yml in the entry field (at the bottom left of the form), then configure the application in the following pages (you need to specify at least the `Public URL`. If you have a subscription for Openshift that gives you access to other gear sizes, you can specify them in the `Gears` field). 

#### Approach 2: use the rhc command line
If you want to use the rhc command line type:
```shell
rhc app create --no-git <APP_NAME> https://raw.githubusercontent.com/vibe13/openshift-origin-cartridge-aerogear-push/master/metadata/manifest.yml
```

If you have a subscription for Openshift that gives you access to other gear sizes, you can specify them running i.e. :
```shell
rhc app create -g medium --no-git <APP_NAME> https://raw.githubusercontent.com/vibe13/openshift-origin-cartridge-aerogear-push/master/metadata/manifest.yml
```

When the installation completes, you will be presented with a list of generated users and passwords similar to the screencap below.  Make sure you save them!

![cartridge_creation](https://raw.githubusercontent.com/vibe13/openshift-origin-cartridge-aerogear-push/master/cartridge-creation.png)

The `JBoss Unified Push Server` cartridge defaults to using MySQL.

### Getting started with the JBoss Unified Push Server

#### Administration Console

Once the server is running access it via `https://{APP_NAME}-{NAMESPACE}.rhcloud.com/ag-push`. For more information on using the console, see the [JBoss Unified Push documentation](https://access.redhat.com/documentation/en-US/Red_Hat_JBoss_Unified_Push/).

#### Login

Temporarily, there is an "admin:123" user.  On _first_ login,  you will need to change the password.

### Access into the application shell

To access into the application shell:
```shell
ssh {APPLICATION_ID}@{APP_NAME}-{NAMESPACE}.rhcloud.com
```

### Manage JBoss EAP configuration

The main configuration file for JBoss EAP is `standalone.xml`  
This file is available in your cartridge repository at location `./jboss-unified-push/standalone/configuration/standalone.xml`.  
This is useful for changing container configurations such as root logger level and so on.  

### Template Repository Layout
```
./jboss-unified-push/usr/template/.openshift/    Location for OpenShift specific files
action_hooks/                                    See the Action Hooks documentation [1]
markers/                                         See the Markers section [2]
```
\[1\] [Action Hooks documentation](http://openshift.github.io/documentation/oo_user_guide.html#action-hooks)
\[2\] [Markers](#markers)


### Environment Variables

The `jboss-unified-push` cartridge provides several environment variables to reference for ease
of use:
```
OPENSHIFT_JBOSS_UNIFIED_PUSH_IP                         The IP address used to bind JBossEAP
OPENSHIFT_JBOSS_UNIFIED_PUSH_HTTP_PORT                  The JBossEAP listening port
OPENSHIFT_JBOSS_UNIFIED_PUSH_CLUSTER_PORT               
OPENSHIFT_JBOSS_UNIFIED_PUSH_MESSAGING_PORT             
OPENSHIFT_JBOSS_UNIFIED_PUSH_MESSAGING_THROUGHPUT_PORT  
OPENSHIFT_JBOSS_UNIFIED_PUSH_REMOTING_PORT              
JAVA_OPTS_EXT                                         Appended to JAVA_OPTS prior to invoking the Java VM
```
For more information about environment variables, consult the
[OpenShift Application Author Guide](http://openshift.github.io/documentation/oo_user_guide.html).

### Markers

You can add marker files to `./jboss-unified-push/usr/template/.openshift/markers/` to enable debugging application code and enable running JBoss EAP with Java7 if present.

#### Debugging application code
`enable_jpda` Will enable the JPDA socket based transport on the java virtual machine running the JBoss EAP 6. This enables you to remotely debug code running inside the JBoss EAP 6.

```
cd ./jboss-unified-push/usr/template/.openshift/markers/
touch enable_jpda
```
#### Running JBoss EAP with Java7
`java7` Will run JBossEAP with Java7 if present. If no marker is present then the baseline Java version will be used (currently Java6)

```
cd ./jboss-unified-push/usr/template/.openshift/markers/
touch java7
```
