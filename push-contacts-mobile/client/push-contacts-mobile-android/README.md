push-contacts-mobile-android: Contacts CRUD Mobile Android with Push Notification
===========================
Author: Daniel Passos (dpassos)  
Level: Intermediate  
Technologies: Java, Android  
Summary: The `push-contacts-mobile-android` quickstart demonstrates how to develop a contacts CRUD mobile Android application with push notification integration.  
Prerequisites: push-contacts-mobile-picketlink-secured  
Target Product: JBoss Unified Push  
Versions: 1.0  
Source: <https://github.com/jboss-developer/jboss-mobile-quickstarts/>  

## What is it?

The `push-contacts-mobile-android` quickstart demonstrates how to develop more advanced Android push applications, centered around a CRUD contacts application.

This client-side Android project must be used in conjunction with the `push-contacts-mobile/server/push-contacts-mobile-picketlink-secured` application, which provide the accompanying server-side functionality. 

When the client application is deployed to an Android device, the push functionality enables the device to register with the running JBoss Unified Push Server OpenShift instance and receive push notifications. The server-side application provides login authentication for the client application and sends push notification requests to the JBoss Unified Push Server in response to new contacts being created. Push notifications received by the Android device contain details of newly added contacts.

## How do I run it?

### 0. System requirements

