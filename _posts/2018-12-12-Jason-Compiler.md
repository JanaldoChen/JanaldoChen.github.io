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

### A sample code of Jason
```c
PROGRAM prog;
declare 
    integer a;
    integer b;
procedure proc;
    parameters
        integer x;
        integer y;
    declare 
        integer z;
    begin
        while x ! 0 do
            if x < 25   then set y = 100 – 4*x
                        else set z = 4.5*x + 1.0
            endif;
        write y;
        read x;
        endwhile
    end;
begin
    read a;
    set b = 99;
    call proc (a, b);
    write b
end.
```

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
<div class="py-5">
    <div class="container">
      <div class="row">
        <div class="col-md-8" ><img class="img-fluid d-block" src="http://pjnlug6p5.bkt.clouddn.com/StringName.png"></div>
      </div>
    </div>
  </div>

#### The Hash table and name table

<div class="py-5">
    <div class="container">
      <div class="row">
        <div class="col-md-8" ><img class="img-fluid d-block" src="http://pjnlug6p5.bkt.clouddn.com/HashName.png"></div>
      </div>
    </div>
</div>


#### The basic organization of the symbol table

<div class="py-5">
    <div class="container">
      <div class="row">
        <div class="col-md-8" ><img class="img-fluid d-block" src="http://pjnlug6p5.bkt.clouddn.com/SymbolTable.png"></div>
      </div>
    </div>
</div>

#### The interaction between the Symbol Table ans the phases of a compiler

Virtually every phase of the compiler will use the symbol table:

* The initialization phase will place keywords, operators, and standard identifiers in it.
* The scanner will place user-defined identifiers and literals in it and will return the corresponding token.
* The parser uses these tokens to create the parse tree, the product of the syntactic analysis of the program.
* The semantic action routines place data type in its entries and uses this information in performing basic type-checking.
* The intermediate code generation phase use pointers to entries in the symbol table in creating the intermediate representation of the program.
* The object code generation phase uses pointers to entries in the symbol table in allocating storage for its variables and constants, as well as to store the addresses of its prodedures and functions.

#### Initializing the Symbol Table

* Before we can start scanning the program, we must initialize the symbol table.
* Initialization involves installing all reserved words, standard identifiers, and operators.
* In production compiler, the symbol table has these stored in memory before execution. 

#### Searching for lexemes and installing them

* After the scanner assembles the lexeme, it must determine if it is already in the symbol table. If so and if it is declared within the current scope, its token is already determined.
* If the lexeme is not already in the symbol table, then it must be installed and assigned a token (usually $$Identifier$$ ). This also requires that we create an entry in the attribute table.
* If the lexeme is already in the symbol table but does not belong to the current scope, a new scope must be opened for it.

#### Creating an attribute table entry and setting attributes

* Once the lexeme is installed in the name table, an entry must be created in the attribute table for it.
* The function $$ InstallAttrib $$ will create the entry, initialize a pointer back to the name table, and set the symbol type and the data type as unknown.
* The function $$ SetAttrib $$ will set the symbol type, token class and the data type as well as link the entry to any others for that scope.

#### Installing data type

* This is done by the semantic analyzer during the processing of declarations.
* Until this point, identifiers have their data type listed as unknown.
* The same function,$$ InstallDataType $$, installs the the correct symbol type as well as the correct data.

#### Scope rules

* Block-structured languages, such as ALGOL, PL/I, Pascal and C, have to allow for the fact that the same identifier may mean completely different things in different scopes.
* Strictly speaking, a block is a program structure in which there can be declarations. E.g., Although it is unusual to place declarations in C blocks (excluding the main block of a function), it is perfectly legal.
* Because such languages resolve name conflicts using a most closely nested scope rule, languages with static scoping rules must have a mechanism for temporarily hiding a variable in an outer scope.
* In implementing a symbol table, this mean either:
  * Separate symbol tables for each scope or
  * identifying the scope to which each identifier belongs.

<div class="py-5">
    <div class="container">
      <div class="row">
        <div class="col-md-6" style=""> <img class="img-fluid d-block" src="http://pjnlug6p5.bkt.clouddn.com/TraceScope1.png" > </div>
      </div>
    </div>
  </div>

