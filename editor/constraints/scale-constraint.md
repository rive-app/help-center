# Scale Constraint

The Scale Constraint allows you to set limits on an object's scale and/or copy the scale properties from a target object. These properties can be independently activated. 

## How to create a Scale Constraint

### 1. Add a Scale Constraint to an object

Use the Constraints section of the Inspector to add a Scale Constraint to an object.

![](../../.gitbook/assets/2021-08-19-16.40.37.gif)

### 2. Choose a target

![](../../.gitbook/assets/2021-08-19-16.44.38.gif)

Use the new constraint's fly-out menu to select a target for this constraint.

![](../../.gitbook/assets/2021-08-19-16.41.20.gif)

### 3. Test that the Scale Constraint is working

Manipulating the target object now causes the constrained object to Scale properties.

## Strength <a id="target"></a>

The Strength property determines how much the constrained object is affected.

A Strength of 0% means the constraint won't have any effect.

A Strength of 50% means half the value from the target will be applied.

## Transform Space

### Source Space

Choose whether this constraint should use World or Local coordinates for the Source Space.

### Destination Space

Choose whether this constraint should use World or Local coordinates for the Destination Space.

### Min/Max Space

Choose whether this constraint should use World or Local coordinates for the Min/Max Space.







