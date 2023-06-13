# After Effects Expression Selector


## How it works

### Legend

| Name  | Description | Output |
| ------------- | ------------- | ------------- |
| Expression  | Determines the Range and Amount of the Effect | [x,y,z] |
| Based On  |Determines the Unit of the Range | Characters / Characters Excluding Spaces / Words / Lines |
| Effect |The Effect on selected parts of the text | / |
| Amount | If not overwritten by the Expression, controls the Amount of Effect applied | [x,y,z] |

![character selector_Character Selector How 2_2023-06-13_15 32 23](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/39feb909-bb52-4501-9462-06ca40ec0a00)

Instead of the Start and End-Sliders of the Range selector, an Expression is used to select a Range in the Source Text. Per _Based On_-Unit, a different amount of _Effect_ can be applied.

>#### Note:
> For simplicity's sake, I will only be using the _Based On_-Unit **Characters** in this explanation. Of course, the same applies for all other units.

### Unique Variables

There are three variables unique to the Expression Selector:
| Name  | Description |
| ------------- | ------------- |
| `textTotal` | the total number of Characters, starting at 1 |
| `textIndex` | the index of each Character, starting at 1 |
| `selectorValue` | the value of a Range Selector, if exists |

### Introduction
Let's just jump straight into a crude but useful example to illustrate how this works.

<sub>Text Layer > Expression Selector > Amount</sub>

```javascript
  if(textIndex==1) {
    0
  } else if(textIndex==2) {
    25
  } else if(textIndex==3) {
    50
  } else if(textIndex==4) {
    75
  } else if(textIndex==5) {
    100
  }

```

![character selector_Character Selector How_2023-06-13_13 57 12](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/9c1b2955-a0cb-44ed-90aa-c214d8c6fdef)

As you can see, we used an If Statement to check for the textIndex. For each textIndex, we've returned a different number.
This is the percentage of how much _Effect_ is applied to the specified range. For `textIndex==1`, it's 0% — hence the first character is completely unaffected. `textIndex==5` is 100% affected, so the full 90° Rotation and Green Color are applied. The states inbetween are automatically interpolated.

>#### Note:
> Return `-100` to have the exact opposite effect applied.

Think of it this way: This whole block of code is executed for each Character in the Source Text. That's why we can ask for the value of `textIndex` five times and get different results for each Character.

![character selector_Character Selector How 3_2023-06-13_15 04 54](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/e373bdaf-5a07-4320-b2e9-0286d6f5c9e4)



In Javascript Talk, this is essentially what's going on:
```Javascript
for(let textIndex=1;i<textTotal;textIndex++) {

  // Expression Selector Code
  
}
```

### Simplify
To simplify the above code, we could do the following:

```javascript
(textIndex-1)*25;
```

And to make this fully dynamic, I'll divide `100%` by `textTotal`. This will then work for any amount of Characters.

```javascript
(textIndex-1)*(100/(textTotal-1));
```


>#### Note:
>Since both `textIndex` and `totalText` start at `1`, I'm subtracting `1` to get the first Character to be `0%`.


## Examples


### Gradient
<sub>Text Layer > Expression Selector > Amount</sub>
```javascript
// gradient
(textIndex-1)*(100/(textTotal-1));
```
![character selector_Character Selector Bsp6_2023-06-12_16 51 36](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/4c426c6d-fea1-4930-ba55-48827e9207b1)


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

### Time Offset
<sub>Text Layer > Expression Selector > Amount</sub>
```javascript
// make sure to change "based On" to get the desired effect
const timeOffset=.1; // in seconds
valueAtTime(time-((textIndex-1)*timeOffset))
```
![Character Selector Bsp7](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/e5b3732e-39f1-4b0e-914d-650c69d5cb0a)


