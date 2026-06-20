# Full Apps Script (`Code.gs`)

## Configuration

```javascript
const SPREADSHEET_ID = "YOUR_SPREADSHEET_ID";
const MAIN_SHEET = "Form Responses 1";

const CLASS_APPROVAL_COL = 17; // Q
const FINAL_APPROVAL_COL = 18; // R
const REQUEST_ID_COL = 23; // W

const ADVISER_EMAIL = "your_email@gmail.com";

const WEB_APP_URL =
"https://script.google.com/macros/s/YOUR_DEPLOYMENT_ID/exec";
```

---

## Form Submit Trigger

```javascript
function onFormSubmit(e) {

  const sheet =
    SpreadsheetApp.openById(SPREADSHEET_ID)
    .getSheetByName(MAIN_SHEET);

  const row = sheet.getLastRow();

  generateRequestID(row);

  sendApprovalEmail(row);

}
```

---

## Generate Request ID

```javascript
function generateRequestID(row) {

  const sheet =
    SpreadsheetApp.openById(SPREADSHEET_ID)
    .getSheetByName(MAIN_SHEET);

  const requestId =
    Utilities.formatDate(
      new Date(),
      Session.getScriptTimeZone(),
      "yyyyMMddHHmmss"
    );

  sheet.getRange(row, REQUEST_ID_COL).setValue(requestId);

}
```

---

## Send Approval Email

```javascript
function sendApprovalEmail(row) {

  const sheet =
    SpreadsheetApp.openById(SPREADSHEET_ID)
    .getSheetByName(MAIN_SHEET);

  const studentName =
    sheet.getRange(row,2).getValue();

  const rollNo =
    sheet.getRange(row,3).getValue();

  const requestId =
    sheet.getRange(row,REQUEST_ID_COL).getValue();

  const approveLink =
    WEB_APP_URL +
    "?action=approve&id=" +
    requestId;

  const rejectLink =
    WEB_APP_URL +
    "?action=reject&id=" +
    requestId;

  const htmlBody = `
  <h2>Leave Approval Request</h2>

  <b>Student :</b> ${studentName}<br>
  <b>Roll No :</b> ${rollNo}<br><br>

  <a href="${approveLink}"
  style="background:green;color:white;padding:12px;text-decoration:none;">
  APPROVE
  </a>

  <br><br>

  <a href="${rejectLink}"
  style="background:red;color:white;padding:12px;text-decoration:none;">
  REJECT
  </a>
  `;

  MailApp.sendEmail({
    to: ADVISER_EMAIL,
    subject: "Leave Approval Request",
    htmlBody: htmlBody
  });

}
```

---

## Web App (Approve / Reject)

```javascript
function doGet(e) {

  const action = e.parameter.action;
  const requestId = e.parameter.id;

  const sheet =
    SpreadsheetApp.openById(SPREADSHEET_ID)
    .getSheetByName(MAIN_SHEET);

  const data =
    sheet.getDataRange().getValues();

  for(let i=1;i<data.length;i++){

    if(data[i][REQUEST_ID_COL-1]==requestId){

      if(action=="approve"){

        sheet.getRange(i+1,CLASS_APPROVAL_COL)
        .setValue("YES");

        sheet.getRange(i+1,FINAL_APPROVAL_COL)
        .setValue("YES");

        sendStudentEmail(i+1);

      }

      if(action=="reject"){

        sheet.getRange(i+1,CLASS_APPROVAL_COL)
        .setValue("NO");

        sheet.getRange(i+1,FINAL_APPROVAL_COL)
        .setValue("NO");

      }

      break;
    }
  }

  return ContentService.createTextOutput(
    "Approval recorded successfully."
  );

}
```

---

## Student Email

```javascript
function sendStudentEmail(row){

  const sheet =
    SpreadsheetApp.openById(SPREADSHEET_ID)
    .getSheetByName(MAIN_SHEET);

  const studentName =
    sheet.getRange(row,2).getValue();

  const rollNo =
    sheet.getRange(row,3).getValue();

  const studentEmail =
    sheet.getRange(row,10).getValue();

  MailApp.sendEmail(
    studentEmail,
    "Leave Approved",
    "Dear " +
    studentName +
    ",\n\nYour Leave / OD request has been approved.\n\nRegister Number : " +
    rollNo
  );

}
```

---

## Local Approval Sheets

```javascript
function onEdit(e){

  if(!e) return;

  const sheet = e.range.getSheet();
  const name = sheet.getName();

  if(
     name!="RK_Deebika_Approve" &&
     name!="SRIBHARATHI_APPROVE"
  ) return;

  if(e.range.getColumn()!=16) return;

  const approval = e.value;

  if(approval!="YES" && approval!="NO") return;

  // Update main sheet logic here
}
```

---

## Weekly Report

```javascript
function weeklyReport(){

  MailApp.sendEmail(
    "advisor@gmail.com",
    "Weekly Leave Report",
    "Attached weekly report."
  );

}
```

---

## Trigger Setup

### Form Submit Trigger

```text
Function : onFormSubmit
Source   : From Spreadsheet
Event    : On Form Submit
```

### Weekly Trigger

```text
Function : weeklyReport
Source   : Time Driven
Schedule : Every Sunday
```

---

## Autocrat Condition

```text
Final Class Adviser Approval = YES
```

Then:

```text
Generate PDF
↓
Send PDF to Student Email
```

---

# Workflow

```text
Student Form
      ↓
Request_ID Generated
      ↓
Email to Adviser
(APPROVE / REJECT)
      ↓
Web App
      ↓
Q = YES
R = YES
      ↓
Student Email
      ↓
Autocrat PDF
      ↓
Weekly Reports
```