<div class="py-5">
    <div class="container">
      <div class="row">
        <div class="col-md-6" style=""> <img class="img-fluid d-block" src="http://pjnlug6p5.bkt.clouddn.com/TraceScope2.png" > </div>
      </div>
    </div>
  </div>

Additionally, each identifier is marked with a pointer to the symbol table entry of the procedure which “owns” it.

#### Opening and closing the scope

Opening the scope involves:

* Creating a new entry in the attribute table.
* Readjusting the name table pointer to point to the new entry (and vice versa).
* Linking the new entry to the other entries in the new scope as well as to the other entries sharing its name.

Closing the scope involves:

- Readjusting the pointer in the name table to point to the next scope outwards for that identifier and possibly,
-  writing the data on disk and clearing the entry.

#### Procedure stack

* This is necessary because we need to be able to handle procedures within procedures
  within procedures.

* A stack provides the LIFO (last-in-first-out) structure that we need.

  ```c
  typedef struct {
      int  proc;
      int  sstart, snext;
  } procstackitem;
  
  typedef   struct    {
      int            top;
      procstackitem  item[MAXSTACK];
  }    procstack;
  ```

## Lexical Analysis

* Lexical analysis (or $$scanning$$) is the process by which the stream of characters is grouped into strings representing the words of a language (called $$lexemes$$) which correspond to specific grammatical elements of that language (called $$tokens$$)
* Tokens are the fundamental building blocks of a program’s grammatical structure, representing such basic elements as identifiers, numericliterals, and specific keywords and operators of the language.
* Lexemes are the character strings assembled from the character stream of a program, and the token represents what component of the program’s grammar they constitute.

### Tokens and Lexemes

* In many instances, there is a one-to-one correspondence between the lexemes and tokens for reserved words and operators.
* User-defined identifiers are usually assigned the token of $$Identifier$$.
* Numbers are usually assigned the token of NumericLiteral or something more specific like $$IntegerLiteral$$.
* Characters and strings are assigned the token of $$CharacterLiteral$$.

### Practical Issues in Lexical Analysis

There are several important practical issues that arise in the design of a scanner:

* Lookahead
* Case sensitivity
* Skipping of lead blanks and comments
* Use of first characters

#### Lookahead characters

Since you cannot determine if you have read beyond the end of a lexeme until you have done so, you must be prepared to handle the “lookahead” character. There are two approaches available:

* Start with a lookahead character and fetch a new one every time the lookahead character is “consumed” by the lexeme.
* Use two functions to manipulate the input stream, one to “get” the next character and one to “unget” the next character, returning it temporarily to the input stream.

#### Case sensitivity

* Although “a” and “A” are regarded as the same character in the English language, they are represented by different ASCII codes. For a compiler to be case insensitive, we need to consider these both as the same letter.
* The easiest way to do this is to convert all letters to the same case.
* Not all languages do this, e.g., C.

#### Skipping lead blanks and comments

* Before reading the first significant character in a lexeme, it necessary to skip past both lead blanks as well as comments.
* One must assume that the scanner can encounter either or both repeatedly and interchangeably before reading the first significant character.

#### Use of first character

* In most programming languages, the first character of a lexeme indicates the nature of the lexeme and token associated with it.

* In most instances, identifiers and reserved words begin with a letter (followed by zero or more letters and digits), numbers begin with a digit and operators begin with other characters.

  ##### Scanning for reserved words and identifiers

  * Once the scanner determines that the first character is a letter, it continues to read characters and concatenate them to the lexeme until it encounters a character other than a letter or digit.
  * If the resultant lexeme is not in the symbol table, it must be a new identifier.

  ##### Scanning for numeric literals

  * After determining that the lexeme begins with a digit, the scanner reads characters, concatenating
    them to the lexeme until it encounters a non-digit.
  * If it is a period, it will concatenate this to the lexeme and resume reading characters until it encounters another non-digit.
  * If it is an “E”, it must then read the exponent.
  * The token associated with the lexeme is either number or the number’s type.

  ##### Scanning for operators and characters literals

  * If the first character is neither a letter nor a digit, the lexeme must be one of the following:

    * an operator
    * a character literal
    * a string literal
  * In scanning an operator:
    * we should be cognizant of how many characters it may contain.
    * we may wish to hand-code the token that will be returned by the symbol table.
  * In scanning a literal, we read characters until encountering the appropriate closing quotation mark.

