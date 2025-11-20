# Claude Code + Gamma: Automated Case Study Generator

Create beautiful, professionally-designed case study presentations in **3 minutes** using Claude Code and Gamma AI.

Stop spending hours formatting slides. Let AI handle the structure, design, and layout while you focus on the content.

## What This Does

This workflow automates case study creation by:
1. **Gathering information** through an intelligent agent that asks clarifying questions
2. **Structuring content** into a proven case study format (Challenge → Solution → Results)
3. **Generating a presentation** via Gamma API using a pre-designed template
4. **Delivering a polished deck** ready to share with prospects

**Time:** 3 minutes
**Cost:** ~3-4 Gamma credits per case study
**Manual work:** Zero slide formatting

## The Problem This Solves

"Do we have a case study for that project?"

That question kills sales momentum. You're on a call, prospect is interested, they ask for proof, and you don't have it ready. Or it's buried in Notion. Or it's outdated. Or the designer needs 2 days to format it.

This workflow fixes that.

## Demo

https://www.linkedin.com/posts/jsukarangsan_i-use-claude-code-gamma-to-create-case-activity-7397336281694941184-I9QX

## How It Works

### The Stack

- **Claude Code** - Orchestrates the workflow and formats content
- **Gamma API** - Generates beautiful presentations from templates
- **Custom Agent** - Asks questions and structures your case study
- **Slash Command** - One-step trigger (`/create-case-study`)

### The Workflow

1. Run `/create-case-study` in Claude Code
2. Agent asks questions about your project (or reads from a file)
3. Content is structured into: Challenge → Solution → Results
4. Formatted content sent to Gamma API
5. Professional presentation appears in your Gamma dashboard

The agent doesn't just format—it helps you **think through the case study**:
- What was the real challenge?
- What did you actually build?
- What were the measurable results?

It asks questions until it has what it needs to create something solid.

## Prerequisites

Before you start, you'll need:

