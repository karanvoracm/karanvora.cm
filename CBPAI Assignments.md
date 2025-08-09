# 📚 CS Assignments Collection

---

## 1️⃣ Assignment CS 1: Google Sheet and App Scripts

**🔗 Link to Spreadsheet:** [View Spreadsheet](https://docs.google.com/spreadsheets/d/1La5JxdDciQslw2UomWwI1tgcmZoRJoCzvQ9dW_6BUCg/edit?usp=sharing)

This activity showcased the use of Google App Scripts for data analysis and automation in Google Sheets.

**Summary of Activity:**
- Generated data
- Used LLM to generate code
- Executed code on Google App Scripts
- Produced automated results

**📄 Code:**
```javascript
// Highlights scores based on thresholds
function highlightScores() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const range = sheet.getRange("B2:F101");
  const values = range.getValues();
  const backgrounds = [];

  for (let i = 0; i < values.length; i++) {
    const rowColors = [];
    for (let j = 0; j < values[i].length; j++) {
      const score = values[i][j];
      if (score < 40) {
        rowColors.push("#f4cccc");
      } else if (score > 80) {
        rowColors.push("#d9ead3");
      } else {
        rowColors.push(null);
      }
    }
    backgrounds.push(rowColors);
  }
  range.setBackgrounds(backgrounds);
}

// Calculates total and average
function calculateTotalAndAverage() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const startRow = 2;
  const numRows = sheet.getLastRow() - 1;
  const scoreRange = sheet.getRange(`B${startRow}:F${startRow + numRows - 1}`);
  const scores = scoreRange.getValues();

  const totals = [];
  const averages = [];

  for (let i = 0; i < scores.length; i++) {
    const row = scores[i];
    const total = row.reduce((sum, val) => sum + val, 0);
    const average = total / row.length;
    totals.push([total]);
    averages.push([Math.round(average * 100) / 100]);
  }

  sheet.getRange("G1").setValue("Total Marks");
  sheet.getRange("H1").setValue("Average Marks");
  sheet.getRange(`G${startRow}:G${startRow + numRows - 1}`).setValues(totals);
  sheet.getRange(`H${startRow}:H${startRow + numRows - 1}`).setValues(averages);
}

// Flags below-average students
function flagBelowAverageStudents() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const startRow = 2;
  const lastRow = sheet.getLastRow();

  const averageRange = sheet.getRange(`H${startRow}:H${lastRow}`);
  const averages = averageRange.getValues().map(row => row[0]);

  const totalAvg = averages.reduce((sum, val) => sum + val, 0) / averages.length;
  const flags = averages.map(avg => [avg < totalAvg ? "Yes" : ""]);

  sheet.getRange("I1").setValue("Below Average?");
  sheet.getRange(`I${startRow}:I${lastRow}`).setValues(flags);
}
