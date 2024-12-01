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

Digits      = Digit, { Digit } ;

Whitespace  = Space | Tab | Newline ;
Space       = " " ;
Tab         = "\t" ;
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

- The left-hand side (RuleName) specifies the name of the rule.
- The right-hand side (Expression) defines its structure.

### Concatenation

Elements listed sequentially must appear in the given order:

```ebnf
Expression = Element1, Element2 ;
```

- Example: Element1 is followed by Element2.

### Alternatives

Multiple options are separated by `|` &#124;, indicating that any one of them can match:

```ebnf
Expression = Option1 | Option2 ;
```

- Example: Matches either Option1 or Option2.
