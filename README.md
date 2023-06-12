# After Effects Expression Selector

## Examples

### Select Specific Character
Create a Text Layer. Apply an Expression Selector and paste this code into the "Amount".
```javascript
// make sure "based On" is set to "Characters"
const characterToSelect = "t"; // change character here
const str = text.sourceText.replace(/[\r\n]/gm, ''); // remove line breaks
str[textIndex-1]==characterToSelect ? 100 : 0;// select character
```
![character selector_BSP_2_2023-06-12_16 25 48](https://github.com/simonheimbuchner/expressionSelector/assets/20266941/3bc3a881-805f-4016-8e63-5db6c8fbe201)
