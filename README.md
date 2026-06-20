# 🎓 Leave / OD Approval Automation System

> A complete Google Forms + Google Sheets + Apps Script + Autocrat based Leave/OD Approval System with Email Approval Buttons and Automatic PDF Generation.

---

# 📌 Project Overview

This system automates the complete leave approval process:

- Student fills Google Form
- Request stored in Google Sheet
- Unique Request_ID generated
- Email sent to Adviser
- Adviser clicks APPROVE / REJECT button
- Sheet updates automatically
- Final approval generated
- PDF Leave Letter created using Autocrat
- PDF sent to Student Email

---

# 🚀 Features

✅ Student Leave/OD Form

✅ Email Approval Button

✅ Adviser Approval Automation

✅ Unique Request ID

✅ Google Apps Script Automation

✅ Final Approval Tracking

✅ PDF Generation using Autocrat

✅ Automatic Email to Student

✅ Weekly Report Support

✅ Parent Notification Support

✅ WhatsApp Integration (Optional)

---

# 🏗 System Architecture

```text
Student
    │
    ▼
Google Form
    │
    ▼
Google Sheet (Form Responses)
    │
    ▼
Generate Request_ID
    │
    ▼
Apps Script
    │
    ▼
Email to Adviser
(APPROVE / REJECT)
    │
    ▼
Web App
(doGet)
    │
    ▼
Google Sheet Updates
    │
    ▼
Final Approval = YES
    │
    ▼
Autocrat
    │
    ▼
Generate PDF
    │
    ▼
Send Email to Student
```

---

# 📂 Project Structure

```
leave-od-approval-system
│
├── README.md
├── AppsScript
│     ├── Code.gs
│
├── Templates
│     ├── Leave_Template.docx
│
├── Images
│     ├── flowchart.png
│     ├── architecture.png
│
└── Documents
      ├── Project_Report.pdf
```

---

# 📑 Google Form Fields

- Student Name
- Register Number
- Department
- Year
- Semester
- Leave Type
- Date From
- Date To
- Reason
- Parent Mobile Number
- Class Adviser
- Mentor

---

# 📊 Google Sheet Columns

| Column | Description |
|----------|------------|
| A | Timestamp |
| B | Student Name |
| C | Register Number |
| D | Department |
| E | Year |
| F | Semester |
| G | Leave Type |
| H | Date From |
| I | Date To |
| J | Student Email |
| K | Parent Mobile Number |
| O | Class Adviser |
| P | Local Approval |
| Q | Class Adviser Approval |
| R | Final Class Adviser Approval |
| S | Approval Date |
| W | Request_ID |

---

# 🔐 Request ID System

Example:

```text
20260309115216
```

Used for:

- Secure Approval
- Prevent Wrong Row Updates
- Reliable Email Buttons

---

# ⚙ Workflow

```text
Student fills form
        ↓
Response stored
        ↓
Request_ID generated
        ↓
Email sent to Adviser
        ↓
APPROVE / REJECT button
        ↓
Web App updates Sheet
        ↓
Q = YES
R = YES
        ↓
Final Approval
        ↓
Autocrat PDF
        ↓
Email sent to Student
```

---

# 📧 Approval Email

Subject:

```
Leave Approval Request
```

Body:

```
Student : Arun

Roll No : 21CS045

[APPROVE]

[REJECT]
```

---

# 🟢 APPROVE Button

```text
https://script.google.com/macros/s/XXXXX/exec?action=approve&id=Request_ID
```

---

# 🔴 REJECT Button

```text
https://script.google.com/macros/s/XXXXX/exec?action=reject&id=Request_ID
```

---

# ⚙ Apps Script Functions

## onFormSubmit()

```javascript
function onFormSubmit(e){
  const sheet =
  SpreadsheetApp.getActiveSpreadsheet()
  .getSheetByName("Form Responses 1");

  const row = sheet.getLastRow();

  sendApprovalEmail(row);
}
```

---

## sendApprovalEmail()

- Sends email to adviser.
- Creates APPROVE and REJECT buttons.
- Uses Request_ID.

---

## doGet()

- Triggered when button is clicked.
- Searches Request_ID.
- Updates:

```text
Q = YES/NO
R = YES/NO
```

---

## onEdit()

Used for:

- RK_Deebika_Approve sheet
- SRIBHARATHI_APPROVE sheet

Updates:

```text
RK Approval
SR Approval
Final Approval
Approval Date
```

---

# 📄 Autocrat Configuration

### Merge Condition

```text
Final Class Adviser Approval = YES
```

---

### Output Type

```text
PDF
```

---

### Email To

```text
Student Email
```

---

# 📅 Weekly Report

Separate reports for:

### RK Deebika

```
RK_Weekly_Report
```

### Sribharathi Natrayan

```
SR_Weekly_Report
```

Contains:

- Student Name
- Roll Number
- Mentor Approval
- Final Approval

Automatically emailed weekly.

---

# 📲 WhatsApp Integration (Optional)

After Form Submission:

Student receives:

- Mentor Group Link
- WhatsApp Message Link

Example:

```text
New Leave / OD Request

Student Name:
Register No:

Please approve.
```

---

# 📞 Parent Notification (Optional)

Can use:

- Twilio
- Exotel
- SMS API

For:

- Leave Approved
- OD Approved

---

# 📈 Future Improvements

- Mentor Email Approval
- Parent Email Notification
- WhatsApp API
- Dashboard Analytics
- QR Code Approval
- Mobile App
- AI Chatbot Support

---

# 🛠 Technologies Used

- Google Forms
- Google Sheets
- Google Apps Script
- Gmail
- Autocrat
- Google Drive

---

# 📊 Flowchart

```text
Student
   ↓
Google Form
   ↓
Google Sheet
   ↓
Request_ID
   ↓
Email Approval
   ↓
Web App
   ↓
Sheet Update
   ↓
Final Approval
   ↓
Autocrat
   ↓
PDF
   ↓
Student Email
```

---

# 👨‍💻 Author

### Chandru P

AI & Data Science

M. Kumarasamy College of Engineering

GitHub:

```text
https://github.com/chandru07072007
```

---

# ⭐ Repository Name


---