## Top-Down Parsing

* Top-down parsing is a parsing-method where a sentence is parsed starting from the root of the parse tree (with the “Start” symbol), working recursively down to the leaves of the tree (with the terminals).
* In practice, top-down parsing algorithms are easier to understand than bottom-up algorithms.
* Not all grammars can be parsed top-down, but most context-free grammars can be parsed bottomup. 

### The LL(1) Jason Grammar

<div>
\begin{align}

 Program : : = & \ Header \ DeclSec \ Block \ . \\
 Header : : = & \ program \ identifier \ ; \\
 DeclSec : : = & \ declare \ VarDecls \ ProcDecls \ \\
 DeclSec : : = & \ ε \\
 VarDeclSec : : = & \ VarDecl \ MoreVarDels \\
 MoreVarDecls : : = & \ VarDecl \ MoreVarDecls \\
 MoreVarDecls : : = & \ ε \\
 VarDecl : : = & \ DataType \ IdList \ ; \\
 DataType : : = & \ real \\
 DataType : : = & \ integer \\
 IdList : : = & \ identifier \ MoreIdList \\
 MoreIdList : : = & \ , \ identifier \ MoreIdList \\
 ProcDecls : : = & \ ProcHeader \ ProcDeclSec \ Block \ ; \\
 ProcHeader : : = & \ procedure \ identifier \ ; \\
 ProcDeclSec : : = & \ ParamDeclSec \ DeclSec \\
 ParamDeclSec : : = & \ parameters \ ParamDecls \\
 ParamDeclSec : : = & \ ε \\
 ParamDecls : : = & \ ParamDecl \ MoreParamDecls \\
 MoreParamDecls : : = & \ ParamDecl \ MoreParamDecls \\
 MoreParamDecls : : = & \ ε \\
 ParamDecl : : = & \ DataType \ identifier \ ; \\
 Block : : = & \ begin \ Statements \ end \\
 Statements : : = & \ Statement \ MoreStatements \\
 MoreStatements : : = & \ ; \ Statement \ MoreStatements \\
 MoreStatements : : = & \ ε \\
 Statement : : = & \ read \ identifier \\
 Statement : : = & \ set \ identifier \ = \ Expression \\
 Statement : : = & \ write \ identifier \\
 Statement : : = & \ if \ Condition \ then \ Statements \ ElseClause \\
 Statement : : = & \ while \ do \ Statements \ endwhile \\
 Statement : : = & \ until \ Condition \ do \ Statements \ enduntil \\
 Statement : : = & \ call \ identifier \ Arglist \\
 Statement : : = & \ ε \\
 Expression : : = & \ Term \ MoreExpression \\
 MoreExpression : : = & \ AddOp \ Term \ MoreExpression \\
 MoreExpression : : = & \ ε \\
 Term : : = & \ Factor \ MoreTerm \\
 MoreTerm : : = & \ MultOp \ Factor \ MoreTerm \\
 MoreTerm : : = & \ ε \\
 Factor : : = & \ identifier \\
 Factor : : = & \ constant \\
 Condition : : = & \ Expression \ Relop \ Expression \\
 AddOp : : = & \ + \\
 AddOp : : = & \ - \\
 MultOp : : = & \ * \\
 MultOp : : = & \ / \\
 RelOp : : = & \ = \\
 RelOp : : = & \ ! \\
 RelOp : : = & \ > \\
 RelOp : : = & \ < \\
 ArgList : : = & \ ( \ Argumnets \ ) \\
 ArgList : : = & \ ε \\
 Arguments : : = & \ identifier \ MoreArguments \\
 MoreArguments : : = & \ , \ identifier \ MoreArguments \\
 ElseClause : : = & \ else \ Statements \ endif \\
 ElseClause : : = & \ endif \\

\end{align}
</div>


### FIRST set for Jason

### Determining the FIRST set for Jason

