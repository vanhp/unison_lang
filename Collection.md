  # $\color{orange}{Collection\hspace{2pt} Data\hspace{2pt} structure}$


 ## $\color{forestgreen}{List}$
 List a built-in data structure that is the work horse in Unison.
 List [] can only contain values of one type at a time, 
 Lists are immutable finite and homogenous.
 It is parameterized on the type of elements of the lists and are eagerly evaluated.

 ### $\color{salmon}{Create \hspace{2pt}list}$

- List.singleton creates a list with one element 
- data.List.fill n expr creates a list of n copies of expr
- data.List.initialize n f. creates a list of length n,filled with the values of
f i as i runs from 0 up to n
- List.replicate n op performs the (possibly effectful) computation op n
times and collects the results in a list
- Nat.rangeandlatest.Nat.rangeClosedcreate lists of all natural numbers (of typeNat)in the specified range


```haskell
desserts : [Text]
desserts = ["Eclair", "Peach cobbler", "Ice cream"]
-- list has 1 item
List.singleton 4
⧨
[4]
-- list with n item
data.List.fill 3 "boing"
["boing", "boing", "boing"]
-- init with code
data.List.initialize 4 (x -> x Nat.* 2)
⧨
[0, 2, 4, 6]
-- replication
test.sample 4 '(List.replicate 3 gen.nat)
⧨
[[0, 0, 0], [0, 0, 1], [0, 1, 0], [1, 0, 0]]
-- range
Nat.range 1 4
⧨
[1, 2, 3]
latest.Nat.rangeClosed 1 4
⧨
[1, 2, 3, 4]

```
### $\color{salmon}{Adding \hspace{2pt}removing,replacing \hspace{2pt}item}$
Adding, removing, and replacing elements of lists
List.+:adds an element to the front of a list:

```haskell
-- add to front +:
1 List.+: [2, 3, 4]
⧨
[1, 2, 3, 4]

-- :+ adds an element to the end of a list
[1, 2, 3] :+ 4
⧨
[1, 2, 3, 4]
-- insert item into list at position
List.insert 1 "green" ["red", "blue"]
⧨
["red", "green", "blue"]

-- intersperseinserts an element between all the elements of a list:

intersperse 0 [1, 2, 3]
⧨
[1, 0, 2, 0, 3]

-- deleteAtremoves a specific element from a list:

deleteAt 2 [5, 3, 8, 9]
⧨
[5, 3, 9]
-- replacereplaces a specific element with another one:

replace 1 "cheese" ["flour", "butter", "eggs"]
⧨
["flour", "cheese", "eggs"]

```
 ### $\color{salmon}{Accessing \hspace{2pt}querying \hspace{2pt} item}$


```haskell
-- List.size gets the number of elements in the list.
List.size [5, 8, 2]
⧨
3
-- List.head gets the first element of the list.

List.head [1, 2, 3]
⧨
Some 1
-- List.last gets the last element.

List.last [1, 2, 3]
⧨
Some 3
-- List.tail gets all but the first element.

List.tail [1, 2, 3]
⧨
Some [2, 3]
-- List.init gets all but the last element.

List.init [1, 2, 3]
⧨
Some [1, 2]
-- List.at gets the element at a given position in the list.

List.at 1 ["a", "b", "c"]
⧨
Some "b"
-- List.take gets a specified number of elements from the front of the list.

List.take 2 ["a", "b", "c"]
⧨
["a", "b"]
-- List.drop removes a specified number of elements from the front of the list.

List.drop 1 ["a", "b", "c"]
⧨
["b", "c"]
-- List.takeWhile gets elements from the front of the list until a given function returns false

List.takeWhile Nat.isEven [2, 4, 6, 7, 8, 9]
⧨
[2, 4, 6]
-- List.dropWhile removes elements from the front of the list until a given function returns false

List.dropWhile Nat.isEven [2, 4, 6, 7, 8, 9]
⧨
[7, 8, 9]
-- List.filter finds all the elements that satisfy someBooleanfunction.

List.filter Nat.isEven [2, 4, 6, 7, 8, 9]
⧨
[2, 4, 6, 8]
-- List.any checks if any elements satisfy someBooleanfunction.

List.any Nat.isEven [2, 4, 6, 7, 8, 9]
⧨
true
-- List.all checks if all elements satisfy someBooleanfunction.

List.all Nat.isEven [2, 4, 6, 7, 8, 9]
⧨
false
-- elem checks if an element is in the list.

elem 2 [1, 2, 3]
true
-- maximum finds the largest element in the list.

maximum [5, 8, 2]
⧨
Some 8
-- minimum finds the smallest element in the list.

minimum [5, 8, 2]
⧨
Some 2

```

 ### $\color{salmon}{Combining \hspace{2pt}List }$

