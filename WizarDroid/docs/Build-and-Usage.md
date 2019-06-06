WizarDroid is built using [Gradle](http://www.gradle.org/). Learn more about it here: [new build system](http://tools.android.com/tech-docs/new-build-system/user-guide).

You will need to install the Android Support Repository from the Android SDK Manager, since WizarDroid uses support library v4.

Include the following in your project's `build.gradle` and WizarDroid library will be downloaded automatically from Maven Central repository.

    repositories {
        mavenCentral()
    }

    dependencies {
        compile 'org.codepond:wizardroid:1.3.0'
    }

WizarDroid is depending on support library v4, which might not be in sync with the one you're using in your project. This issue is known as [transitive dependency issue](http://www.concretepage.com/build-tools/gradle/gradle-exclude-transitive-dependency-example). You can simply add the following to build.gradle to solve it:

```
configurations {
    all*.exclude module: 'support-v4'
}
```

Alternatively,  you can include `wizardroid` folder as a sub-module of your project. See [Google's tutorial](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Dependencies-Android-Libraries-and-Multi-project-setup) if you are unfamiliar with how to set up a multi-module Gradle project. 

## Configuring Proguard to work with WizarDroid

To get WizarDroid working with Proguard, you will need to add the following to your proguard file:

	#Wizardroid
	-keepnames class * { @org.codepond.android.wizardroid.ContextVariable *;}