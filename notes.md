

# BASH

## finding largest files on hard drive in terminal

The following yields files around 5GB. You can grab ones that exceed that by adding another zero the the number.

```bash
mdfind 'kMDItemFSSize > 2000000000'
```


# HTML/css

## CSS - to upper case, and to capitalize first letter of each word

```html
text-transform: uppercase
text-transform: capitalize
```

## Using relative and absolute position to place tooltips

We know that it's important to provide a relatively-positioned container if you want to
use x and y coordinates for an absolutely positioned div (this comes in handy for tooltips).

It's important to remember, however, that the relatively positioned tooltip is the one that provides
the coordinate grid for the absolutely positioned div. When giving the tooltip coordinates, for example,
you must ensure that the grid you're working is, itself, appropriately positioned, so that your tooltip's
coordinates aren't relative to, say, the _body_ tag in your HTML document, but the particular div that
you're working with on screen.

#JavaScript

## JavaScript: The Good Parts, first edition

### Chapter 2

This chapter discusses the grammar of JavaScript.

##### Names of variables

##### Numbers

- exponents are expressed in the form _n e x_ where n is a number literal, n is the
  multiplier, annd x is the exponent. e.g., 100 = 1e2.

##### Strings

- These consist of characters that are 16 bits wide. The escape character is /, and allows
  you to inset special characters. You can also use the \u convention to specify charcters using
  their numerical codes.

##### Statements

- Each <script></script> tag contains a compilation unit which is compiled, and then
  immediately executed. JavaScript throws all of these together into a global namespace,
  so in order to avoid namespace conflicts, executing these in separate functions is a must.

The list of statements consists of:

- **var/let/const statements** which instantiate and potentially assign a variable.

- **expression, disruptive, try, if, switch, while, for, do** statements

- Note that usually, **for** loops look like this:

```javascript
for (let i=0; i<10; i++){
}
```

You can, however, run these loops on all of the properties of an object as well:

```javascript
let someObject = {name: 'Tom', occupation: 'mouse catcher', yearsInTheGame: 12}
let key;
for (key in someObject){
console.log(key)
}
```

Also, note that you can get all of the object's properties in another way, using  
_Object.getOwnPropertyNames(someObject)_

##### Expressions

- these include **literals, variable names, expressions in parentheses, expressions with prefix operators, expressions with infix operations, ternary operators, function invocations, array and object access (i.e., refinement), as well as new and delete statments**

### Chapter 3

In JavaScript, nearly everything is an object. There are only 5 types that aren't:

- numbers
- strings
- booleans
- nulls
- undefineds

Consequently, functions are objects, arrays are objects, regular expressions are objects, as
is everything else. Objects are collections of properties, where each property has a name and
value, where the value is anyting except **undefined**. These are the essential JS
data structure, and may contain many other objects. This structure can be represented by a tree.

JS contains a specific prototype linkage feature, which means that objects inherit the properties
of others. This can reduce object initialization time and memory consumption.

- If a property name is a legal javascript name, you don't need to use quotation marks around
  it (e.g., **someProperty**); these properties' values are retrieved using the
  **someObject.someProperty** syntax. If a property's name is not a legal JS name, however,
  you'll need to use quotes (e.g.,**'some-property'**). For retrieval of these values, we use the
  **someObject['some-property']** syntax.

- When assigning variables from object properties, if a certain property's value is
  non-existant, you can use a default value for the assignement:

e.g., **const someThing = someObject.property1 || 'default value'**

- Objects are always passed by reference, and are _never_ copied!

- Each object created from an object literaly is linked to _Object.prototype_, a standard JS object. This is done
  because storing the properties of multiple identical objects is inefficient and redundant. To avoid this redundancy,
  we store many properties of objects in their prototype objects; if THOSE objects don't have those properties,
  JS tries to retrieve them from THEIR prototypes. This is called delegation. NB: if the requested property exists nowhere
  on the prototype chain, its value is **undefined**.

- Checking an object's property's type is usually done using the **typeof** operator. When checking this, however,
  any one of the properties further up on the prototype chain can have a type, so you may actually be looking at the
  property types that belong to objects further up the chain. To ensure that you're looking at the properties of the
  objects themselves, we use the **.hasOwnProperty('propertyName')** method, because it doesn't look any further up the
  prototype chain. Additionally, some of the properties that we may be examining can be functions, and oftentimes, we're
  not interested in these. Consequently, we may want to ignore any properties that are of type function. We combine the
  function-ignoring idea with the **.hasOwnProperty('propertyName')** idea below:

```javascript
let name;
  for (name in someObject){
  if typeof someObject[name] !== 'function'{
  console.log(name)
  }
}
```

- deleting a property from an object only removes it from the particular object you're dealing with. If an
  object has a particular property, and its prototype has a property of the same name, however, the prototype's
  property may end up being visible after deleting that particular objet's property.

### Chapter 4 - Functions

- There are multiple methods of invoking a function, but all of them come with specific characteristics that pertain to
  closures, the **this** keyword, and arguments.

#### Method invocation pattern

- When a function is stored as a property of an object, it's called a method. When it's invoked, the **this** keyword
  refers to the object that contains the function property.

#### Function invocation pattern

- When the **this** keyword is used within a function that's NOT the property of an object, it's used within an invoked
  function. Consequently, if you'd like to use an invoked function within a method (i.e., a function that's a peroperty of
  an object) which uses **this**, you can't actually use it in the standard way. There are two solutions. Before ES6, you used
  to have to assign **this** to a variable, typically named **that**, and then refer to **that** in your inner function.

```javascript
const someObject = {
  value: 1,
  multiplyByRandom: function(){
  const that = this;
  const multiply = function(){
  that.value = that.value \*Math.Random()
  }
  multiply()
  }
}
```

Otherwise, you can also use arrow functions within methods, because inside an arrow function, the **this** keyword refers to
whatever **this** referred to in its parent context.

```javascript
const someObject = {
  value: 1,
  multiplyByRandom: function(){
  const multiply = ()=>{
  this.value = this.value \*Math.Random()
  }
  multiply()
  }
}
```

## ES6 class

### Module 1: VAR, LET, CONST

#### a.

While VARs are scoped to a function, LET and CONST are scoped to block (i.e., curly brackets).

#### b.

You can declared VARs multiple times in the same scope, but not LETs or CONSTs.

#### c.

CONSTs _ARE NOT_ immutable! That is to say, you
can absolutely change the properties of a const object; the data type simply means that consts can't be reassigned.

(you CAN freeze the values of an object, however, you can use \*Object.freeze(varName))

#### d.

Before, you had to use Immediately Invoked Function Expressions (IFFEs) to make sure your code
executed without having variable names leak into the browser-level window namespace:

```javascript
(function(){})()
```

Now, however, because CONSTs and LETs are scoped to the block, you don't need to worry about this!

Additionally, values of variables within for-loops used to leak, because they weren't function-scoped.
That is to say, using

```javascript
for(var i=0; i<10; i++){
  console.log(i)
  setTimeout(()=>{
  console.log(`The value is now ${i}`)
  }, 1000)
}
```

Would log out that the value was 10 every single time. Why? Because the last value of i would be 10, and the
_.setTimeout()_ functions only run 1 second after the final assignment.

To avoid this, we can use block scope, using LET and CONST!

#### e.

Temporal dead zone: if you try to access a VAR variable before you've defined + declared it, you'll see that its
value is undefined. If you try to access a LET or CONST before they're defined, you'll get a reference error. They
still get hoisted, but simply can't be accessed before being defined — that's what Temporal Dead Zone refers to.

### Module 2: Arrow functions

#### Arrow functions intro

- You can't name arrow functions (they're always anonymous) but you can save them to a name variable, thus:

