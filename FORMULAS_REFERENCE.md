# FORMULAS_REFERENCE.md

# TS Apprentice Interview - Formula Reference

This document serves as the technical reference for key application logic used within the TS Apprentice Interview Power App.

The purpose of this document is to provide a centralized location for business rules, formulas, and application behavior so future updates can be made without reverse-engineering the app.

---

# Global Variables

## App Start

```powerfx
Set(varGeneralQuestions, 4);
Set(varCategoryQuestions, 6);
```

### Purpose

Defines base question allocation values used during interview generation.

---

# Session Creation

## Session Initialization

Triggered from:

```text
scrInfo
btnStart
```

Creates:

```text
varIntSessionID
varIntID
```

via:

```powerfx
IntSessionPatch.Run(...)
```

### Purpose

Creates the interview record and returns the SharePoint session identifier used throughout the interview process.

---

# Question Bank Logic

## Data Source

```text
AP_INT
```

Required columns:

```text
Question
Answer
Cat
Sub
```

### Category Values

```text
General
Electric
Mechanical
Pneumatics
Pumps
Hydraulics
Conveyors/Brakes
Flames
```

### Sub Values

```text
A
B
```

---

# Standard Interview Distribution

## Base Allocation

Each selected category receives:

```text
2 Category A Questions
1 Category B Question
```

### Formula Rule

```text
A = 2
B = 1
Total = 3 Questions
```

---

# Interview Target

Total Interview Size:

```text
24 Questions
```

Questions are dynamically expanded using compensation logic when fewer than 24 questions are generated.

---

# Compensation Logic

When total generated questions are below 24:

```powerfx
varMissingQuestions
```

calculates the difference.

### Compensation Priority

```text
1. Electric
2. Mechanical
3. Pneumatics
4. Pumps
5. Hydraulics
6. Conveyors/Brakes
7. Flames
```

### Rule

```text
General never receives compensation questions.
```

---

# Refresh Logic

## Purpose

Refresh replaces only unanswered questions while preserving interview integrity.

Triggered by:

```text
btnRefreshInt
```

---

## Preserve Graded Questions

```powerfx
ClearCollect(
    colInterviewQuestions,
    Filter(
        colInterviewQuestions,
        ID in colQGrade.QuestionID
    )
)
```

### Result

Refresh only rebuilds:

```text
Ungraded Questions
```

---

## Refresh Rules

### Preserve

```text
Question Category
Question Type (A/B)
Graded Questions
Compensation Allocation
```

### Prevent

```text
Duplicate Questions
Category Rebalancing
A/B Mismatch
```

---

## Replacement Logic

```text
A → A
B → B
```

Question types are never crossed during refresh.

---

# Bonus Question Logic

Triggered by:

```text
Button1
```

---

## Scenario 1

### Electric Selected

Add:

```text
2 Mechanical A Questions
3 Mechanical B Questions
```

Variables:

```powerfx
Set(varBonusCat,"Mechanical");
Set(varBonusA,2);
Set(varBonusB,3);
```

---

## Scenario 2

### Electric Not Selected

Add category:

```text
Electric
```

Add:

```text
2 Electric A Questions
3 Electric B Questions
```

Variables:

```powerfx
Set(varBonusCat,"Electric");
Set(varBonusA,2);
Set(varBonusB,3);
```

---

## Bonus Rules

After activation:

```powerfx
Set(varBonusUsed,true)
```

Result:

```text
Bonus can only be used once.
Refresh becomes disabled.
```

Notification:

```powerfx
Notify(
    varBonusCat & " Questions Added - Refresh Disabled",
    NotificationType.Information
)
```

---

# Question Grading

## Correct

Stored as:

```powerfx
IsCorrect = "Correct"
```

Collection:

```text
colQGrade
```

---

## Incorrect

Stored as:

```powerfx
IsCorrect = "Incorrect"
```

Collection:

```text
colQGrade
```

---

# Interview Completion Logic

Triggered by:

```text
btnCompleted
```

---

## Minimum Answer Threshold

Display logic:

```text
20 Questions Answered
```

If fewer than 20:

```text
Minimum not met
```

displayed on completion button.

---

# Score Calculation

## Percent Correct

```powerfx
Correct Questions
÷
Total Answered Questions
×
100
```

Stored in:

```text
varCorrect
```

---

# Pass/Fail Logic

```powerfx
If(
    varCorrect >= 80,
    "Passed",
    "Failed"
)
```

---

## Threshold

```text
Pass  = 80% or Higher
Fail  = Below 80%
```

Stored in:

```text
varIntStatus
```

---

# Write-In Questions

Triggered by:

```text
btnWriteInQuestion
```

Stored in:

```text
ApprWI_Questions
```

and

```text
colWIQGrade
```

---

## Required Fields

```text
Question
Answer
Category
```

Validation prevents:

```text
Blank Question
Blank Answer
Invalid Category
```

---

## Write-In Result Values

```text
Correct
Incorrect
```

---

# Placement Workflow

## Placement Questions

Generated after completion.

Current question set:

```text
Work 3rd Shift?
Work at Heights?
Become Rappel Certified?
Work in Confined Spaces?
Pass Swim Test?
Come in Contact with Chlorinated Water?
Work Outside?
```

Stored in:

```text
colPlacementQuestions
```

---

# Interview Recommendations

## TAP Recommendation

Controls:

```text
rdTAP
txtTAPNotes
```

---

## Recommended Role

Controls:

```text
rdRole
txtRoleNotes
```

---

## Venue Recommendations

Controls:

```text
txtVenue1
txtVenue2
txtVenue3
```

Stores:

```text
Top 3 Venue Choices
```

---

# SharePoint Lists

## AP_INT

Primary Interview Question Bank.

Purpose:

```text
Question Generation
```

---

## Int_Data

Primary Interview Session Table.

Stores:

```text
Interviewers
Apprentice
Results
Totals
Status
Session Information
```

---

## ApprWI_Questions

Write-In Question Repository.

Stores:

```text
Write-In Questions
Write-In Results
Interviewer Information
Session ID
```

---

# Power Automate Flows

## IntSessionPatch

Purpose:

```text
Create Interview Session
Return Session ID
```

---

## Appr_PDF_Email

Purpose:

```text
Generate PDF
Email PDF
Archive Report
```

---

# Key Collections

## colSelectedCats

Stores:

```text
Selected Interview Categories
```

---

## colInterviewQuestions

Stores:

```text
Generated Interview Questions
```

---

## colQGrade

Stores:

```text
Interview Question Results
```

---

## colWIQGrade

Stores:

```text
Write-In Question Results
```

---

## colPlacementQuestions

Stores:

```text
Placement Review Questions
```

---

## colPlacementAnswers

Stores:

```text
Placement Responses
```
