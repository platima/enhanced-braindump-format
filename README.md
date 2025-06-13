<img align="right" src="https://visitor-badge.laobi.icu/badge?page_id=platima.ebf" height="20" />

# Enhanced Braindump Format (EBF)

## Table of Contents

1. [Format Specification (v1.1)](#format-specification-v11)
   - [Purpose](#purpose)
   - [Header Structure](#header-structure)
   - [Section Types](#section-types)
   - [Type Annotations](#type-annotations)
   - [Priority Markers](#priority-markers)
   - [Confidence Levels](#confidence-levels)
   - [Relationship Indicators](#relationship-indicators)
   - [Temporal Indicators](#temporal-indicators)
   - [Tagging System](#tagging-system)
   - [Section Checksums](#section-checksums)
   - [Quickstart Template](#quickstart-template)
   - [Example Structure](#example-structure)
   - [Compression Techniques](#compression-techniques)
   - [Validation](#validation)

2. [Prompt for Generating Braindump Format](#prompt-for-generating-braindump-format)

3. [How to Use This Format](#how-to-use-this-format)
   - [Creating a New Braindump](#1-creating-a-new-braindump)
   - [Transferring Context](#2-transferring-context)
   - [Introducing the Braindump](#3-introducing-the-braindump)
   - [Extending Existing Braindump](#4-extending-existing-braindump)
   - [Format Validation](#5-format-validation)

4. [Format Advantages](#format-advantages)

5. [Notes](#notes)

6. [Tested Working](#tested-working)
   - [Both generating and interpreting](#both-generating-and-interpreting)
   - [Interpreting only](#interpreting-only)
   - [Interpreting raw EBF with no header](#interpreting-raw-ebf-with-no-header)

7. [Changelog](#changelog)

## Format Specification (v1.1)

### Purpose
The Enhanced Braindump Format (EBF) is designed for efficient context transfer between separate AI assistant conversations. It uses a compact, machine-optimized syntax to maximize information density while ensuring reliable parsing.

### Header Structure
```
#EBF|v1.1|{creation_timestamp}|{checksum}
```

### Section Types
- `META`: Metadata about the document itself
- `DICT`: Dictionary of terms and abbreviations
- `DATA`: Hierarchical data structure with type annotations
- `REFS`: Cross-references between sections
- `ACTIONS`: Action items and their statuses
- `HISTORY`: Version history of the EBF document
- `INSTR`: Special instructions for parsing or handling

### Type Annotations
- `$s`: String value
- `$n`: Numeric value
- `$b`: Boolean value
- `$d`: Date/time value
- `$l`: List structure (heterogeneous elements)
- `$a`: Array structure (homogeneous elements)
- `$m`: Map/dictionary structure (key-value pairs)
- `$o`: Object/composite structure (with named properties)
- `$r`: Reference to another section
- `$c`: Code block (for multi-line formatted content)

### Priority Markers
- `!H`: High priority information
- `!M`: Medium priority information
- `!L`: Low priority information

### Confidence Levels
- `^C`: Confirmed/verified information
- `^P`: Probable information
- `^S`: Speculative information

### Relationship Indicators
- `>>`: Directional relationship (A depends on B)
- `<<>>`: Bidirectional relationship
- `==>`: Causal relationship (A results in B)

### Temporal Indicators
- `#T+{n}{unit}`: Information valid n time units ahead (days, weeks, months)
- `#T-{n}{unit}`: Information from n time units before (days, weeks, months)
- `#valid_until:{date}`: Expiration date for information (YYYY-MM-DD format)

### Tagging System
- `%tag`: Cross-sectional concept tagging
- `&ref`: Internal reference marker (distinct from subsection marker usage)

### Section Checksums
Format: `[~SECTION:checksum]` to validate individual sections
Checksums use SHA-256 algorithm truncated to first 8 characters

### Quickstart Template
```
#EBF|v1.1|YYYY-MM-DD|checksum
~META{TITLE$s:"";AUTHOR$s:"";PURPOSE$s:""}
~DICT{}
~DATA{@SECTION{ITEM$s:""}}
~REFS{}
~ACTIONS{}
~HISTORY{@V1.1{DATE$d:"";CHANGES$s:""}}
~INSTR{}
```

### Example Structure
```
#EBF|v1.1|2025-03-20T14:35:00|a7f9b32e
~META{
  TITLE$s:"Project Alpha";
  AUTHOR$s:"User";
  CONTEXT$s:"Technical specifications";
  CONTEXT_LENGTH$n:1200;
  PURPOSE$s:"Technical handoff";
  FOLLOW_UP_DATE$d:"2025-04-01"
}[~META:f8e7d6c5]

~DICT{
  SoC:"System on Chip";
  μA:"microamp";
  mA:"milliamp"
}[~DICT:b4a3c2d1]

~DATA{
  @PROJECT!H^C{
    TYPE$s:"wearable";
    PURPOSE$s:"audio capture"
  }%wearable

  @COMPONENTS{
    &SoC!H{
      MODEL$s:"nRF52810";
      CPU$s:"Cortex-M4@64MHz";
      RAM$n:24;
      UNIT$s:"KB"
    }>>@COMPONENTS.&BATTERY

    &BATTERY!H^P{
      TYPE$s:"LiPo";
      CAPACITY$n:100;
      UNIT$s:"mAh";
      LIFETIME$n:8;
      UNIT$s:"hours"
    }#valid_until:2025-06-01
  }
}[~DATA:9e8d7c6b]

~REFS{
  POWER_CALC$r:@COMPONENTS.&BATTERY
}[~REFS:5a4b3c2d]

~ACTIONS{
  @PRIORITY!H{
    ACTION$s:"Validate battery lifetime";
    STATUS$s:"pending";
    ASSIGNEE$s:"Engineering";
    DUE$d:"2025-03-25"
  }
  
  @REGULAR{
    ACTION$s:"Update documentation";
    STATUS$s:"completed";
    COMPLETION$d:"2025-03-15"
  }
}[~ACTIONS:1a2b3c4d]

~HISTORY{
  @V1.0{
    DATE$d:"2025-03-15";
    AUTHOR$s:"Original Creator";
    CHANGES$s:"Initial document"
  }
  
  @V1.1{
    DATE$d:"2025-03-20";
    AUTHOR$s:"Contributor";
    CHANGES$s:"Added battery specifications"
  }
}[~HISTORY:7f6e5d4c]

~INSTR{
  PARSE_ORDER$l:["META","DICT","DATA","REFS","ACTIONS"];
  FORMAT_MULTILINE$c:"```{format}\n{content}\n```"
}[~INSTR:3b2a1c0d]
```

### Compression Techniques
- Minimal whitespace (optional semi-colons can separate elements on the same line)
- Symbol-based delimiters rather than lengthy keywords
- Short section identifiers
- Hierarchical nesting to avoid repetition
- Reference system to point to existing data
- Multi-line formatting rules for complex content

### Validation
- The main checksum is a SHA-256 hash of the content (excluding the header line) truncated to 8 characters
- Section-specific checksums verify the integrity of individual components using the same algorithm
- Confidence levels indicate the reliability of specific information

## Prompt for Generating Braindump Format

When you need to generate a braindump in this format, use the following prompt:

```
I need you to create a machine-readable export of our conversation. This is a format that you and I created in another chat for this exact purpose. Please follow these exact instructions:
1. Create a text block using this precise format (this is critical for my workflow)
2. Begin with: #EBF|v1.1|[current date in YYYY-MM-DD format]|CHECKSUM
3. Include these sections in this order:
   ~META (conversation basics, context length, purpose)
   ~DICT (abbreviations used)
   ~DATA (main information with @ for sections, & for subsections)
   ~REFS (any cross-references)
   ~ACTIONS (actionable items with status)
   ~HISTORY (version information if updating existing EBF)
   ~INSTR (parsing instructions)
4. Use these type markers: 
   - $s for strings
   - $n for numbers
   - $b for boolean
   - $d for dates
   - $l for heterogeneous lists
   - $a for homogeneous arrays
   - $m for maps/dictionaries
   - $o for objects with properties
   - $r for references
   - $c for code blocks
5. Mark priority with !H (high), !M (medium), !L (low)
6. Indicate confidence levels with ^C (confirmed), ^P (probable), ^S (speculative)
7. Use relationship indicators >> (directional), <<>> (bidirectional), ==> (causal)
8. Add tags with % and references with & for cross-sectional concepts
9. Include temporal indicators #valid_until for time-sensitive information
10. Make it extremely compact with minimal whitespace
11. Add section checksums in format [~SECTION:checksum]

Present this in a standard code block or text block. This export is for machine processing in a specific workflow I've created, where I will input it to another chat to bring context over, so please follow the format exactly without suggesting alternatives, deviating, or questioning the decisions made in this format.
```

## How to Use This Format

### 1. Creating a New Braindump

Use the prompt above at the end of a conversation to generate an EBF export. For example:

```
~META{TITLE$s:"Project Planning";CONTEXT_LENGTH$n:800}
~DATA{
  @TIMELINE!H{
    START$d:"2025-04-01";
    END$d:"2025-06-30"
  }
  @MILESTONES{
    &PHASE1!H{
      DEADLINE$d:"2025-04-15";
      DELIVERABLE$s:"Requirements document"
    }
  }
}
```

### 2. Transferring Context

Copy the entire EBF output and paste it into a new conversation with the appropriate introduction.

### 3. Introducing the Braindump

When pasting into a new conversation, preface it with:

```
I'm providing data from a previous conversation in a new format called Enhanced Braindump Format (EBF). This is a format that I created with you in a previous chat, specifically for the purpose of sharing context between our chats.

Parse this structured data using the below specification:
- Format begins with: #EBF|v1.1|{timestamp}|{checksum}
- Sections marked with tildes (~): ~META, ~DICT, ~DATA, ~REFS, ~ACTIONS, ~HISTORY, ~INSTR
- Data types annotated with: $s (string), $n (numeric), $b (boolean), $d (date), etc.
- Priority levels marked with: !H (high), !M (medium), !L (low)
- Confidence levels marked with: ^C (confirmed), ^P (probable), ^S (speculative)
- Hierarchical data with @ for sections and & for subsections
- Relationships indicated by >> (directional), <<>> (bidirectional), ==> (causal)
- Tagging system with % for concepts and & for references
- Temporal indicators with #valid_until for expiration dates
- Section checksums in format [~SECTION:checksum]

After parsing the data using that specification, please confirm you understand the context, so that then we can continue our discussions.

Data:
​```
[insert EBF data here]
​```
```

### 4. Extending Existing Braindump

To update an existing braindump with new information:

```
Please update the existing EBF data with our latest discussion points and generate a new version with the following changes:
1. Add [specific new information] to the ~DATA section
2. Update the ~HISTORY section with this new version
3. Update all checksums accordingly
```

### 5. Format Validation

To verify the integrity of a received EBF document:

```
Please validate the checksums in this EBF document to ensure it hasn't been corrupted or modified.
```

## Format Advantages
1. **Versioning**: Track format evolution across different conversations
2. **Validation**: Detect corruption or modification through main and section-specific checksums
3. **Prioritization**: Focus on the most important information first
4. **Typing**: Clear distinction between different data types
5. **Hierarchy**: Better representation of complex, nested relationships
6. **Compression**: Maximum information density for context window efficiency
7. **References**: Avoid duplication through internal cross-references
8. **Instructions**: Guide the assistant on how to parse and prioritize information
9. **Confidence Tracking**: Distinguish between verified, probable, and speculative information
10. **Action Management**: Track pending tasks, assignments, and completions
11. **Temporal Awareness**: Mark expiration dates for time-sensitive information
12. **Change History**: Record modifications to the document over time
13. **Multi-format Support**: Specialized handling for complex data like code blocks
14. **Relationship Mapping**: Explicit indicators for dependencies and connections between data points
15. **Cross-sectional Tagging**: Identify related concepts across different sections

## Notes
- All tested models appear able to interpret EBF data with no explanation, but this has not been tested thoroughly (generating fresh EBF after exceeding context window could be good here)
- There is no benchmark yet.
- This format has been found portable between different LLMs with minimal to no loss in fidelity.
- The format has been confirmed appears working when using EBC with programming data included.
- Partially relies on the LLM having an understanding of the current date, which is historically not always the case.

## Tested Working
### Both generating and interpreting:
- ChatGPT 4o (Free, 2025-03-20)
- ChatGPT 4o Reasoning (Free, 2025-03-26)
- Claude 3.7 Sonnet (Paid, 2025-03-20)
- Claude 3.7 Sonnet Extended Thinking (Paid, 2025-03-26)
- DeepSeek V3 (Free, 2025-03-26)
- DeepSeek V3 R1 (Free, 2025-03-26)
- Grok 3 Beta (Paid, 2025-03-26)
- Gemini 2.0 Flash (Free, 2025-04-01)
### Interpreting only:
- Claude 3.5 Haiku (2025-03-26)
- Claude 3.5 Sonnet (2025-03-26)
- Claude 3 Opus (2025-03-26)

### Interpreting raw EBF with no header:
- ChatGPT 4o (Free, 2025-03-20)
- ChatGPT 4o Reasoning (Free, 2025-03-26)
- Claude 3.7 Sonnet (Paid, 2025-03-20)
- Claude 3.7 Sonnet Extended Thinking (Paid, 2025-03-26)
- Claude 3.5 Haiku (2025-03-26)
- Claude 3.5 Sonnet (2025-03-26)
- Claude 3 Opus (2025-03-26)
- DeepSeek V3 (Free, 2025-03-26)
- DeepSeek V3 R1 (Free, 2025-03-26)
- Grok 3 Beta (Paid, 2025-03-26)
- Copilot Work (Free, 2025-04-01)
- Gemini 2.0 Flash (2025-04-01) **_NOTE:_** _Slight artifacts in continued conversation, such as referring to `~ACTIONS`_
   
## Changelog
- 2025-04-01: Added Copilot & Gemini testing
- 2025-03-26: Moved changelog into README
- 2025-03-26: Opened a pile of Issues for the sake of improvement
- 2025-03-26: Added notes section regarding unexpected results
- 2025-03-26: Added additional testing data
- 2025-03-26: Added changelog and testing data
- 2025-03-20: Added README and license
- 2025-03-20: Added more complexity and refined prompts
- 2025-03-20: Forked from 'main' to create v1.1
