## Changelog

### Version 1.1 (20 March 2025)

#### New Features

- **Extended Data Types**
  - Added `$a` for homogeneous arrays
  - Added `$o` for object structures with properties
  - Added `$c` for code blocks and multi-line content
  - Enhanced `$l` definition for heterogeneous lists
  - Enhanced `$m` definition for map/dictionary structures

- **Confidence Level System**
  - Added `^C` for confirmed/verified information
  - Added `^P` for probable information 
  - Added `^S` for speculative information

- **Relationship Indicators**
  - Added `>>` for directional relationships
  - Added `<<>>` for bidirectional relationships
  - Added `==>` for causal relationships

- **Temporal Awareness**
  - Added `#T+{n}{unit}` for future-valid information
  - Added `#T-{n}{unit}` for past information references
  - Added `#valid_until:{date}` for expiration dates

- **Cross-referencing System**
  - Added `%tag` for concept tagging across sections
  - Added `&ref` for internal reference markers (distinct from subsections)

- **New Sections**
  - Added `~ACTIONS` section for tracking tasks and status
  - Added `~HISTORY` section for version tracking

- **Validation Improvements**
  - Added section-specific checksums `[~SECTION:checksum]`
  - Standardized on SHA-256 algorithm (truncated to 8 characters)

#### Documentation Improvements

- Added quickstart template for new users
- Enhanced example structure with consistent formatting
- Added concrete examples in the "How to Use" section
- Expanded prompt template with clearer instructions
- Added detailed instructions for validation and updating
- Clarified symbol usage to avoid parsing ambiguities

#### Format Optimizations

- Standardized hierarchical structuring
- Improved readability with optional formatting guidelines
- Resolved potential symbol conflicts
- Enhanced metadata capabilities in ~META section

#### Compatibility Notes

- Format remains backward compatible with v1.0 parsers
- Version identifier in header clearly marks v1.1 documents
- ~HISTORY section tracks version changes when updating from older formats

### Version 1.0 (20 March 2025) - Initial Release

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
