# Specification of the Hat programming language

---
layout: default
---

# Specification of the Hat programming language

The hat programming language is based on lambda calculus in continuation passing style (CPS).
This page describes a specification of the language and differences from lambda calculus in direct style.

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
