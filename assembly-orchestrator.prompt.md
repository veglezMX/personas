# Assembly Orchestrator Utility Prompt

**Version**: 1.0  
**Purpose**: Assemble modular Task-Specific Expert components into a single, ready-to-use system prompt.

This is a **utility prompt** that you (the user) run to combine your expert prompt components into the final prompt text that will be sent to an LLM. It performs the same assembly steps that a code-based orchestrator would do, but via LLM reasoning instead.

---

## What This Utility Does

1. **Merges YAML configurations** (default-config.yaml + optional profile override)
2. **Generates a configuration block** from merged config values
3. **Injects configuration block** at the top of `main.prompt.md` (or replaces `{CONFIGURATION_SECTION}` placeholder)
4. **Retains placeholders** in the prompt body (e.g., `{units}`, `{forecastDays}`) - LLM resolves these by referencing the config block
5. **Optionally loads instruction playbook** (if specified in config)
6. **Assembles components in correct order**:
   - Shared rules
   - Main prompt with config block + placeholders
   - Instruction playbook (if present)
   - Output template
7. **Validates** assembly completeness and configuration coverage
8. **Returns** a single system message ready to paste into your LLM interface

---

## Instructions for Use

### Step 1: Gather Your Component Files

You will need to paste the contents of these files (in order):

**Required Components:**
1. **Shared Rules** (`prompts/shared-rules.md`)
2. **Main Prompt Template** (`prompts/main.prompt.md`)
3. **Default Config** (`variables/default-config.yaml`)
4. **Output Template** (from `outputs/`, identified by `outputTemplate` key in config)

**Optional Components:**
5. **Profile Override** (`profiles/profile-*.yaml`) - if using a specific profile
6. **Instruction Playbook** (`prompts/instruction-playbooks/*.md`) - if specified in config via `instructionPlaybook` key

### Step 2: Paste Components Below

Copy this entire prompt into your LLM, then fill in each section with your actual file contents.

---

## INPUTS: Paste Your Component Files Here

### 1. Shared Rules
**File**: `prompts/shared-rules.md`

```markdown
[PASTE SHARED-RULES.MD CONTENTS HERE]
```

---

### 2. Main Prompt Template
**File**: `prompts/main.prompt.md`

```markdown
[PASTE MAIN.PROMPT.MD CONTENTS HERE - THIS SHOULD CONTAIN {PLACEHOLDERS}]
```

---

### 3. Default Config
**File**: `variables/default-config.yaml`

```yaml
[PASTE DEFAULT-CONFIG.YAML CONTENTS HERE]
```

---

### 4. Profile Override (OPTIONAL)
**File**: `profiles/profile-*.yaml` (leave empty if using defaults only)

```yaml
[PASTE PROFILE YAML CONTENTS HERE, OR LEAVE EMPTY TO USE DEFAULTS]
```

---

### 5. Output Template
**File**: `outputs/[template-name].md` (identified by `outputTemplate` key in merged config)

**Note**: You must paste the output template file that corresponds to the `outputTemplate` value in your merged config (default + profile). If you're unsure which template to use, merge the configs first (steps below) to see the `outputTemplate` value.

```markdown
[PASTE OUTPUT TEMPLATE CONTENTS HERE]
```

---

### 6. Instruction Playbook (OPTIONAL)
**File**: `prompts/instruction-playbooks/[playbook-name].md` (leave empty if not specified in config)

**Note**: Only paste this if the merged config contains an `instructionPlaybook` key. If absent, leave empty.

```markdown
[PASTE INSTRUCTION PLAYBOOK CONTENTS HERE, OR LEAVE EMPTY IF NOT USED]
```

---

## YOUR TASK: Assembly Instructions

You are an assembly orchestrator. Follow these steps precisely:

### Step 1: Merge YAML Configurations

1. Parse the **Default Config** YAML
2. If a **Profile Override** is provided:
   - Parse the Profile YAML
   - Merge profile values over defaults (profile keys override default keys)
   - For lists/arrays: use profile value entirely (do not concatenate)
3. If no profile is provided, use defaults as-is
4. Result: A single **Merged Configuration** object

**Output for Step 1**:
```yaml
# MERGED CONFIGURATION
[Show the final merged config here]
```

---

### Step 2: Generate Configuration Block and Inject

**IMPORTANT**: Do NOT replace placeholders throughout the prompt body. Instead:

1. **Generate a Configuration Block** from **Merged Configuration**:
   - Create a YAML-style section listing all key-value pairs
   - Format:
     ```markdown
     ## Configuration Variables
     
     The following configuration is active for this task:
     - key1: value1
     - key2: value2
     - listKey: item1, item2, item3
     ```