### Implementing the Parse Table
```c
const int prodtable[][NUMTOKENS+3] = {
    /*Program*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*Header*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*DeclSect*/  { 4, 0, 3, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*VarDeclSect*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 5, 0, 0, 0, 0, 5, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*MoreVarDecls*/{ 7, 0, 0, 0, 0, 0, 0, 0, 0, 0, 6, 0, 7, 0, 0, 6, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*VarDecl*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 8, 0, 0, 0, 0, 8, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*DataType*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,10, 0, 0, 0, 0, 9, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*IdList*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,11,
        0},
    /*MoreIdList*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0,13,12, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*ProcDecls*/ {15, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,14, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*ProcDecl*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,16, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*ProcHeader*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,17, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*ProcDeclSect*/{18, 0,18, 0, 0, 0, 0, 0, 0, 0, 0,18, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*ParamDeclSec*/{20, 0,20, 0, 0, 0, 0, 0, 0, 0, 0,19, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*ParamDecls*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,21, 0, 0, 0, 0,21, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*MoreParmDcls*/{23, 0,23, 0, 0, 0, 0, 0, 0, 0,22, 0, 0, 0, 0,22, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*ParamDecl*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,24, 0, 0, 0, 0,24, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*Block*/ {25, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*Statements*/  { 0,26, 0, 0, 0, 0, 0, 0, 0,26, 0, 0, 0, 0,26, 0,26, 0,26,
        26,26, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*MoreStatmnts*/{ 0, 0, 0, 0,28,28,28,28,28, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0,27, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*Statement*/ { 0,35, 0, 0,36,36,36,36,36,32, 0, 0, 0, 0,29, 0,30, 0,34,
        33,31, 0, 0, 0, 0, 0,36, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        
        0},
    /*Expression*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,37,
        37},
    /*MoreExpr.*/ { 0, 0, 0,39,39,39,39,39,39, 0, 0, 0, 0, 0, 0, 0, 0,39, 0,
        0, 0, 0,38,38, 0,39,39, 0, 0,39,39,39, 0, 0, 0, 0,
        0},
    /*Term*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,40,
        40},
    /*MoreTerm*/  { 0, 0, 0,42,42,42,42,42,42, 0, 0, 0, 0, 0, 0, 0, 0,42, 0,
        0, 0,41,42,42,41,42,42, 0, 0,42,42,42, 0, 0, 0, 0,
        0},
    /*Factor*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,43,
        44},
    /*Condition*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,45,
        45},
    /*AddOp*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0,46,47, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*MultOp*/  { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0,48, 0, 0,49, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0},
    /*RelOp*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0,50, 0, 0, 0,52,53,51, 0, 0, 0, 0,
        0},
    /*ArgList*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0,55, 0, 0, 0, 0, 0,54, 0, 0, 0,
        0},
    /*Arguments*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,56,
        0},
    /*MoreArgs.*/ { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0,57, 0, 0, 0, 0, 0,58, 0, 0,
        0},
    /*ElseClause*/  { 0, 0, 0, 0,59, 0,60, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
        0}
};
```

### The Parsing Algorithm
```
Place the start symbol in a node and push it onto the stack.
Fetch a token
REPEAT
  Pop a node from the stack
  IF it contains a terminal, match it to the current token (no match indicates a parsing error) and fetch another token
  ELSE IF it contains a nonterminal, look it up in the production table using the nontermina and the current token. Place the variables in REVERSE order on the stack
UNTIL the stack is empty
```

## Semantic Analysis

### What is Semantic Analysis?

Semantic analysis is the task of ensuring that the declarations and statements of a program are semantically correct, i.e, that their meaning is clear and consistent with the way in which control structures and data types are supposed to be used.

### What Does Semantic Analysis Involve?

Semantic analysis typically involves:
  * *Type checking* – Data types are used in a manner that is consistent with their definition (i. e., only with compatible data types, only with operations that are defined for them, etc.)
  * *Label Checking* – Labels references in a program must exist.
  * *Flow control checks* – control structures must be used in their proper fashion (no GOTOs into a FORTRAN DO statement, no breaks outside a loop or switch statement, etc.)

### Where Is Semantic Analysis Performed in a Compiler?

* Semantic analysis is not a separate module within a compiler. It is usually a collection of procedures called at appropriate times by the parser as the grammar requires it.
* Implementing the semantic actions is conceptually simpler in recursive descent parsing because they
are simply added to the recursive procedures.
* Implementing the semantic actions in a tableaction driven LL(1) parser requires the addition of a third type of variable to the productions and the necessary software routines to process it.

### What Does Semantic Analysis Produce?

