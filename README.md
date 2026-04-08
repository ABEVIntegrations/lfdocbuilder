# Laserfiche Workflow Documentation Generator

A Claude Code skill that automatically converts Laserfiche workflow files (`.wfi` / `.xml`) into polished, professional HTML documentation with two audience-targeted views:

- **End-User Guide** -- plain-language explanation of what the workflow does, when it runs, and what to expect
- **Technical Reference** -- full admin-level breakdown with activity maps, token inventories, field references, and configuration details

## Features

- Parses Laserfiche Workflow Designer exports (`.wfi` and `.xml`)
- Recursively maps all activities, branches, conditions, and loops
- Translates internal class names into human-readable activity descriptions
- Generates a single standalone HTML file with:
  - Fixed navigation bar and sidebar
  - Color-coded sections (green for users, blue for admins)
  - Professional typography (DM Sans + JetBrains Mono)
  - At-a-glance summary card
  - Activity tree visualization with icons
  - Token and field inventory tables
- Identifies disabled activities and exception paths
- Handles sub-workflow references, complex token expressions, and unknown activity types gracefully

## Quick Start

### Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) installed, **or** access to [Claude.ai](https://claude.ai) with the Projects feature
- A Laserfiche workflow file (`.wfi` or `.xml`) exported from Workflow Designer

---

## Installation

### Option A: Claude Code CLI

1. **Clone this repository:**

   ```bash
   git clone https://github.com/YOUR_USERNAME/lfdocbuilder.git
   ```

2. **Install the skill:**

   ```bash
   claude install skill/laserfiche-workflow-doc.skill
   ```

   This registers the skill so Claude Code can use it automatically when you reference a workflow file.

3. **Use it:** Open Claude Code in any directory containing a `.wfi` file and ask Claude to document it (see [Sample Prompts](#sample-prompts) below).

### Option B: Claude.ai (Web)

1. **Download the skill file** from this repository: `skill/laserfiche-workflow-doc.skill`

2. **Create a new Project** on [claude.ai](https://claude.ai):
   - Click **Projects** in the sidebar
   - Click **Create Project**
   - Give it a name like "Laserfiche Workflow Docs"

3. **Add the skill to your project:**
   - In your project, go to **Project Knowledge**
   - Upload the `laserfiche-workflow-doc.skill` file

4. **Use it:** Start a new conversation within that project, upload your `.wfi` file, and ask Claude to document it.

### Option C: Manual Setup (Claude Code CLI)

If you prefer not to use `claude install`, you can add the skill manually:

1. Copy the `skill/laserfiche-workflow-doc.skill` file to your Claude Code skills directory:

   - **macOS/Linux:** `~/.claude/skills/`
   - **Windows:** `%USERPROFILE%\.claude\skills\`

2. Restart Claude Code to pick up the new skill.

---

## Sample Prompts

Once the skill is installed, you can use natural language to trigger it. Here are some examples:

### Basic Documentation

```
Document this workflow
```
> Upload or reference a `.wfi` file in the same directory.

```
Analyze ASSR_CreateNewParcel.wfi and generate documentation
```

```
What does this workflow do?
```

### Targeted Requests

```
Create documentation for this Laserfiche workflow. 
I need to share it with non-technical staff in our assessor's office.
```

```
Generate a technical reference for this workflow -- 
I need to understand the branching logic and token flow before editing it.
```

```
Document this LF workflow. Focus on the error handling paths 
so I can troubleshoot why documents end up in the exceptions folder.
```

### Batch / Comparison

```
I have three workflow files in this folder. 
Document each one and highlight any shared tokens or field dependencies.
```

### Explanation-Focused

```
I just inherited this workflow from the previous admin. 
Can you explain what it does and flag anything that looks misconfigured?
```

```
Our assessor's office needs to train new staff on this process. 
Create a user-friendly guide from this workflow file.
```

---

## Repository Structure

```
lfdocbuilder/
├── skill/
│   └── laserfiche-workflow-doc.skill    # Claude Code skill (ZIP archive)
│       ├── SKILL.md                     # Main skill instructions
│       └── references/
│           ├── activity-types.md        # Activity class name mappings
│           └── html-template.md         # CSS design system & HTML patterns
├── examples/
│   └── ASSR_CreateNewParcel.wfi         # Sample workflow file for testing
└── README.md
```

### What's Inside the Skill

| File | Purpose |
|------|---------|
| `SKILL.md` | Core instructions -- tells Claude how to parse workflow XML, what to extract, and how to structure the two documentation views |
| `references/activity-types.md` | Maps Laserfiche activity class names (e.g., `ForEachEntryActivityProxy`) to human-readable names and behavior descriptions |
| `references/html-template.md` | Complete CSS design system with color palette, typography, layout components, callout boxes, and activity tree styling |

---

## Output

The skill generates a single standalone HTML file containing:

| Section | Audience | Color Accent |
|---------|----------|-------------|
| Workflow At-a-Glance | Everyone | Neutral |
| End-User Guide | Staff / non-technical | Green |
| Technical Reference | Admins / developers | Blue |

The HTML is fully self-contained with inline CSS and only depends on Google Fonts CDN for typography.

---

## Supported Activity Types

The skill recognizes and documents these Laserfiche activity categories:

- **Control Flow** -- If/Else branching, For Each loops, Parallel execution, Sequences
- **Repository Operations** -- Search, Move, Rename, Delete, Create Folder
- **Metadata Operations** -- Assign Template, Set Fields, Retrieve Field Values, Remove Template
- **Token/Variable Operations** -- Create Token, Parse Text, Format Token, Calculate Value
- **Communication** -- Send Email
- **Process Control** -- Start Sub-Workflow, Wait for Event, Delay, Terminate
- **User Interaction** -- User Task, Assign User Task

Unknown activity types are interpreted from the XML and flagged in the output.

---

## Example

The `examples/` directory includes `ASSR_CreateNewParcel.wfi`, a real-world assessor's office workflow that creates new parcel records. Use it to test the skill:

```
Analyze examples/ASSR_CreateNewParcel.wfi and generate documentation
```

---

## Contributing

Contributions are welcome! If you work with Laserfiche and encounter activity types or workflow patterns that aren't handled well, please open an issue with:

1. A description of the activity type or pattern
2. A sanitized XML snippet if possible
3. What the expected documentation output should look like

---

## License

MIT License. See [LICENSE](LICENSE) for details.
