# TS Apprentice Interview

## Overview

TS Apprentice Interview is a Power Apps solution designed to conduct final Ride & Show apprentice evaluations through a structured, standardized interview process.

The application combines randomized technical questioning, interviewer scoring, write-in question support, placement recommendations, venue recommendations, and automated PDF reporting into a single interview workflow.

The system is intended for Technical Services trainers, supervisors, and interview panels responsible for apprentice placement and qualification decisions.

---

## Features

- Randomized interview question selection
- Multi-category interview configuration
- Automated interview scoring
- Dynamic question refresh logic
- Bonus question functionality
- Multiple interviewer support
- Write-in question capability
- Placement evaluation workflow
- TAP recommendation workflow
- Venue recommendation workflow
- Session-based tracking
- SharePoint-based reporting
- Automatic PDF generation
- Email distribution workflow

---

## Architecture

The system is built using Power Apps, SharePoint, and Power Automate.

```text
Power Apps
    ↓
SharePoint Lists
    ↓
Interview Session Tracking
    ↓
Scoring & Recommendations
    ↓
PDF Generation
    ↓
Email Delivery
```

### Core Components

#### Power Apps

Primary interview application.

#### SharePoint Lists

Data storage layer.

#### Power Automate

Handles:

- Session creation
- PDF generation
- Email distribution

---

## Interview Categories

The application supports the following interview categories:

- General
- Electric
- Mechanical
- Pneumatics
- Pumps
- Hydraulics
- Conveyors/Brakes
- Flames

### Default Categories

Every interview begins with:

```text
General
Mechanical
```

Additional categories are selected before the interview begins.

---

## Interview Logic

### Standard Question Distribution

For every selected category:

```text
2 Category A Questions
1 Category B Question
```

Result:

```text
3 Questions Per Category
```

---

### Interview Target

Standard interview size:

```text
24 Questions
```

Questions are dynamically generated from the question bank based on the categories selected.

---

## Question Distribution Logic

If fewer than 24 questions are generated through category selection, additional questions are assigned using the following compensation priority:

1. Electric
2. Mechanical
3. Pneumatics
4. Pumps
5. Hydraulics
6. Conveyors/Brakes
7. Flames

### Rule

```text
General never receives compensation questions.
```

---

## Refresh Logic

The Refresh function rebuilds only unanswered questions.

### Rules

- Preserve graded questions
- Preserve question category
- Preserve question type (A or B)
- Randomize replacement questions
- Prevent duplicate questions
- Maintain interview distribution
- Maintain compensation allocation
- Rebuild only ungraded questions

---

## Bonus Question Logic

The application includes a single-use bonus question feature.

### If Electric Is Selected

Add:

```text
2 Mechanical Category A Questions
3 Mechanical Category B Questions
```

### If Electric Is Not Selected

Add:

```text
Electric Category
2 Electric Category A Questions
3 Electric Category B Questions
```

### Bonus Rules

- Can only be used once
- Refresh becomes disabled after use
- Added questions immediately become part of the interview

---

## Scoring Logic

Interviewers score each question as:

```text
Correct
Incorrect
```

The application tracks:

- Total Correct
- Total Incorrect
- Percent Correct

### Final Result

```text
Pass  = 80% or Greater
Fail  = Below 80%
```

---

## Write-In Questions

Interviewers can create custom interview questions during any interview session.

Each write-in question records:

- Question
- Answer
- Category
- Result
- Interviewer
- Date
- Session ID

Write-in questions are stored separately for reporting and auditing purposes.

---

## Placement Review

After interview completion, placement review questions are presented.

Examples:

- Work 3rd Shift?
- Work at Heights?
- Become Rappel Certified?
- Work in Confined Spaces?
- Pass a Swim Test?
- Come in Contact with Chlorinated Water?
- Work Outside?

These responses become part of the final recommendation package.

---

## Recommendations

### TAP Recommendation

Stores:

- Recommendation
- Notes

### Role Recommendation

Stores:

- Recommended Role
- Notes

### Venue Preferences

Stores:

- First Choice Venue
- Second Choice Venue
- Third Choice Venue

---

## Reporting

Upon completion:

- Interview results are recorded
- Session records are updated
- Scoring is calculated
- Recommendations are captured
- PDF report is generated
- Email workflow is triggered

---

## Screens

### scrMaintenance

Administrative access and maintenance mode.

### scrHome

Application landing screen.

### scrInfo

Apprentice information, interviewer information, and category selection.

### scrON

Primary interview screen.

Functions include:

- Question generation
- Category navigation
- Grading
- Refresh logic
- Bonus questions
- Write-in questions
- Progress tracking

### scrReview

Final interview review and recommendation workflow.

---

## SharePoint Lists

### AP_INT

Question Bank

#### Columns

| Column | Purpose |
|----------|----------|
| Question | Interview Question |
| Answer | Expected Answer |
| Cat | Category |
| Sub | A / B Classification |
| Orig | Original Source Reference |

---

### Int_Data

Interview Session Storage

Stores:

- Apprentice Information
- Interviewers
- Session IDs
- Results
- Total Correct
- Total Incorrect

---

### ApprWI_Questions

Write-In Question Repository

Stores interviewer-generated questions used during interviews.

---

## Power Automate Flows

### IntSessionPatch

Creates and initializes interview session records.

### Appr_PDF_Email

Generates PDF reports and distributes them through email.

---

## Solution Information

### Solution Name

```text
TSApprenticeInterview
```

### Publisher Prefix

```text
pqs
```

---

## Repository Structure

```text
TS-Apprentice-Interview
│
├── README.md
├── CHANGELOG.md
│
├── src
│   ├── CanvasApps
│   └── Other
│
└── canvas-source
    ├── Assets
    ├── Connections
    ├── DataSources
    ├── Other
    ├── Src
    └── pkgs
```

---

## Development Workflow (GitHub)

This project uses GitHub for Power Apps source control.

### Export and Update Process

```powershell
pac solution export --name TSApprenticeInterview --path solution.zip --managed false --overwrite

pac solution unpack --zipfile solution.zip --folder src --packagetype Unmanaged

pac canvas unpack --msapp src\CanvasApps\pqs_tsapprenticeinterview_1ff86_DocumentUri.msapp --sources canvas-source

git add .
git commit -m "Describe change"
git push
```

---

## Key Source Files

Primary Power Fx source code is stored in:

```text
canvas-source/Src
```

### Important Files

```text
App.fx.yaml
scrHome.fx.yaml
scrInfo.fx.yaml
scrMaintenance.fx.yaml
scrON.fx.yaml
scrReview.fx.yaml
```

These files contain the application's business logic, interview generation logic, scoring logic, refresh functionality, recommendation workflow, and reporting functions.

---

## Future Enhancements

Planned enhancements include:

- Enhanced analytics
- Historical reporting dashboard
- Interview trend analysis
- Placement outcome tracking
- Additional recommendation workflows
- Expanded export options

---

## CHANGELOG

All notable changes are tracked in:

```text
CHANGELOG.md
```