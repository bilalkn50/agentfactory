---
name: teams-transcript-summarizer
description: Summarize Microsoft Teams meeting transcripts into structured notes. Use when processing meeting transcripts from Teams to extract key decisions, action items, discussion topics, and attendee participation. Triggers on requests like "summarize this meeting", "extract action items from transcript", or when given .vtt, .txt, or .docx transcript files.
---

# Teams Transcript Summarizer

Converts MS Teams meeting transcripts into structured markdown summaries with decisions, action items, topics, and participation breakdown.

## Workflow

1. **Receive transcript** - Accept as pasted text, .txt file, .docx file, or .vtt file
2. **Parse speakers** - Extract unique participants from "Speaker: text" patterns
3. **Identify content** - Extract decisions, action items, topics discussed
4. **Generate summary** - Output structured markdown per template
5. **Save to output folder** - Save the summary to the skill's output directory

## Input Handling

**Plain text / Pasted content**: Process directly - look for `Speaker Name: dialogue` patterns

**VTT files**: Strip timestamps, merge consecutive lines from same speaker

**DOCX files**: Extract text content, then process as plain text

## Summary Template

Output this structure:

```markdown
# Meeting Summary: [Topic/Title if identifiable]

**Date**: [Extract from transcript or mark as "Not specified"]
**Duration**: [If determinable from timestamps]
**Attendees**: [List all unique speakers]

## Key Decisions
- [Decision 1]
- [Decision 2]

## Action Items
| Action | Owner | Due Date |
|--------|-------|----------|
| [Task] | [Name] | [Date or TBD] |

## Discussion Topics

### [Topic 1]
Brief summary of what was discussed.

### [Topic 2]
Brief summary of what was discussed.

## Attendee Participation
- **[Name]**: [Brief role/contribution summary]
- **[Name]**: [Brief role/contribution summary]

## Notable Quotes
> "[Significant quote]" - [Speaker]
```

## Processing Guidelines

- **Action items**: Look for phrases like "will do", "I'll", "action item", "follow up", "by [date]", assignments
- **Decisions**: Look for "decided", "agreed", "confirmed", "approved", "let's go with"
- **Topics**: Group related dialogue segments, infer topic from context
- **Participation**: Note who spoke most, who made decisions, who took actions

## Edge Cases

- **No clear speakers**: Summarize content without participation breakdown
- **Very short meetings**: Provide condensed summary without full template
- **Technical jargon**: Preserve domain-specific terms, don't simplify

## Output File Handling

After generating the summary, save it to the output folder:

**Output location**: `.claude/skills/teams-transcript-summarizer/output/`

**Filename format**: `meeting-summary-YYYY-MM-DD-HHMMSS.md`
- Use the meeting date if extracted from transcript
- Otherwise use the current timestamp when processing

**Steps**:
1. Create the output directory if it doesn't exist: `.claude/skills/teams-transcript-summarizer/output/`
2. Generate filename using meeting date or current timestamp
3. Write the complete markdown summary to the file
4. Confirm the save location to the user

**Example output path**: `.claude/skills/teams-transcript-summarizer/output/meeting-summary-2026-01-10-143022.md`

Always inform the user where the summary was saved after processing.
