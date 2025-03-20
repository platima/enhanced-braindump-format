## Changelog

### Version 1.0 (20th March 2025) - Initial Release

#### Core Features

- **Format Structure**
  - Defined header format `#EBF|v1.0|{timestamp}|{checksum}`
  - Established five core sections: ~META, ~DICT, ~DATA, ~REFS, ~INSTR
  - Implemented hierarchical structure with @ for sections and & for subsections

- **Data Type System**
  - Introduced basic type annotations: $s (string), $n (numeric), $b (boolean)
  - Added structural types: $d (date/time), $l (list), $m (map/dictionary), $r (reference)

- **Priority System**
  - Implemented priority markers: !H (high), !M (medium), !L (low)

- **Validation**
  - Added document-level checksum for integrity verification

- **Compression Techniques**
  - Minimal whitespace design
  - Symbol-based delimiters
  - Hierarchical nesting to avoid repetition
  - Reference system to reduce duplication

#### Documentation

- Basic prompt templates for generating EBF exports
- Instructions for transferring context between conversations
- Guidelines for introducing braindumps to new conversations
- Example structure with sample implementation

