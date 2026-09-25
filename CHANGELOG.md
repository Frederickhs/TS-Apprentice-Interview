# Changelog

All notable changes to the **TS Apprentice Interview** application are documented in this file.

This project follows a **controlled update model**:

- Logic changes are versioned
- Application behavior changes are documented
- Documentation updates are tracked
- Production releases receive explicit version numbers

---

## [v1.0.0] – 9/24/26 - Initial Stable Release

**Status:** Approved  
**Impact:** Baseline Release

### Added

#### Core Interview Engine

- Randomized interview question selection
- Multi-category interview support
- Category-based interview generation
- Dynamic question allocation
- Session-based interview tracking
- Multi-interviewer support

#### Interview Categories

- General
- Electric
- Mechanical
- Pneumatics
- Pumps
- Hydraulics
- Conveyors/Brakes
- Flames

#### Interview Logic

- General category automatically included
- Mechanical category automatically included
- Dynamic question generation based on selected categories
- Question pools separated by Category A and Category B
- Compensation logic to maintain minimum interview size

#### Standard Interview Rules

Per selected category:

```text
2 Category A Questions
1 Category B Question
```

Interview target:

```text
24 Questions
```

#### Compensation Priority Logic

Additional questions are assigned using:

1. Electric
2. Mechanical
3. Pneumatics
4. Pumps
5. Hydraulics
6. Conveyors/Brakes
7. Flames

#### Refresh Logic

- Refresh affects only ungraded questions
- Preserves graded questions
- Maintains category allocation
- Prevents duplicate questions
- Preserves A-to-A replacement
- Preserves B-to-B replacement
- Maintains compensation logic

#### Bonus Question System

Added single-use bonus functionality:

If Electric is selected:

```text
2 Mechanical A Questions
3 Mechanical B Questions
```

If Electric is not selected:

```text
Add Electric Category
2 Electric A Questions
3 Electric B Questions
```

Additional rules:

- Refresh disabled after use
- Bonus available once per interview

#### Scoring

- Correct grading
- Incorrect grading
- Percent correct calculation
- Automatic pass/fail determination

Pass Threshold:

```text
80% or higher
```

Fail Threshold:

```text
Below 80%
```

#### Write-In Questions

Added support for:

- Custom interviewer questions
- Category assignment
- Correct / Incorrect grading
- Session linkage
- Audit tracking

#### Placement Workflow

Added placement recommendation section including:

- Work 3rd Shift
- Work at Heights
- Become Rappel Certified
- Work in Confined Spaces
- Pass Swim Test
- Chlorinated Water Exposure
- Outdoor Work

#### Recommendations

Added:

##### TAP Recommendation

- Recommendation selection
- Recommendation notes

##### Role Recommendation

- Recommended role
- Supporting notes

##### Venue Selection

- Venue Choice 1
- Venue Choice 2
- Venue Choice 3

#### Reporting

- Interview session records
- Interview pass/fail status
- Correct question totals
- Incorrect question totals
- Interview history storage

#### PDF Generation

Added automated PDF workflow:

- Interview summary
- Question results
- Interview panel information
- Apprentice information
- Recommendations
- Venue preferences

#### Power Automate Integration

Added:

##### IntSessionPatch

Creates interview session records.

##### Appr_PDF_Email

Generates and distributes interview reports.

#### SharePoint Integration

Added support for:

##### AP_INT

Primary interview question bank.

##### Int_Data

Interview session repository.

##### ApprWI_Questions

Write-in question repository.

---

### Screens

Added:

```text
scrMaintenance
scrHome
scrInfo
scrON
scrReview
```

#### Screen Functions

##### scrMaintenance

Administrative access and maintenance mode.

##### scrHome

Application landing page.

##### scrInfo

- Apprentice information
- Interviewer information
- Category selection

##### scrON

- Question generation
- Grading
- Refresh
- Bonus questions
- Write-in questions

##### scrReview

- Interview summary
- Placement review
- Recommendations
- PDF generation

---

### Source Control

Added GitHub repository support.

Power Platform source control includes:

```text
Solution Export
Solution Unpack
Canvas App Unpack
GitHub Version Control
```

---

### Documentation

Created:

```text
README.md
CHANGELOG.md
```

---

> [!IMPORTANT]
>
> This version represents the initial production-ready release of the TS Apprentice Interview application.
>
> All future functionality, bug fixes, workflow updates, reporting changes, and interview logic modifications must be versioned through this changelog.

---

## Version Control

- This repository is the **single source of truth** for TS Apprentice Interview logic.
- Production implementations should reference the repository version in use.
- All revisions must:
  1. Be committed to GitHub
  2. Be documented in this changelog
  3. Be released under an explicit version number

---

### Versioning Explanation

This repository uses **Semantic Versioning**:

```text
MAJOR.MINOR.PATCH
```

### MAJOR Version (1.x.x → 2.x.x)

Reserved for changes that alter core interview behavior.

Examples:

- Pass/fail threshold changes
- Question distribution changes
- Category structure changes
- Recommendation workflow redesign

### MINOR Version (1.1.x)

Reserved for new functionality that does not break existing behavior.

Examples:

- New recommendation workflows
- Additional reports
- New interview screens
- Analytics features
- Additional export options

### PATCH Version (1.0.1)

Reserved for bug fixes and documentation updates.

Examples:

- Formula corrections
- UI fixes
- Text corrections
- Documentation updates
- Logic cleanup without behavior changes