* Part of semantic analysis is producing some sort of representation of the program, either object code
or an intermediate representation of the program.
* One-pass compilers will generate object code without using an intermediate representation; code generation is part of the semantic actions performed during parsing.
* Other compilers will produce an intermediate representation during semantic analysis; most often it will be an abstract syntax tree or quadruples.

### What is an Attribute Grammar?

* An attribute grammar is an extension to a context-free grammar that is used to describe features of a
programming language that cannot be described in BNF or can only be described in BNF with great difficulty.
* Examples
  * Describing the rule that real variables can be assigned integer values but the reverse is not true is difficult to describe completely in BNF.
  * Sebesta says that the rule requiring that all variable must be declared before being used is impossible to describe in BNF.

#### The Attribute Grammar of Jason

<div>
\begin{align}

 Program : : = & \ Header \ DeclSec \ Block \ . \\
 Header : : = & \ program \ identifier \ &#64;AddProgName \ ; \\
 DeclSec : : = & \ declare \ VarDecls \ ProcDecls \ \\
 DeclSec : : = & \ ε \\
 VarDeclSec : : = & \ VarDecl \ MoreVarDels \\
 MoreVarDecls : : = & \ VarDecl \ MoreVarDecls \\
 MoreVarDecls : : = & \ ε \\
 VarDecl : : = & \ DataType \ IdList \ ; \\
 DataType : : = & \ real \ &#64;PushReral \\
 DataType : : = & \ integer \ &#64;PushInt \\
 IdList : : = & \ identifier \ &#64;DeclVar \ MoreIdList \\
 MoreIdList : : = & \ , \ identifier \ &#64;DeclVar \ MoreIdList \\
 MoreIdList : : = & \ &#64;PopType \\
 ProcDecls : : = & \ ProcHeader \ ProcDeclSec \ Block \ &#64;CloseProc \ ; \\
 ProcHeader : : = & \ procedure \ identifier \ &#64;AddProcName \ ; \\
 ProcDeclSec : : = & \ ParamDeclSec \ DeclSec \\
 ParamDeclSec : : = & \ parameters \ &#64;SetParamLink \ ParamDecls \\
 ParamDeclSec : : = & \ ε \\
 ParamDecls : : = & \ ParamDecl \ MoreParamDecls \\
 MoreParamDecls : : = & \ ParamDecl \ MoreParamDecls \\
 MoreParamDecls : : = & \ ε \\
 ParamDecl : : = & \ DataType \ identifier \ &#64;DeclParam \ ; \\
 Block : : = & \ begin \ &#64;StartBlock \ Statements \ end \ &#64;EndBlock \\
 Statements : : = & \ Statement \ MoreStatements \\
 MoreStatements : : = & \ ; \ Statement \ MoreStatements \\
 MoreStatements : : = & \ ε \\
 Statement : : = & \ read \ identifier \ &#64;GenRead \\
 Statement : : = & \ set \ identifier \ &#64;PushId \ = \ Expression \ &#64; GenAssn \\
 Statement : : = & \ write \ identifier \ &#64;GenWrite \\
 Statement : : = & \ if \ Condition \ then \ &#64;StartIf \ Statements \ ElseClause \\
 Statement : : = & \ while \ &#64;StartWhile \ do \ Statements \ endwhile \ &#64;FinishLoop \\
 Statement : : = & \ until \ &#64;PrepareLoop \ Condition \ &#64;StartUntil \ do \ Statements \ enduntil \ &#64;FinishLoop \\
 Statement : : = & \ call \ identifier \ &#64;StartCall \ Arglist \ &#64;EndCall \\
 Statement : : = & \ ε \\
 Expression : : = & \ Term \ MoreExpression \\
 MoreExpression : : = & \ AddOp \ Term \ &#64;CalcExpresion \ MoreExpression \\
 MoreExpression : : = & \ ε \\
 Term : : = & \ Factor \ MoreTerm \\
 MoreTerm : : = & \ MultOp \ Factor \ &#64;CalcTerm \ MoreTerm \\
 MoreTerm : : = & \ ε \\
 Factor : : = & \ identifier \ &#64;PushId \\
 Factor : : = & \ constant \ &#64;PushConst \\
 Condition : : = & \ Expression \ Relop \ Expression \ &#64;EvalCondition \\
 AddOp : : = & \ + \ &#64;PushAddOp \\
 AddOp : : = & \ - \ &#64;PushAddOp \\
 MultOp : : = & \ * \ &#64;PushMultOp \\
 MultOp : : = & \ / \ &#64;PushMultOp \\
 RelOp : : = & \ = \ &#64;PushRelOp \\
 RelOp : : = & \ ! \ &#64;PushRelOp \\
 RelOp : : = & \ > \ &#64;PushRelOp \\
 RelOp : : = & \ < \ &#64;PushRelOp \\
 ArgList : : = & \ ( \ Argumnets \ ) \\
 ArgList : : = & \ ε \\
 Arguments : : = & \ identifier \ &#64;AddArgument \ MoreArguments \\
 MoreArguments : : = & \ , \ identifier \ &#64;AddArgument \ MoreArguments \\
 MoreArguments : : = & \ &#64;EndArgList \\
 ElseClause : : = & \ else \ &#64;StartElse \ Statements \ endif \ &#64;FinishIf \\
 ElseClause : : = & \ endif \ &#64;FinishIf \\

