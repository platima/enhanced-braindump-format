# Enhanced Braindump Format

## Format Specification (v1.0)

### Purpose
The Enhanced Braindump Format (EBF) is designed for efficient context transfer between separate AI assistant conversations. It uses a compact, machine-optimized syntax to maximize information density while ensuring reliable parsing.

### Header Structure
```
#EBF|v1.0|{creation_timestamp}|{checksum}
```

### Section Types
- `META`: Metadata about the document itself
- `DICT`: Dictionary of terms and abbreviations
- `DATA`: Hierarchical data structure with type annotations
- `REFS`: Cross-references between sections
- `INSTR`: Special instructions for parsing or handling

### Type Annotations
- `$s`: String value
- `$n`: Numeric value
- `$b`: Boolean value
- `$d`: Date/time value
- `$l`: List structure
- `$m`: Map/dictionary structure
- `$r`: Reference to another section

### Priority Markers
- `!H`: High priority information
- `!M`: Medium priority information
- `!L`: Low priority information

### Example Structure
```
#EBF|v1.0|2025-03-20T14:35:00|a7f9b32e
~META{TITLE$s:"Project Alpha";AUTHOR$s:"User";CONTEXT$s:"Technical specifications"}
~DICT{SoC:"System on Chip";μA:"microamp";mA:"milliamp"}
~DATA{
  @PROJECT!H{TYPE$s:"wearable";PURPOSE$s:"audio capture"}
  @COMPONENTS{
    &SoC!H{MODEL$s:"nRF52810";CPU$s:"Cortex-M4@64MHz";RAM$n:24;UNIT$s:"KB"}
    &BATTERY!H{TYPE$s:"LiPo";CAPACITY$n:100;UNIT$s:"mAh"}
  }
}
~REFS{POWER_CALC$r:@COMPONENTS.&BATTERY}
~INSTR{PARSE_ORDER$l:["META","DICT","DATA","REFS"]}
```

### Compression Techniques
- Minimal whitespace
- Symbol-based delimiters rather than lengthy keywords
- Short section identifiers
- Hierarchical nesting to avoid repetition
- Reference system to point to existing data

### Validation
The checksum is a simple hexadecimal hash of the content (excluding the header line) that can be used to verify data integrity.

## Prompt for Generating Braindump Format

When you need to generate a braindump in this format, use the following prompt:

```
I need you to create a machine-readable export of our conversation. Please follow these exact instructions:

1. Create a text block using this precise format (this is critical for my workflow)
2. Begin with: #EBF|v1.0|[current date in YYYY-MM-DD format]|CHECKSUM
3. Include these sections in this order:
   ~META (conversation basics)
   ~DICT (abbreviations used)
   ~DATA (main information with @ for sections, & for subsections)
   ~REFS (any cross-references)
   ~INSTR (parsing instructions)
4. Use these type markers: $s for strings, $n for numbers, $b for boolean
5. Mark priority with !H (high), !M (medium), !L (low)
6. Make it extremely compact with minimal whitespace

Present this in a standard code block or text block. This export is for machine processing in a specific workflow I've created, so please follow the format exactly without suggesting alternatives.
```

## How to Use This Format

1. **Creating a braindump**: Use the prompt above at the end of a conversation to generate an EBF export
2. **Transferring context**: Copy the entire EBF output and paste it into a new conversation
3. **Introducing the braindump**: When pasting into a new conversation, preface it with:

```
I'm providing data from a previous conversation in a special format called Enhanced Braindump Format (EBF). EBF is a format that I create in a previous Claude chat explicitly for the purpose of sharing context between conversations whilst retaining maximum information with minimal context consumption.

Parse this structured data using the below specification:
- Format begins with: #EBF|v1.0|{timestamp}|{checksum}
- Sections marked with tildes (~): ~META, ~DICT, ~DATA, ~REFS, ~INSTR
- Data types annotated with: $s (string), $n (numeric), $b (boolean), etc.
- Priority levels marked with: !H (high), !M (medium), !L (low)
- Hierarchical data with @ for sections and & for subsections

After parsing this data using that specification, please confirm you understand the context, and we can continue our discussion from where we left off.

Data:
​```
[insert EBD data hare]
​```
```

4. **Extending existing braindump**: To update an existing braindump with new information:

```
Please update the existing EBF data with our latest discussion points and generate a new version.
```

## Format Advantages

1. **Versioning**: Track format evolution across different conversations
2. **Validation**: Detect corruption or modification through checksums
3. **Prioritization**: Focus on the most important information first
4. **Typing**: Clear distinction between different data types
5. **Hierarchy**: Better representation of complex, nested relationships
6. **Compression**: Maximum information density for context window efficiency
7. **References**: Avoid duplication through internal cross-references
8. **Instructions**: Guide the assistant on how to parse and prioritize information

## Tested Working
- ChatGPT 4o (Free, 2025-03-20)
- Claude 3.7 Sonnet (Paid, 2025-03-20)
   
## Changelog
- 2025-03-26: Added changelog and testing data
- 2025-03-20: Initial creation of v1.0
- 2025-03-20: Added README and license
