# Agent Factory Skills

A collection of Claude Code skills for automating repetitive meeting and scheduling workflows.

## Demo

[![Watch the demo](https://img.shields.io/badge/Loom-Watch%20Demo-blueviolet)](https://www.loom.com/share/e0d244f28b00459e88fea846d50e856f)

[▶️ Watch the video walkthrough](https://www.loom.com/share/e0d244f28b00459e88fea846d50e856f)

## Skills Overview

| Skill | Replaces | Time Saved | Quality Improvement |
|-------|----------|------------|---------------------|
| [teams-transcript-summarizer](#teams-transcript-summarizer) | Manual meeting note-taking | 15-30 min/meeting | Consistent structure, no missed details |
| [meeting-email-composer](#meeting-email-composer) | Writing follow-up emails from scratch | 10-15 min/email | Professional formatting, complete action items |
| [event-scheduler](#event-scheduler) | Calendar apps, manual tracking | 2-5 min/event | Organized markdown files, date consolidation |

---

## teams-transcript-summarizer

**What it replaces:** Manually reviewing meeting recordings or transcripts, taking notes, and organizing them into a coherent summary.

**Before:** Watch/read entire transcript, identify key points, format notes, track who said what, list action items manually.

**After:** Paste transcript or provide file path, receive structured summary instantly.

**Time saved:** 15-30 minutes per meeting

**Quality improvements:**
- Standardized format for all meeting summaries
- Automatic extraction of decisions, action items, and owners
- Participation tracking across attendees
- No risk of missing important details during manual review

---

## meeting-email-composer

**What it replaces:** Manually drafting follow-up emails after meetings, copying/pasting relevant information, formatting action items.

**Before:** Open meeting notes, copy key points, write email intro/outro, format action items, add recipients, proofread.

**After:** Point to a meeting summary file, provide recipient details, get a ready-to-send email.

**Time saved:** 10-15 minutes per email

**Quality improvements:**
- Professional, consistent email tone
- All action items properly attributed with owners and due dates
- No forgotten follow-up items
- Proper formatting for readability

---

## event-scheduler

**What it replaces:** Manual calendar management, sticky notes, scattered reminders, or opening calendar apps for quick scheduling.

**Before:** Open calendar app, navigate to date, create event, fill in details, save.

**After:** Natural language request creates organized markdown files automatically.

**Time saved:** 2-5 minutes per event

**Quality improvements:**
- All events stored in searchable markdown format
- Multiple events per day consolidated in single file
- Flexible date input (natural language supported)
- Version-controlled event history

---

## Usage

Invoke skills using slash commands:

```
/teams-transcript-summarizer   # Summarize a meeting transcript
/meeting-email-composer        # Create follow-up email from summary
/event-scheduler               # Schedule an event
```

Or trigger naturally by describing your need:
- "Summarize this meeting transcript"
- "Email this meeting summary to the team"
- "Schedule a meeting with John on Friday"
