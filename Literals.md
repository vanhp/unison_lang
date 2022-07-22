
# $\color{orange}{Literals}$
A literal expression is a basic form of Unison expression. Unison has the following types of literals:

## $\color{forestgreen}{Numbers}$

- A64-bit unsigned integerof type Nat (which stands for natural number)consists of digits from 0 to 9. The smallest Nat is 0 and the largest is
18446744073709551615.
- A64-bit signed integerof type Int consists of a natural number immediately preceded by either + or - .For example,
4 is a Nat,whereas +4 is an Int.The smallest Int is -9223372036854775808
and the largest is +9223372036854775807.
- A64-bit floating point number of type Float
consists of an optional sign (''+''/''-''), followed by two natural numbers separated by . .Floating point literals in Unison are IEEE 754-1985 double-precision numbers. For example 1.6777216 is a valid floating point literal.


## $\color{forestgreen}{Text}$
- A text literalof type Text is any sequence of Unicode characters between pairs of ".The escape character is \,so a"
can be included in a text literal with the escape sequence \"
.The full list of escape sequences is given in the Escape Sequences section below. For example, "Hello, World!" is a text literal. A text literal can span multiple lines. Newlines do not terminate text literals, but become part of the literal text.
- A character literalof type Char consists of a? character marker followed by a single Unicode character, or a singleescape sequence.For example,
?a,?🔥 or ?\t.


## $\color{forestgreen}{Boolean}$
- There are two Boolean literals: true and false,and they have type
Boolean.


## $\color{forestgreen}{Byte}$
- A byte literal starts with 0xs.For example 0xsdeadbeef A hash literal begins with the character #.See the section Hashes for details on the lexical form of hash literals. 


## $\color{forestgreen}{Hash}$
- A hash literal is a reference to a term or type. The type or term that it references must have a definition whose hash digest matches the hash in the literal. The type of a hash literal is the same as the type of its referent.
#a0v829 is an example of a hash literal.


## $\color{forestgreen}{List \hspace{2pt}literal}$
- A literal list has the general fText"],and
[1, 2, 3] are list literals. The expressions that form the elements of the list all must have the same type. If that type is T,then the type of the list literal is.base.List T or [T].


## $\color{forestgreen}{Function \hspace{2pt}literal}$
- A function literal or lambda has the form
p1 p2 … pn -> e,where p1 through pn are regular identifiers and e
is a Unison expression (thebodyof the lambda). The variables p1 through
pn are local variables in e,and they are bound to any values passed as arguments to the function when it’s called (see the section Function Application for details on call semantics). For example
x -> x Nat.+ 2 is a function literal.

## $\color{forestgreen}{Tuple \hspace{2pt}literal}$
- A tuple literalhas the form (v1,v2, …, vn) where v1 through vn
are expressions. A value (a, b) has type (A,B) if a has type A and b
has type B.The expression (a) is the same as the expression
a.The nullary tuple () (pronounced “unit”) is of the trivial type()
.See tuple typesfor details on these types and more ways of constructing tuples.


## $\color{forestgreen}{Termlink \hspace{2pt}literal}$
- termLink or typeLink literal takes the form termLink a UnisonTerm or
typeLink Optional where the argument to termLink is a Unison term and the argument to typeLink is a Unison type. termLink produces a value of type
Link.Term and typeLink produces a value of type Type