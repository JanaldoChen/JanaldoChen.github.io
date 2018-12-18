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
















