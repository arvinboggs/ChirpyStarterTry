---
title: Google Sheets Custom Functions
date: 2023-06-04 12:00:00 +0800
category: tutorial
tags: 
image:
  path: /images/google-sheets-custom-functions.png
  width: 1280   # in pixels
  height: 720   # in pixels
---

# Google Sheets Custom Function

## Introduction
Google Sheets, like other spreadsheet applications, has built-in functions that the user can use to interpret or manipulate data. Functions like *SUM* and *AVERAGE* come in handy to accomplish common scenarios. However, people may encounter challenging tasks which leave the built-in functions inadequate. Modern spreadsheet applications each have their own implementation of providing users a way to create their own functions. For Google Sheets, this feature is called App Script. Fortunately, we don't have to learn a proprietary scripting language for this because App Script is in JavaScript — a programming language commonly used by front-end web developers (and full-stack web developers like me).

In this tutorial, we are going to implement an App Script function that outputs the bottom-most numeric value of a given range of cells. This function is useful for calculating the aggregate (for example sum, average, minimum, maximum) value of the ending balance from multiple columns.

## Step By Step Instructions
{% include youtube.html id="D9Es-XO1rC0" %}
1. Open a web browser and do 1 of the following:
   - Navigate to an existing Google Sheet document.
   - Navigate to [https://docs.google.com/spreadsheets](https://docs.google.com/spreadsheets).
1. From the menu bar, click "Extensions" > "App Script".
1. App Script editor will open in a new browser tab.
1. Each of the top-level functions will be available for use in the spreadsheet.
1. For this tutorial, let's implement the function, **BottomNumber**.
    ``` javascript
    function BottomNumber(vRange) {
      var pOut;
      for (var pRowIndex = 0; pRowIndex <= vRange.length - 1; pRowIndex++) {
        var pRow = vRange[pRowIndex];
        for (var pColIndex = 0; pColIndex <= pRow.length - 1; pColIndex++) {
          var pCell = pRow[pColIndex];
          if (pCell == null)
            continue;
          if (!isNaN(pCell)
            && (typeof(pCell) == 'number'))

            pOut = pCell;
        }
      }
      return pOut;
    }
    ```
1. Although not required, it is a good idea to rename the project for easy identification.
1. Save the project.
1. Go back to the spreadsheet.
1. Upon typing "equals" (=), the name of your custom function will not be among the list of functions. So, do not be alarmed if you cannot see your custom function.
1. Use the custom function. 
  ```
   =BottomNumber(C6:C16)
  ```

## Resources
I've shared the sheet I used for this tutorial [via this link](https://docs.google.com/spreadsheets/d/1lYH37G2c8Q4v6NUaAGP0wMCmfiW-6nI28pIbA64dewE). Don't forget to make a copy of that document to be able to edit and access the script.

## Closing
That's it. That's all you have to do to create and use a custom function in Google Sheets.

  