1. **Claude Code** installed ([Download here](https://claude.com/claude-code))
2. **Gamma Pro account** with API access ([Sign up](https://gamma.app))
3. **A Gamma template** (we'll set this up below)
4. **Basic command line** knowledge

## Installation

### 1. Clone This Repository

```bash
git clone https://github.com/yourusername/claude-gamma-case-studies.git
cd claude-gamma-case-studies
```

### 2. Set Up Claude Code

Copy the `.claude` directory to your project:

```bash
# If using in an existing project
cp -r .claude /path/to/your/project/

# Or work directly in this repo
# (Claude Code will detect the .claude directory automatically)
```

The `.claude` directory contains:
- `.claude/commands/create-case-study.md` - The slash command
- `.claude/agents/case-study-writer.md` - The intelligent agent

### 3. Configure Gamma API

#### Get Your Gamma API Key

1. Sign up for [Gamma Pro](https://gamma.app/signup)
2. Go to Settings → API Keys
3. Create a new API key
4. Copy the key (starts with `sk-gamma-`)

#### Create Your Gamma Template

1. Log into [Gamma](https://gamma.app)
2. Create a new presentation
3. Design your case study template with placeholders:
   - Title slide
   - Challenge section
   - Solution section
   - Results section
   - Testimonial section (optional)
4. Save as a template
5. Get the template ID from the URL: `gamma.app/docs/[template-id]`

**Note:** The current setup uses template ID `g_seds5bke4felj8x`. You can:
- Use this template (if shared publicly)
- Create your own and update the template ID in `.claude/commands/create-case-study.md`

#### Set Environment Variable

Create a `.env` file in your project root:

```bash
# Copy the example file
cp .env.example .env

# Edit with your API key
echo "GAMMA_API_KEY=sk-gamma-your-api-key-here" > .env
```

**Important:** Never commit your `.env` file to git. The `.gitignore` is already configured to exclude it.

### 4. Verify Setup

Open your project in Claude Code and run:

```
/create-case-study
```

If setup is correct, the agent will ask: "What information do you have about this project?"

## Usage

### Option 1: Interactive Mode (Recommended for First Time)

1. Open Claude Code in your project directory
2. Run `/create-case-study`
3. Answer the agent's questions:
   - Client context (industry, size, role)
   - Challenge (problem, business impact, urgency)
   - Solution (what you built, how long, technologies)
   - Results (metrics, outcomes, quotes)
4. Review the structured content
5. Confirm to generate the Gamma presentation

**Example interaction:**

```
You: /create-case-study

Agent: What information do you have about this project?

You: We built an AI lead scoring system for a SaaS company

Agent: Great! Let's structure this. What industry is the client in?

You: EdTech, K-12 learning management software

Agent: What specific problem were they facing?

[Continue answering questions...]

Agent: Here's the case study I've created:

[Shows formatted content]

Looks good? I'll send this to Gamma.

You: Yes

Agent: ✅ Case study generated!
📊 Cost: ~3 credits
🔗 View at: https://gamma.app
```

### Option 2: From Markdown File

If you already have case study content:

1. Create a markdown file with your case study:

```markdown
# EdTech SaaS: 340% Increase in Sales Qualified Leads

How a mid-market education technology company transformed their sales process

## THE CHALLENGE

A B2B EdTech SaaS company selling learning management software to K-12 school districts was struggling with inefficient prospecting. Their sales team spent 8-10 hours per week manually researching school districts. Lead scoring was entirely manual and inconsistent across reps.

## THE SOLUTION

We built an AI-powered prospect research and lead scoring system that automated the entire qualification process. The system monitored state education databases, local news sources, and board meeting minutes. Claude AI analyzed this data to identify buying signals.

## THE RESULTS

• Sales Qualified Leads increased by 340% (from 15 to 66 per month)
• Research time reduced from 8 hours to 45 minutes per week per rep
• Win rate improved from 12% to 31% on qualified opportunities
• Average sales cycle shortened by 39% (6.2 months to 3.8 months)

"We were flying blind before. Now we're having conversations with districts that are actively looking, and we're showing up with insights about their specific needs."
— VP of Sales
```

2. Run the command with the file:

```
/create-case-study @path/to/case-study.md
```

3. Gamma presentation generated instantly

### Option 3: Direct Text Input

Paste complete case study text directly:

```
/create-case-study

[Paste your full case study text]
```

## Template Customization

To use your own Gamma template:

1. Create your template in Gamma
2. Get the template ID from the URL
3. Update `.claude/commands/create-case-study.md`:

```markdown
- Template Gamma ID: `g_seds5bke4felj8x`  # Replace with your template ID
```

4. Update the request body section:

```json
{
  "gammaId": "your-template-id-here",
  "prompt": "[Complete case study text]"
}
```

## Cost

**Gamma API Credits:**
- Each case study generation: ~3-4 credits
- Gamma Pro plan includes credits monthly
- Check your balance at: https://gamma.app/settings/billing

**Development:**
- Claude Code: Free (with Claude Pro subscription)
- This workflow: Free and open source

## Troubleshooting

### "GAMMA_API_KEY not set"

**Problem:** Environment variable not loaded

**Solution:**
```bash
# Verify .env file exists
ls -la .env

# Check content (don't commit this!)
cat .env

# Restart Claude Code to reload environment
```

### "Template not found" or "Invalid gammaId"

**Problem:** Template ID is incorrect or not accessible

**Solution:**
1. Verify template exists in your Gamma account
2. Check template ID in URL matches ID in command file
3. Ensure template is not in trash
4. Create a new template and update the ID

### "API key invalid"

**Problem:** Gamma API key format is wrong

**Solution:**
- API keys start with `sk-gamma-`
- Regenerate key in Gamma settings if needed
- No spaces or quotes in .env file

### Agent doesn't ask questions

**Problem:** Agent file not loaded correctly

**Solution:**
```bash
# Verify agent file exists
ls -la .claude/agents/case-study-writer.md

# Restart Claude Code
# Try running command again
```

## Examples

See the `examples/` directory for sample case studies:
- `examples/edtech-saas.md` - Lead scoring automation
- `examples/design-agency.md` - Design system transformation
- More coming soon

## Architecture

### File Structure

```
.
├── .claude/
│   ├── commands/
│   │   └── create-case-study.md          # Slash command orchestrator
│   └── agents/
│       └── case-study-writer.md          # Intelligent content agent
├── examples/
│   └── edtech-saas.md                    # Sample case study
├── .env.example                          # Environment template
├── .gitignore                            # Git exclusions
├── README.md                             # This file
└── LICENSE                               # MIT License
```

### How It Works Technically

1. **Slash Command** (`/create-case-study`) is detected by Claude Code
2. **Orchestrator** loads command file, determines input method
3. **Agent** (if needed) asks questions, structures content
4. **Formatter** creates Gamma-compatible prompt string
5. **API Call** posts to `https://public-api.gamma.app/v1.0/generations/from-template`
6. **Response** returns `generationId` for tracking
7. **User** accesses presentation in Gamma dashboard

### Template-Based Generation

This workflow uses **template-based generation**, not free-form AI slides.

**Why?**
- Consistent branding across all case studies
- Professional design quality
- Faster generation (pre-designed layout)
- Predictable output structure
- Easy to update design for all future case studies

**The pattern:**
- Design template once (human quality)
- Generate content with AI (automation)
- Populate via API (consistent output)

This is the difference between "AI makes slides" (chaos) and "AI populates proven templates" (production-ready).

## Contributing

Contributions welcome! Areas for improvement:

- [ ] Additional case study templates
- [ ] Support for other presentation tools (Pitch, Beautiful.ai)
- [ ] Multi-page case study generation
- [ ] Integration with CRM systems
- [ ] Automated screenshot/image inclusion
- [ ] Video case study scripts

Open an issue or submit a PR.

## License

MIT License - See [LICENSE](LICENSE) file for details.

## Credits

Built by [Jon Sukarangsan](https://twitter.com/jonsukarangsan) | [Summer Friday & Partners](https://summerfriday.co)

Inspired by the need to stop losing sales momentum over missing case studies.

## Resources

- [Gamma API Documentation](https://developers.gamma.app)
- [Claude Code Documentation](https://docs.claude.com/claude-code)
- [Case Study Writing Best Practices](https://example.com) - Coming soon

## Support

- **Issues:** [GitHub Issues](https://github.com/yourusername/claude-gamma-case-studies/issues)
- **Discussions:** [GitHub Discussions](https://github.com/yourusername/claude-gamma-case-studies/discussions)
- **Twitter:** [@jonsukarangsan](https://twitter.com/jonsukarangsan)

---

**Stop formatting slides. Start shipping case studies.**
