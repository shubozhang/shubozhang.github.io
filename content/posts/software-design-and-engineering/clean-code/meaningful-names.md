---
title: "Meaningful Names"
date: 2021-01-16
menu:
    sidebar:
        name: Meaningful Names
        identifier: meaningful-names
        parent: clean-code
        weight: 10
---


### Use Intention-Revealing Names
The name of a variable, function, or class, should answer all the big questions. It should tell you 
* why it exists
* what it does 
* how it is used. 

#### Bad example:
If a name requires a comment, then the name does not reveal its intent. 
```
int d; // elapsed time in days
```
The name `d` reveals nothing. It does not evoke a sense of elapsed time, nor of days. 

#### Good example:
We should choose a name that specifies what is being measured and the unit of that measurement:
```
int elapsedTimeInDays; 
int daysSinceCreation; 
int daysSinceModification; 
int fileAgeInDays;
```

### Avoid Disinformation
Programmers must avoid leaving false clues that obscure the meaning of code. 
* We should avoid words whose entrenched meanings vary from our intended meaning. For example, `hp`, `aix`, and `sco` 
  would be poor variable names because they are the names of Unix platforms or variants. 

* Naming a grouping of accounts that properly reflect its property. 
  * `accountList` if it’s a List. 
  * `accountSet` if it’s a Set. 
  * `accountMap` if it’s a Map.
  * just plain `accounts` .


### Make Meaningful Distinctions
* Number-series naming (a1, a2, .. aN) is the opposite of intentional naming.
* Noise words are another meaningless distinction.
    ```
    getActiveAccount(); 
    getActiveAccounts(); 
    getActiveAccountInfo();
    ```

### Use Pronounceable Names

### Use Searchable Names
Suggestion: single-letter names can ONLY be used as local variables inside short methods. 
The length of a name should correspond to the size of its scope.


### Avoid Encodings


### Avoid Mental Mapping
One difference between a smart programmer and a professional programmer is that the professional understands that 
`clarity` is king. Professionals use their powers for good and write code that others can understand.

### Class Names
Classes and objects should have noun or noun phrase names like Customer, WikiPage, Account, and AddressParser. Avoid words 
like Manager, Processor, Data, or Info in the name of a class. A class name should not be a verb.


### Method Names
Methods should have verb or verb phrase names like postPayment, deletePage, or save. Accessors, mutators, and predicates 
should be named for their value and prefixed with get, set, and is according to the javabean standard


### Don’t Be Cute

### Pick One Word per Concept
Pick one word for one abstract concept and stick with it. 
For instance, it’s confusing to have `fetch`, `retrieve`, and `get` as equivalent methods of different classes. 

Likewise, it’s confusing to have a controller and a manager and a driver in the same code base. What is the essential 
difference between a DeviceManager and a Protocol-Controller? Why are both not controllers or both not managers? 
Are they both Drivers really? The name leads you to expect two objects that have very different type as well as having 
different classes.

A consistent lexicon is a great boon to the programmers who must use your code.

### Don’t Pun
Avoid using the same word for two purposes. Using the same term for two different ideas is essentially a pun.

### Use Solution Domain Names
Remember that the people who read your code will be programmers. So go ahead and use computer science (CS) terms, 
algorithm names, pattern names, math terms, and so forth. 

The name AccountVisitor means a great deal to a programmer who is familiar with the VISITOR pattern. 
What programmer would not know what a JobQueue was? There are lots of very technical things that programmers have to do. 
Choosing technical names for those things is usually the most appropriate course.

### Use Problem Domain Names
When there is no “programmer-eese” for what you’re doing, use the name from the problem domain. 
At least the programmer who maintains your code can ask a domain expert what it means.
Separating solution and problem domain concepts is part of the job of a good programmer and designer. 
The code that has more to do with problem domain concepts should have names drawn from the problem domain.


### Add Meaningful Context
Imagine that you have variables named firstName, lastName, street, houseNumber, city, state, and zipcode. 
Taken together it’s pretty clear that they form an address. But what if you just saw the state variable being used alone 
in a method? Would you automatically infer that it was part of an address?

You can add context by using prefixes: addrFirstName, addrLastName, addrState, and so on. At least readers will 
understand that these variables are part of a larger structure. Of course, a better solution is to create a class 
named Address. Then, even the compiler knows that the variables belong to a bigger concept.

### Don’t Add Gratuitous Context
In an imaginary application called “Gas Station Deluxe,” it is a bad idea to prefix every class with GSD. 
Frankly, you are working against your tools. You type G and press the completion key and are rewarded with a mile-long 
list of every class in the system. Is that wise? Why make it hard for the IDE to help you?

Likewise, say you invented a MailingAddress class in GSD’s accounting module, and you named it GSDAccountAddress. 
Later, you need a mailing address for your customer con- tact application. Do you use GSDAccountAddress? Does it sound 
like the right name? Ten of 17 characters are redundant or irrelevant.

Shorter names are generally better than longer ones, so long as they are clear. Add no more context to a name than is necessary.
The names accountAddress and customerAddress are fine names for instances of the class Address but could be poor names 
for classes. Address is a fine name for a class. If I need to differentiate between MAC addresses, port addresses, 
and Web addresses, I might consider PostalAddress, MAC, and URI. The resulting names are more precise, which is the 
point of all naming.

### Final Words
The hardest thing about choosing good names is that it requires good descriptive skills and a shared cultural background. 
This is a teaching issue rather than a technical, business, or management issue. 
As a result many people in this field don’t learn to do it very well.

People are also afraid of renaming things for fear that some other developers will object. 
We do not share that fear and find that we are actually grateful when names change (for the better). 

Most of the time we don’t really memorize the names of classes and methods. We use the modern tools to deal with details 
like that so we can focus on whether the code reads like paragraphs and sentences, or at least like tables and 
data structure (a sentence isn’t always the best way to display data). You will probably end up surprising someone 
when you rename, just like you might with any other code improvement. 

Don’t let it stop you in your tracks. Follow some of these rules and see whether you don’t improve the readability of your code. 
If you are maintaining someone else’s code, use refactoring tools to help resolve these problems. 
It will pay off in the short term and continue to pay in the long run.
