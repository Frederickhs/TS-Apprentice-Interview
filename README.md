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

## Prerequisites

Before configuring or updating the app, confirm you have:

- Access to the Power Platform environment containing the
  `TSApprenticeInterview` solution
- Power Platform CLI (`pac`) installed and authenticated to that environment
- Access to the SharePoint site and lists used by the app
- Permission to edit and publish the app and its data connections
- Access to configure and test the `IntSessionPatch` and `Appr_PDF_Email`
  Power Automate flows

Connection references, environment variables, SharePoint site URLs, and email
recipients are environment-specific. This repository does not specify their
production values; confirm them with the app owner before deployment.

## Setup and Configuration

1. Confirm the target Power Platform environment and SharePoint site.
2. Verify that the `AP_INT`, `Int_Data`, and `ApprWI_Questions` lists exist and
   that their columns match the app's expectations in this README and
   [FORMULAS_REFERENCE.md](FORMULAS_REFERENCE.md).
3. Review the `AP_INT` question bank for the supported `Cat` and `Sub` values,
   and verify that it has enough questions for the intended interview
   categories.
4. Configure the app's SharePoint connections and the required flow connections
   in the target environment.
5. Confirm that `IntSessionPatch` can create a session and that
   `Appr_PDF_Email` can generate and distribute the report to the intended
   recipients.
6. Test the full interview workflow, including question generation, scoring,
   recommendations, PDF generation, and email delivery, before production use.

Use the organization's Power Platform deployment process for importing or
publishing the solution. Exact environment configuration and deployment steps
are not included in this repository.

## Data Protection

The app uses SharePoint for question-bank data, interview sessions, and write-in
questions, and sends completed reports through an email flow. Apply
environment-appropriate SharePoint and flow permissions, especially because
the `AP_INT` list contains expected answers and `Int_Data` stores apprentice
and interviewer information.

- Restrict question-bank access to people who need to maintain or administer it.
- Grant access to interview records, write-ins, PDFs, and email outputs only to
  authorized users.
- Follow organizational retention and privacy requirements for interview and
  apprentice data.
- Do not put credentials, personal data, or confidential environment details in
  source control or issue reports.

## Deployment and Repository Update

The **Development Workflow (GitHub)** section below contains the Power Platform
CLI commands for exporting the unmanaged solution and unpacking the Canvas App
source. Before using them, confirm `pac` is authenticated to the intended
environment. Review the source diff and test the app and flows before release.

The listed `git add`, `git commit`, and `git push` commands publish repository
changes. Stage only the intended files and follow the team's review and release
process; the commands are instructions and should only be run when a source
update is intended.

## Troubleshooting

- **Questions or categories do not load:** Verify the `AP_INT` connection,
  list access, and that question rows use supported `Cat` and `Sub` values.
- **Interview sessions are not created or updated:** Check the app's connection
  to `Int_Data` and confirm that `IntSessionPatch` is available and configured
  for the target environment.
- **Question count or refresh behavior is unexpected:** Review category
  selection, available questions, and the distribution and refresh rules in
  [FORMULAS_REFERENCE.md](FORMULAS_REFERENCE.md).
- **Scoring or final result is unexpected:** Confirm that questions were
  graded as Correct or Incorrect and review the 80% pass threshold described
  above.
- **PDF or email is missing:** Verify that `Appr_PDF_Email` is enabled and its
  connections and intended recipients are configured. Check the flow's run
  history for the reported failure.

When reporting an issue, include the environment, workflow step, approximate
time, and any displayed error. Do not include credentials or unnecessary
personal information.

## Ownership and Support

This repository does not identify named app, Power Platform, SharePoint, or
flow owners. Before deployment, confirm who is responsible for:

- App and solution ownership
- Question-bank and interview-record maintenance
- Power Automate flow and email configuration
- Production deployment approval and support

Use the team's established support process and include relevant, non-sensitive
error details when requesting help.

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