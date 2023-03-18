> Written with [StackEdit](https://stackedit.io/).
> ## DOMAIN MODELING WITH DATALOG by Norbert Wojtowicz
> 3 thigns to biuld a domanin (system:
> 
> **STREAM** - semantyc and order (qeue systems)
> 
> **TREE** - hierachy (ruby stack?)
> 
>**MESH** - graph
They are the data structures that matter

Relational db is terrible when it comes to implicit joins

Entity attribute value triple store - known as RDF

### DATA STRUCTURE THAT REPRESENTS A DATABASE

[ entity            attribute        value] - eav structure

{:user/name  "richhickey" }
{:user/name  "tonsky" }
{:user/name  "pitchyless" }

**e** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**v**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **a**

11&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:user/name &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;richhickey

22&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:user/name &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;tonsky

33&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:user/name &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pitchyless

**id = entity**

**each row is the datum**

### Pattern matching - Constants

Is a vector of 3 elements that represents the **eva** structure in the db

[11 _ _] = finds [11 :user/name richhickey]

[_ _ "pichyless"] = finds [33 :user/name pitchyless

[_ :user/name	_} = finds all registers once all of them have :user/name

[	22 :user/name "tonsky" ] - finds nothing, there's no data where all pf them are true.

### Pattern matching - Bindings

** Variables - starts with a ? mark**

[?e _ _]

[_ ?v _]

[_ _ ? _a]

### Pattern matching - Bindings - Mix and Match
[?e :user/name "picthless"]

[11 :user/name ?v]

[?e :user/name ?v]

### BASIC QUERY
[ :find ?name
: where
[11 :user/name ?name]]

**result:**
e{{"richhickey"]}

data lo is a declarative logic programming language, the order doesn't matter
[ :find ?id
: where
[?id :user/name ?"tonsky]]

**result:**
e{{22]}

[ :find ?name
: where
[_ :user/name ?name]]

**result:**
e{{"richhickey"]
["tonsky"]
["pitchyless"}

### SETS - removes duplicates
[ :find ?attr

:where

[_ ?attr _]

**is returns all the attributes in the system**

**result:**

e[{ :user/name ]}

### SET - LISTS - SCALARS

[ :find ?name

:where

[_ :user/name ?name]

**result:**

e{{"richhickey"]

["tonsky"]

["pitchyless"}

[ :find [?name ...]

:where

[_ :user/name ?name]

**returns a vector of value

**result:**

e{{"richhickey"]

["tonsky"]

["pitchyless"}

[ :find ?name .

:where

[_ :user/name ?name]]


**result:**

"tonsky"

**why returns "tonsky"?** because set doesn't have order, so as the all values match it will return a random data


### MULTIPLE BINDINGS

[ :find  ?id ?name

: where

[?id :user/name ?name]]

**result:**

e{{11 "richhickey"]

[22 "tonsky"]

[33 	"pitchyless"}


### ENTITY

{ :user/name "richhickey"

: user/email "rich@.com"}

{ :user/name "richhickey"

: user/email "rich@e.com"}

**this will add new data into the the table**

### JOIN

[ :find ?email

:where

[?id :user/name "richhickey"]

[?id :user/email ?email]]

**result**

e{["rich@e.com"]}

[ :find ?name

:where

[?id :user/name ?name]

[?id :user/email "rich@e.com"]]

**result**

e{["richhickey"]}

[ :find ?name

:where

[?id :user/name ?name]

[?id :user/email "fake]]

**result**

e{} - empty, there's no match

**we can also have multitple unkowns**

[ :find ?name ?email

:where

[?id :user/name ?name]

[?id :user/email ?email]

**result**

e{["richhickey" "rich@e.com"]

["tonsky" "nikita@e.com"]

["pitchyless" "norbert@e.com"]}

### REFERENCE

{ :org/name "clojure"}

{ :repo/slug "clojure/clojure'

:repo/owner 44}

{ :repo/slug "tonsky/datasript"

:repo/owner 22}

**for better understanding of users, we can replace the 44 and 22 reference numbers for

{ :repo/slug "clojure/clojure'

:repo/owner [ :org/name "clojure"}

{ :repo/slug "tonsky/datasript"

:repo/owner [:user/name "tonsky"]}

**In the data log the schemas are applied during the reading not during the writing**
entities can sometime behave both as organization and users in the case above


### FOLLOW REFERENCES

[ :find ?repo

:where

[?p :user/name "tonsky"]

[?r :repo/owner ?p]

[? :repo/slug ?repo]]

**result**

e{["tonksy/datascript"]}

[ :find ?name

:where

[?p :user/name ?name]

[?r :repo/owner ?p]

[? :repo/slug "tonsky/datascript"]]

**result**

e{["tonsky"]}

[ :find ?name ?repo

:where

[?p :user/name ?name]

[?r :repo/owner ?p]

[? :repo/slug ?repo]]

**result**

e{["tonsky" "tonsky/datascript]}

**if we are instersted in organization, we can switch user/name with org/name**

[ :find ?name ?repo

:where

[?p :org/name ?name]

[?r :repo/owner ?p]

[? :repo/slug ?repo"]]

**result**

e{["clojure" "clojure/clojure"]}

[ :find ?name ?repo

:where

(or  [?p :org/name ?name]

[ ?p :user/name ?name])

[?r :repo/owner ?p]

[? :repo/slug ?repo"]]

**result**

e{["tonsky" "tonsky/datascript]

["clojure" "clojure/clojure"]}

### RULES??? - 18 MINS


### CIRCULAR REFERENCES

{ :repo/slug "pithyless/datascript"

:repo/owner [:user/name "pythless"

:repo/fork [ :repo/slug

"tonksy/datacript']}

### FIND FORKS

[ :find ?repo

:where

[?r :repo/slug ?repo]

[?r :repo/fork ]]

**result**

e{[ "pithyless/datascript"]|}

[ :find ?repo

:where

[?r :repo/slug ?repo]
[(missing ?r :repo/fork)]]

**result**

e{['clojure/clojure|

["tonsky/datascript]}

### LIST FORKS

[ :find ?orig ?fork

:where

[?orig-id :repo/slug ?orig]

[?fork-id :repo/slug ?fork]


**result**

e{["tonsky/datascript"

"pithyless/datascirpt"]}


### MULTIPLE VALUES

{ :repo/slug "clojure/clojure"

:repo/lang ['clojure" "java"]}

{ :repo/slug "tonsky/javascript"

:repo/lang ["clojure" "javascript"]}

{ :repo/slug "pithyless/datacript"

:repo/lang ["clojure" "javascript"]}

**To delete a register in database: use false**

e&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; v&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; tx &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;op
&nbsp;&nbsp;

22 :user/name tonsky  1000  true

22 :user/name tonsky  1000  false


**and update adding a new register**

22 :user/name tonsky  1000  true

22 :user/name tonsky  1000  false

22 :user/name **nikita**  1000  true