\end{align}
</div>

```c
const struct prodarraytype  prodarray[] =   {
    /* Program */ {Nonterm, NTHeader},    {Nonterm, NTDeclSec},
    {Nonterm, NTBlock},             {Term, tokperiod},
    
    /* Header */  {Term, tokprogram},             {Term, tokidentifier},
    {Action, AcAddProgName},  {Term, toksemicolon},
    
    /* DeclSec */ {Term, tokdeclare},   {Nonterm, NTVarDecls},
    {Nonterm, NTProcDecls},
    /* DeclSec */ /* <Nil> */
    
    /* VarDeclSec*/ {Nonterm, NTVarDecl},     {Nonterm, NTMoreVarDecls},
    
    /*MoreVarDcls*/ {Nonterm, NTVarDecl},     {Nonterm, NTMoreVarDecls},
    /*MoreVarDcls*/ /* <Nil> */
    
    /* VarDecl */ {Nonterm, NTDataType},    {Nonterm, NTIdList},
    {Term, toksemicolon},
    
    /* DataType */  {Term, tokreal},    {Action, AcPushReal},
    /* DataType */  {Term, tokinteger},   {Action, AcPushInt},
    
    /* IdList */  {Term, tokidentifier},    {Action, AcDeclVar},
    {Nonterm, NTMoreIdList},
    
    /* MoreIdLst*/  {Term, tokcomma},   {Term, tokidentifier},
    {Action, AcDeclVar},    {Nonterm, NTMoreIdList},
    /* MoreIdList*/ {Action, AcPopType},
    
    /* ProcDecls*/  {Nonterm, NTProcDecl},    {Nonterm, NTProcDecls},
    /* ProcDecls*/  /* <Nil> */
    
    /* ProcDecl*/ {Nonterm, NTProcHeader},  {Nonterm, NTProcDeclSec},
    {Nonterm, NTBlock},     {Action, AcCloseProc},
    {Term, toksemicolon},
    
    /* ProcHeader*/ {Term, tokprocedure},     {Term, tokidentifier},
    {Action, AcAddProcName},  {Term, toksemicolon},
    
    /* ProcDeclSec*/{Nonterm, NTParamDeclSec},  {Nonterm, NTDeclSec},
    
    /* ParamDclSec*/{Term, tokparameters},    {Action, AcSetParamLink},
    {Nonterm, NTParamDecls},
    /* ParamDclSec*//* <Nil> */
    
    /* ParamDecls*/ {Nonterm, NTParamDecl},   {Nonterm, NTMoreParamDecls},
    
    /*MorePrmDcls*/ {Nonterm, NTParamDecl},   {Nonterm, NTMoreParamDecls},
    /*MorePrmDcls*/ /* <Nil> */
    
    /* ParamDecl */ {Nonterm, NTDataType},    {Term, tokidentifier},
    {Action, AcDeclParam},    {Term, toksemicolon},
    
    /* Block */ {Term, tokbegin},   {Action, AcStartBlock},
    {Nonterm, NTStatements},  {Term, tokend},
    {Action, AcEndBlock},
    
    /* Statements*/ {Nonterm, NTStatement},   {Nonterm, NTMoreStatements},
    
    /*MoreStatmnts*/{Term, toksemicolon},   {Nonterm, NTStatement},
    {Nonterm, NTMoreStatements},
    /*MoreStatmnts*//* <Nil> */
    
    /* Statement */ {Term, tokread},    {Term, tokidentifier},
    {Action, AcGenRead},
    /* Statement */ {Term, tokset},     {Term, tokidentifier},
    {Action, AcPushId},   {Term, tokequals},
    {Nonterm, NTExpression},  {Action, AcGenAssn},
    /* Statement */ {Term, tokwrite},     {Term, tokidentifier},
    {Action, AcGenWrite},
    /* Statement */ {Term, tokif},      {Nonterm, NTCondition},
    {Term, tokthen},    {Action, AcStartIf},
    {Nonterm, NTStatements},  {Nonterm, NTElseClause},
    /* Statement */ {Term, tokwhile},   {Action, AcPrepareLoop},
    {Nonterm, NTCondition},   {Action, AcStartWhile},
    {Term, tokdo},      {Nonterm, NTStatements},
    {Term, tokendwhile},    {Action, AcFinishLoop},
    /* Statement */ {Term, tokuntil},   {Action, AcPrepareLoop},
    {Nonterm, NTCondition},   {Action, AcStartUntil},
    {Term, tokdo},      {Nonterm, NTStatements},
    {Term, tokenduntil},    {Action, AcFinishLoop},
    /* Statement */ {Term, tokcall},    {Term, tokidentifier},
    {Action, AcStartCall},    {Nonterm, NTArglist},
    {Action, AcEndCall},
    /* Statement */ /* <Nil> */
    
    /* Expression */{Nonterm, NTTerm},    {Nonterm, NTMoreExpression},
    
    /* MoreExpr */  {Nonterm, NTAddOp},   {Nonterm, NTTerm},
    {Action, AcCalcExpression}, {Nonterm, NTMoreExpression},
    /* MoreExpr */  /* <Nil> */
    
    /* Term */      {Nonterm, NTFactor},    {Nonterm, NTMoreTerm},
    
    /* MoreTerm */  {Nonterm, NTMultOp},    {Nonterm, NTFactor},
    {Action, AcCalcTerm},   {Nonterm, NTMoreTerm},
    /* MoreTerm */  /* <Nil> */
    
    /* Factor */  {Term, tokidentifier},    {Action, AcPushId},
    /* Factor */  {Term, tokconstant},    {Action, AcPushConst},
    
    /* Condition */ {Nonterm, NTExpression},  {Nonterm, NTRelOp},
    {Nonterm, NTExpression},  {Action, AcEvalCondition},
    
    /* AddOp */ {Term, tokplus},    {Action, AcPushAddOp},
    /* AddOp */ {Term, tokminus},   {Action, AcPushAddOp},
    
    /* MultOp */  {Term, tokstar},    {Action, AcPushMultOp},
    /* MultOp */  {Term, tokslash},   {Action, AcPushMultOp},
    
    /* RelOp */ {Term, tokequals},    {Action, AcPushRelOp},
    /* RelOp */ {Term, toknotequal},    {Action, AcPushRelOp},
    /* RelOp */ {Term, tokgreater},   {Action, AcPushRelOp},
    /* RelOp */ {Term, tokless},    {Action, AcPushRelOp},
    
    /* ArgList */ {Term, tokopenparen},   {Nonterm, NTArguments},
    {Term, tokcloseparen},
    /* ArgList */ /* <Nil> */
    
    /* Arguments */ {Term, tokidentifier},    {Action, AcAddArgument},
    {Nonterm, NTMoreArguments},
    
    /* MoreArgs */  {Term, tokcomma},   {Term, tokidentifier},
    {Action, AcAddArgument},  {Nonterm, NTMoreArguments},
    /* MoreArgs */  {Action, AcEndArgList},
    
    /* ElseClause */{Term, tokelse},    {Action, AcStartElse},
    {Nonterm, NTStatements},  {Term, tokendif},
    {Action, AcFinishIf},
    
    /* ElseClause */{Term, tokendif},   {Action, AcFinishIf}
};

struct prodindexrec {
    int prstart, prlength;
};

const struct prodindexrec prodindex[] = {
    {0, 0},   {0, 4},   {4, 4},   {8, 3},  {11, 0},  {11, 2},
    {13, 2},  {15, 0},  {15, 3},  {18, 2},  {20, 2},  {22, 3},
    {25, 4},  {29, 1},  {30, 2},  {32, 0},  {32, 5},  {37, 4},
    {41, 2},  {43, 3},  {46, 0},  {46, 2},  {48, 2},  {50, 0},
    {50, 4},  {54, 5},  {59, 2},  {61, 3},  {64, 0},  {64, 3},
    {67, 6},  {73, 3},  {76, 6},  {82, 8},  {90, 8},  {98, 5},
    {103, 0}, {103, 2}, {105, 4}, {109, 0}, {109, 2}, {111, 4},
    {115, 0}, {115, 2}, {117, 2}, {119, 4}, {123, 2}, {125, 2},
    {127, 2}, {129, 2}, {131, 2}, {133, 2}, {135, 2}, {137, 2},
    {139, 3}, {142, 0}, {142, 3}, {145, 4}, {149, 1}, {150, 5},
    {155, 2}
};
```

