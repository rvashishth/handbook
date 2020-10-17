- [References](#references)
- [TODOs](#todos)
- [General Concepts](#general-concepts)
  - [Operators](#operators)
  - [Default Groovy Method](#default-groovy-method)
- [Closures](#closures)
    - [Closure as method argument](#closure-as-method-argument)
- [Collections](#collections)
  - [List and Array](#list-and-array)
  - [Maps](#maps)
- [Groovy Utility Code Snippets](#groovy-utility-code-snippets)


## References 

Groovy language documentation

https://docs.groovy-lang.org/latest/html/documentation/

Blog

https://mrhaki.blogspot.com/

## TODOs

- [ ] Invoking methods without parenthesis
- [ ] `pojo.find { }` and `list.find {}`
- [ ] Calling a method with closure 
- [ ] How do we call a method by just method name and curly bracket after it. Here method don't accept closure argument. i.e. Persion.find {}
- [ ] 

## General Concepts

**groovy.transform.CompileStatic** annotation

We can multiply string in groovy
```groovy
def stringmultiply = 'foo'*3
println stringmultiply
// foofoofoo
```

### Operators

- **Elvis** and **Elvis assignment operator**

```groovy
// ternary
def name = name ? name : 'foo'
// elvis
def name = name ?: 'foo'
// elvis assignment
def name ?= 'foo'
```

- **Safe navigation** 

```groovy
// this will return null, if person itself is null
def name = person?.name
```

- **Direct field access operator** 

if we do `user.name` groovy calls `user.getName` method. If we need to use the field value instead. do `user.@name`.


- **Method pointer operator**

Use this to use a method as argument instead of closure. This allow us to store method reference on an instance in a variable. and later pass that variable where closure is expected

- Method reference operator(java 8 `::`)

- **Regular Expression **
  - pattern operator, the pattern operator `~` provides a simple way to create a java.util.regex.Pattern instance
  - find operator `=~`
  - match operator `==~`


- **Spread Operator** `*.`

It essentially shortcut to `list.collect { it.variable }`  it works only for iterator.

To get list of all employee names `def = employees*.name` it's also null safe. can be nested `cars*.models*.name`

We can also use spread operator as java script to add list and map in another list/map
```groovy
def items = [4,5]                      
def list = [1,2,3,*items,6]  
```

- **range operator**
```groovy
def list = 1..10
```

- Safe index operator

In order to cop with array index out of bounds use safe index `?[i]`

- `in` operator 

- **Coercion operator** use for casting incompatible types

```groovy
Integer x = 123
String s = x as String 
```

```groovy
def list = ['Grace','Rob','Emmy']
assert ('Emmy' in list)   
```
###  Default Groovy Method

https://docs.groovy-lang.org/latest/html/api/org/codehaus/groovy/runtime/DefaultGroovyMethods.html

- find 
- any 


## Closures

block of code, which act as a function. Take argument, return values and can be assigned to a variable.

`{ [optionalParameters ->] statements }`

parameters in groovy can be typed or untyped

A closure is essentially an instance of `groovy.lang.Closure` and can be assigned to any variable

We can call a closure like an normal method
```groovy
def cnum = { 123 }
println cnum()
```

or call a closure using call method.
```groovy
def value = { 123 }.call()
```

Unlike a method, a closure always returns a value when called.

`it` is an implicit default parameter for closure. 
```groovy
def greet = {"hello $it"}
```

To strictly declare a closure without argument, an empty argument list if mandatory
```groovy
def noArgClosure = { -> 'no arg closure'}
```

**varargs** if last argument of closure is variable lenght(or an array) then closure 
can accept multiple argument of that type
```groovy
def concat1 = { String... args -> args.join('') }           
assert concat1('abc','def') == 'abcdef'                     
def concat2 = { String[] args -> args.join('') }            
assert concat2('abc', 'def') == 'abcdef'
```

**Delegation strategy** inside the closure, groovy provide three types of  `this` references.

Closure class provide additional methods like `getThisObject()`,`getOwner()` and `getDelegate()`

- `this` corresponds to the enclosing class where the closure is defined. It can also be inner class.
- `owner` corresponds to the enclosing object where the closure is defined, which may be either a class or a closure
- `delegate` corresponds to a third party object where methods calls or properties are resolved whenever the receiver of the message is not defined

```groovy
def cl = { getOwner() }
def cl2 = { getThisObject() }
def cl2 = { getDelegate() }
def cl5 = { this }
def cl3 = { owner }
def cl4 = { delegate }
```

Using `delegate` we can change the value of `this`(value of `delegate`) and the closure behavior will change accordingly
```groovy
def p = new Person(name: 'Norman')
def t = new Thing(name: 'Teapot')

def upperCasedName = { delegate.name.toUpperCase() }

upperCasedName.delegate = p
assert upperCasedName() == 'NORMAN'
upperCasedName.delegate = t
assert upperCasedName() == 'TEAPOT'
```

Some of the functional programming behavior provided by closure are 
- currying
- memoization
- composition
- trampoline
  
#### Closure as method argument

```groovy
class Jenkins {
  def envName = 'nonprod'
  void withEnv( username, password, Closure closure) {
    if(username && password)
      closure( envName, "logging to $envName using username: $username password: $password")
  }
}

new Jenkins().withEnv('rvashish','******') { String env, String msg ->
  println msg
  println env
}

// can also call method without parenthesis
new Jenkins().withEnv 'rvashish','******',{ String env, String msg ->
  println msg
  println env
}
```

## Collections

### List and Array

Below will create a `java.util.ArrayList`
```groovy
def list = [1,2,3]
def list2 = ['a',"string hun main", 123]
```

to create a list of different type use `as`
```groovy
def list = [1,2,3] as LinkedList
```

groovy supports negative index to access elements from end of list
```groovy
def letters = ['a', 'b', 'c', 'd']
assert letters[-1] == 'd'
```

Get sub list using range, or list of specific index items
```groovy
def letters = ['a', 'b', 'c', 'd']
// this will return item at 1 and 3 position
assert letters[1,3] == ['b','d']

// this will return sublist using range
assert letters[2..3] = ['c','d']
```

We can add element in list using reverse arrow operator 
```groovy
def nums = [1,2]
nums << 4
assert [1,2,4] == nums
```

Since array and list syntax is same, to create array type has to be defined explicitly
```groovy
// either data type
String[] arrStr = ['Ananas', 'Banana', 'Kiwi'] 
// or casting
def numArr = [1, 2, 3] as int[] 
```

### Maps

Also called dictionary, associative array

Groovy maps are instance of `LinkedHashMap` until mentioned otherwise 

Groovy map can be accessed using dot notation as javascript

```groovy
def colors = [red: '#FF0000', green: '#00FF00', blue: '#0000FF']
assert colors.green  == '#00FF00' 

// can also add a new entry using same dot notation
colors.yellow = "#FF9900"
```

We can also use a variable key as JS but has to use parenthesis for this
```groovy
def key = 'name'
def person = [(key): 'Guillaume']
assert person.containsKey('name')
```
## Groovy Utility Code Snippets

**Convert long milliseconds into minute and seconds string**

```groovy
static String elapsedTimeInMin(long milliseconds)  {
    long seconds = milliseconds / 1000
    int minutes =  seconds / 60
    int extraSec = seconds % 60
    "${minutes}m${extraSec}s"
  }
```

**loop through list items to match any one and return boolean as result**

  ```groovy
  boolean matchesReferEnabledPathRegex = uriList.any {
          Pattern.matches(".*${it}.*", request.getRequestURI())
      }
  ```

**Creates a method which takes a list and closure and return transformed list**

```groovy
def transform(List elements, Closure action) {                    
    def result = []
    elements.each {
        result << action(it)
    }
    result
}
```