* [Java 7](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [Maven 3.1.1](http://maven.apache.org)
* Latest [Android SDK](https://developer.android.com/sdk/index.html)
* Latest Platform Version and [Platform version 19](http://developer.android.com/tools/revisions/platforms.html)
* Latest [Android Support Library](http://developer.android.com/tools/support-library/index.html) and [Google Play Services](http://developer.android.com/google/play-services/index.html)

### 1. Prepare Maven Libraries

This quickstart is designed to be built with Maven. It requires the JBoss Unified Push Maven repository and Google libraries.

You must have the JBoss Unified Push Maven repository available and Maven configured to use it. For more information, see the [Configure Maven](https://github.com/jboss-developer/jboss-developer-shared-resources/blob/master/guides/CONFIGURE_MAVEN.md#configure-maven-to-build-and-deploy-the-quickstarts) or the README distributed with the [JBoss Unified Push Maven repository](https://www.jboss.org/download-manager/file/jboss-unified-push-1.0.0.Beta1-maven-repository.zip).

Google does not ship all the required libraries to Maven Central so you must deploy them locally with the helper utility [maven-android-sdk-deployer](https://github.com/mosabua/maven-android-sdk-deployer) as detailed here.

1. Checkout maven-android-sdk-deployer

        git clone git://github.com/mosabua/maven-android-sdk-deployer.git

2. Set up the environment variable `ANDROID_HOME` to contain the Android SDK path
3. Install Maven version of Android platform 19

        cd /path/to/maven-android-sdk-deployer/platforms/android-19
        mvn install -N

4. Install Maven version of google-play-services

        cd /path/to/maven-android-sdk-deployer/extras/google-play-services
        mvn install -N

5. Install Maven version of compatibility-v4

        cd /path/to/maven-android-sdk-deployer/extras/compatibility-v4
        mvn install -N

6. Install Maven version of compatibility-v7-appcompat

        cd /path/to/maven-android-sdk-deployer/extras/compatibility-v7-appcompat
        mvn install -N


### 2. Register Application with Push Services

First, you must register the application with Google Cloud Messaging for Android and enable access to the Google Cloud Messaging for Android APIs and Google APIs. This ensures access to the APIs by the JBoss Unified Push Server when it routes push notification requests from the application to the GCM. Registering an application with GCM requires that you have a Google account.

1. Log into the [Google Cloud Console](https://console.developers.google.com)
2. In the `Projects` view, click `Create Project`.
3. In the `PROJECT NAME` field type a project name, select the check box to agree to the Terms of Service and click `Create`.
4. Reload the page and in the `Projects` view click the project you just created. Make note of the `Project Number`.
5. Click `APIs and auth`>`APIs` and change the status of `Google Cloud Messaging for Android` to `ON`.
6. Click `APIs and auth`>`Credentials` and under `Public API access` click `Create new Key`.
7. Click `Server Key` and click `Create`. Make note of the `API KEY`.

Second, you must register the application and an Android variant of the application with the JBoss Unified Push Server. This requires a running JBoss Unified Push Server OpenShift instance and uses the unique metadata assigned to the application by GCM. For more information about deploying, configuring and using the JBoss Unified Push Server, see the [JBoss Unified Push documentation](http://docs.jboss.org/unifiedpush/unifiedpush.pdf) and [JBoss xPaaS Services for OpenShift](https://developers.openshift.com/en/xpaas.html#_mobile_services).

1. Log into the JBoss Unified Push Server OpenShift instance console.
2. In the `Applications` view, click `Create Application`.
3. In the `Name` and `Description` fields, type values for the application and click `Create`.
4. When created, under the application click `No variants`.
5. Click `Add Variant`.
6. In the `Name` and `Description` fields, type values for the Android application variant.
7. Click `Android` and in the `Google Cloud Messaging Key` and `Project Number` fields type the values assigned to the project by GCM.
8. Click `Add`.
9. When created, expand the variant name and make note of the `Server URL`, `Variant ID`, and `Secret`.

### 3. Customize and Build Application

The project source code must be customized with the unique metadata assigned to the application variant by the JBoss Unified Push Server OpenShift instance and GCM. 

1. Open `QUICKSTART_HOME/push-contacts-mobile/client/push-contacts-mobile-android/src/org/jboss/aerogear/unifiedpush/quickstart/Constants.java` for editing.
2. Enter the application variant values allocated by the JBoss Unified Push Server OpenShift instance and GCM for the following constants:

        String UNIFIED_PUSH_URL = "<pushServerURL e.g https://{OPENSHIFT_UNIFIEDPUSHSERVER_URL}/ag-push >";
        String VARIANT_ID = "<variantID e.g. 1234456-234320>";
        String SECRET = "<variantSecret e.g. 1234456-234320>";
        String GCM_SENDER_ID = "<senderID e.g Google Project ID only for android>";

3. Save the file.
4. Build the application

        cd QUICKSTART_HOME/push-contacts-mobile/client/push-contacts-mobile-android
        mvn compile


### 4. Test the Application

#### 0. Prerequisites

1. The JBoss Unified Push Server OpenShift instance must be running before the application is deployed to ensure that the device successfully registers with the JBoss Unified Push Server on application deployment.
2. The `push-contacts-mobile/server/push-contacts-mobile-picketlink-secured` application must be running before attempting to log into the mobile client application to ensure successful login. For more information, see the README distributed with the `push-contacts-mobile-picketlink-secured` application.

#### 1. Deploy for Testing

The application can be tested on physical Android devices only; push notifications are not available for Android emulators. To deploy, run and debug the application on an Android device attached to your system, on the command line enter the following:

        cd QUICKSTART_HOME/push-contacts-mobile/client/push-contacts-mobile-android
        mvn clean package android:deploy android:run


Application output is displayed in the command line window.

#### 2. Log In

When the application is deployed to an Android device, you can log into it and begin using the CRUD functionality. Note that access to the application is restricted to users registered with the server-side application and to assist you in getting started a number of default users are preconfigured.

1. Launch the application on the Android device.
2. Log into the application using one of the default user credentials ('_maria:maria_','_dan:dan_', or '_john:john_').

	After successful login, you are presented with a list of existing Contacts residing on the server.

	![contacts list home screen](doc/contacts-list.png)

3. Click a contact to open the Edit screen where you can modify the contact's details.

	![contact details](doc/contact-details.png)

#### 3. Send a Push Message

You can send a push notification to your device by creating a new Contact.  
You can also send a push notification to your device using the `push-contacts-mobile/client/contacts-mobile-webapp` application by completing the following steps (for more information, see the README distributed with the `contacts-mobile-webapp` application):

1. Open the web interface of the `push-contacts-mobile/client/contacts-mobile-webapp` application in a browser at the following URL: <http://localhost:8080/jboss-contacts-mobile-webapp/> .
2. Add a new Contact.

This automatically triggers a push notification request to the JBoss Unified Push Server OpenShift instance and subsequently the push notification displays on the mobile device.

![contact details](doc/notification.png)