## Intermediate Code Generation

* The semantic processing procedures need to create some intermediate representation of the program.
* One such representation is called quadruples or threeaddress codes.
  * Quadruples contain an assembler-level operator and three operands which do not depend on any particular machine architecture.
  * These can include:
    * assignment (for scalar values or array elements)
    * the arithmetic operations
    * conditional and unconditional gotos and their labels
    * identifying arguments and calling procedures

### Quadruples

* Quadruples consist of four fields: one operation and up to three operands.
* Quadruples also known as three address codes.
* Quadruples can be implemented using an enumerated type to represent the operations and pointers to symbol table entries to represent the operands.

#### A sample program in Jason
Source Code
```
PROGRAM prog;
declare 
    integer i;
    real x, y;
procedure readnums;
    parameters
        integer i;
        real y;
    begin
        read i;
        read y
    end;
begin
    call readnums(i, y);
    set x = i + y;
    write x
end.
```
The Quadruples
```
0 <label, readnums,_,_,>
1 <read, i,_,_,>
2 <read, y,_,_,>
3 <return, _,_,_,>
4 <label, prog,_,_,>
5 <arg, i,_,_,>
6 <arg, y,_,_,>
7 <call, readnums,_,_,>
8 <call, temp_0,_float,i>
9 <+, x,temp_0,y>
10  <write, x,_,_,>
11  <return, _,_,_,>
```
The  Intermediate Code
```
0 readnums:
1   read i 
2   read y 
3   return 
4 prog:
5   arg i 
6   arg y 
7   call readnums 
8   temp_0 := _float(i)
9   x := temp_0 + y
10    write x 
11    return 
```
#### Generating Quadruples