2. **Inject Configuration Block** into Main Prompt:
   - If the **Main Prompt Template** contains a `{CONFIGURATION_SECTION}` placeholder:
     - Replace that placeholder with the generated configuration block
   - If no `{CONFIGURATION_SECTION}` placeholder exists:
     - Prepend the configuration block at the top of the main prompt
     - Add a separator (`---`) between config block and prompt body

3. **Retain All Other Placeholders**:
   - Keep placeholders like `{units}`, `{forecastDays}`, `{inputFormat}` in the prompt body unchanged
   - The LLM will resolve these by referencing the configuration block

**Why This Approach?**:
- **Token Efficiency**: Values stated once in config block, referenced multiple times via placeholders
- **Clarity**: LLM sees both concrete values (config) and semantic usage (placeholders)

**Output for Step 2**:
```markdown
# INSTANTIATED MAIN PROMPT (WITH CONFIGURATION INJECTION)
[Show the configuration block at top, followed by main prompt with placeholders RETAINED]
```

---

### Step 3: Load Optional Instruction Playbook

1. Check the **Merged Configuration** for an `instructionPlaybook` key
2. If present and the user pasted playbook content in Section 6:
   - Include it in assembly (will go between main prompt and output template)
3. If absent or empty:
   - Skip this step, note "No instruction playbook specified"

**Output for Step 3**:
```markdown
# INSTRUCTION PLAYBOOK STATUS
[Either "No instruction playbook specified" OR show the playbook contents]
```

---

### Step 4: Assemble Final System Prompt

Concatenate components in this exact order:

1. **Shared Rules** (from Section 1)
2. **Instantiated Main Prompt** (from Step 2) - This includes:
   - Configuration block at the top
   - Main prompt body with placeholders RETAINED (not replaced)
3. **Instruction Playbook** (from Step 3, if present)
4. **Output Template** (from Section 5)

Use clear section dividers (e.g., `---` or `## SECTION NAME`) between components for readability.

**Output for Step 4**:
```markdown
# ASSEMBLED SYSTEM PROMPT
[Show the complete, ready-to-use system message here with:
 - Shared Rules first
 - Configuration block + Main prompt with placeholders
 - Optional instruction playbook
 - Output template last]
```

---

### Step 5: Validation

Check the **Assembled System Prompt** for issues:

1. **Configuration Block Presence**: Verify the configuration block is present at the top of the main prompt section
2. **Placeholders Retained**: Confirm placeholders like `{variable}` are RETAINED in the prompt body (this is correct behavior)
3. **Configuration Completeness**: Check that all placeholders used in the prompt body have corresponding entries in the configuration block
4. **Output template match**: Verify the pasted output template matches the `outputTemplate` value in merged config
5. **Required sections**: Confirm all required components are present

**Output for Step 5**:
```markdown
# VALIDATION REPORT

## Configuration Block
[Confirm config block is present with all merged values, OR flag if missing]

## Placeholders Status
[Confirm placeholders are RETAINED in prompt body (expected behavior)]

## Configuration Completeness
[List any placeholders in prompt body that DON'T have corresponding config entries, OR "✓ All placeholders covered in config block"]

## Output Template Match
[Confirm template name matches config, OR flag mismatch]

## Assembly Completeness
[Confirm all required sections present, OR list missing components]

## Status
[Either "✅ READY TO USE" or "⚠️ ISSUES FOUND - SEE ABOVE"]
```

---

## FINAL OUTPUT

After completing all steps, provide:

1. **Merged Configuration** (YAML)
2. **Validation Report** (with status)
3. **Assembled System Prompt** (complete, ready-to-paste)

Organize your response clearly with headers so the user can easily find the final system prompt.

---

## Example Minimal Usage

If you have a minimal expert with no profile override and no instruction playbook:

- Paste: shared-rules.md, main.prompt.md, default-config.yaml, output-template.md
- Leave Sections 4 and 6 empty
- The assembler will use defaults and skip optional components

---

## When to Use This Utility

**Use this when**:
- You want to manually test different profile combinations
- You don't have a code-based orchestrator
- You need to inspect the exact prompt text before runtime
- You're debugging placeholder mismatches

**Don't use this when**:
- You have a production orchestrator (use that instead)
- You only want to check placeholder substitution (use Utility 3 instead)
- You're generating new components (use Utilities 1 & 2 instead)

---

## Related Utilities

- **Utility 1**: Generate `main.prompt.md` templates
- **Utility 2**: Generate `profiles/profile-*.yaml` files
- **Utility 3**: Quick placeholder substitution (single file)
- **Utility 4 (this file)**: Full runtime assembly (multi-file)

See [Utility Prompts Guide](../guides/utility-prompts.md) for the complete utility prompt suite.