```haskell
-- List.++ concatenates two lists.

[1, 2, 3] List.++ [4, 5, 6]
⧨
[1, 2, 3, 4, 5, 6]
-- List.join concatenates a whole list of lists.

List.join [[1, 2, 3], [], [4, 5], [6]]
⧨
[1, 2, 3, 4, 5, 6]
-- intercalate inserts a list between other lists.

intercalate [10, 20] [[1, 2], [], [3]]
⧨
[1, 2, 10, 20, 10, 20, 3]
-- List.zip makes a list of pairs from two lists, each with elements of both lists occuring at the same position.

List.zip ["red", "green", "blue"] [5, 8, 2]
⧨
[("red", 5), ("green", 8), ("blue", 2)]

```
 ### $\color{salmon}{Spliting \hspace{2pt}Slicing \hspace{2pt} rearranging,List}$
Splitting, slicing, and rearranging list
```haskell
-- List.reverse flips the order of the elements of a list:

List.reverse [5, 8, 2]
[2, 8, 5]
-- data.Heap.sort puts the elements of a list in ascending order:

data.Heap.sort [5, 8, 2]
[2, 5, 8]
-- data.Heap.sortDescending puts the elements of a list in descending order:

data.Heap.sortDescending [5, 8, 2]
[8, 5, 2]
-- data.List.sortBy sorts a list on some specified property of the elements:

data.List.sortBy Text.size ["four", "six", "three"]
⧨
["six", "four", "three"]
-- List.split breaks a list into sub-lists on a delimiter matching a condition:

List.split (x -> x Nat.== 0) [1, 2, 3, 0, 4, 0, 2, 1, 0, 0, 1]
⧨
[[1, 2, 3], [4], [2, 1], [], [1]]
-- splitAt breaks a list into two lists, at a specific index:

splitAt 2 [1, 2, 3, 4, 5]
⧨
([1, 2], [3, 4, 5])
-- List.span breaks a list into two lists, at the first element for which a condition returns false

List.span (x -> Universal.lt x 3) [1, 2, 3, 4, 5]
⧨
([1, 2], [3, 4, 5])
List.span Nat.isEven [1, 2, 3]
⧨
([], [1, 2, 3])
-- List.halve splits a list into two lists of roughly equal size:

List.halve [1, 2, 3, 4]
⧨
([1, 2], [3, 4])
List.halve [1, 2, 3]
⧨
([1], [2, 3])
-- data.List.distinct and distinctBy remove any duplicate elements from the list.

data.List.distinct [5, 8, 8, 5, 5, 2, 2]
⧨
[5, 8, 2]
distinctBy Nat.isEven [5, 8, 8, 5, 5, 2, 2]
⧨
[5, 8]
-- List.sliceextracts a sub-list from a list:

List.slice 2 5 [5, 8, 8, 5, 5, 2, 2]
⧨
[8, 5, 5]
-- powerslice returns all contiguous sub-lists of a list:

powerslice [1, 2, 3]
⧨
[[], [1], [1, 2], [2], [1, 2, 3], [2, 3], [3]]
-- tails returns all suffixes of a list, longest first:

tails [1, 2, 3]
⧨
[[1, 2, 3], [2, 3], [3], []]
-- inits returns all prefixes of a list, shortest first:

inits [1, 2, 3]
⧨
[[], [1], [1, 2], [1, 2, 3]]



```

 ### $\color{salmon}{traversing, \hspace{2pt}transforming \hspace{2pt} list}$

```haskell
-- List.map applies a function to every element of a list, collecting the results:

List.map Text.size ["one", "two", "three"]
⧨
[3, 3, 5]
-- List.flatMap applies a list-valued function to every element of a list, collecting the results in one list:

List.flatMap (n -> data.List.fill n "boing") [0, 1, 2]
⧨
["boing", "boing", "boing"]
-- mapIndexed applies a function to every element of a list together with the index of the element in the list:

mapIndexed (n x -> (n, x)) ["rip", "rap", "rup"]
⧨
[(0, "rip"), (1, "rap"), (2, "rup")]
-- List.foldl iterates over a list from left to right, accumulating elements into a result using a given function:

List.foldl (Text.++) "It's " ["Super", "Duper", "Awesome"]
⧨
"It's SuperDuperAwesome"
-- List.foldr iterates over a list from right to left, accumulating elements into a result using the given function:

List.foldr (Text.++) " Cool" ["Really", "Rather", "Super"]
⧨
"ReallyRatherSuper Cool"
-- data.List.foldbapplies a function to every element of a list and then combines the results using a binary function:

data.List.foldb Text.size (Nat.+) 0 ["abc", "def", "ghi"]
⧨
9
-- List.scanl iterates over a list just likeList.foldl,but returning all intermediate results:

List.scanl (Nat.+) 0 [1, 2, 3, 4, 5]
Nonempty 0 [1, 3, 6, 10, 15]
-- List.scanr iterates over a list just likeList.foldr,but returning all intermediate results:

List.scanr (Nat.+) 0 [1, 2, 3, 4, 5]
Nonempty 15 [14, 12, 9, 5, 0]
```

 ### $\color{salmon}{Nonempty \hspace{2pt}list}$
