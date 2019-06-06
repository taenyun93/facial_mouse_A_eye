One of the core functions of a wizard framework is to allow passing of data among steps. In WizarDroid this can be achieved very easily. In this tutorial we will set up a simple form which will span among several steps. Check out the sample project for a [complete example](https://github.com/Nimrodda/WizarDroid/tree/master/wizardroid-sample).

Using WizarDroid Context is useful when you wish to pass data from the hosting Activity/Fragment to the wizard steps or to pass data from one step to another. To do so, you annotate field variables with *@ContextVariable*.

### @ContextVariable annotation

This annotation tells WizarDroid that the annotated field should be persisted in the Wizard Context. This context remains even if the wizard was paused or destroyed.

**NOTE:** Currently WizarDroid supports only the persistence of **primitive types** ( _Int_, _Short_, _Char_, _String_, _Boolean_, _Long_, _Double_, _Byte_, _Float_), _Date_ and **complex** types which implement _Serializable_ or _Parcelable_. Arrays are currently not supported.

To persist data in WizarDroid's Context, simply annotate the fields you wish to persist with _@ContextVariable_. For example:

    public class FormStep1 extends WizardStep {

        @ContextVariable
        private String firstname;
        @ContextVariable
        private String lastname;

WizarDroid will then persist these fields in the Wizard Context, which will be available for all steps in the running wizard. The persisted fields are referred to as **Context Variables**.

In order to access a **Context Variable**, the field names and types **MUST** be consistent throughout the wizard. The value of this field will be bound automatically by WizarDroid using reflection. In this case our target step will look like this:

    public class SummaryStep extends WizardStep {

        @ContextVariable
        private String firstname;
        @ContextVariable
        private String lastname;
