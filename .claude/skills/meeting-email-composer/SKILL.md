---
name: meeting-email-composer
description: |
  Compose professional emails from teams-transcript-summarizer output files.
  This skill should be used when users want to send meeting summaries via email,
  share meeting notes with stakeholders, or create follow-up emails from meeting transcripts.
  Triggers on requests like "email this meeting summary", "compose email from meeting notes",
  or "send meeting summary to team".
allowed-tools: Read, Write, Bash, AskUserQuestion
---

# Meeting Email Composer

Transforms meeting summary files into professional, ready-to-send emails.

## Workflow

```
1. Get file path → 2. Read content → 3. Collect email details → 4. Generate email → 5. Display output → 6. Save to file
```

### Step 1: Get File Path

Prompt the user for the path to the meeting summary file:

> "Please provide the path to the teams-transcript-summarizer output file you want to convert to an email."

Default location hint: `.claude/skills/teams-transcript-summarizer/output/`

### Step 2: Read Content

Read the specified file and extract key information:
- Meeting title/topic
- Date and attendees
- Key decisions
- Action items
- Discussion topics summary

### Step 3: Collect Email Details

Ask the user for the following information using AskUserQuestion or direct prompts:

| Field | Required | Description |
|-------|----------|-------------|
| To | Yes | Primary recipient email address(es) |
| CC | No | Carbon copy email address(es) |
| BCC | No | Blind carbon copy email address(es) |
| Recipient Name | Yes | Name to use in greeting (e.g., "Team", "John") |
| Sender Name | Yes | Your name for the signature |

**Important**: Never hardcode any names or email addresses. Always collect from user.

### Step 4: Generate Email

Create a professional email with:

#### Subject Line
Generate an appropriate, professional subject based on meeting content:
- Format: `Meeting Summary: [Topic] - [Date]`
- Or: `Action Items from [Meeting Topic] - [Date]`
- Or: `Follow-up: [Meeting Topic]`

#### Email Body Structure

```
Hi [RECIPIENT_NAME],

[Opening paragraph - brief context about the meeting]

[Key Decisions section - if any decisions were made]

[Action Items section - formatted as bullet points with owners]

[Brief summary of main discussion points]

[Closing - next steps or call to action if applicable]

Regards,
[YOUR_NAME]
```

### Step 5: Display Output

Display the complete email in ready-to-send format:

```
═══════════════════════════════════════════════════════════════
                         EMAIL DRAFT
═══════════════════════════════════════════════════════════════

To:      [email addresses]
CC:      [email addresses or "None"]
BCC:     [email addresses or "None"]
Subject: [generated subject line]

───────────────────────────────────────────────────────────────

[Email body here]

═══════════════════════════════════════════════════════════════
```

### Step 6: Save to Output Folder

After displaying the email, save it to the output folder.

#### Output Directory
```
.claude/skills/meeting-email-composer/output/
```

Create this directory if it doesn't exist using Bash: `mkdir -p`.

#### File Naming Convention
```
email_[meeting-topic]_[YYYY-MM-DD]_[HHMMSS].txt
```

- **meeting-topic**: Sanitized meeting topic (lowercase, spaces replaced with hyphens, special characters removed)
- **YYYY-MM-DD**: Date of email generation
- **HHMMSS**: Time of email generation (for uniqueness)

Example: `email_q1-planning-review_2026-01-10_143052.txt`

#### File Content Format
Save the email in plain text format with headers:

```
================================================================================
EMAIL DRAFT - Generated: [timestamp]
================================================================================

To:      [email addresses]
CC:      [email addresses or "None"]
BCC:     [email addresses or "None"]
Subject: [generated subject line]

--------------------------------------------------------------------------------

[Email body here]

================================================================================
Source: [path to original meeting summary file]
================================================================================
```

#### Confirmation Message
After saving, display:
```
Email saved to: .claude/skills/meeting-email-composer/output/[filename]
```

## Email Writing Guidelines

### Tone
- Professional and clear
- Concise - avoid unnecessary words
- Action-oriented where appropriate
- Neutral and factual

### Content Transformation Rules

| Meeting Summary Section | Email Transformation |
|------------------------|---------------------|
| Key Decisions | Bullet list under "Decisions Made:" |
| Action Items | Bullet list with owner names, due dates |
| Discussion Topics | Brief 1-2 sentence summaries of key points |
| Attendee Participation | Omit (not relevant for email) |
| Notable Quotes | Include only if highly relevant |

### Length Guidelines
- Keep email body under 300 words when possible
- Prioritize action items and decisions over discussion details
- Link to full notes if more detail needed: "Full meeting notes available upon request."

## Edge Cases

- **No action items**: Focus on decisions and key takeaways
- **No decisions**: Focus on discussion summary and any pending items
- **Very short meeting summary**: Create a brief, correspondingly short email
- **Multiple recipients**: Use "Hi Team," or "Hi All," as appropriate
- **Missing CC/BCC**: Show as "None" in output

## Example Output

```
═══════════════════════════════════════════════════════════════
                         EMAIL DRAFT
═══════════════════════════════════════════════════════════════

To:      john.smith@company.com, jane.doe@company.com
CC:      manager@company.com
BCC:     None
Subject: Meeting Summary: Q1 Planning Review - January 10, 2026

───────────────────────────────────────────────────────────────

Hi Team,

Following our Q1 Planning Review meeting, here is a summary of the key outcomes.

**Decisions Made:**
- Approved the new project timeline starting February 1st
- Agreed to allocate additional resources to the mobile team

**Action Items:**
- John: Complete budget proposal by January 15th
- Jane: Schedule follow-up with vendors by January 12th
- All: Review updated roadmap and provide feedback by EOW

**Discussion Highlights:**
We reviewed the current project status and identified potential risks in the delivery timeline. The team agreed that proactive communication with stakeholders is essential.

Please reach out if you have any questions or need clarification on any items.

Regards,
Sarah

═══════════════════════════════════════════════════════════════

Email saved to: .claude/skills/meeting-email-composer/output/email_q1-planning-review_2026-01-10_143052.txt
```
