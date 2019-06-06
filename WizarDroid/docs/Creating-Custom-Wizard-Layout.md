In some cases you will want to create your own custom wizard layout. This is very simple to implement with WizarDroid. All you have to do is extend **[WizardFragment](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard_fragment.html)** and set up a layout for the wizard. The following tutorial will guide you through the process.

* Set up the layout for the wizard
* Create a Fragment that inherits from **[WizardFragment](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard_fragment.html)** and override *[onSetup()](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard_fragment.html#a1cab74608d86fe37163d7465d2bda988)* to define the wizard's flow

###1.	Wizard Layout

WizarDroid is built on top of Android's [ViewPager](http://developer.android.com/reference/android/support/v4/view/ViewPager.html) and therefore you **must** have a **ViewPager** in your wizard layout with an ID: *step_container*. Notice that the ID is already created in WizarDroid. Instead of using "@+id/", use "@id/". Since WizarDroid is using the support library v4, this ViewPager must be the support version of ViewPager. Consider the following code snippet for details:

	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	android:layout_width="fill_parent"
	android:layout_height="fill_parent">
	
	<View   android:id="@+id/horizontal_line"
	    android:layout_width="match_parent"
	    android:layout_height="1dp"
	    android:background="@android:color/darker_gray"
	    android:layout_above="@+id/wizard_button_bar"/>
	
	<!-- Layout for wizard controls -->
	<LinearLayout
	    android:id="@+id/wizard_button_bar"
	    android:orientation="horizontal"
	    android:layout_width="match_parent"
	    android:layout_height="wrap_content"
	    android:layout_alignParentBottom="true"
	    >
	    <Button
	        android:id="@+id/wizard_previous_button"
	        android:text="@string/action_previous"
	        android:enabled="false"
	        android:layout_width="0dp"
	        android:layout_height="wrap_content"
	        android:layout_weight="1"
	        style="?android:attr/borderlessButtonStyle"
	        />
	    <View
	        android:layout_width="1dp"
	        android:layout_height="match_parent"
	        android:background="@android:color/darker_gray"
	        />
	    <Button
	        android:id="@+id/wizard_next_button"
	        android:text="@string/action_next"
	        android:layout_width="0dp"
	        android:layout_height="wrap_content"
	        android:layout_weight="1"
	        style="?android:attr/borderlessButtonStyle"
	        />
	</LinearLayout>
	
	    <!--
	            **********************************************************************
	            **You MUST have this ViewPager as the container for wizard's steps  **
	            **********************************************************************
	    -->
	<android.support.v4.view.ViewPager
	    xmlns:android="http://schemas.android.com/apk/res/android"
	    android:id="@id/step_container"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:layout_above="@+id/horizontal_line"/>
	
	</RelativeLayout>

###2.	Inheriting from WizardFragment

The next step in creating your own custom wizard layout is to inherit from **[WizardFragment](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard_fragment.html)** and wire your controls events to make calls to WizarDroid API through the inherited *[wizard](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard.html)* field member. Call method *[goNext()](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard.html#a5d822f5eb8a4e5df02c0336d7872d84a)* and *[goBack()](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard.html#a1a1e45fed5cc547f1927db2b7401fe7b)* accordingly. Refer to [Wizard](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard.html) for more info about other useful methods.

	public class TutorialWizard extends WizardFragment {

	    //You must have an empty constructor according to Fragment documentation
	    public TutorialWizard() {
	    }
	    /**
	     * Binding the layout and setting buttons hooks
	     */
	    @Override
	    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
	        View wizardLayout = inflater.inflate(R.layout.wizard, container, false);
	        nextButton = (Button) wizardLayout.findViewById(R.id.wizard_next_button);
	        nextButton.setOnClickListener(this);
	        previousButton = (Button) wizardLayout.findViewById(R.id.wizard_previous_button);
	        previousButton.setOnClickListener(this);
	
	        return wizardLayout;
	    }
	
	    //You must override this method and create a wizard flow by
	    //using WizardFlow.Builder as shown in this example
	    @Override
	    public WizardFlow onSetup() {
	        return new WizardFlow.Builder()
	                .setActivity(this)                      //First, set the hosting activity for the wizard
	                .setContainerId(R.id.step_container)    //then set the layout container for the steps.
	                .addStep(TutorialStep1.class)           //Add your steps in the order you want them
	                .addStep(TutorialStep2.class)           //to appear and eventually call create()
	                .create();                              //to create the wizard flow.
	    }
	    
	    /**
	     * Triggered when the wizard is completed.
	     * Overriding this method is optional.
	     */
	    @Override
	    public void onWizardComplete() {
	        //Do whatever you want to do once the Wizard is complete
	        //in this case I just close the activity, which causes Android
	        //to go back to the previous activity.
	        getActivity().finish();
	    }
	
	    @Override
	    public void onClick(View v) {
	        switch(v.getId()) {
	            case R.id.wizard_next_button:
	                //Tell the wizard to go to next step
                	wizard.goNext();
	                break;
	            case R.id.wizard_previous_button:
	                //Tell the wizard to go back one step
                	wizard.goBack();
	                break;
	        }
	        updateWizardControls();
	    }
	
		/**
		 * Updates the UI according to current step position
		 */ 
	    private void updateWizardControls() {
	        previousButton.setEnabled(!wizard.isFirstStep());
	        nextButton.setText(wizard.isLastStep()
	                ? R.string.action_finish
	                : R.string.action_next);
	    }
	}

Each Wizard inherits from [WizardFragment](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard_fragment.html) and then have access to *[wizard](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard.html)* field, which is the engine driving the wizard. The most important methods are *[goNext()](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard.html#a5d822f5eb8a4e5df02c0336d7872d84a)* and *[goBack()](http://nimrodda.github.io/WizarDroid/javadoc/classorg_1_1codepond_1_1wizardroid_1_1_wizard.html#a1a1e45fed5cc547f1927db2b7401fe7b)* which control the flow of the wizard.

To see how to create the wizard steps, refer to [Simple Wizard Scenario Tutorial](Simple-Wizard-Tutorial).
