# Examples


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
![Character Selector Bsp7_1_1](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/b371fcf6-429b-4c28-9661-608e683f055c)


