# After Effects Expression Selector


## How it works

The Expression Selector works much like the Range Selector in many ways.

First, add an Animator to a text Layer. Then add an Expression Selector.
Choose the type of text you want to affect (Characters/Words/Lines) in the **Based On**-Property.
Next, add the type of effect (Opacity, Position, ...) you want to create using the "Add"-Dropdown next to the Animator.

**Now onto expressioneering.**

The Expression Selector doesn't work like other properties in After Effects.
For a start, there are three variables unique to the Expression Selector:
```javascript
textTotal // the total number of Characters/Lines/Words in the sourceText property
textIndex // the current Character/Line/Word
selectorValue // the value of the Amount Property
```

Think of it this way: The Expression Selector is wrapped by a for loop. For every Character/Line/Word, it executes the code snipped once, updating `textIndex` every time. 
For each textIndex-loop, you can return a number between `-100` and `100`— with `0` being "no effect", `100` being "full effect", and `-100` being "opposite full effect". 
To illustrate how you can use this, here's a crude but useful expression and it's outcome:

<sub>Text Layer > Expression Selector > Amount</sub>
```javascript
// imagine a for loop around this block of text
// for(let textIndex=1;i<textTotal;textIndex++) {

  if(textIndex==1) {
    100
  } else if(textIndex==2) {
    50
  } else if(textIndex==3) {
    0
  } else if(textIndex==4) {
    -50
  } else if(textIndex==5) {
    -100
  }
 
// }
```
![character selector_Character Selector How_2023-06-12_17 47 42](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/38a2692b-3cc7-4adf-af0f-add5247c9414)

Actually, the output is three-dimensional, so `100` is just a shortcut for `100,100,100`.
The output does affect the underlying dimensions of the effects, so `[100,0,0]` would only affect the first dimension, `[100,100,0]` the first two and so on.



## Examples

### Select Specific Character
<sub>Text Layer > Expression Selector > Amount</sub>
```javascript
// make sure "based On" is set to "Characters"
const characterToSelect = "*"; // change character here
const str = text.sourceText.replace(/[\r\n]/gm, ''); // remove line breaks
str[textIndex-1]==characterToSelect ? 100 : 0;// select character
```
![character selector_BSP_2_2023-06-12_16 30 59](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/95332c96-aa95-497b-b0a1-dd5cc4d62ef3)

### Select Specific Word
<sub>Text Layer > Expression Selector > Amount</sub>
```javascript
// make sure "based On" is set to "Words"
const wordToSelect = "and";
const words = text.sourceText.replace(/[\r\n]/gm," ").split(" ").filter(n => n); // split into words 
words[textIndex-1]==wordToSelect ? 100 : 0;// select if match
```

![character selector_BSP_3_2023-06-12_16 37 51](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/9967f98f-2650-4edf-9d18-aa0c2cf9ea2d)

### Select Line Based On a Character
<sub>Text Layer > Expression Selector > Amount</sub>
```javascript
// make sure "based On" is set to "Lines"
const stringToSelect = "»";
const lines = text.sourceText.split(/\r?\n|\r|\n/g); // split by line break
lines[textIndex-1].indexOf(stringToSelect)!=-1 ? 100 : 0;// select if match
```
![character selector_Character Selector Bsp4_2023-06-12_16 39 23](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/77bba7b1-0a1d-4f66-9d59-961637c6a793)

### Select Line Based On Even/Odd
<sub>Text Layer > Expression Selector > Amount</sub>
```javascript
// make sure "based On" is set to "Lines"
textIndex%2 ? 100 : 0; //swap 100 & 0 to invert the effect
```
![character selector_Character Selector Bsp5_2023-06-12_16 44 36](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/2ee3e9de-c5ef-42dd-a0de-dcead4dd5e93)

### Gradient
<sub>Text Layer > Expression Selector > Amount</sub>
```javascript
// gradient
textIndex/textTotal*100
```
![character selector_Character Selector Bsp6_2023-06-12_16 51 36](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/4c426c6d-fea1-4930-ba55-48827e9207b1)
