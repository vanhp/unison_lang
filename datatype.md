
# $\color{orange}{Unison\hspace{2pt} Data\hspace{2pt} type}$
Unison is a strong statically type language that also have powerful type inference. There is no needed to specify type on most situation. On a few circumstance when the system can't deduce the type of argument then it will ask you to provide the type.


## $\color{forestgreen}{Type \hspace{2pt}variable}$

Any regular identifer may be used as variable name and in lowercase.


 ### $\color{salmon}{Polymophic \hspace{2pt}type}$
Polymorphic or generic type may be instantiated as any type e.g. [] may instantiated 
as Int [Int],or Boolean [Boolean] etc...


 #### $\color{coral}{Scope \hspace{2pt}type}$
Type variables introduced by a type signature for a term 
remain in scope throughout the definition of that term.

```haskell
ex1 : x -> y -> x
ex1 a b =
   temp : x -- has type x
   temp = a
   a

 ex2 : x -> y -> x
ex2 a b =
    doesn’t refer to x in outer scope
    id : ∀ x . x -> x -- shardow outer x
    id v = v
    temp = id 42
    id a
```
## $\color{forestgreen}{Kind \hspace{2pt} of \hspace{2pt}type}$

Unison type always has a kind which have this form:
A nullary type constructor or ordinary type has kind of Type.
A type constructor has kind $\color{orange}{k1 \rightarrow k2}$ where k1 and k2 are kinds.
For example List,a unary type constructor, has kind Type -> Type
as it takes a type and yields a type. 
A binary type constructor like -> has kind Type -> Type -> Type ,as it takes two types 

## $\color{forestgreen}{Function \hspace{2pt}type}$

The type $\color{orange}{X \rightarrow Y}$ is a type for functions that take arguments of type $\color{orange}{X}$ and yield results of type $\color{orange}{Y}$
Application of the binary type constructor -> associates to the right, 
so the type $\color{orange}{X \rightarrow Y \rightarrow Z}$ is the same as the type $\color{orange}{X \rightarrow (Y \rightarrow Z)}$ 

## $\color{forestgreen}{High-Order \hspace{2pt}type}$

A type constructor of kind $\color{orange}{(Type \rightarrow Type) \rightarrow Type}$
is a higher-order type constructor (it takes a unary type constructor and yields a type).


## $\color{forestgreen}{Tuple \hspace{2pt}type}$
A type that can contain any type as the same time and has positional significan.
The $\color{orange}{Type(A,B)}$ is a type for binary tuples (pairs) of values, one of type A and another of type B
The type (A) is not a tuple type
Tuple a b c is same as (a,b,c)
The nullary tuple type () is the type of the unique value also written () and is pronounced “unit”.

### $\color{salmon}{Tuple \hspace{2pt}pattern \hspace{2pt} decomposition}$

A simple way to match component of Tuple and then bind that value to variable to be used later at the same time using at1,at2,at3 functions of the Tuple type 
```haskell
getByteSize : (Text, Bytes, Text) -> Nat
getByteSize tuple3 =
  use Bytes ++
  first = at1 tuple3
  bytes = at2 tuple3
  last = at3 tuple3
  Bytes.size (toUtf8 first ++ bytes ++ toUtf8 last)
getByteSize ("Shepherds", 0xsdeadbeef, "Pie")
⧨
16 
-- or do this inside a function only

getByteSize : (Text, Bytes, Text) -> Nat
getByteSize tuple3 =
  use Bytes ++
  "tuple decomposition 👇"
  let
    (first, bytes, last) = tuple3
    Bytes.size (toUtf8 first ++ bytes ++ toUtf8 last)

```


## $\color{forestgreen}{Built-in \hspace{2pt}type}$
- Nat is the type of 64-bit natural numbers, also known as unsigned integers. 
They range from 0 to 18,446,744,073,709,551,615.
- Int is the type of 64-bit signed integers. 
They range from -9,223,372,036,854,775,808 to +9,223,372,036,854,775,807.
- Float is the type ofIEEE 754-1985 double-precision floating point numbers.
- Boolean is the type of Boolean expressions whose value is true or false.
- Bytes is the type of arbitrary-length 8-bit sequences. 
- Text is the type of arbitrary-length strings of Unicode text.
- Char is the type of a single Unicode character.
- The trivial type () (pronounced “unit”) is the type of the nullary tuple. 
- There is a single data constructor of type () and it’s also written ().


### $\color{salmon}{Built-in \hspace{2pt}type \hspace{2pt} constructor}$
Unison has the following built-in type constructors. 
$\color{orange}{(\rightarrow)}$ is the constructor of function types. A $\color{orange}{X \rightarrow Y}$ is the type of functions from $\color{orange}{X}$ to $\color{orange}{Y}$

Tuple is the constructor of tuple types. See tuple types for details on tuples.
List is the constructor of list types. 
A type List T is the type of arbitrary-length sequences of values of type T.
The type [T] is an alias for List T
abilities.Request is the constructor of requests for abilities. A type Request A T
is the type of values received by ability handlers for the ability A
where current continuation requires a value of type T.

## $\color{forestgreen}{User \hspace{2pt}define \hspace{2pt} type}$
use type keyword to define a new type. 
the type  may have modifier like
1. unique
2. structural
```haskell
structural type Optional a = Some a | None
```
The $\color{orange}{=}$ symbol splits the definition into aleft-hand side and right-hand side,much like term definitions.
- The left side has:
     1.  the name for the data type (Optional)
     2.  declares a new type constructor with that name (Optional)
     3.  argument for the type (a)
- The right-hand side is data constructor may  has:
    1. zero or more data constructors separated by | 
    2. name of data constructor (Some,None)
    3. type of argument
    4. may also refer to the name given to the type in the left-hand side, 
        in which case it is a recursive type declaration.





In Unison there are two type of objects. 
1. Type and 
2. term.

Type is a composite object that may have complex component.
Term is every thing else that is not type.


A type definition needs a modifier of
unique or structural before it the values after the equals sign are known as
data constructors. Each data constructor is separated by a pipe which means that the
Genre type can be Poetry or Fiction or CookBooks etc.


### $\color{salmon}{Type \hspace{2pt}constructor}$
A constructor is a function that instantiated object of that type
Unison type constructor is called data constructor it appear on the right side of =
and can be any name. It is a function whose job is to create a type as indicate on the left side.
It takes the input argument and return the type to the left side 


### $\color{salmon}{Type \hspace{2pt}modifier}$
 Unison have two type modifier structural and unique modifier.
 Structural signify that the type is structurally identical and may replace with similar Structure
 Unique modifer indicate that the type is one of a kind and can not be substituted with any type and
 its name is not interchangeble.


 #### $\color{coral}{Structure \hspace{2pt}modifer}$
 Use structural to indicate that name is not important so the 
 type is identical to each other and interchangeble even with difference name.
 when the data constructor and paramenter are the same type
 it serves as generic T a place holder for specific type
 ```haskell
structural type Weekday = Mon | Tues | Wed | Thurs | Fri
```

 #### $\color{coral}{Unique \hspace{2pt}modifer}$
 Use to indicate that the name is semantically important can't be substituted
 ```haskell
unique type Genre
    = Fiction
    | Poetry
    | CookBooks
    | Science
    | Biography
    

structural type Either a b = Right b | Left a
unique type Box a = Box b
```
 where a and b are type parameter or place holder (T)
 on the right side of = is a type constructor

 Either type
 Either data type is a built-in data type. 
 It use to represent case where a value can be one or the other 
structural type Either a b = Right b | Left a
