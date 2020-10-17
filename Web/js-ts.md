## [Functional Programming](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)

- Pure Functions
- Impure functions – with side effects
- Immutable variables – always create constants
- No loops – always recursion
- Higher order functions – take function as parameter or return or do both
- Closure – is a function’s scope that’s kept alive by keeping a reference to that function
- Curried function – is a function which takes only one parameter at a time

# JS

## JS Notes

- Like array we can access a string character using [number] i.e. `string[0]` will return character at 0 index in the given string
- `typeof(value)` always returns string
- `typeof(null)` is `"object"`
- Rest parameters
- Object spread operator
- Destructuring
- [`this`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) - strict, non-strict, function, arrow function, bind
- in JavaScript, `true && expression` always evaluates to `expression`, and `false && expression` always evaluates to `false`
- In JS even array are object. But not everything is on object
- Array functions - some(), map(), filter(), reduce()
- Named export vs default export
- **Computed property names** </br>
  ```
  const name1 = 'key1';
  let newObj = {[name1]:'value', 'key2':'value2'}
  ```
- `console.table(), console.log({object}) console.trace()` we can even use css in console.log
- **tagged template literal** i.e. styled component function calls with template literal
- **trailing comma** [makes version-control diffs cleaner](https://efficientuser.com/2018/08/22/ecmascript-trailing-commas/)
- A function without a return type always returns `undefined`
- **Web Workers**

#### Object, OOP

- **JS Object** We can access object variables using either dot `person.name` or bracket notation `person['name']`. But only using bracket notation we can add dynamic variable names on the fly. <br/>

  ```
  let myDataName = 'height';
  let myDataValue = '1.75m';
  person[myDataName] = myDataValue;
  person.height
  ```

- Ways to create object instance

  - Constructor function
  - Declare object literal
  - Use `Object()`
  - Use `Object(objLiteral)`
  - Using existing object `Object.create(existingObj)`

- Constructor function(= class) - function name start with capital letter, doesn't return anything, use new keyword with function call.

  ```
  function Person(name){
      this.name = name;
      this.greeting = function(){
          alert('hello'+this.name);
      }
  }

  let rahul = new Person('rahul');
  ```

# TS

### Types

- One of TypeScript’s core principles is that type checking focuses on the shape that values have. called **duck typing** or **structural typing**
- Type assertions/Type cast<br/>

  ```
  let names: any = ['foo', 'bar'];

  const nameCount = (<Array<string>>names).length;
  const nameCount2 = (names as Array<String>).length
  console.log({nameCount,nameCount2})
  ```

- Union types

  ```
  // Use it like enum
  type LockState = "Locked" | "Unlocked";
  const isLocked: LockState = "Locked";

  // Or union type for function argument
  function myfunc(argu1: string | number){}
  ```

- Function as type

  ```
  const myFunction: (arg: string) => boolean =
  (arg: string) => {
    return false;
  }

  // or create a function with just the return type
  const myFunction = (arg1: string) : string => "hello";
  ```

- JSON object member can be accessed either using dot notation or using array/bracket notation

  ```
  const Person = { name: "rahul"}
  const name1: string = Person.name
  //OR
  const name2: string = Person["name"]
  ```

- JS Object are also called associative arrays
- Only using bracket notation we can add dynamic variable names in an object

  ```
  const prop1 = 'name'
  const prop2 = 'age'

  const Person = {
    [`${prop1}`]: "rahul",
    [`${prop2}`]: 29
  }
  ```

- Create a object type in which object can have multiple dynamic key names with types

  ```
  interface Person {
    fullName: string;
    age: number;
  }

  interface Persons {
      [key: string]: Person;
  }

  const person1: Person = { fullName: "rahul", age: 29};

  let persons: Persons = {};

  persons["rahul"] = person1;

  console.log(persons);
  ```

### Misc

- Optional chaining `let name = foo?.bar?.name`
- Nullish Coalescing `let name = foo ?? bar()` if `foo` is `null` or `undefined` get the value of `name` from `bar()`

## Good Reads

- [What is callback event loop in JS , exactly](https://youtu.be/8aGhZQkoFbQ)