The List type allows lists to potentially be empty. For lists that should not be allowed to be empty, use the type Nonempty.A number of operations onList produce values of type Nonempty to ensure that the result has at least one element. For example:

```haskell
List.scanl : (b ->{e} a ->{e} b) -> b -> [a] ->{e} Nonempty b
List.scanr : (a ->{e} b ->{e} b) -> b -> [a] ->{e} Nonempty b
nonEmptySubsequences : [a] -> [Nonempty a]
data.List.groupBy : (v ->{e} k) -> [v] ->{e} data.Map k (Nonempty v)
groupConsecutive : [a] -> [Nonempty a]
groupMap : (a ->{e} k)
           -> (a ->{e} v)
           -> [a]
           ->{e} data.Map k (Nonempty v)
groupSublistsBy : (a ->{e} a ->{e} Boolean) -> [a] ->{e} [Nonempty a]

```
 ### $\color{salmon}{Conversion \hspace{2pt}to/from \hspace{2pt}List}$

```haskell
-- List.nonempty attempts to convert a list to a Nonempty:

List.nonempty [1, 2, 3]
⧨
Some (Nonempty 1 [2, 3])

List.nonempty []
⧨
None
-- List.toSet constructs a data.Set from a List,and data.Set.toList converts the other way:

data.Set.toList (List.toSet [5, 8, 8, 5, 5, 2, 2])
⧨
[2, 5, 8]
-- List.toMap converts a list of key-value pairs to a data.Map,and data.Map.toList converts the other way:

data.Map.toList (List.toMap [(5, 8), (8, 5), (5, 2), (2, 0)])
⧨
[(2, 0), (5, 2), (8, 5)]
-- fromCharList converts a list of characters to Text,and to CharList converts the other way:

fromCharList [?a, ?b, ?c]
⧨
"abc"
toCharList "abc"
⧨
[?a, ?b, ?c]
-- Bytes.fromList converts a list of Nat numbers to Bytes,and Bytes.toList converts the other way:

Bytes.fromList (Nat.range 0 4)
⧨
0xs00010203
Bytes.toList 0xsdeadbeef
⧨
[222, 173, 190, 239]
toList! enumerates all elements on aStream,as a list:

toList! do
  use Stream emit
  emit 1
  emit 2
  emit 3
  ⧨
[1, 2, 3]
```



 An empty list is simply represented with 
 [] or with List.empty
 Pattern match on List
 List has its own syntax for this

### $\color{salmon}{Head \hspace{2pt}-tail \hspace{2.pt}matching \hspace{2.pt}with \hspace{2.pt}+:\hspace{2.pt}and \hspace{2.pt}:+\hspace{2.pt}operator}$
#### $\color{coral}{Head \hspace{2pt}matching\hspace{2.pt}+:}$
You can use pattern matching to scrutinize the first (left-most) element of a list with the +: syntax.
```haskell
match ["a", "b", "c"] with
  head +: tail -> head
  _            -> "empty list"
⧨
"a"
```
The +: is unpacking the first element head of the list to its text value "a"
while keeping the remaining elements the tail as a List.
#### $\color{coral}{Empty \hspace{2pt}matching}$
The underscore will match the "empty list" case. 
```haskell
_ -> "empty list" same as [] -> "empty list"
-- or
List.empty -> "empty list"
```
All three are valid ways of testing for the empty list case.
#### $\color{coral}{Tail \hspace{2pt}matching\hspace{2.pt}:+}$
You can also pattern match on the last (right-most) element of a list using the :+ syntax
```haskell
match ["a", "b", "c"] with
  firsts :+ last -> last
  _              -> "empty list"
⧨
"c"

-- match more than one value of the list

match ["a", " b", "c "] with
  [first, second] ++ remainder ->
    use Text ++
    first ++ " yes!"
  _                            -> "fallback"
⧨
"a yes!"

-- don't care about value

match ["a", " b", "c "] with
  [_, _] ++ remainder -> "list has at least two elements"
  _                   -> "fallback"
⧨
"list has at least two elements"
```

 ## $\color{forestgreen}{Map}$
 A dictionary type of key-value pair. Create it with list of tuple
 ```haskell
Map.fromList [(1, "a"), (2, "b"), (3, "c")]
⧨
Bin 3 2 "b" (Bin 1 1 "a" Tip Tip) (Bin 1 3 "c" Tip Tip)
-- single object
Map.singleton 1 "a"

 ```
## $\color{forestgreen}{Set}$
A collection of all unique item and unordered.

Create set from map
```haskell
map = Map.fromList [(1, ()), (2, ()), (3, ())]
Set map
⧨
Set (Bin 3 2 () (Bin 1 1 () Tip Tip) (Bin 1 3 () Tip Tip))
```