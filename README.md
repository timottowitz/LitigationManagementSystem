# LitigationManagementSystem

AI Studio Alternative system Development
https://aistudio.google.com/prompts/19p358mV-jqkkmVoXgFm7NZWPm7rfAPTE

03 Alterantive System
https://chatgpt.com/canvas/shared/680fb870d91c8191b4af8e3c92dadd47

Litigation Dashboard – Final Specification

1  Purpose

Provide a single, end‑to‑end blueprint that a developer can follow to build the Litigation Command Center in Airtable (Interface Designer), wire all automations, and apply styling/permissions—all from a clean base already seeded with the four CSV imports (Cases, Deadlines, Discovery, Motions & Hearings).

2  Data Model (post‑import)

Table

Key Field

Records

Core Fields

Cases

Case ID

 62

Client Name, Case Type, Phase, Venue, Status, Next Due Date & Type, Next Notes, Case Health (formula), Days Until Next Deadline (formula), Discovery Completion % (formula)

Deadlines

Deadline ID

 32

Case (Linked), Deadline Name, Deadline Type, Deadline Date, Status, Notes, Deadline Status (formula), Deadline Age (formula), Compliance Status (formula)

Discovery

Discoverable ID

 46

Case (Linked), Item, Status

Motions & Hearings

Motion/Hearing ID

 34

Case (Linked), Type, Scheduled Date, Status

2.1 Required Formula Fields (copy‑paste ready)

Cases → Case Health

IF(
  OR(
    COUNT(FILTER(Deadlines, AND(IS_BEFORE({Deadline Date}, DATEADD(TODAY(), 7, 'days')), NOT(IS_BEFORE({Deadline Date}, TODAY())))))>0,
    COUNT(FILTER({Missing Pleadings}, NOT(BLANK({Missing Pleadings}))))>0
  ),
  "RED",
  IF(
    COUNT(FILTER(Deadlines, AND(IS_BEFORE({Deadline Date}, DATEADD(TODAY(), 30, 'days')), NOT(IS_BEFORE({Deadline Date}, TODAY())))))>0,
    "YELLOW",
    "GREEN"
  )
)

Cases → Days Until Next Deadline

MIN(
  ARRAYJOIN(
    VALUES(FILTER(Deadlines, IS_AFTER({Deadline Date}, TODAY()))),
    "Deadline Date"
  )
) - TODAY()

Deadlines → Deadline Status

IF(IS_BEFORE({Deadline Date}, TODAY()),
  "Past Due",
  IF(IS_BEFORE({Deadline Date}, DATEADD(TODAY(), 7, 'days')),
     "Due This Week",
     IF(IS_BEFORE({Deadline Date}, DATEADD(TODAY(), 30, 'days')),
        "Due This Month",
        "Upcoming")))

Deadlines → Deadline Age

IF(IS_BEFORE({Deadline Date}, TODAY()),
  "Overdue by " & TODAY()-{Deadline Date} & " days",
  "Due in " & {Deadline Date}-TODAY() & " days")

Deadlines → Compliance Status

IF({Status}="Complete","Completed",
  IF(IS_BEFORE({Deadline Date}, TODAY()),"Missed","Upcoming"))

Cases → Discovery Completion %

IF(COUNT(FILTER(Discovery,{Case}=RECORD_ID()))=0,0,
  ROUND(
    COUNT(FILTER(Discovery,AND({Case}=RECORD_ID(),{Status}="Complete"))) /
    COUNT(FILTER(Discovery,{Case}=RECORD_ID())) * 100,0))

3  Interface Structure (4‑page stack)

3.1 Overview (Landing)

Component

Source

Config

Header

—

Logo + title “Litigation Case Management” + subtitle

Stats Row

Cases & Deadlines

• Total Active Cases  • Red Status Cases  • Deadlines This Week  • Avg Discovery Complete

Urgent Matters List

Deadlines

Filter Deadline Status ∈ {Past Due, Due This Week} → sort by Deadline Date asc, show 5, expandable

Deadline Distribution Chart

Deadlines

Bar (x = week, y = # deadlines)

Case Health Pie/Bar

Cases

Group by Case Health

Active Cases Table

Cases

Filter Status = Active, sort Case Health (RED→GREEN), visible cols: Case Name, Phase, Next Due Date, Case Health, Days Until Next Deadline; row color = Case Health; record click → Case Detail

3.2 Case Detail (Record‑scoped)

Header – Case Name, Phase, Case Health (color badge)

Meta – Client Name, Venue, Case Type, Status

Timeline – All Deadlines (Gantt or chronological list with status colors)

Discovery Progress – Percent bar + table of discovery items

Motions & Hearings – Table grouped by Status (Pending, Scheduled, Complete)

Notes & History – Long‑text feed (allow new note append)

3.3 Compliance Dashboard

Stat – Overall Compliance % (# Completed / Total)

Bar Chart – Deadlines grouped by Compliance Status

Compliance Table – Deadlines grouped by Compliance Status, sort by Deadline Date, conditional row colors

Weekly Trend – Line chart: completion rate by ISO week

Missed List – Filter Compliance Status = Missed, quick‑action checkbox Mark Resolved

3.4 Performance Dashboard

Phase Distribution – Cases by Phase (stacked bar)

Discovery Completion – Horizontal bar per case

Time Series –

Deadline compliance % over last 12 months

Case resolution rate (closed per quarter)

Avg days in each phase

Workload Split – Pie charts by Client, Attorney, Venue

4  Interactive Controls (Global)

Search – Full‑text across Cases (Name & Client)

Filters – Phase, Case Type, Venue, Date range (Deadline Date)

Quick Buttons

Show Red Cases (adds filter)

This Week’s Deadlines

Discovery Status (opens Performance Dashboard → Discovery section)

Add New Case (opens form)

Export – “Print / Save PDF” action for any dashboard page

5  Styling Guidelines

Usage

Color

Urgent / RED

#FF5A5F

Warning / YELLOW

#FFC107

Healthy / GREEN

#46B29D

Headers / Nav

#324A5F

All cards: rounded-2xl shadow-md p-4 (approx.)

Responsive: two‑column grid ≥ 1024 px, single‑column on mobile; Stats row collapses into 2×2 grid under 640 px.

6  Automations

Name

Trigger

Logic

Action

Daily Deadline Alert

Schedule: daily 07:00 MX CDT

Deadlines where Deadline Date ∈ [Today, Today+7] ∧ Status≠Complete

Email (to you+ Alyson) with bullets list

Critical 24 h Warning

daily 08:00

Deadlines Deadline Date = Tomorrow ∧ Status≠Complete

Email ⚠️ urgent flag

Weekly Overview

weekly Mon 09:00

Active Cases summary + 30‑day deadlines

Email summary report

Past Due Alert

daily 09:00

Deadlines Deadline Date < Today ∧ Status≠Complete

Email escalation

Case Health → RED

When Cases Case Health changes to RED

Immediate notify

Email/SMS via Twilio

Data QA Check

weekly Fri 16:00

Records missing required fields

Email checklist

7  Permissions & Refresh

Roles

Attorneys – read/write on Cases they own

Staff – write Deadlines/Discovery/Motions

Management – full access

Interface access – View‑only for externals, editable for internal roles.

Data refresh – Interface auto‑refresh every 24 h (setting inside Interface Designer).

8  Implementation Checklist

Import the four CSVs (map ID fields correctly).

Create formula fields (Section 2.1).

Design the four Interface pages per Section 3.

Apply global styling (Section 5).

Wire Automations (Section 6).

Set permissions & sharing (Section 7).

Test with sample workflows, iterate.

Go Live; schedule monthly review.
