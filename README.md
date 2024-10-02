# selectorInspector
Query a selector and a property and if the combination exists in your stylesheet, this function will return the property value.

## Example CSS:
```css
.calendarNavbarLink {
  margin-left: 20px;
}

.today {
  background-color: lawngreen;
}

.weekend {
  background-color: lightsalmon;
}
```

__________


## `convertStylesheetToObject(stylesheet)` function
```js
// GET ALL STYLESHEETS
let styleSheetCount = document.styleSheets.length;

// GET LAST STYLESHEET
let lastStyleSheet = document.styleSheets[(styleSheetCount - 1)];

// CONVERT STYLESHEET TO OBJECT
const convertStylesheetToObject = (stylesheet) => {
  
  // CREATE AND POPULATE STYLE ARRAY
  let styleArray = [];
  [...stylesheet.cssRules].forEach((cssRule) => styleArray.push(cssRule.cssText));

  styleArray = styleArray.map((style) => ({
    'selectors': style.split('{')[0].split(',').map((selector) => selector.trim()),
    'styles': style.split('{')[1].replace('}', '').trim()
  }));

  styleArray.forEach((style) => style.styles = style.styles.split(';'));
  
  styleArray.forEach((style, s) => {
    let stylesValue = {};
    styleArray[s].styles.forEach((styleDeclaration) => {
      if (styleDeclaration !== '') {
        stylesValue[styleDeclaration.split(':')[0].trim()] = styleDeclaration.split(':')[1].trim();
      }
    });
    styleArray[s].styles = stylesValue;
  });
  
  return styleArray;
}  

console.log(convertStylesheetToObject(lastStyleSheet));
```

__________


## `getStyleValue(selector, property, styleArray)` function
```js
const getStyleValue = (selector, property, styleArray) => {
  
  let styleValue = 'No value found.';
  
  const selectorFilter = styleArray.filter((element) => element.selectors.includes(selector));
  
  if (selectorFilter.length > 0) {
    const propertyFilter = selectorFilter.filter((element) => Object.keys(element.styles).includes(property));
    if (propertyFilter.length > 0) {
      styleValue = propertyFilter.reverse()[0].styles[property];
    }
  }
  
  return styleValue;
}

let styleArray = [
  {
    "selectors": [
      ".calendarNavbarLink"
    ],
    "styles": {
      "margin-left": "20px"
    }
  },
  {
    "selectors": [
      ".today"
    ],
    "styles": {
      "background-color": "lawngreen"
    }
  },
  {
    "selectors": [
      ".weekend"
    ],
    "styles": {
      "background-color": "lightsalmon"
    }
  }
];

console.log(getStyleValue('.weekend', 'background-color', styleArray));
```