```javascript
const doAThing = (thing) => {console.log(`${thing}`)}
```

#### Returning objects with arrow functions

To return an object with an arrow function, we use parentheses around the object to be returned

- const newArray = array.map(item => ({'property1': 'value1', 'itemValue': item}))

In ES6, however, we can simplify this further. When returning objects using this notation, and if _one of the attributes
is a constant_ defined elsewhere, all we need to do to create that is give the name of the attribute as the name of the
constant. For example, if we were to define _property1_, we'd have the following code:

- const property1='some value'

const newArray = array.map(item=>({property1, 'itemValue':item}))

To display these results as a table, you can also use _console.table(newArray)_

#### Arrow functions and the _this_ keyword

Arrow functions inherit the value of the _this_ keyword from whichever function they're contained in. Whenever
you're dealing with a new function, the _this_ keyword changes meaning and is no longer bound to anything — UNLESS, you're
using arrow functions! That is to say,

```javascript
someElement.addEventListener('click', function(){
console.log(this)
})
```

will log the element. The following, however

```javascript
someElement.addEventListener('click', function(){
console.log(this)

setTimeout(function(){
console.log(this)
})
})
```

will log the window, because in the current case, _this_ isn't bound to anything. How do we log it in the _setTimeout()_
function? By using arrow functions!

```javascript
someElement.addEventListener('click', function(){
console.log(this)

setTimeout(()=>{
console.log(this)
})
})
```

  Why did that work? Because arrow functions inherit the value of _this_ from the parent function!

#### Switching two values of 2 variables in ES6

This is as simple as

```javascript
[first, second]=[second, first]
```

This works because the array on the right side of the equals operator is destructured, and the value of the variable *first*
is saved to the variable *first* in the array on the left of the equals operator. 

#### Default function arguments

Previously, when declaring a function, you needed to check if all necessary variables were passed in, and if they weren't,
give them default values, like below:

```javascript
function addSomeNumbers(number1, number2, number3){

number1 = number1 || 0
number2 = number1 || 0
number3 = number1 || 0

return number1 + number2 + number3
}
```

Now, however, we can declare the default values in the function itself!

```javascript
function addSomeNumbers(number1 = 0, number2 = 0, number3 = 0){
return number1 + number2 + number3
}
```

What if, however, you want to exclude the middle value from the function and run something like

```javascript
addSomeNumbers(1,,4)
```

? You'll need to do so this way:

```javascript
addSomeNumbers(1,undefined,4)
```

which falls back on the default value for that particular argument.

#### When not to use arrow functions

