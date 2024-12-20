# Specification of the Hat programming language

The hat programming language is based on lambda calculus in continuation passing style (CPS).
This page describes a specification of the language.

# EBNF Syntax Definition

This document separates the **lexical definitions** (used for tokenization) and **syntactic definitions** (used for parsing).

---

## Lexical Definitions (Tokens)

```ebnf
Token        = Significant | Whitespace | Comment ;
Significant  = Symbol | String | "(" | ")" ;

Symbol      = SymbolChar, { SymbolChar } ;
SymbolChar  = Letter | Digit | SpecialChar ;
Letter      = "a"..."z" | "A"..."Z" | "_" ;
Digit       = "0"..."9" ;
SpecialChar = "!" | "?" | "*" | "+" | "-" | "/" | "=" | "<" | ">" | "$" | "%" | "&" | "|" | "." | "^" ;

String      = '"', { StringCharacter }, '"' ;
StringCharacter = Character | EscapeSequence ;
Character   = ? any valid character except '"', '\', and Newline ? ;
EscapeSequence  = "\", ( '"' | "\" | "n" | "t" | "r" ) ;

Whitespace  = " " | "\t" | Newline ;
Newline     = "\n" | "\r\n" ;

Comment     = LineComment | BlockComment ;
LineComment = ";", { LineCommentCharacter }, Newline ;
LineCommentCharacter = ? any valid character except Newline ? ;

BlockComment = "#|", { BlockCommentContent }, "|#" ;
BlockCommentContent = BlockCommentText | BlockComment ;
BlockCommentText    = ? character sequence not containing #| or |# ? ;
```

## Syntactic Definitions (Grammar)

```ebnf
HatProgram  = { Statement } ;
Statement   = Includer | Definition ;
Includer    = "(", "include", String, ")" ;
Definition  = "(", "define", Symbol, Function, ")" ;
Function    = "^", Head, Body ;
Head        = "(", { Symbol }, ")" ;
Body        = [ LineMetadata ], Expression, { Expression }, [ Function ] ;
Expression  = Symbol | String | "(", Function, ")" ;
LineMetadata = "(", "_LINE_", Symbol, { String }, ")" ;
```

---

# EBNF Notation Used

The EBNF (Extended Backus-Naur Form) notation used in the definitions adheres to common conventions for describing the syntax of formal languages.
Below is an explanation of the elements and constructs in this EBNF:

## Key Elements and Constructs

### Rule Definition

Syntax rules are defined with the format:

```ebnf
RuleName = Expression ;
```

- The left-hand side (`RuleName`) specifies the name of the rule.
- The right-hand side (`Expression`) defines its structure.

### Concatenation

Elements listed sequentially must appear in the given order:

```ebnf
Expression = Element1, Element2 ;
```

- Example: `Element1` is followed by `Element2`.

### Alternatives

Multiple options are separated by `|`, indicating that any one of them can match:

```ebnf
Expression = Option1 | Option2 ;
```

- Example: Matches either `Option1` or `Option2`.

### Repetition

`{ ... }` indicates zero or more repetitions of the enclosed expression:

```ebnf
Expression = { Element } ;
```

- Example: Matches `Element` repeated zero or more times.

### Optional Elements

`[ ... ]` indicates that the enclosed expression is optional (zero or one occurrence):

```ebnf
Expression = [ Element ] ;
```

- Example: Matches `Element` if present, but it can be omitted.

### Groupings

Parentheses `( ... )` are used to group expressions and clarify precedence:

```ebnf
Expression = ( Element1, Element2 ) ;
```

- Example: Matches `Element1` followed by `Element2` as a single grouped unit.

### Terminal Symbols

Strings or characters enclosed in quotation marks represent literal values:

```ebnf
Terminal = "literal" ;
```

- Example: Matches the exact string `"literal"`.

### Character Ranges

Ranges specify a set of characters using the `...` operator:

```ebnf
Digit = "0"..."9" ;
```

- Example: Matches any single digit between `0` and `9`.

### Special Characters

Some non-printable or special characters (e.g., whitespace, newlines) are represented with descriptive names:

```ebnf
Newline = "\n" | "\r\n" ;
```

- Example: Matches a line feed (`\n`) or a carriage return followed by a line feed (`\r\n`).
