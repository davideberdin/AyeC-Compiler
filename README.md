# AyeC-Compiler
Compiler Project at Uppsala University

This project has been made by Anton Weber and Davide Berdin to fulfil the course of Compiler Project.
AyeC compiler will understand a subset of the C language called uC Language.

- The Parser uses JavaCC (https://javacc.java.net)
- The rest of the code has been written in Java 

Credits for the page goes to Alexandra Jimborean

<html xmlns="http://www.w3.org/1999/xhtml"><head>
<title>The uC language</title>
<meta name="version" content="S5 1.0">
<link rel="stylesheet" href="ui/my-slides.css" type="text/css" media="projection" id="slideProj">
<link rel="stylesheet" href="ui/opera.css" type="text/css" media="projection" id="operaFix">
<link rel="stylesheet" href="ui/print.css" type="text/css" media="print" id="slidePrint">
<link rel="stylesheet" href="ui/outline.css" type="text/css" media="screen" id="outlineStyle">
<script src="ui/slides.js" type="text/javascript"></script><style type="text/css"></style>
</head>
<body>

<div class="layout">

<div id="currentSlide"></div>
<div id="header"></div>
<div id="footer">
<div id="controls"></div>
</div>

</div>
<div class="presentation">

<div class="slide">
<p>
</p><h1>The uC language</h1>
<p></p>
<p>uC is a small subset of C, corresponding to a typical imperative,
procedural language.  The following sections describe in more detail
which language elements are present.</p>

<p>Every uC program is also a valid C program. The syntax and semantics
of uC is the same as that for full C, within the restrictions
described here.</p>

</div><div class="slide">
 <h1>Lexical elements</h1>

<ul>
<li class="incremental">Decimal integer constants and character literals.
A character literal contains either a single printable
character, or the <code>\n</code> escape sequence (line break).
A character literal denotes an integer constant whose
value is the representation code of the character.</li>

<li class="incremental">Alphanumeric identifiers: non-empty sequences of
letters or digits starting with a letter.
An underscore is treated as a letter.</li>

<li class="incremental">Keywords: <code>char</code>, <code>else</code>, <code>if</code>, <code>int</code>, <code>return</code>, <code>void</code>, and <code>while</code>.</li>

<li class="incremental">Special symbols:
<code>!=</code>, <code>!</code>, <code>&amp;&amp;</code>, <code>(</code>, <code>)</code>, <code>*</code>, <code>+</code>, <code>,</code> (comma), <code>-</code>,
<code>/</code>, <code>;</code>, <code>&lt;=</code>, <code>&lt;</code>, <code>==</code>,
<code>=</code>, <code>&gt;=</code>,
<code>&gt;</code>, <code>[</code>, <code>]</code>, <code>{</code>, <code>}</code>.</li>

<li class="incremental">White space characters: blank (32), newline (10),
carriage return (13), form feed (12), and tab (9).
The numbers are the ASCII representation codes for the
characters.

<p>Comments: <code>/*</code> followed by anything and terminated
by <code>*/</code>, and <code>//</code> followed by anything
and terminated by end of line.</p></li>
</ul>


</div><div class="slide">
 <h1>Syntax</h1>

<ul>
<li class="incremental">Primary expressions: constants, identifiers,
function calls, array indexing, and expressions
within parentheses.

<p>Unary expressions with the <code>!</code> and <code>-</code>
unary operators.</p>

<p>Binary expressions with the <code>+</code>, <code>-</code>,
<code>*</code>, <code>/</code>,
<code>&lt;</code>, <code>&gt;</code>,
<code>&lt;=</code>, <code>&gt;=</code>,
<code>==</code>,
<code>!=</code>,
<code>&amp;&amp;</code>, and <code>=</code> operators.</p>

<p>Statements: expression statements, the empty statement,
<code>if</code> statements with or without <code>else</code>,
<code>while</code> statements, <code>return</code> statements,
and compound statements (blocks), i.e., statements
enclosed in <code>{ }</code>.</p>

<p>Local variable declarations are only permitted at the
top-level function body block, not in nested blocks.</p></li>

<li class="incremental">Variable declarations: base type followed by
variable name, and for arrays followed by the
array size (an integer constant) in square brackets.

<p>Multi-dimensional arrays, pointers, and structures
are not included.</p>

<p>Initializes in variable declarations are not included.</p></li>

<li class="incremental">Function definitions: return type or <code>void</code>,
function name, parameter list, and body (compound
statement) in that order.

<p>The parameter list is either <code>void</code>, meaning no parameters, or a
sequence of variable declarations separated by <code>,</code> (comma).
An array parameter in a function head is written without array
size, i.e., with only the brackets.</p>

<p>An external (library) function can be declared by writing
a function definition without body, terminated with a
<code>;</code> (semi-colon).</p>

<p>Variable-arity functions are not included.</p></li>
</ul>


</div><div class="slide">
 <h1>Program execution</h1>

<ul>
<li class="incremental">Execution starts at the user-defined function <code>main</code>
which takes no parameters and returns <code>int</code>.
Execution ends when <code>main</code> returns.</li>

<li class="incremental">The standard library is uC-specific since uC excludes
variable-arity functions, and this makes <code>printf</code>
and <code>scanf</code>-like functions impossible.
To use the library, include the following declarations
at the start of your uC source file:</li>
</ul>


<pre class="example">        void putint(int i);       // prints to stdout
        void putstring(char s[]); // prints to stdout
        int getint(void);         // reads from stdin
        void getstring(char s[]); // reads from stdin
</pre>


</div><div class="slide">
 <h1>Notes</h1>


<ul>
<li class="incremental">Array parameters to functions are passed by reference,
as in full C, but the formal parameter still behaves
like an array variable and not as a pointer variable.
For example:
<pre class="example">void f(int a[], int b[])
{
    a[3] = 27;  // legal
    a = b;      // illegal in uC, legal in full C
}
</pre></li>
</ul>


</div><div class="slide">
 <h1>Example</h1>


<pre class="example">/* This is an example uC program. */
void putint(int i);

int fac(int n)
{
    if (n &lt; 2)
        return n;
    return n * fac(n - 1);
}

int sum(int n, int a[])
{
    int i;
    int s;

    i = 0;
    s = 0;
    while (i &lt; n) {
        s = s + a[i];
        i = i + 1;
    }
    return s;
}

int main(void)
{
    int a[2];

    a[0] = fac(5);
    a[1] = 27;
    putint(sum(2, a)); // prints 147
    return 0;
}
</pre>


</div><div class="slide">
 <h1>Informal grammar for uC</h1>

<p class="first">This is an informal context-free grammar for uC:</p>

<ul>
<li class="incremental">The start symbol is <code>program</code>.</li>

<li class="incremental">Keywords and special symbols are written within double-quotes.</li>

<li class="incremental"><code>/empty/</code> denotes the empty string.</li>

<li class="incremental"><code>intconst</code> and <code>ident</code> denote classes of lexical elements.</li>

<li class="incremental">Associativity and precedence for expression operators is not expressed.</li>

<li class="incremental">The grammar has not been adjusted to fit any particular parsing method.</li>
</ul>


<pre class="example">program         ::= topdec_list
topdec_list     ::= /empty/ | topdec topdec_list
topdec          ::= vardec ";"
                  | funtype ident "(" formals ")" funbody
vardec          ::= scalardec | arraydec
scalardec       ::= typename ident
arraydec        ::= typename ident "[" intconst "]"
typename        ::= "int" | "char"
funtype         ::= typename | "void"
funbody         ::= "{" locals stmts "}" | ";"
formals         ::= "void" | formal_list
formal_list     ::= formaldec | formaldec "," formal_list
formaldec       ::= scalardec | typename ident "[" "]"
locals          ::= /empty/ | vardec ";" locals
stmts           ::= /empty/ | stmt stmts
stmt            ::= expr ";"
                  | "return" expr ";" | "return" ";"
                  | "while" condition stmt
                  | "if" condition stmt else_part
                  | "{" stmts "}"
                  | ";"
else_part       ::= /empty/ | "else" stmt
condition       ::= "(" expr ")"
expr            ::= intconst
                  | ident | ident "[" expr "]"
                  | unop expr
                  | expr binop expr
                  | ident "(" actuals ")"
                  | "(" expr ")"
unop            ::= "-" | "!"
binop           ::= "+" | "-" | "*" | "/"
                  | "&lt;" | "&gt;" | "&lt;=" | "&gt;=" | "!=" | "=="
                  | "&amp;&amp;"
                  | "="
actuals         ::= /empty/ | expr_list
expr_list       ::= expr | expr "," expr_list
</pre>

<h3>Expression operator precedence table</h3>

<h4>Prefix unary operators</h4>

<p class="first">14: <code>-</code> <code>!</code></p>


<h4>Infix operators</h4>

<p class="first">13L: <code>*</code>  <code>/</code></p>

<p>12L: <code>+</code>  <code>-</code></p>

<p>10L: <code>&lt;</code> <code>&gt;</code> <code>&lt;=</code> <code>&gt;=</code></p>

<p>9L:  <code>==</code> <code>!=</code></p>

<p>5L:  <code>&amp;&amp;</code></p>

<p>2R:   <code>=</code></p>


<p>The numbers to the left indicate precedence; larger numbers indicate
higher precedence.  L indicates left-associative operators and R
indicates right-associative operators.  The table only contains the C
operators that are included in uC.</p>

</div>

</div>

</body></html>