1. When _this_ refers to an object (because arrow functions reference the window object when using _this_ by default, unless
   this has already been defined within the arrow function's context).

2. When we need a method to bind to an object, involving _this_.

```javascript
const someObject = {
age: 22,
increaseAge (){
this.age++;
}
}
```

  3. When using the _arguments_ object

#### Convert between nodeList and array

Since _document.querySelectorAll('.class')_ returns a list of nodes rather than an array, we need to convert its
output to an array in ordert to use standard array functions like *.filter* and *.map*. We do so by using:

```javascript
const arrayFromNodeList = Array.from(nodeList)
```

Usig ES6, we can also use:

```javascript
const arrayFromNodeList = [...document.querySelectorAll('.class')]
```

#### Using .reduce()

Reduce functions when we need to perform a single function on all of the items in an array, with each of those being used 
as some form of cumulative input. We need not only a reduce statement, but a function to use with it.

```javascript
const reducer = (accumulator, item)=> accumulator + item

const total = arrayName.reduce(reducer)
```

#### Toggling class on and off for DOM elements

```javascript
el = document.querySelectorAll('.important')
el.classList.toggle('class-on')
```

This adds the class 'class-on' to the element if it's not already present, and removes it if it already exists.

#### Selecting items that have a particular attribute

This can be done using the name of the attribute, thus:

```javascript
document.querySelectorAll('[some-attribute]')
```

#### Selecting items only based on part of their text content

```javascript
item.textContent.includes('some phrase')
```

This can be used in

```javascript
array.filter(item=>item.textContent.includes('some phrase'))
```

## Array destructuring

When value is an array:

```javascript
const [index] = value
```

is equal to

```javascript
const index = value[0]
```

and

```javascript
const [x,y] = d3.mouse()
```

is equal to

```javascript
const x = d3.mouse()[0]
```

and

```javascript
const y = d3.mouse()[1]
```

### Module 3: Template strings

#### Template strings

You can perform calculations within template literals, thus:

```javascript
const someVar = 5

const answer = `The answer is ${someVar + 10}`
```

#### HTML from template literals

We can save HTML as a variable, and then insert it into an element using the *.innerHTML* function.

```javascript
const school = {name: 'harvard', 'majors': 141 }

const sampleHTML = `
  <div> Welcome to ${school.name}! We have <span> <b>${school.majors}</b> </span> majors!</div>
`
document.body.innerHTML = sampleHTML;
```

#### Looping over array to insert HTML with template literals

We can use standard JS functionality to insert multiple HTML elements at once, as if using a for loop. 

```javascript
const schools = [{name: 'harvard', 'majors': 141 }, {name: 'yale', 'majors': 231 }]

const sampleHTML = `
  <ul>
  ${schools.map(school=>{`<li>This is ${school.name}! It has ${school.majors} majors!</li>`}).join('')}
  </ul>
  `
document.body.innerHTML = sampleHTML;
```


#### Using ternary operators with template literals

```javascript
const school = {name: 'harvard', 'majors': 141, 'color':'red' }

const sampleHTML = `
  <div> 
  <p>${school.name} has ${school.majors}</p>
  ${school.color ? `'s official color is ${school.color}` : ''}
  </div>
  `
document.body.innerHTML = sampleHTML;
```


#### Using functions within string literals

```javascript
const school = {name: 'harvard', 'majors': 141, 'students':10000, 'teachers':5000 }

function teachersPerStudent(studentNumber, teacherNumber){
  return studentNumber/teacherNumber
} 

const sampleHTML = `
  <div> 
  <p>${school.name} has ${school.majors}</p>
  <p>It also has ${teachersPerStudent(school.students, school.teachers)} teachers per student!</p>
  </div>
  `
document.body.innerHTML = sampleHTML;
```


#### Passing template literal strings to functions (e.g., for formatting), aka Tagged templates

This refers to breaking the template up into a number of tags, which we'll format using a particular function 
which takes in a strings + values array (meaning, that you can format the string template in one way, and 
programmatically format the values you've subbed into the template in another way)

```javascript
const name1 = 'Jon'
const name2 = 'Jerry'

function highlight(strings, ...values) {
      //NB if values is size n, strings will always be size n+1  
     
      let returnedString = ''

      strings.map((string, i) => {
        returnedString += `${string} <i>${values[i]||''}</i>`
      })

      return returnedString;
    }

const rawSentence = `Hey, ${name1}, meet my friend ${name2}!`
const editedSentence = highlight `Hey, ${name1}, meet my friend ${name2}!`

document.querySelector('body').innerHTML = editedSentence;
```

#### New ES6 string methods (each of these returns a boolean value)

```javascript
const myName = 'Jonathan Nameguy'
```

to check whether the first part of a string matches a set of characters after n initial characters, we use:

```javascript
myName.startsWith('nathan', 2)
```

to check whether a string ends in a particular pattern of chars, we can likewise use:

```javascript
myName.endsWith('than', 8)
```

where 8 is the number of chars we read from the original string (in this case, it's 'Jonathan').

To see whether a particular pattern of chars is included anywhere in a string, we can use

```javascript
myName.includes('guy')
```

In order to repeat a string multiple times, you use *.repeat(n)*:

```javascript
const hey = `hey${'y'.repeat(10)}`
console.log(hey) 
```

will yield 
> heyyyyyyyyyyy

We can also use this for something like a leftPad function:

```javascript
function leftPad(string, len = 20){
  return `${' '.repeat(len - string.length)}${string}`
}
```

NB: how we set the default function length value at 20, above!


### Module 5: Destructuring 

#### Destructuring: objects

If you need to make variables from each object property, you can _destructure_ that object. Let's say that you have an object of

```javascript
const person = {'firstName', 'Jerry', 'lastName':'Maguire', 'age':40}

const firstName = person.firstName;
const lastName  = person.lastName;
const age = person.age;
```

you can destructure an object much more simply:

```javascript
const {firstName, lastName} = person;
```

We don't necessarily need to include all the object properties for these properties.

You can also rename destructured properties as you're destructuring an object if the need calls for it:

```javascript
const {firstname: name, age: years} = person;
```

If an object doesn't have the relevant values when you're setting values via destructuring, you can set default, fallback values:

```javascript
const {firstName = 'some name' , occupation = 'law guy', nickname = 'jimborino', lastName = 'some last name'} = person;
```

#### Destructuring: arrays

This is very similar to desctructuring objects:

```javascript
const someArray = ['Jerry', 'Mary', 'Simon']

const [lawGuy, biblicalLady, bookPublisher] = someArray;
```

If an array is longer than the number of variables you've included, nothing happens — they get discarded.

If we want to save the first n values to their own variables, but save the remainder to an array, we can use the rest operator:

```javascript
const allWriters = ['Hitch','Didion','Steinbeck','Hemingway']

const [essayist1, essayist2, ...fictionWriters] = allWriters;
```

#### Destructuring: functions

If an object is _returned_ from a function, you destructure its properties into variables on return.

Additionally, if you're _passing_ an object to a function, and you'd rather not use a whole bunch of 
object property accessor notations in order to access the function's argument, like so:

```javascript
function calculateMealPayment(preTax, taxPercent=0.14, tip=0.2){
  let payment = 0
  payment = preTax + preTax*taxPercent + tip * preTax;
  return payment;
}


- const mealCost = {'preTax' = 100, 'taxPercent'=0.16, 'tip' = 0.15}

- calculateMealPayment(mealCost.preTax, mealCost.taxPercent, mealCost.tip)
```

There's an easier way to do it using ES6, by destructuring the arguments we pass into the function. Note that
for this we need to pass an _object_ in, rather than passing in individual values, like we did in the above
example.

```javascript
function calculateMealPayment({preTax, taxPercent=0.14, tip=0.2}){
  let payment = 0
  payment = preTax + preTax*taxPercent + tip * preTax;
  return payment;
}

calculateMealPayment(mealCost)
```

In order to account for failing to pass an object in, we can set a default object for the function:


```javascript
function calculateMealPayment({preTax, taxPercent=0.14, tip=0.2} = {}){
  let payment = 0
  payment = preTax + preTax*taxPercent + tip * preTax;
  return payment;
}
```


### Module 6: Iterables and Loops

#### for-of loops, pt 1: intro

These are a semantically superior way of writing for loops. Instead of lengthy code of type

```javascript
const friends = ['Gerald','George','Marcia','Mary']

for (let i =0; i<friends.length, i++){
  console.log(friends[i])
}
```

we can use the following:

```javascript
const friends = ['Gerald','George','Marcia','Mary'] 

for (const friend of friends){
  console.log(friend)
}
```

We can also break out of these loops, like regular loops.

```javascript
for (const friend of friends){
  if (friend==='George'){break}
}
```

Another benefit of the for-of loop is that it isn't affected by any changes to an array's prototype. 
While a standard for loop would include the added array methods in the array items that it would iterate over, 
a for-of loop does not.


#### for of loops, pt 2: use cases

There are arrays iterators that are contained within the *arrayName.entries()* property. 

We can go through each item in the array manually, by looking through each entry, thus:

```javascript
*arrayName.entries().next()
```

Each of these has a boolean *done* property, which indicates if the iterator is done or not; they also have a
*values* array, which contains the index of the array item and its value.  

If we iterate over this array of entries for the following array:

```javascript
const friends = ['Gerald','George','Marcia','Mary']
```

we can get the index by destructuring the items in the values array, thus:

```javascript
for (const [i, friend] of friends.entries()){
  console.log(`${friend} is friend number ${i}`)
}
```

This type of loop is also useful when we're iterating over the *arguments* object, particularly when
you're not sure of the number of arguments that someone will pass:

```javascript
function addNumbers(){
}

addNumbers(1,2,3,4,5,10,100,200)
```

The arguments passed into *addNumbers()* are not an array, but an object. If you don't want to convert
the *arguments* object into an array using *Array.from()*, you can iterate over this object using a for-of
loop.

```javascript
function addNumbers(){
    let total = 0;

    for (const num of arguments){
      total+=num;
    }

    return total;
  }
```

Finally, you can also iterate over array-like nodelists and HTMLCollections using for of loops.


#### for-of loops pt. 3: objects

We can't simply iterate over an object. 

There are two solutions:

1. There will be an *Object.entries()* method, but it is not universally implemented yet. 

2. There is an object method to get an object's keys: *Object.keys(objectName)*, which returns an 
array of keys. You can run a for-of loop over each of these. 

Having said this, you can always use the *for ... in* syntax to iterate over all properties (inherited and not)
of an object.

### Module 7: arrays

#### Array.from() & Array.of()

We can make arrays from nodeLists using the array object's *Array.from()* method. Since, however,
we often need to map over the resulting array anyway, *Array.from()* takes two arguments: the array-ish
object we're converting to an array, and a map function, which manipulates the array at hand.

```javascript
const $pTags = document.querySelectorAll('p')

const pTagsText = Array.from($pTags, pTag=>{
    return pTag.textContent;
})
```

The pTagsText array will contain the text of all the paragraph tags on the page. 


#### Array.find() and Array.findIndex()

If you have a single, specific object that you want to access in an array because each object is unique, 
you don't need to use *arrayName.filter()*. Rather, it's faster to use *arrayName.find()*, which 
returns a single object with the appropriately filtered value.

*Array.of()*, meanwhile, creates an array from a set of arguments that you pass it.

You can, likewise, get the index of a particular object in an array using *arrayName.findIndex()*.


#### Array.some() and Array.every()

If we want to check whether a single item in an array meets a particular criterion, we use the *arrayName.some()*
method:

```javascript
const people = ['Tom', 'Dick', 'Jane', 'Harry', 'Vanessa', 'Tom']

people.some(person=> person === 'Tom')
```

Once 'Tom' is found in the array, the function returns 'true'.

To find out if _all_ array items fit a certain criterion, we use *Array.every()*. 

```javascript
const peopleAges = [12, 15, 65, 13, 2, 105]

peopleAges.every(age => age >= 20)
```

## Creating + placing DOM elements with vanilla JS

1. Create the tag
2. Set its HTML
3. Place the tag in the appropriate location

```javascript
const $body = document.querySelector('body')
const $pTag = document.createElement('p')

const sentence = '<i>Just</i> a little paragraph!'

$pTag.innerHTML = sentence;

$body.appendChild($pTag);
```


## Triggering events to take place after a D3 transition

There are two ways of chaining multiple transitions together. In the first, we make sure to calculate the timing of these
transitions in order to set the start of the subsequent transition immediately after the first finishes. This method, however,
is somewhat flawed because execution time may be longer than we expect. e.g.,

```javascript
const transition1 = \$item
  .transition()
  .duration(500)
  .style('fill','green')

const transition2 = \$item
  .transition()
  .delays(500)
  .duration(500)
  .style('fill','red')
```

So if transition1 actually executes in 550ms, but the delay we set leads to transition2 firing after 500ms, we've got an overlap.
This may not be a drastic bug in some situations, but for best practice, there's another way of chaining transitions:

```javascript
const transition2 = () =>{
  \$item
  .transition()
  .delays(500)
  .duration(500)
  .style('fill','red')
  }

const transition1 = ()={
  \$item
  .transition()
  .duration(500)
  .style('fill','green')
  .on('end',()=>{

  const transition1Done = false;

  if(!transition1Done) transition2()
  transition1Done = true;
  }

  })
```

## Working with arrays

It's good practice to never edit arrays in place using _.forEach()_, but rather use _.map()_ to create a new array based on the values of the original.

## Adding another package to node so that others pulling repo automatically have it included on install

```bash
npm install packageName --save
```

## CDN - content delivery network

Pulls the data from a single server and distributes across many others, allowing
users to access that data from the closest server on this network; these data are cached.

## nodes vs. d3 objects vs. jquery objects vs. etc.

The DOM consists of nodes, which have children, parents, and siblings — that's the DOM tree, and it forms the basis for the
document object model. Elements, which are nodes that are directly represented in HTML, are just one type of node
(others include the document node, which we access with commands such as _document.getElementById()_).

When we use the d3 object in order to select a particular node, that selection returns a _d3 selection_ rather than the node!

This distinction is important because those selection (whether they're arrays/objects/etc.) have different properties.

## Making slight tweaks to chains of functions

Occasionally, you've written a function which, later, turns out to need to have multiple small differences in output based
on some other variable. You can, of course, add multiple _if_ statements in order to account for this. What may be easier,
however, passing a true/false value to the function you've written.

Then, at the instances where this value is going to decide how the program runs (e.g., which values are passed to
your function) you can use ternary operators to change code flow.

## Working with dates

We can use _new Date(someString)_ to create a date object from a string. We can then use

```javascript
item.getDate() or item.getYear()
```

to find the specific date parts (https://stackoverflow.com/questions/1531093/how-do-i-get-the-current-date-in-javascript).
Note that using _getMonth()_ will mean that you need to add 1 to the result, because January is counted as 0 in JS.

## Promises

Promises are a way to make sure certain steps are performed in order in JS. A promise is created thus:

```javascript
new Promise((resolve,reject)=>{
  if(somethingBad) {
  reject(somethingBad) //logs the error
  }
  else {
  resolve()
  }
  })
```

Usually, it's placed inside a function, thus:

```javascript
function countNumber(){
  return new Promise((resolve,reject)=>{
  if(errorOccurrs){reject(errorOccurrs)}
  else{resolve()}
  })
  }
  ```

JavaScript executes things asynchronously, and promises allow us to avoid some of the issues with this. Let's say you want to
only execute certain functions after a particular function has been executed. We can use promises thus:

```javascript
countNumber()
  .then(()=>{
  doTheNextThing();
  doOtherThings();
  })
  .catch(console.log) //this logs the error thrown following if _reject(arg1)_ runs.
```

## D3: Entering and exiting items using a particular key, and the general enter(), exit(), update() pattern

NB: useful Bostock block: https://bl.ocks.org/mbostock/3808218

Most of the time, updating data in D3 doesn't require object constancy. You have a master array, and you view/hide certain parts of it.
At other times, however, we may need to incoporate some object constancy when loading data. If, for example, we are updating the
data tied to our DOM elements, but only _certain_ elements must be updated while others remain the same (albeit with updated
attributes or styles), we need to make sure that our script can differentiate between a wholly new data point about to be bound to a
new DOM element, and an extant data point which is _already_ bound to a DOM element, but that needs to have its attributes updated.

We do this using _keys_. Specifically, when pairing data, we make sure that data joins are performed using a paritcular data key,
rather than an index number (which is the default):

```javascript
let \$playerDivs = d3.selectAll('div.soccer-player')
  .data(data, d=>d.player-name)
```

If there are already player divs paired with a previous array of player names, and if there are a number of player names
in the previous selection that match the current data, it is _those_ that will be selected using the above syntax.

We can then update these existing divs, thus:

```javascript
 \$playerDivs  
  .st('background-colour', 'red')
```

Next, we can add the new elements present in the data join but that don't yet have DOM elements associated with them:

```javascript
let $newPlayerDivs =$playerDivs  
  .enter()
  .append('div.soccer-player')
  .at('background-color','green')
```

At any point after this, we can then select the DOM elements that no longer have data paired with them using
the .exit() selection and remove them:

```javascript
 \$playerDivs
  .exit()
  .remove()
```

There remains, however, a key step: we need to merge the newly added DOM elements with the previously added DOM elements into
a single selection if we want to be able to deal with all of them simultaneously/ run the equivalent of d3 for-loops on them
all at once:

```javascript
let $mergedPlayers =$newPlayerDivs
  .merge(\$playerDivs)
  .text(d=>d.player-name)
```

Note that it doesn't matter whether you merge the updated selection with the newly-appended one, or the newly appended
selection with the updated one:

```javascript
$newPlayerDivs
.merge($playerDivs)
```

is the same as

```javascript
$playerDivs
.merge($newPlayerDivs)
```

## Map vs forEach

Running a loop over an array using _forEach_ manipulates the array in-place and doesn't create a new one. Using
map, however, returns a new array.

## loading data in jetpack

d3 jetpack allows you to load data in the following manner:

```javascript
d3.loadData(path, (error,reponse)={

})
```

where response is an array of files loaded.

## Nesting in d3

```javascript
d3.nest()
  .key(d=>d.key)
  .entries(data)
```

## Selecting elements within other elements using class names

If we're not sure which types of elements are the parents but want to select only the child elements of a particular group of
parent nodes, we can select them in d3 using the following syntax

```javascript
d3.selectAll('.class-name element)
```

This selects all elements that are the children of the elements that are classed with _.class-name_.

## The difference between _.datum()_ and _.data()_

Most of the time when creating a data join, we use one standard sequence:

```javascript
d3.selectAll('div.example')
  .data([arrayOfObjects])
  .enter()
  .append('div)
```

This is a data join. Once you've chained the _.enter()_ statement to the function, you're dealing with a selection
of elements that are not yet created (i.e., exclusively the new ones).

If there are graphical elements already present here, there are selected using

```javascript
d3.selectAll('div')
  .data(arrayOfObjects)
```

This will update the data associated with the extant graphical items.

If, however, you want to select all the DOM elements that don't have data elements associated with them
now that you've bound them to a new array of different length than what they've been previously associated with, you can do
the following:

```javascript
d3.selectAll('div)
  .data(arrayOfDifferentLength)
  .exit()
```

and manipulate them that way.

However, when you know that you won't be needing to update the data associated with any of the DOM elements you're dealing with,
you can inject the data them directly, without making a join, using _.datum()_

```javascript
d3.selectAll('div')
  .datum(arrayOfObjects, d=> d.attribute)
```  

## Using a voronoi grid to snap to the closest data point

d3.voronoi() is a way of subdividing an SVG/chart's space into polygons surrounding points: wherever your mouse
pointer lands, that area will be closer to one data point (on a 2-d surgace) than any other in the data set,
and this technique helps to establish which points those are.

There are a number of things which we need for this. First, we save the voronoi function:

```javascript
const voronoi = d3.voronoi()
```

Next, we must create voronoi polygons associated with each data point we're including in our chart. Oftentimes when wanting
to implement the voronoi grid, we deal with groups of points because we've nested them (as we often do with paths when
drawing multi-line charts, such as here http://blockbuilder.org/juanprq/334022f178debf3001d2c534926391d0
or here http://blockbuilder.org/emeeks/037488ed37f0e1cbfe32), so if we've done that, we need to revert them to a single,
flat array:

```javascript
const flatArray = d3.merge(data.map(d=>d.values))
```

The function also requires that we indicate the specific rules for drawing the polygons around each of our points! Specifically,
we need to indicate the x & y coordinate values in our data set, as well as the scales we use to map them to the SVG.

Additionally, we need to indicate the size of the polygon that comprises the outer limits of the voronoi grid. We do so below:

```javascript
voronoi
  .x(d => scaleX(d.date))
  .y(d => scaleY(d.views_sum))
  .extent([
  [0, 0],
  [width, height]
  ])
```

We then create a new group, which will contain our voronoi DOM elements:

```javascript
const $voronoiGroup =$svgElement
  .append('g')
  .at('class', 'g-voronoi')
```

Inside of it, we'll create a d3 selection of path elements, since that's essentially which voronoi polygons are:

```javascript
const $voronoiPaths =$voronoiGroup
  .selectAll('path')
  .data(voronoi.polygons(flatArray))
  .enter()
  .append('path')
  .attr("d", d => (d ? "M" + d.join("L") + "Z" : null)) //this step draws the paths from the voronoi data
```

It's important to remember, however, that we need to make sure that we're highlighting the paths that are
associated with the voronoi elements we're mousing over! To make sure we're on the right track, let's make sure
we've established how to create the path elements.

Let's say there are 10 different lines, with dates on the x axis and some value (e.g., points each game)
on the x axis, in our chart, each representing a single athlete.

```javascript
const $athletes =$svgElement
  .selectAll('g.athlete')
  .data(nestedData)
```  

We can create a new group element for each athlete to house their lines, and we can also add a data attribute to each of
those groups, wherein we give each athlete's name:

```javascript
$athletesEnter =$athletes
  .enter()
  .append('g.athlete')
  .at('data-name', d => d.key)

  \$athletesEnter
  .append('path')
```

We can finally indicate how each of these atheltes' line charts will look:

```javascript
\$svgElement
.selectAll('.athlete path') //selects the path WITHIN each athlete group
.datum(d => d.values)
.at('d', line) //NB this is the _d3.line().x().y()_ function, which we set up in the same manner
//as the voronoi line function, earlier
```

Let's get back to mouseover events! Since we've got an athlete name for each individual, we can then
go ahead and grab whichever data is associated with each voronoi polygon, and grab the athlete group containing
that same data attribute (i.e., because we want to select more than just an individual point, but a whole line):

```javascript
function handleVoronoiEnter(d) {
  const athleteName = d.data.name; //this is an example; if you inspect the voronoi data elements, each will have a
  //set of coordinates for a polygon, as well as data attached to it; that data is the same as the original
  //data joined to each of those points, so the same data you used to specify the _data-name_ attribute can be accessed here!
  //In this example, I'm assuming it's located in d.data.name;

      $svgElement
      	.select(`[data-name='${athleteName}'] path`) //this gets the elements with the specified athlete name as the data-attribute,
      //and then the path within that element.
      	.st('stroke', '#f33')

  }
```

And voila! Just add this as an on-mouseenter function to your voronoi code and you're set!

```javascript
 \$voronoiPaths
  .on('mouseenter', handleVoronoiEnter)
```

## Remove duplicate values from arrays

Similar to python, ES6 allows us to create a de-duplicated array from another array thus:

```javascript
let uniqueArray = [...new Set(array)];
```

## First element css

```css
.first-of-type
```

## Closures

We have access to variables defined in enclosing function(s) even after the enclosing function which defines these
variables has returned.

"In essence, a closure is a block of code which can be executed at a later time, but which maintains the environment
in which it was first created - i.e. it can still use the local variables etc of the method which created it,
even after that method has finished executing."
https://stackoverflow.com/questions/1700514/can-someone-explain-it-to-me-what-closure-is-in-real-simple-language/1700627#1700627
https://stackoverflow.com/questions/111102/how-do-javascript-closures-work
https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8

## Splitting a string and limiting the number of items in the resulting array

It's common enough to split a string using _.split()_, but this function actually takes 2 arguments: the
first is the sequence of characters that will be used to split your string, and the second, an optional
one, which demarcates the maximum number of items in the resultant array.

```javascript
"hello, this, is, an, array, to, be, split".split(", ", 2)
```

will result in the following array:

```javascript
["hello", "this"]
```

## Getting an attribute from a particular object (e.g., "this")

D3 has a getter function to perform this. The following function, for example,
will retrieve the x coordinate of the currently selected object:

```javascript
d3.select(this).attr("x")
```

## Using the _this_ keyword in d3 jetpack

D3 jetpack doesn't allow the use of the _this_ keyword, so in order to grab the current DOM element,
you must refer to it in a different way; specifically, by referring to the index of the item you've
selected in the array of DOM nodes:

```javascript
function(d, i, n){
  const thisObject = d3.select(n[i])
  }
```

## Accessing the parent object of a particular DOM element

In order to access a DOM node's parent in D3, we can get the node of any D3 object using _.node()_ :

```javascript
const thisObject = d3.select(this).node()
```

We can then access its parent by using _.node().parentNode_:

```javascript
const parentObject = thisObject.parentNode;
```

## Replacing all occurrences of a phrase within a larger strings

We use the /text/g format to do this:

```javascript
"Some 11 Text 11 You'd 11 Replace".replace(/11/g, ' ')
```

## Executing the html written inside of a string while displaying this strings

Using D3, there's a simple way to do this, using the _.html()_ function. This will include the
html inside your string as html rather than as text. For line breaks, for example,

```javascript
obj.html(d.text_and_html)
```

## Joining two arrays on a common key

While it's relatively simple to create objects/entries that include a combination of data fields in SQL or pandas, the data structures
available in JavaScript are much less suitable for doing so.

When necessary, it is best to do this as part of the data pre-processing step. If it's necessary to perform this during the front-end
step, it's possible to create a third, key object, which you employ as the "join key."

For example, if you've got an array of objects:

```javascript
congressmenAge = [
  {name: James,
  age: 50},

{name: Rogers,
age: 71},

{name: Whittaker,
age: 45},
]
```

and want to create objects that include the home state of these congressmen, included in this array:

```javascript
congressmenHomeState = [
  {name: James,
  state: Iowa},

{name: Rogers,
state: Tennessee},

{name: Whittaker,
state: Texas},
]
```

The simplest way to do so would be to create a new key object, thus:

```javascript
keyObject = {}
  congressmenAge.forEach(function(d){
  keyObject[d.name] = congressmenHomeState[d.name]
  })
```

In later parts of code, you can then always use the names of each congressman as a key value to get their age using the keyObject.

# Mapping

## Working with GDAM: How to deal with a data set that has poor geographic overlap with a shapefile

Often, you may work with a data file that doesn't overlap well with regional boundary data. How to address this issue?

1. Let's first make sure that, at the very least, country-level data overlaps. To do so, you can simply match
   your data to the GDAM data on a country-code level.

2. Let's then try to to join each of the regions in the GDAM files to our data. How to do so? Check out
   the section titled "Joining df1 to df2 when df2 contains multiple potential spellings of the join value in each cell"
   below.

3. To figure out where the remaining disconnect lies between these two data files, prior to dropping the 'ID'
   column at the end of step 2, check which countries still have missing values thus

```python
df_data[df_data['ID'].isnull()]['country_name'].value_counts()
```

This should give you the number of blank regions in each country. You can then correct any issues towards the start of
your script this way:

```python
df_data.loc[(df_data.region_name=='Community of Madrid', 'region_name')] = 'Comunidad de Madrid'
```

## SQL in QGIS

SQL IN QGIS

An example string would look like this:

```
"City" in ('New York','Philadelphia','Boston')
```

## Converting GeoJSON to topoJSON

We use geo2topo for this:

```
geo2topo file1.geojson > file2.json
```

## Heat maps vs. hotspot analysis (Gettis-Ord statistic, etc.)

"Heatmaps are typically about creating a raster interpolation of the density of observations given point data. Hotspot analysis is about locating areas of statistically significant geospatial clusters of observations with a particular attribute within a larger sample population. Put more simply, if we were mapping crime, a heatmap could let us see where crime is taking place, full stop; a hotspot analysis could show where crime is taking place at higher than expected levels given the existing population." -https://gis.stackexchange.com/questions/24175/how-can-i-reproduce-getis-ord-gi-hot-spot-analysis-tool-in-qgis

## Command line cartography

Note that using http://mapshaper.org/ for changing shapefile granularity and converting it
to other file formats is likely easier.

### CONVERTING GeoJSON TO NDJSON

```
ndjson-split 'd.features' < INPUT.json > OUTPUT.ndjson
ndjson-split 'd.features' < all_autowgs84.geojson > all_autowgs84.ndjson
```

### DELETING IRRELEVANT PROPERTIES

```
ndjson-filter 'delete d.properties.VARNAME_1, true, delete d.properties.ENGTYPE_1, true, delete d.properties.ID_0, true, delete d.properties.ID_1, true, delete d.properties.ISO, true, delete d.properties.NL_NAME_1, true, delete d.properties.TYPE_1, true, delete HASC_1, true, delete d.properties.CCN_1, true, delete d.properties.CCA_1, true’ < afg_admin.ndjson > afg_admin_filtered.ndjson
```

### TRANSFORMING NDJSON > JSON

```
ndjson-reduce < afg_admin_filtered.ndjson | ndjson-map '{type: "FeatureCollection", features: d}' > afg_admin_filtered.json
```

### TRANSFORMING GeoJSON > TopoJSON

```
geo2topo -o afg_admin_no_proj.json afg_admin.json
```


### SIMPLIFYING TopoJSON

```
toposimplify -p 1 -f < afg_admin_no_proj.json > afg_admin_no_proj-simple.json
```

### QUANTIZING SIMPLIFIED TopoJSON

```
topoquantize 1e5 < afg_admin_no_proj-simple.json > afg_admin_no_proj-simple-quantized_1e5.json
```

# Python + Pandas

## Finding out which items in list 1 aren't in list 2

```
set(list1) - set(list2)
```

## Converting data from daily/other time interval to weekly/hourly/etc (daily, weekly, monthly here):

```python
df['count'].resample('D', how='sum')
df['count'].resample('W', how='sum')
df['count'].resample('M', how='sum')
```

## Converting a series to list

```python
pd.Series.tolist()
```

## Glob (import glob)
Glob is a library that allows you to search for all files that meet a certain
set of criteria, returning a list of path names.

```python
glob.glob(r'/folder/\*.filetype')
```
  this would return a list of all paths to files of the particular file type in question.

## String literals in python

There are two ways of indicating a string literal/presence of escape characters. The more specific one
entails the use of the backslash to indicate the fact that the following character should be escaped.

The more general, one, however, allows _all_ the characters in a particular string to be read as escape characters (this
is akin to applying the backslash character to every single character in the string). It entails using the _r_
character preceding the opening quote of a string.

```python
string_literal = r'http://www.website.com'
```

## Masking: not conditions

Oftentimes, masking takes the form of

```python
df[df['colname'] == "X" ]
```

Frequently, however, we want to mask a data frame according by specifying the condition that
it _doesn't_ meet, in order to filter that particular result out of the data set. We perform this operation thus,
using the tilde operator:

```python
df[ ~ (df['colname1']=="X") & (df['colname2']=="Y") ]
```

## Running a loop across multiple lists simultaneously

The standard pythonic way of running loops takes the form of

```python
for x in list:
  x=x^2
```

The syntax for iterating across multiple lists (in this example, with the same length), however, is somewhat
different:

```python
for x, y in zip(list_x, list_y):
  x=x^2
  y=y^2
```

## Awkward df reshaping: Melting

Sometimes, our data frames are awkwardly shaped. Take, for example, a df (dfConggressmen) with the
following shape (i.e., columns):

| Month | CongressmanJones | CongressmanWilliams | CongressmanHarris |
| ----- | ---------------- | ------------------- | ----------------- |
| Jan   | 15               | 2                   | 11                |
| Feb   | 7                | 12                  | 4                 |
| Mar   | 4                | 11                  | 9                 |

Each of these shows the number of bill that each congressman sponsored during each month. This sort of shape is great
if we're interested in calculating the total number of bills sponsored with each passing month, but it's also
an awkward format if we want to, say, pair this file with another data set and add coordinates to every Congressman's
HQ (i.e., each row would need 6 additional columns, 3 for 3 Congressmen's latitudes and 3 for 3 of their longitudes).

Needless to say, this becomes a somewhat frustrating data wrangling scenario quickly. Pandas has a function to resolve
these issues with minimum hassle, and will allow us to turn this data frame into the much more managable:

| Month | Congressman         | BillsPassed |
| ----- | ------------------- | ----------- |
| Jan   | CongressmanJones    | 15          |
| Jan   | CongressmanWilliams | 2           |
| Jan   | CongressmanHarris   | 11          |
| Feb   | CongressmanJones    | 7           |
| Feb   | CongressmanWilliams | 12          |
| Feb   | CongressmanHarris   | 4           |

How do we do this?

```python
pd.melt(dfConggressmen, id_vars=['Month'], var_name='Congressman', value_name='BillsPassed')
```

Note: the _id_vars_ parameter lets you specify which columns to keep, and _var_name_ specifies the name that all
the other values from the top row, which will be grouped into a new column, will be called.

The _values_ parameter specifies the name of the column that will contain the values for the newly created column
from the top row variables.

## Converting between data types

When using _pd.melt_, the categories that used to be in the top row of the data frame, and which have now been melted
into the previously mentioned _Congressman_ column, would have become data typed as objects if they had been numerical (i.e.,CongressmanJones = 1; CongressmanWilliams = 2; CongressmanHarris = 3). This isn't especially useful to deal with,
so the best way to convert these to integers would be to first convert them to strings, and then to ints:

```python
df[Congressmen] = df[Congressmen].astype(str).astype(int)
```

##Converting coordinates to lat/long
In order to work with projections, we'll need to install _pyproj_. Certain code snippets online incorporate the use
of Basemap (from mpl_toolkits.basemap import Basemap) but I haven't needed it for this sort of thing/didn't spend too
much time exploring the necessity for it.

```python
import pyproj as pp
```

First, you need to find out the coordinate system used to derive the x/y coordinates you've got to work with. Once
you've figured this out, save this variable. You'll also need to save the projection you'll be transforming the
x + y coords into.

```python
Brit_Natl_Grid = pp.Proj(init='epsg:27700') # projection you're transforming from (i.e., input) 
WGS84 = pp.Proj(init='epsg:4326') # projection  you're transforming to (i.e., output) 
```

If you're dealing with a single set of coordinates, you can get their lon/lat pairings thus:

```python
lon, lat = pp.transform(Brit_Natl_Grid, WGS84, x_coord, y_coord)
```

Still, usually we do this in pandas, and are working with columns of text. There is likely an easier way to do this
in place, but here's a quick hack that gets results. First, convert each of the coord columns to a list:

```python
geo_x = df['geometry_x'].tolist()
geo_y = df['geometry_y'].tolist()
```

We then iterate this previous function through each item across both of these lists:

```python
list_of_coords = []
  for x, y in zip(geo_x, geo_y):
  coords = {}
  lon = 0
  lat = 0
  lon, lat = pp.transform(Brit_Natl_Grid, WGS84, x, y)
  coords['lon'] = lon
  coords['lat'] = lat
  list_of_coords.append(coords)
  ```

We can then take this list of dictionaries and transform them to a data frame, which we then merge with
the original df:

```python
coords_only_df = pd.DataFrame(list_of_coords)

complete_df = pd.concat([df, coords_only_df], axis=1)
```

## Ranking variables

Oftentimes, you want to rank a variable in a way such that not every row receives its own individual
ordinal designation. You may, for example, want to rank sections of rows within a data frame, such that

| Month | Rank  | Value |
| ----- | ----- | ----- |
| 18    | 1     | 2     |
| 18    | 1     | 12    |
| 19    | 2     | 98    |
| 19    | 2     | 10    |
| 20    | 3     | 60    |
| 20    | 3     | 18    |

There's a relatively painless way to do this using Pandas:

```python
df['Rank']=df['Month'].rank(ascending=True, method = 'dense')
```

The _method = 'dense'_ is the crucial bit of code here, and allows you to rank individual items within
the Month column, rather than do so on a by-row basis.

## Saving a data frame to TSV

This is relatively simple, and employs the _.to_csv()_ function, thus:

```python
df.to_csv("df_name.tsv", sep='\t')
```

## Removing the pandas index from a dF in order to output to a CSV file

Again, this is a relatively straightforward procedure using the same function as above:

```python
df.to_csv("df_name.tsv", sep='\t', index=False)
```

## Evaluating a string to a variable name/object name:
_repr()_ returns a value that, when passed to _eval()_ or _exec()_ yields an object with the same value. -https://stackoverflow.com/questions/4010840/generating-variable-names-on-fly-in-python

Instead of doing this, however, it's better to use a dictionary, or lists.

```python
for x in list_of_variable_names:
```

## Creating multiple data frames from a single, global data frame programatically:

We first create an empty dictionary. Then, for each string/int/float in the list of variable names
(indicated below as _range(1985,2018)_), we create a dictionary key. The data corresponding to this
key will be generated by referring to the data frame, df, using the _global()_ function, which
precedes it, as below.

```python
df = pd.read_csv("data.csv")
df_dict = {}

for i in range(1985,2018):
    df_dict[i] = (globals()['df'])[(globals()['df'])['year']==str(i)]
```    

## Splitting single rows whose cells contain multiple items that must be split into several rows (while retaining 
## all other data from previous rows):

If a data frame has several rows whose values you must split, and place in adjacent rows with remaining cells
which contain otherwise duplicate data, we first turn all letters to lower case.

```python
df_dict.text = df_dict.text.apply(lambda x: x.lower())
```

Next, split the column on the required string, turn the data into a series with the top level of column values
moved from column names to categories/repeated fields

```python
s = df_dict.text.str.split("dear abby: ").apply(pd.Series, 1).stack()
```

Then, set the index:

```python
s.index = s.index.droplevel(-1)
```

And set the name of the column:

```python
s.name = 'text'
```

Then remove the old column in your initial data frame:

```python
del df_dict['text']
```

And join it to the newly created series.

```python
df_dict = df_dict.join(s)
```

- https://stackoverflow.com/questions/12680754/split-explode-pandas-dataframe-string-entry-to-separate-rows -https://stackoverflow.com/questions/17116814/pandas-how-do-i-split-text-in-a-column-into-multiple-rows

## Merging 2 data frames:

If we've got _df1_ and _df2_:

```python
pd.merge(df1, df1, left_on="column_to_join_in_left_df", right_on="column_to_join_in_right_df")
```

## Splitting up a string into several parts in each row of a df column

To do so, split a string into two parts — the string used here is ' (' — and select the first,
or second, part of the resultant array.

```python
jan_df['state']=jan_df['region'].apply(lambda x: x.split(' (')[0])
```

## Replacing strings using regex: STRINGS IN W/IN PATTERN

First, ensure that you've imported the regex library:

```python
import re
```

Then, apply a lambda function to search for whatever pattern we're referring to — say, whatever's
between brackets — within a string. To do so, we need to use groups.

```python
r'\( (.\*?) \)'
```

In the code above, there are two groups — the first, outer one, which contains the string with two
parentheses, and the inner one, which contains whatever characters ( indicated by .\*, which means any
character, repeating any number of times) are contained within them. we can specify
which part of a matched string, X, we use, by using this group parameter:

```python
re.search(r'\((.\*?)\)',x).group(1)
```

Putting it all together, the code reads thus:

```python
raw_df['new_column']=raw_df['old_column'].apply(lambda x: re.search(r'\((.\*?)\)',x).group(1))
```

## Deleting all but a selected number of columns in a data frame

This is v simple: simple subset the df with the list of columns you'd like to keep, thus

```python
df_filtered = df[['column_1', 'column_2', 'column_16']]
```

Note the double bracket.

## Filtering out duplicate rows

```python
df.drop_duplicates()
```

## Finding out the name of the column containing the maximum value for every row

```python
df['max']=df[['column1','column2','column3']].idxmax(axis=1)
```

## For a particular column, filling in null values with values of another column

For one project, we worked on some tricky geographic manipulation. We had a shapefile with a certain number
of regions; our data, which needed to be bound to each of these regions, occasionally had different spelling
of region names.

How to get around this?

First: For all of the regions in our data, we tried to match them, by name, with the geo data. For
any data region that didn't have a pairing, we manually matched geo regions from the shapefile — which we
exported to an alphabetically-ordered, side-by-by side list in Excel — to our regions.

Second, since our data had insufficient coverage compared to the shapefile we used, we needed to
use country-level averages from our data to fill in any regional blanks. How did we do this?

```python
df.loc[df['some_regional_null_values'].isnull(),'some_regional_null_values'] = df['country_avg']
```

## Renaming columns correctly

While it's possible to use df.columns =['newName1','newName2','newName13'] to set column names, this may not always work; the correct way,
particularly when doing so for specifi columns, is to use the following:

```python
df = df.rename(columns={'oldName1': 'newName1', 'oldName2': 'newName2'})
```

## Get specific part of a date from a datetime object

```python
df['month'] = df['date_columnb'].dt.month
```

## Converting a string to a column

```python
df['col'] = pd.to_datetime(df['col'])
```

## Stripping parts of a string from a column in a df

### For white space:

```python
df.column_name.map(lambda x: x.strip(' '))
```

### For other parts of a string

Use _.rstrip()_ and _.lstrip()_ (right strip and left strip):

```python
df.column_name.map(lambda x: x.lstrip('undesired part of string'))
```

### To strip data for only a section of a df

```python
df.loc[(df.column_name=='value to subset column_name', 'column_to_be_edited')] = df.column_to_be_edited.map(lambda x: x.lstrip('some string'))
```

## Null values

### Filling in column's null values with values from another column

```python
df.loc[(pd.isnull(df.COLUMN_WITH_NULL_VALUES), 'COLUMN_WITH_NULL_VALUES')] = df.COLUMN_WHOSE_VALUES_WILL_BE_SUBSTITUTED_IN
```

## Joining df1 to df2 when df2 contains multiple potential spellings of the join value in each cell

To do so, we can create a dictionary which matches any value in df2 to the index in df1. Let's say we're
dealing with a df that contains data, _df_data_, and another df that contains geographic information, _df_geo_.

Each row of a single column in _df_geo_ contains multiple potential matches for the key in _df_data_, thus:

| Common_name | Possible_names | Value |
| ----------- | -------------- | ----- |
| NYC         | New York      | NYC   | New York City | 20 |
| LA          | Los Angeles   | LA    | Hollywood | 10 |

We should create a dictionary that matches up all the possible match keys in 'Possible_names' to an index. We can do it thus:

```python
d = dict((y,ids) for ids, val in enumerate(df.Possible_names.str.split(r'\|', expand=True).values) for y in val if y is not None)
```

For each row in the df, we break out the possible name alternatives by splitting the value in each respective cell
according to the break character (| in this case).

If the Possible_names cell isn't blank, we pair the list of enumerated values therein to each row's ID. We end up with
a dictionary, where all the possible alternate names are linked to a single ID.

Next, add a column to _df_data_ called ID. We'll be using the dictionary for this — specifically, if _df_data_ looks like this,

| City_name     | Data_value |
| ------------- | ---------- |
| 'New York'    | 400        |
| 'Los Angeles' | 800        |

We'd be mapping over the 'City*name' column to see if it matches any of the city names in \_df_geo*. If it does, we
will give it the ID we assigned to each of those locations:

```python
df_data['ID'] = df_data.City_name.map(d)
```

We can then merge the two dataframes, and drop the ID column:

```python
df_data.merge(df_geo, left_on='ID', right_index=True, how='left').drop(['ID'], axis=1)
```

## Generating random values in a column using numpy

You can generate a random number between some floor and a ceiling for each cell in a column thus:

```python
df['random_number_column'] = np.random.randint(floor_num, ceiling_num, df.shape[0])
```

## Accessing index as an array

```python
df.index.values
```

## Converting row to be a header

```python
df.columns = df.iloc[n]
```

where n indicates the row number (0 indexed). You should then be able to drop that row using

```python
df.reindex(df.index.drop(n))
```

## Calculating a rolling average

We can do this for a single column:

```python
df['rolling_avg'] = data['raw_values'].rolling(5).mean()
```

Where 5 refers to the window. Note that the window has a huge number of settings
(https://docs.scipy.org/doc/scipy/reference/signal.html#window-functions); if these aren't specified,
you can pass _.rolling()_ a number which will indicate the number of rows to use as the window. Additionally, it's worth
mentioning that we don't necessarily have to calculate the average — we can calculate other statistics, such as the sum.

If you want to apply this rolling average to ALL the columns in the dataframe:

```python
df.rolling(5).mean()
```

## Dropping certain rows

If you need to drop rows using their index, we can do so using the _.iloc_ function.

```python
df2 = df1.iloc[n:]
```

This drops all rows prior to row _n_.

## Removing the index column name

Occasionally, you'll set the index from a column, and it'll remain, inconveniently, named. To remove this, simply use this:

```python
df.index.name = None
```

## Dropping all rows with negative values

```python
df[(df > 0).all(1)]
```

## Correlating _ALL_ columns in a df with a particular series/list

If you're trying to correlate columns to another column in a data frame:

```python
df.apply(lambda x: x.corr(someSeries, method='spearman'))
```

You can also use _.corrwith()_ to calculate a correlation between one df's columns and another column in a second df.

## Row-wise average in a pandas data frame

```python
df.mean(axis=1)
```

## Using df.loc[]

When you know the row index, you can read the row, as a series, using 

```python
df.loc[n]
```

You can also read this as an array using the _native series property *.values*_, thus:

```python
df.loc[n].values
```

This can be done with any series notation (e.g., _df['series_name']_). You can also access the value of 
a particular cell at the intersection of a known row/column name thus:

```python
df.loc[n, column_name]
```

## Indices in python

Both the row names *AND* the column names are actually of type index (although different types of index). 
You can see this via the output of

```python
print(type(df.index))
print(type(df.columns))
```

## Quick overview of data frame in python

To get a quick sense of the shape of the df, we call its *.shape* property:

```python
df.shape
```

## Removing the index name once you've set a column as an index

```python
df.index.name = None
```


## Setting an index as a column

If the index doesn't have a name, we'll nee to give it one:

```python
df.index.name = 'new_name'
```

We then reset the index with

```python
df.reset_index(inplace=True)
```

or with 

```python
df = df.reset_index()
```

## Axes in pandas

Axis 0 is vertical, i.e., columns
Axis 1 is horitozontal, i.e., rows


|------------------------------------------------|
|            |  A      |  B     |                |
|------------------------------------------------|
|      0     | 0.626386| 1.52325| -> axis=1    ->|
|            |         |        |                |
|            |         |        |                |
|            |↓axis=0 ↓|        |                |
|            |         |        |                |

## Using pd.concat to concatenate series to a df

If a list of dictionaries has dicts whose properties/keys have a value that you'd like to add to a data frame,
you can cocatenate the series to the df. Let's say we've got the following df:

| Month | CongressmanJones | CongressmanWilliams | CongressmanHarris |
| ----- | ---------------- | ------------------- | ----------------- |
| Jan   | 15               | 2                   | 11                |
| Feb   | 7                | 12                  | 4                 |
| Mar   | 4                | 11                  | 9                 |

and we have another congressman that we'd like to add:

```python
CongressmanRowles = [
{'Feb': 2},
{'Mar': 12}
]
```

How do we add congressman Rowles? We use the months as the index, and add it thus:

```python
pd.concat([df,CongressmanRowles], axis=1)
```

Even though Congressman Rowles has no values for January, pandas takes care of null values automatically:


| Month | CongressmanJones | CongressmanWilliams | CongressmanHarris |CongressmanRowles |
| ----- | ---------------- | ------------------- | ----------------- |----------------- |
| Jan   | 15               | 2                   | 11                |Nan               |
| Feb   | 7                | 12                  | 4                 |2                 |
| Mar   | 4                | 11                  | 9                 |12                |

## Getting memory usage of a df

```python
df.info(memory_usage='deep')
```

Gives a quick overview of the memory usage. If you want the memory usage of each series, you can use

```python
df.memory_usage(deep=True)
```

and, if you want the total bytes used, tack on a *.sum()* statement at the end, like so:

```python
df.memory_usage(deep=True).sum()
```

Note that we need to pass the *memory_usage='deep'* parameter because otherwise, pandas will output the 
memory usage that the reference addresses for each series in the df take up, which means that we'll have a 
_minimum_ estimate rather than the actual memory usage. 

## Cutting down memory for dfs using the categories type

Oftentimes, we may have string data in our cells that refers to categories, or that we seek use categorically 
rather than in a non-specific way. We can check the number of unique values we'll treat as categories using:

```python
sorted(df.col_name.unique())
```

There are, likely, only a few unique strings. Consequently, we don't really _need_ each of the strings, 
which are really categories, to be stored as strings, because that's pretty wasteful in terms of memory. Thus, 
we can make that column/series into *categories* type; under the hood, this creates a hash table which 
pandas uses to look up category names when displaying things to us, hashing the category name to a much 
lower-memory-requirement variable. 

```python
df['col_name'] = df['col_name'].astype('category')
```

Using this will allow for significantly faster processing of data. Note that if most of your category strings
are actually unique, adding a hash table will require even _more_ memory than the series would have needed. 

You can also create a hierarchy of values (i.e., make values ordinal) for the categories that you create:

```python
df['col_name'] = df['col_name'].astype('category', categories = ['first','second','third'], ordered=True)
```

You can then sort the values on that column:


```python
df.sort_values(by='col_name'])
```

and additionally, use the value of each category as a mask when subsetting the dataframe:


```python
df[df['col_name']>'second']
```

## Subsetting data frame using .loc

There are a couple of differente notation types that we can use to subset a data frame. Take the example above,
which we use most of the time. We can, however, also subset using the standard R notation, where 
we use the *.loc[]* notation, which gets all rows and columns that fit a specific condition:

> df.loc[column_condition, row_condition]

If we want to get all rows for a particular column condition, 

we use 

```python
df.loc[df.col_name> value,:]
```

## Basic python stuff:

The following are the data structures used: lists, tuples, dictionaries, strings, sets and frozensets. New to me:
tuples. What's up with those? They are, apparently, much like lists apart from the fact that they are immutable — i.e.,
you cannot change their value once they're created.
