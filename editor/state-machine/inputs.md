# Inputs

Inputs are the contract between designers and developers. As Designers, we use Inputs as a way to control the transitions in our state machine by assigning them as conditions. Developers tie into the inputs at runtime and define conditions through code that can change those inputs.

### **Creating a new Input**

To create a new Input, use the plus button in the input panel. After hitting the plus button, you’ll be prompted to select the type of input you want to create. There are three types of inputs; booleans, triggers, and numbers.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 14.48.52.gif" alt=""><figcaption></figcaption></figure>

## Input Types

We can use three types of inputs depending on the situation and type of interactive content: booleans, triggers, and numbers. We'll discuss each of these inputs below.

Be sure to rename your inputs as these names will be important for your developer counterpart.

### **Boolean**

A boolean can hold either a true or false value.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 14.53.03.gif" alt=""><figcaption><p>Boolean for a switch</p></figcaption></figure>

Booleans are useful when we want to make assets like switches, or check for more complex logic like a match 3 game.

### **Trigger**

Triggers are similar to booleans, but can only become true for a short time.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 14.55.44.gif" alt=""><figcaption><p>Trigger for attack animation</p></figcaption></figure>

These inputs are great to use for things like gun fire, jumps, or as a simple way to toggle between multiple animations.

### **Number**

A number input give you a number box that can be any integer.

<figure><img src="../../.gitbook/assets/CleanShot 2023-09-06 at 14.58.15.gif" alt=""><figcaption><p>Numer input for rating animation</p></figcaption></figure>

Number inputs are easily the most flexible input. They are great to use when you want to track progress, or play a specific state thats in an array like a character skin.
