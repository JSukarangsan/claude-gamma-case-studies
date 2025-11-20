# Create Case Study Command

You are executing the `/create-case-study` command. This is an orchestrator that:

1. **Gets or creates case study content** (from file or via agent)
2. **Calls the Gamma API** to generate a presentation using the template
3. **Returns the generation ID and Gamma URL** to the user

## Workflow

### Step 1: Get Case Study Content

**Option A: User provides a file path**
- Use Read tool to load the markdown file
- Extract the core case study content (title, challenge, solution, results, quote)
- Skip the detailed technical sections for the Gamma presentation

**Option B: User provides partial info or asks for help**
- Use Task tool to invoke the `case-study-writer` agent
- Agent will ask questions and gather complete information
- Agent will create structured case study content
- You'll receive the formatted content back from the agent

**Option C: User provides complete text directly**
- Use their text as-is for the case study content

### Step 2: Format Content for Gamma API

Take the case study content and format it as a single prompt string with these sections:
- Title and subtitle on first lines
- "THE CHALLENGE" section
- "THE SOLUTION" section
- "THE RESULTS" section (bullet points)
- Client testimonial quote (if available)

Keep the content focused and concise - the template will handle the visual layout.

### Step 3: Call Gamma API (Create from Template)

**Important API Details:**
- Endpoint: `POST https://public-api.gamma.app/v1.0/generations/from-template`
- Authentication: `X-API-KEY` header with value from `$GAMMA_API_KEY`
- Content-Type: `application/json`
- Template Gamma ID: Use `$GAMMA_TEMPLATE_ID` from environment

**Request Body Structure:**
```json
{
  "gammaId": "[Value from $GAMMA_TEMPLATE_ID]",
  "prompt": "[Complete case study text from agent]"
}
```

**Notes:**
- Using the create-from-template endpoint to populate the pre-designed template
- The `prompt` field should contain the full formatted case study
- Template already has the layout, design, and structure defined
- Parameter is `gammaId` (not templateId) and `prompt` (not inputText)

### Step 4: Make the API Call

Use the Write tool to create a temporary JSON file, then Bash to make the API call:

1. **Write the JSON request file:**
   - Use Write tool to create `/tmp/gamma-request.json`
   - Include the `gammaId` and formatted `prompt` content
   - Proper JSON escaping will be handled automatically

2. **Make the curl request:**
```bash
source .env && curl --request POST \
  --url https://public-api.gamma.app/v1.0/generations/from-template \
  --header 'Content-Type: application/json' \
  --header "X-API-KEY: $GAMMA_API_KEY" \
  --data @/tmp/gamma-request.json
```

**Note:** The gammaId in the JSON file should use the value from `$GAMMA_TEMPLATE_ID` environment variable.

3. **Parse the response:**
   - Extract the `generationId` from the JSON response
   - This is what you'll show to the user

### Step 5: Handle Response

The API returns a generation ID. Inform the user:
- The case study has been generated
- It will appear in their Gamma dashboard in a separate tab
- They can access it at https://gamma.app

**Cost Transparency:**
- This generation costs approximately 3-4 Gamma credits
- Remind user if they want to check their credit balance

## Error Handling

If the API call fails:
- Check if `GAMMA_API_KEY` is set in the environment
- Check if `GAMMA_TEMPLATE_ID` is set in the environment
- Verify the API key format (should be `sk-gamma-xxxxxxxx`)
- Verify the template ID format (should be `g_xxxxxxxxx`)
- Show the error message from Gamma API
- Suggest checking their Gamma Pro subscription status

## Success Output

Show the user:
1. ✅ Case study content created
2. ✅ Gamma slide generated
3. 📊 Cost: ~3-4 credits
4. 🔗 View in your Gamma dashboard: https://gamma.app

Then display the case study content for their reference.
