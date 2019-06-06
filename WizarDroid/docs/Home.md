WizarDroid is a lightweight Android library, developed by CodePond.org, that addresses a feature that Android is surprisingly missing, Wizards.
WizarDroid is built on top of Android's [ViewPager](http://developer.android.com/reference/android/support/v4/view/ViewPager.html) and is compatible with Android API 9+ using Android [Support Library](http://developer.android.com/training/basics/fragments/support-lib.html).

Let's say you have a step-by-step process in your App for filling up a form. The user moves from one screen to another and normally in the last step will see a summary and confirm the input. **If your flow is hard-coded, you are doing it WRONG!**.

### Key advantages:

* Built-in basic wizard layout with paging and slide animation
* Wizard context for persisting data in the wizard
* Compatible with other libraries such as [ActionBarSherlock](http://actionbarsherlock.com/)
* Support for nested fragments
* Wizard's flow is defined in one place and can be maintained easily
* Simple API for controlling wizard's flow in runtime 


### How does it work?

WizarDroid is built on top of Android's [ViewPager](http://developer.android.com/reference/android/support/v4/view/ViewPager.html)and uses the [Support Library](http://developer.android.com/training/basics/fragments/support-lib.html) to ensure compatibility with devices that use Android API 9 and above.

You build your Wizard by creating a **Fragment** which inherits from **BasicWizardLayout** and steps by inheriting from **WizardStep**. Once those are done, you set up your wizard flow and give it a go! You don't need to worry about passing data from one step to another using cumbersome callbacks, because WizarDroid **does that for you!**

You can also define your own wizard's UI controls and call WizarDroid API directly to do the job. This enables you to use libraries such as ActionBarSherlock, etc.



# License

WizarDroid is available under the [MIT License](https://github.com/Nimrodda/WizarDroid/blob/master/license).
