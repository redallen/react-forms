import Grid from '@material-ui/core/Grid'
import RouterLink from 'next/link';
import Link from '@material-ui/core/Link';
import RawComponent from '@docs/raw-component';

import ListOfContents from '../../src/helpers/list-of-contents';

<Grid container item>
<Grid item xs={12} md={10}>

# Initialize On Mount

Data Driven Forms provides a way how you can easily initialized a field when the field is mounted (re-mounted).

Just pass a `initializeOnMount` prop and set it to `true`.

The field will use the `initialValue` set in the schema (<RouterLink href="/renderer/component-api#formgroupwrappedcomponents"><Link>initialValue</Link></RouterLink>) or in the renderer (<RouterLink href="/renderer/renderer-api#optionalprops"><Link>initialValues</Link></RouterLink>).

`initialValue` has higher priority than a value from `initialValues`.

## Example


```jsx
{
  component: componentTypes.TEXT_FIELD,
  name: 'name',
  initializeOnMount: true,
  initialValue: 'this value will be set'
}
```

## When to use it?

This feature comes handy if you need change a value when an user traverses a form, which shows and hides fields, and the value is not set by the user. Very useful case is used it wizard forms, where you can set different value for the same input according the way the user went in the wizard form by using this option combined with <RouterLink href="/renderer/component-api#commonpropsforallformfields"><Link>hideField</Link></RouterLink> prop.

<RawComponent source="initialize-mount" />

## Clear the value

If you need clear the value after unmounting, you can do it by using <RouterLink href="/renderer/unmounting"><Link>clearOnUnmount</Link></RouterLink>.

</Grid>
<Grid item xs={false} md={2}>
  <ListOfContents file="renderer/initialize-mount" />
</Grid>
</Grid>
