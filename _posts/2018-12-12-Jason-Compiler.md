---
layout: post
title:  "Jason Compiler"
date:   2018-12-12
excerpt: "Jason(Just Another Simple Original Notion) compiler construction"
tag:
- Jason
- Compiler
comments: false
---

# Jason Compiler

## Jason Language Defiition
The Jason(Just Another Simple Original Notion) programming language is an ALGOL-derived limited-purpose, programming language specifically designed to illustrate the principles of compiler construction.

### The Lexical Elements

#### The classes of token in Jason are:

* Keyword
* identifier
* constant
* operator

#### The Keywords of Jason are:
<table border = '0' style = "width:80%;align:center">
  <tr>
    <td> BEGIN  </td> <td> END  </td> <td> PARAMETERS </td> <td> SET </td>
  </tr>
  <tr>
    <td> CALL  </td> <td> ENDUNTIL  </td> <td> PROCEDURE </td> <td> THEN </td>
  </tr>
  <tr>
    <td> DECLARE  </td> <td> ENDWHILE  </td> <td> PROGRAM </td> <td> UNTIL </td>
  </tr>
  <tr>
    <td> DO  </td> <td> IF  </td> <td> READ </td> <td> WHILE </td>
  </tr>
  <tr>
    <td> ELSE  </td> <td> INTEGER  </td> <td> REAL </td> <td> WRITE </td>
  </tr>
  <tr>
    <td> END </td> <td>&nbsp;</td> <td>&nbsp;</td> <td>&nbsp;</td>
  </tr>
</table>

#### The Tokens of Jason are:
<div>
\begin{align}
 Identifer ::= & \ Letter(Letter|Digit)^* \\

 Letter ::= & \ A|B|C|D|E|F|G|H|I|J|K|L|M|N|O|P|Q|R|S|T|U|V|W|Y|Z \\

 Constant ::= & \ Digit^*\{.\}Digit^* \\

 Digit :: = & \ 0|1|2|3|4|5|6|7|8|9 \\

 Operator ::= & \ .|;|,|(|)|=|<|>|!|+|-|*|/ \\

\end{align}
</div>

### The Grammar of Jason
<div>
\begin{align}
 Program : : = & \ Header \ DeclSec \ Block \ . \\
 Header : : = & \ program \ identifier \ ; \\
 DeclSec : : = & \ declare \ VarDecls \ ProcDecls \ |\  ε \\
 VarDecls : : = & \ VarDecls \  VarDecl \ | \ VarDecl \\
 VarDecl : : = & \ DataType \ IdList ; \\
 DataType : : = & \ real \ | \ integer \\
 IdList : : = & \ IdList \ , \ identifier \ | \ identifier \\
 ProcDecls : : = & \ ProcDecls \ ProcDecl \ | \ ProcDecl \ | \ ε \\
 ProcDecl : : = & \ ProcHeader \ ProcDeclSec \ Block \ ;\\
 ProcHeader : : = & \ procedure \ identifier \ ; \\
 ProcDeclSec : : = & \ ParamDeclSec \ DeclSec\\
 ParamDeclSec : : = & \ parameters \ ParamDecls \ | \ ε\\
 ParamDecls : : = & \ ParamDecls \ ParamDecl \ | \ ParamDecl \\
 ParamDecl : : =& \ DataType \ identifier \ ; \\
 Block : : = & \ begin \ Statements \ end \\
 Statements : : = & \ Statements \ ; \ Statement \ | \ Statement\\
Statement : : = & \ read \ identifier \\
				& | \ set identifier = Expression \\
				& | \ write \ identifier \\
				& | \ if \ Condition \ then \ Statements \ ElseClause \\
				& | \ while \ Condition \ do \ Statements \ endwhile \\
				& | \ until \ Condition \ do \ Statements \ enduntil \\
				& | \ call \ identifier \ Arglist \\
				& | \ ε\\

 ElseClause ::= & \ else \ Statements \ endif \ | \ endif \\
 ArgList : : = & \ ( \ Arguments \ ) \ | \ ε\\
 Arguments : : = & \ Arguments \ , \ Argument \ | \ Argument \\
 Condition : : = & \ Expression \ RelOp \ Expression \\
 Expression : : = & \ Expression \ AddOp \ Term \ | \ Term \\\
 Term : : = & \ Term \ MultOp \ Factor \ | \ Factor \\
 Factor : : = & \ identifier \ | \ constant \\
 Rel Op : : = & \ = \ | \ ! \ | \ > \ | \ < \\
 AddOp :: = & \ + \ | \ - \\
 MultOp : : = & \ * \ | \ / \\
\end{align}
</div>

### The Symbol Table

The symbol table consists of four structures:

* a string table, contains  the lexemes as one long array of characters.
* a name table, containging the starting position of the lexemes within the string as their lengths.
* an attribute table, containing the various attributes of the lexemes, including the token associated with it.
* a hash table,  which points the name table.

#### The basic organization of the symbol table

![The basic organization of the symbol table.](http://pjnlug6p5.bkt.clouddn.com/SymbolTable.png)

#### The information stored in the symbol table

<table border ="0" frame="hsides">
    <tr>
        <th>Lexeme</th>
        <th>Token Class</th>
        <th>Symbol Type</th>
        <th>Data Type</th>
        <th>Value</th>
        <th>Scope</th>
    </tr>
    <tr>
      <td>While</td>
        <td>TokWhile</td>
        <td>Keyword</td>
        <td>None</td>
        <td>0</td>
        <td>Global</td>
    </tr>
    <tr>
      <td>+</td>
        <td>TokPlus</td>
        <td>Operator</td>
        <td>None</td>
        <td>0</td>
        <td>Global</td>
    </tr>
    <tr>
      <td>x</td>
        <td>TokIdentifier</td>
        <td>Variable</td>
        <td>Integer</td>
        <td>0</td>
        <td>Sample</td>
    </tr>
    <tr>
      <td>8</td>
        <td>TokConstant</td>
        <td>Literal</td>
        <td>Integer</td>
        <td>8</td>
        <td>Global</td>
    </tr>
</table>







