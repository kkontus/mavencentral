# mavenCentral() and jcenter() Gradle plugin

This Gradle plugin is used for ``mavenCentral()`` and ``jcenter()`` library publishing.
It is created with a help from 
[Jernej Virag](https://www.virag.si/2015/01/publishing-gradle-android-library-to-jcenter/) and 
[nuuneoi](https://inthecheesefactory.com/blog/how-to-upload-library-to-jcenter-maven-central-as-dependency/en) blogs.


*This plugin is tested with* **Gradle/Gradle Wrapper = 2.14.1** *and* **Gradle Build Tools 2.2.0**

Code below is example of usage, you only need to change extra properties inside ``exc {...}`` in your library module ``build.gradle``, set credentials inside ``gradle.properties`` or ``local.properties`` and add plugin classpath to root level ``build.gradle`` file:

Add the following to your root level ``build.gradle``

```
classpath 'com.android.tools.build:gradle:2.2.0'
classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.6'
classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'
```

```
plugins {
    id "com.jfrog.bintray" version "1.7.1"
    id "com.github.dcendents.android-maven" version "1.4.1"
}
```


Add the following to your root level ``gradle.properties`` or ``local.properties``

```
 # MAVEN
 signing.keyId=xxxxxxxx //gpg key
 signing.password=xxxxxxxx //gpg pass
 signing.secretKeyRingFile=/Users/<your_user>/.gnupg/secring.gpg
 ossrhUsername=xxxxxx //sonatype user
 ossrhPassword=xxxxxx //sonatype password
 stagingRepositoryUrl=https://oss.sonatype.org/service/local/staging/deploy/maven2/
 snapshotRepositoryUrl=https://oss.sonatype.org/content/repositories/snapshots/
 
 # JCENTRAL
 bintray.user=xxxxxx //bintray user
 bintray.apikey=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx //bintray api key
 bintray.gpg.password=xxxxxx //same as signing.password - gpg pass
 bintray.oss.user=xxxxxx //same as ossrhUsername - sonatype user
 bintray.oss.password=xxxxxx //same as ossrhPassword - sonatype user
```

Add the following to your library module folder ``build.gradle`` below your other Gradle configs
```
//... 
//rest of your library module gradle config code
//...

group = "com.github.kkontus.stringhelper"                // Maven Group ID for the artifact
archivesBaseName = "stringhelper"
//version = "1.0.7-SNAPSHOT"
version = "1.0.7"

ext {
    //project info
    projectName = "String Helper"
    packagingType = "aar"
    publishedGroupId = group
    publishedArtifactId = "some artifact id"
    projectDescription = "String Helper Library"
    siteUrl = 'https://github.com/kkontus/StringHelper'      // Homepage URL of the library
    libraryName = "stringhelper"
    repoName = "stringhelper"

    //scm info
    scmConnection = "scm:git:http://github.com/kkontus/StringHelper.git"
    scmDeveloperConnection = "scm:git:https://github.com/kkontus/StringHelper.git"
    scmUrl = "https://github.com/kkontus/StringHelper"

    //license info
    licenseName = "The Apache License, Version 2.0"
    licenseUrl = "http://www.apache.org/licenses/LICENSE-2.0.txt"

    //developer info
    devId = "kkontus"
    devName = "Kristijan Kontus"
    devEmail = "kristijan.kontus@gmail.com"
}

//apply from: 'central.gradle'
apply from: 'https://raw.githubusercontent.com/kkontus/mavencentral/master/central.gradle'
```

With this last line you have an option to use this plugin directly from Github by applying:

``apply from: 'https://raw.githubusercontent.com/kkontus/mavencentral/master/central.gradle'`` 

or you can download it as raw file and put it inside your library module folder and use it like 

``apply from: 'central.gradle'``


To run Gradle script go to library module and run:

``../gradlew uploadArchives`` for mavenCentral()

``../gradlew bintrayUpload`` for jcenter()
