**1. [Calling `notifyCompleted()` throws an exception.](https://github.com/Nimrodda/WizarDroid/issues/44)**

Make sure that you're calling this method in the correct event. For instance, I got a few reports were the developer called this method from `EditText#onTextChanged()`, which is fired after **each and every** key pressed, instead of calling it once from `EditText#afterTextChanged()`.
Refer to [Controlling the wizard flow dynamically](Controlling-wizard-flow-dynamically) for more information.

**2. [I get a `NullPointerException` while trying to `findViewById` from a wizard step](http://stackoverflow.com/questions/24748965/how-to-inflate-a-view-from-a-fragment)**

This issue occurs if you have a layout xml file named `wizard.xml` in your project and are trying to inflate it in the wizard step. WizarDroid already has a layout file named `wizard.xml`, which will cause it to collide with yours. This will change in future releases.

**3. Is it possible to disable back button or in general, prevent user from going back step?**

No, currently this feature is not supported.

**4. I want to skip one step based on user input, how do I do that?**

Currently, this feature is not supported. But, this is definitely in the backlog. So stay tuned ;)

**5. Could not find com.android.support:support-v4:X.Y.Z Required by: org.codepond:wizardroid:X.Y.Z** 

Please install the Android Support Repository from the Android SDK Manager.

**6. I'm using the latest version of support library v4 and I get an error when I try to build with Gradle that there are two versions of the support library v4**

WizarDroid is depending on support library v4, which might not be in sync with the one you're using in your project. This issue is known as [transitive dependency issue](http://www.concretepage.com/build-tools/gradle/gradle-exclude-transitive-dependency-example). You can simply add the following to build.gradle to solve it:

```
configurations {
    all*.exclude module: 'support-v4'
}
```
