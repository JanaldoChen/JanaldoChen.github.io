---
layout: post
title:  "Jason Compiler"
date:   2018-12-12
excerpt: "Jason Compiler"
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
<table border = '0' style = "width:80%">
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

## The Symbol Table

#### The information stored in the symbol table

The symbol table is used to store essential information about every symbol contained within the program. It will contain the following types of information for the input strings in a source program:

* The lexeme$$(input \ string)$$ itself
* Corresponding token
* Its semantic component $$(e.g., variable, operator, constant, procedure, etc.)$$
* Data type
* Value $$(for \ data \ type)$$
* Scope $$(e.g., Global, sample)$$
* Pointers to other entries$$(When \ necessary)$$

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

#### The types defiition

```c
enum tokentype {tokbegin, tokcall, tokdeclare, tokdo, tokelse, tokend,
    tokendif, tokenduntil,  tokendwhile, tokif, tokinteger,
    tokparameters, tokprocedure, tokprogram, tokread,
    tokreal, tokset, tokthen, tokuntil, tokwhile, tokwrite,
    tokstar, tokplus, tokminus, tokslash, tokequals,
    toksemicolon, tokcomma, tokperiod, tokgreater, tokless,
    toknotequal, tokopenparen, tokcloseparen, tokfloat,
    tokidentifier, tokconstant, tokerror, tokeof
};

enum symboltype {stunknown, stkeyword, stprogram,
    stparameter, stvariable, sttempvar,
    stconstant, stenum, ststruct, stunion, stprocedure,
    stfunction, stlabel, stliteral, stoperator
};

enum datatype  {dtunknown, dtnone, dtprogram, dtprocedure,
    dtinteger, dtreal
};

enum tagtype   {tint, treal};
union valtype  {
    int ival;
    float rval;
};

struct valrec  {
    enum tagtype tag;
    union valtype val;
};
```

### The structure of the symbol table

The symbol table consists of four structures:

* a string table, contains  the lexemes as one long array of characters.

  ```c
  char stringtable[STRINGTABLESIZE];
  ```

* a name table, containging the starting position of the lexemes within the string as their lengths.

  ```c
  struct nametabtype  {
      int strstart;
      int strlength;
      int symtabptr;
      int nextname;
  }; 
  struct nametabtype nametable[TABLESIZE];
  ```

* an attribute table, containing the various attributes of the lexemes, including the token associated with it.

  ```c
  struct symtabtype   {
      enum symboltype symtype;
      enum tokentype tok_class;
      enum datatype dataclass;
      int owningprocedure;
      int thisname;
      int outerscope, scopenext;
      struct valrec value;
      char label[LABELSIZE];
  };
  struct symtabtype symtab[SYMTABLESIZE];
  ```

* a hash table,  which points the name table.

  ```c
  int  hashtab[HASHTABLESIZE];
  ```

##### Hash Functions

* The hash function takes the lexeme and produces a nonunique value from it which is a starting point in a list of entries, one of which is the one that we seek.

* We want our hash value to produce as few hash collisions as possible.

* Our first temptation would be to use a hash function such as: Sum(ASCII(Lexeme)) MOD SomeValue
  This is not a good hash function because cat and act would have the same hash value.

  ```c
  /*
   * HashCode() -     A hashing function which uses the characters
   *        from the end of the token string.  The algorithm comes
   *        from Matthew Smosna of NYU.    
   */
  int  hashcode(char string[], int length)
  {
       int i, numshifts, startchar;
       unsigned code;
  
       numshifts = (int) min(length, (8*sizeof(int)-8));
       startchar = ((length-numshifts) % 2);
       code = 0;
  
       for (i = startchar;  i <= startchar + numshifts - 1;  i++)
            code = (code << 1) + string[i];
  
       return(code % HASHTABLESIZE);
  }
  ```
#### The string table and name table

![The string table and name table](http://pjnlug6p5.bkt.clouddn.com/StringName.png)

#### The Hash table and name table

![The Hash table and name table](http://pjnlug6p5.bkt.clouddn.com/HashName.png)


#### The basic organization of the symbol table

![The basic organization of the symbol table.](http://pjnlug6p5.bkt.clouddn.com/SymbolTable.png)

#### The interaction between the Symbol Table ans the phases of a compiler

Virtually every phase of the compiler will use the symbol table:

* The initialization phase will place keywords, operators, and standard identifiers in it.
* The scanner will place user-defined identifiers and literals in it and will return the corresponding token.
* The parser uses these tokens to create the parse tree, the product of the syntactic analysis of the program.
* The semantic action routines place data type in its entries and uses this information in performing basic type-checking.
* The intermediate code generation phase use pointers to entries in the symbol table in creating the intermediate representation of the program.
* The object code generation phase uses pointers to entries in the symbol table in allocating storage for its variables and constants, as well as to store the addresses of its prodedures and functions.


















