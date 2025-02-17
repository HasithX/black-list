# black-list
jgjyfhj
Here's a step-by-step solution using Google Apps Script to display detailed customer information in a sidebar when clicking a "View Details" button in your summary sheet:

### 1. Set Up Your Sheets

**Sheet1 (Summary):**
```
| Customer ID | Name    | Account Name | Case | View Details |
|-------------|---------|--------------|------|--------------|
| 001         | John    | Account A    | Open | [Button]     |
```

**Sheet2 (Details):**
```
| Customer ID | Photo URL          | Account Photo URL | Case Photo URL | KYC Status |
|-------------|--------------------|--------------------|----------------|------------|
| 001         | https://image1.jpg | https://image2.jpg | https://doc.pdf| Verified   |
```

### 2. Create the View Details Buttons

1. In Sheet1, insert drawings (buttons) in the "View Details" column
2. Assign this script to each button: `showDetails()`

### 3. Apps Script Code

```javascript
function showDetails() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const ui = SpreadsheetApp.getUi();
  const sheet1 = ss.getSheetByName("Sheet1");
  const sheet2 = ss.getSheetByName("Sheet2");
  
  // Get clicked row data
  const activeCell = sheet1.getActiveCell();
  const row = activeCell.getRow();
  const customerId = sheet1.getRange(row, 1).getValue();

  // Get details from Sheet2
  const detailsData = sheet2.getDataRange().getValues();
  const customerDetails = detailsData.find(r => r[0] === customerId);

  // Create HTML template
  const htmlOutput = HtmlService.createHtmlOutput(`
    <div style="padding:20px">
      <h3>Customer Details for ID: ${customerId}</h3>
      <div style="display:grid; grid-template-columns: 1fr 1fr; gap: 20px;">
        <div>
          <h4>Customer Photo</h4>
          <img src="${customerDetails[1]}" style="width:200px;height:auto;">
        </div>
        <div>
          <h4>Account Photo</h4>
          <img src="${customerDetails[2]}" style="width:200px;height:auto;">
        </div>
        <div>
          <h4>Case Document</h4>
          <a href="${customerDetails[3]}" target="_blank">View Document</a>
        </div>
        <div>
          <h4>KYC Status</h4>
          <p>${customerDetails[4]}</p>
        </div>
      </div>
      <button onclick="google.script.host.close()" style="margin-top:20px">Close</button>
    </div>
  `).setWidth(800).setHeight(600);

  ui.showSidebar(htmlOutput);
}
```

### 4. Implementation Steps

1. **Prepare Your Data:**
   - Ensure Sheet2 has proper image URLs in the photo columns
   - Use actual image URLs that are publicly accessible

2. **Create Buttons:**
   - In Sheet1, go to Insert > Drawing
   - Create a button shape and save it
   - Right-click the inserted drawing and select "Assign Script"
   - Type `showDetails`

3. **Set Up Script:**
   - Go to Extensions > Apps Script
   - Paste the provided code
   - Save and name your project

4. **Authorization:**
   - When first running, authorize the script following Google's prompts

### 5. Usage

1. Click any "View Details" button in Sheet1
2. A sidebar will appear showing:
   - Customer photo
   - Account photo
   - Case document link
   - KYC status
3. Click "Close" to dismiss the sidebar

### Features

- Responsive grid layout
- Image previews
- Document links
- Auto-sizing sidebar
- Clean UI design

### Notes

1. For non-public images, use Google Drive share links and convert them to direct URLs
2. Add error handling if Customer ID isn't found
3. Customize the HTML/CSS to match your preferred styling
4. Add loading indicators if images take time to load

This solution provides a professional, user-friendly interface while keeping your data organized across sheets. The sidebar display prevents sheet clutter and offers better visual presentation for media files.
