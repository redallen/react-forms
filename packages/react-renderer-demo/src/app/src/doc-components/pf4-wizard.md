This a custom component. OnSubmit will send only values from visited steps.

Don't forget hide form controls by setting \`showFormControls\` to \`false\` as a prop of the renderer component.

# PF4 Wizard

## Props

| Prop  | Type | Default |  Description |
| ------------- | ------------- | ------------- | ------------- |
| title  | string  | undefined  | Title in header (will show header) |
| description  | string  | undefined  | Description in header |
| buttonLabels  | object  | see below  | Labels for buttons |
| inModal  | bool  | undefined  | show form in modal  |
| isCompactNav  | bool  | undefined  | see Patternfly |
| setFullWidth  | bool  | undefined  | see Patternfly  |
| setFullHeight  | bool  | undefined  | see Patternfly  |
| isDynamic  | bool  | undefined  | will dynamically generate steps navigation (=progressive wizard), please use if you use any condition fields which changes any field in other steps (wizards with conditional steps are dynamic by default) |
|showTitles|bool|undefined|If true, step titles will be shown in the wizard body|
|predictSteps|bool|undefined|If true, dynamic wizard will predict steps in the navigation.|

## Default buttonLabels

```jsx
{
  cancel: 'Cancel',
  back: 'Back',
  next: 'Next',
  submit: 'Submit',
}
```
You can rewrite only selection of them, e.g.

```jsx
{
  submit: 'Deploy',
}
```

(Others will stay default)

## Docs for steps

| Props  | Type  |  Description |
| ------------- | ------------- | ------------- |
| stepKey  | string, number | For first step: 1, otherwise anything |
| nextStep  | object/stepKey of next step | See below |
| fields  | array | As usual |
| substepOf | string | Substep title (steps are grouped by this title) |
| title | string | Step title |
| buttons | node, func | Custom buttons component|
|showTitle|bool|If true, step titles will (not if false) be shown in the wizard body|
|customTitle|node|Use if you want to render as the title different/custom title (for example, title with a popover|
|disableForwardJumping|bool|When use return to this step, jumping forward in the navigation is disabled.|


- nextStep can be stepKey of the next step
- or you can branch the way by using of object:

```jsx
nextStep: {
        when: 'source-type', // name of field, where deciding value is stored
        stepMapper: {
          aws: 'aws', // value: 'stepKey' of next step
          google: 'google',
          ...
        },
},
```

- another option is to use custom function. The custom function receives as the first argument an object with values and the function has to return a `stepKey` in string.

```jsx
nextStep: ({ values }) => (values.aws === '123' &&& values.password === 'secret') ? 'secretStep' : 'genericStep'
```

### Buttons

Each step can implement its own buttons.

```jsx
const Buttons = () => <div>Hello</div>;

[{
  title: 'foo-step',
  stepKey: '1',
  name: 'foo',
  buttons: Buttons,
  fields: [{
   name: 'foo-field',
  }],
}]
```

The components receives these props:

|Props|Description|
| --- | -------- |
|ConditionalNext|Conditional next button|
|SubmitButton|Default submit button.|
|SimpleNext|Default next button.|
|formOptions|formOptions|
|disableBack|If it's first step, disable back is true.|
|handlePrev|Function to handle back button.|
|nextStep|Next step field from the schema.|
|FieldProvider|FieldProvider|
|handleNext|Function to handle next click.|
|buttonsClassName|Classname of buttons.|
|buttonLabels|Object with labels.|
|renderNextButton|Function which completely handle the next/submit button.|


## How to do substeps

Field in Wizard fields should contain `substepOf` <`string`> which is title of the primary step. Steps with the same substepOf are grouped together by the title of primary step.

### Example

Structure:

```jsx
                     //            index
Select Type          // step       1
Configuration        // step       2
  Security           // substep    2 (there is no step 'configuration')
  Credentials        // substep    3
Summary              // step       4
```

```jsx
Schema: [
  {
    stepKey: '1',
    title: 'Select Type',
    nextStep: 'security'
  },
  {
    stepKey: 'security',
    title: 'Security',
    nextStep: 'credentials',
    substepOf: 'Configuration'
  },{
    stepKey: 'credentials',
    title: 'Credentials',
    nextStep: 'summary',
    substepOf: 'Configuration'
  },{
    stepKey: 'summary',
    title: 'Summary'
  },
]
```

Progressive Wizard works same way. It checks if previous step has the same \`substepOf\` value and if so, it grouped them together.
If the value is different, a new primary step is created with the step as a substep.

## First step

First step should have `stepKey: 1` or as a string: `'1'`

## Variants of Wizard

### Simple wizard

- steps navigation is visible, user can jump back and forward (after visiting the step)
- does not contain any conditional steps
- if you need to change one step according to another (dynamic fields), do not use this (there is no way how to check which fields was changed), instead of that use **progressive wizard**
- if user change some field and make the form invalid, he cannot jump forward until he fix the form

![simplewizard](https://user-images.githubusercontent.com/32869456/58427234-56725680-809f-11e9-8e22-3ce7286b30d2.gif)

### Progressive wizard

- steps are visible as user visits them
- user can jump only back
- use `isDynamic` prop to enforce it
- use `predictSteps` to allow navigation to show future steps
    - if you have any conditional fields in the step, you should use `disableForwardJumping` in the step definition, to disable jumping forward in the navigation, otherwise user could miss the changed fields in next steps.

![progressivewizard](https://user-images.githubusercontent.com/32869456/58427241-5b370a80-809f-11e9-8e79-a4a829b8d181.gif)

## Useful links

[PF4 wizard implementation](https://www.patternfly.org/v4/documentation/react/components/wizard/)

[Wizard design](https://www.patternfly.org/v4/design-guidelines/usage-and-behavior/wizard)