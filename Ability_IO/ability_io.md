
 # <span style="color:lightblue">Abilities</span>

## <span style="color:forestgreen">A Unison way of manage side effect due to I/O relate operation such as:</span>
- writing to a database
- reading from the file system
- making a network call
- getting a random number
- altering a global variable

##  <span style="color:forestgreen">Side effect</span>
Effect in functional paradyme refer to situation that is not a normal operation of function.
A function should only operate on the input and return the output. This function is call pure function since all its computation is under the control of the function. A non-pure function is function that do IO which depend on external entity that is outside of the function control.

##  <span style="color:forestgreen">Managing side effect with Ability</span>

There are 2 type of side effect 
1. Operation that halt the program
2. Operation that alter the state of program 

 One way to manage these side effects to making it explicit any opearation that dealing with it must handling it explicitly
example of effectful code
```java
 try {
     database.getUser();
 }catch{database.error "error"}
``` 
-  this code depend on database which is out off control of function that call it and may halt the operation if something bad happen like exception.

Unison manage effectful code with spectial function call ability.

### <span style="color:salmon">Halting operation</span>

Abilities handle these type of effect by treat it like logging the result to a file and continue on to the next task. 

- Roughly, an ability can be broken down into two things, an effect interface which specifies
some operations that the effect performs, and handlers which provide behavior to those operations.
- When a program uses an ability, the program halts its execution, hops over to the responsible handle block,
- finds the matching operation, performs the behavior specified there, and then resumes the program.

Unison has a build-in construct to handle this with pattern matching which is treat it as another case of the pattern match construct.

### <span style="color:salmon"> Altering program state </span>
Another type of side effect is effect that alter state of program
for example modified global variable, read input from user. Logical of the program may depend on it.

- Abilities manage this type of effect by separate the code that create effect from the code that
handling the effect.

- An ability pairs an interface which describes an effect's operations
- with handlers that dictate how the effect should actually be performed.

``` java
kvStore {
   KVStore.put "id" 5
   KVStore.get "id"
 } handle with mapBasedStorage Map.empty
```
where the effect handler mapBasedStorage look like this:
```f#
    mapBasedStorage map =
        case (KVStore.put key value -> resume) ->
            updated = put key value map
            resume () with mapBasedStorage updated
            ...
```            
```f#
usingAbilitiesPt1.stopIfTrue : (a -> Boolean) -> a ->{Abort} a
```
- the signature says: take a predicate and a value then return a value or abort
- Abort is what we call an ability in Unison since it may stop execution of program

- ability declaration
```f#
structural ability Abort where abort : {Abort} a
```
- The keyword structural or unique specifies if the ability is unique by its name or by its structure
- followed by the name of the ability and the keyword where
- one or more request constructors:type signatures that declare what operations an ability can perform

--  implementation of usingAbilitiesPt1.stopIfTrue
usingAbilitiesPt1.stopIfTrue : (a -> Boolean) -> a ->{Abort} a
usingAbilitiesPt1.stopIfTrue predicate a =
  if predicate a then abort else a

-- Combining multiple abilities in one function is easy to do in Unison by
-- adding the ability to the curly braces separated by commas.
store.stopIfTrue : (a -> Boolean) -> a ->{Abort, Store a} a
store.stopIfTrue predicate a =
  Store.put a
  if predicate a then abort else a

-- to let the callers of our function know that it's ok to effectfully test the predicate.
effectfulPredicate.stopIfTrue : (a ->{g} Boolean) -> a ->{g, Abort} a
-- The key here is that the potential ability is a lowercase variable (like {g} or {e})
-- which needs to be in the signature of the predicateandin the return type of the overall function.
-- Functions inherit the ability requirements of the functions that they call.That's why signatures like
-- List.map : (a ->{????} b) -> [a] ->{????} [b]
-- have a generic ability requirement in both the transformation function and their return type.
--  It's common to combine operations on Unison data structures like List or Optional
-- with abilities for functional effect management!
-- The abilities that a function performs are visible in curly braces { }
-- to the right side of the function arrow in a type signature.
-- Multiple ability requirements are represented in curly braces in a comma separated list:
-- {Abort, Exception, Stream Text, g}
-- Generic ability requirements are typically single lowercase letter variables in curly braces like {e}
-- Pure functions perform no abilities and are represented with empty braces{}

