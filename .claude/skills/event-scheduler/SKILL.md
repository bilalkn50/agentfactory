---
name: event-scheduler
description: Schedule and manage events by storing them in markdown files organized by date. Use when users want to add events, appointments, or reminders. Triggers on requests like "schedule an event", "add event on [date]", "create appointment", or when given event name, date, and description.
---

# Event Scheduler

Manages events by storing them in markdown files organized by date. Each date gets its own file, and multiple events on the same date are consolidated in a single file.

## Workflow

1. **Collect event details** - Get event name, date, and description from user
2. **Validate date** - Parse and validate the date format
3. **Check existing file** - Look for existing file with that date
4. **Create or update** - Either create new file or append to existing
5. **Confirm to user** - Report what was saved and where

## Required Information

Ask the user for these details if not provided:

| Field | Format | Example |
|-------|--------|---------|
| **Event Name** | Text | "Team Meeting" |
| **Date** | YYYY-MM-DD | "2026-01-15" |
| **Description** | Text | "Weekly sync with the development team" |

## Date Handling

- Accept dates in various formats: `YYYY-MM-DD`, `DD/MM/YYYY`, `Month DD, YYYY`
- Convert all dates to `YYYY-MM-DD` format for filenames
- For relative dates like "tomorrow" or "next Monday", calculate the actual date

## File Structure

**Output location**: `.claude/skills/event-scheduler/output/`

**Filename format**: `YYYY-MM-DD.md` (based on event date)

**File template** (for new files):

```markdown
# Events for YYYY-MM-DD

## [Event Name]
**Time**: [If provided, otherwise omit]
**Description**: [Event description]

---
```

**Appending to existing files**:

When a file for that date already exists, append the new event:

```markdown
## [New Event Name]
**Time**: [If provided, otherwise omit]
**Description**: [New event description]

---
```

## Processing Steps

### Step 1: Parse User Input
- Extract event name, date, and description from user message
- If any required field is missing, ask the user using AskUserQuestion tool

### Step 2: Validate and Format Date
- Convert date to YYYY-MM-DD format
- Validate it's a real date

### Step 3: Check for Existing File
- Look for file at `.claude/skills/event-scheduler/output/YYYY-MM-DD.md`
- Read file contents if it exists

### Step 4: Create or Update File
**If file does not exist**:
1. Create new file with header and event

**If file exists**:
1. Read existing content
2. Append new event section before the final `---`

### Step 5: Confirm to User
- Report the event was saved
- Show the file path
- If file was updated, mention other events on that date

## Example Interactions

**User**: "Schedule a meeting with John on 2026-01-20 to discuss project timeline"

**Action**:
1. Parse: Name="Meeting with John", Date="2026-01-20", Description="Discuss project timeline"
2. Check: No existing file for 2026-01-20
3. Create: `.claude/skills/event-scheduler/output/2026-01-20.md`
4. Confirm: "Event 'Meeting with John' scheduled for 2026-01-20"

**User**: "Add dentist appointment on 2026-01-20 at 3pm - annual checkup"

**Action**:
1. Parse: Name="Dentist appointment", Date="2026-01-20", Time="3pm", Description="Annual checkup"
2. Check: File exists for 2026-01-20 (has "Meeting with John")
3. Update: Append new event to existing file
4. Confirm: "Event 'Dentist appointment' added to 2026-01-20 (you now have 2 events on this date)"

## Edge Cases

- **Past dates**: Allow scheduling for past dates (for record-keeping)
- **Duplicate events**: Allow multiple events with same name on same date
- **Missing description**: Use event name as description if not provided
- **Time zones**: Treat all dates as local time

## Output Confirmation

Always confirm with the user:
- Event name that was saved
- Date the event is scheduled for
- File path where it was saved
- Number of total events on that date (if more than one)