* Expressions
  ```
  set z = a * x + b * y
  ```
  ```
  t1 := a * x
  t2 := b * y
  t3 := t1 + t2
  ```
* IF-THEN-ELSE-ENDIF
  ```
  if x > y then set z = x
           else set z = y
  endif;
  ```
  ```
    if x <= y goto L1
    z := x
    goto L2
  L1:
    z := y
  L2:
  ``` 
* IF-THEN-ENDIF
  ```
  if y > 0 then set z = x/y
  endif;
  ```
  ```
    if y <= 0 goto L3
    z := x/y
  L3:
  ```
* WHILE-DO
  ```
  while x > 0 then set x = x/2.0
  endwhile;
  ```
  ```
  L4:
    if x <= 0 goto L5
    x := x/2.0
    goto L4
  L5:
  ```
* Call ProId(x, y)
  ```
  call ProId(x, y);
  ```
  ```
  arg x
  arg y
  call ProId
  ```
* A Procedure
  ```
  procedure ProcId;
  parameters
    integer x;
    real y;
  begin ... end;
  ```
  ```
  ProId:
    param x
    param y
    ...
    return
  ```

### Changing The Parsing Algorithm TO Accommodate Semantic Actions
```
Processing context-free expressions requires the use of a stack. The Parsing algorithm uses a stack:
Place the start symbol in a node and push it onto the stack.
Fetch a token
REPEAT
  Pop a node from the stack
  IF it contains a terminal, match it to the current token (no match indicates a parsing error) and fetch another token
  ELSE IF it contains a nonterminal, look it up in the production table using the nonterminal and the current token. Place the variables in REVERSE order on the stack
  ELSE IF it contains an action, call the appropriate procedure to perform the action
UNTIL the stack is empty
```