-- Using ability

-- Abilities handler
-- handler is a Unison function which supplies the implementation or behavior for a given effect.

-- The following code tries to call effectfulPredicate.stopIfTrue in a pure function
--  (remember, the {} means the function does not perform abilities).
nonEmptyName : Text -> {} Text
nonEmptyName name =
  stopIfTrue (text -> text === "") name

-- The error we get back is:
-- The expression in red needs the {Abort} ability, but this location does not have access to any abilities.
-- 4 |   stopIfTrue (text -> text === "") name

-- provide a handler
usingAbilitiesPt1.nonEmptyName : Text -> Text
usingAbilitiesPt1.nonEmptyName name =
  optionalName : Optional Text
  optionalName =
    toOptional!
      '(effectfulPredicate.stopIfTrue (text -> text === "") name)
  Optional.getOrElse "Unknown Name" optionalName

-- the toOptional! handler function eliminate the need of ability hee the signature
toOptional! : '{g, Abort} a ->{g} Optional a
-- By convention, handlers that do not return a delayed computation, like
-- toOptional! or toDefault! end with an exclamation mark !
-- to distinguish them from their counterparts which return delayed computations.s
-- function expects a delayed computation which must be in side '()

-- no delay computation
toDefault! : '{g} a -> '{g, Abort} a ->{g} a
-- return delay computation
toDefault : '{g} a -> '{g, Abort} a -> '{g} a
Abort.toBug : '{g, Abort} a ->{g} a

store.stopIfTrue : (a -> Boolean) -> a ->{Abort, Store a} a

store.nonEmptyName : Text -> Text
store.nonEmptyName name =
  storeIsHandled : '{Abort} Text
  storeIsHandled =
    '(withInitialValue
        "Store Default Value"
        '(store.stopIfTrue (text -> text === "") name))
  abortIsHandled : Optional Text
  abortIsHandled = toOptional! storeIsHandled
  Optional.getOrElse "Optional Default Value" abortIsHandled
-- eliminating the Store ability first with the withInitialValue handler
-- ensures the Store has been seeded with some value so if subsequent functions call Store.get
-- they're guaranteed to return something.
-- Then we eliminate the Abort ability by transforming it into a Optional value
-- the order in which the handlers are applied can change the value returned! use () to maintain order

-- Top Level definition
-- abilities cannot be left unhandled "at the top level" of a file
tryEmit1 : '{Stream Text}()
tryEmit1 _ = Stream.emit "Hello World"

tryEmit2 : '{Stream Text}()
tryEmit2 = 'let
  Stream.emit "Hello World"

tryEmit3 : '{Stream Text}()
tryEmit3 = '(Stream.emit "Hello World")

-- Abilities are a property of thefunctionthat they're exercised in, not a property of a value
-- can be thought of as decorating or being attached to the thefunction arrow.
-- Note that the trick of adding a thunk to a value that performs an ability will
-- enable your code totypecheck,but the application of a
-- handler is the only way to run an effectful function in a watch expression

-- IO abilities
-- Unison have build-in IO handler abilities provide by the runtime
-- function that perform IO
nameGreet : '{IO, Exception} ()
nameGreet _ =
  use Text ++
  printLine "Enter your name:"
  name = !console.getLine
  printLine ("Hello " ++ name)

-- the console.getLine a delay computation and printLine are both IO bound and effectful
-- the top level of a Unison program, a function which performs IO
-- can only be called via the UCM with the run command
-- The run command expects a delayed computation with a signature of
-- '{Exception} () or '{IO} ()or both.
-- Currently returning a value other than unit is not supported.

