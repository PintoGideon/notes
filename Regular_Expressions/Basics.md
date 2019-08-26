### Regular expressions

Regular expressions is a method of working with
patterns.

### Creating a RegEx in JS.

Regular expressions are objects in JS. Inside the wuotes, we have the pattern.

```javascript
let regex1= new RegExp("Hello");
let regex2- /word/;

```

Once you have a regex object, you can then use it with one of the methods on RegExp Constructor or the String object wrapper.

```javascript
let txt = 'Programming courses always start with a hello world example';
let regex1 = new RegExp('hello');
console.log(regex1.test(txt)); //true
```

### Using Regular Expressions in Javascript

The second method available to us is exec().

```javascript
let regex1 = new RegExp('Hello');
regex1.exec(txt); // [0:"hello", index:41, input:""]
txt.match(regex1); // Same output as above
txt.search(regex1); // 41
```

### The replace methods is one of the commonly used methods in JS

We are looking for a pattern "hello" in the txt input and replace that with hi.

```javascript
txt.replace(regex1, 'h1');
txt.split(regex1);
```

### The split method.

```javascript
let txt = 'Programming courses always starts with a hello world example';

let regex1 = /\s/;
txt.split(regex1); //Creates an array of all of the strings in an array.
```

### RegEx object methods

- test: returns true if pattern is found in the passed string: false if not.
- exec: returns an array of matches.
- toString: returns a string of the regular expression syntax.

### String Methods

- match: returns an array of matches just like exec on RegExp.
- search: returns an index of the matched string
- replace: replace matches with a string
- split: splits a string into an array.

### Regular Expression Flags

g- global, match more than one occurrence.
i- case insensitive match, case doesn't matter
m- multi-line match

```javascript
let txt = 'Programming courses alwayS starts with a hello world example';

let regex1 = /s\s/gi;
txt.match(regex1);
[s, s, s];
```

The RegEx object works a bit differently.

```javascript
regex1.exec(txt); //s
regex1.exec(txt); // S
```

**_Regexpal is a tool to test out regular expressions_**

When the engine looks for a match, it works through this string one character at a time as it encounters each character, It determines if it fits the pattern or not. If the pattern consists of multiple character, it must determine the first character and then the next character and so on until it finds a match or determines it is not a match.

If no match is found, the engine must rewind the string to the next character in the sequence to start all over again.

A quick illustration here would be this:

```javascript

let re=/hello/

//Here is the string

here help hello

```

The JS engine check if 'h' matches the pattern, it does so it moves to 'e' and then 'l' and but stops there as the next recurring 'e' in 'here' to does not match the patter. Now it rewinds to the character 'e' in 'here' and checks if it matches the character 'h' in 'hello'.

### Metacharacters

### Wildcard

The wildcard is represented by a '.' (period).
A wildcard can be anything. When a period is used in a regular expression, it represents any single character with the exception of some control characters like new line.

Here is a string:
how is it so hot?

and here is the regular expression
/h.t/g

The output that matches this expressio is 'hot'.

**_ A period only matches a single character _**

### Escaping Metacharacters

Now what if our pattern had a period.

---

For example:

String: This could be the final word.
Pattern: /d./g

---

This pattern matches two character in the string. The 'd' with the space and the 'd' with the period.

I want to find the 'd' with a period though.

---

Pattern: /d\./g

---

**_ Check the cheat sheet to check the characters that needs to be escaped_**

However, we can escape any character as the backslash only tells the engine to consider the character following it as a literal character.

### Matching Control Characters

- \t represents a tab
- \v represents a vertical tab
- \n represents a newline
- \r represents a carriage return

```javascript
let phoneNums=["801-766-8754", "801-545=5454", "435-666-1212"]

//Loop through the array
// Check if each value matches the pattern
let filteredNums=phoneNums.map(phoneNum=>{
let pattern='/801\-/g';
if (phoneNum.match(pattern)){
return num;
}
}

```

### Using Characer Sets to find matches

We can specify a set of characters in brackets.
For example:

gray or grey?

```javascript
let re = /gr[ae]y/;
```

### Specifying a Range

In a character set, a hyphen represents a range. Each character set matches a single character.

```javascript

Exception 0xAF89F
/0x[0-9A-F][0-9A-F]/g
Ouput: 0xAF

```

The carrot (^) is a negation meaning we don't want to match one of those characters.

### Metachacters you may need to escape in a character set

- Hyphen(-)
- Carrot(^)
- \ (hash symbol)
- Right bracket (])

### Shorthands for character sets

-\d [0-9] (Number Character)
-\w [a-zA-z0-9_] (Word Character)
-\s [\t\r\n] (Space, tabs, newLine)

### Indicating Character Repitition

- Matches one or more occurrences
  ? Matches zero or more occurrences

* Matches zero or more occurrences

### Greediness and Laziness in Regular Expressions

By Default, regular expressions are greedy. They try to match all the characters in the pattern. Instead of grabbing everything we can, the expression can be made lazy to grab as little as it can. How do we make a regular expression greedy?

```javascript

//String
617-447-7451

//Pattern
/\d{3}=\d{3}-\d{4}/

```

### Regular expression to check for a valid phone number of the types ((717-222-8012), 717-717-71717, 717.717.7171)

```javascript

(function(){
let phone=document.getElementById("phone");
let regex=/\(?\d{3}\)?[-.]?\d{3}[-.]?\d{4}/;

phone.addEventListener("keyup",function(){

if(regphone.test(phone.value)){
phone.classList.remove('red");
phone.classList.add("green");
}
else{
phone.classList.remove('green');
phone.classList.add('red');
});
})();

```

Each element in the DOM tree is represented by an object
and these objects have APIs so that they can be manipulated. For instance, a link (e.g. The a element in the tree above) can have it's "href" attribute change in several ways:

```javascript
var a = document.links[0];
a.href = 'sample.html';
a.protocol = 'https';
a.setAttrbite('href', 'https://example.com');
```

### Important !!

When accepting untrusted input e.g: user generated content such as text comments, value in URL parameters, messages from third-party sites , etc, it is imperative that the data be validated before use, and properly escaped when displayed. Failing to do this can allow a hostile user to perform a variety of attacks, ranging from the potentially benign, such as providing bogus user info like a negative age, to the serious such as running scripts every time a user looks at a page that includes information, potentially propogating the attack in the process, to the catastrophic , such as deleting data in the server.

### Pitfalls to avoid

Scipts in HTML have "run-to-completion" semantics meaning that the browser will generally run the script uninterrupted before doing anything else scuh as firing further events or continuing to parse the document.

### DOM trees

A node A is inserted into a node B when the insertion steps are invoked with A as the argument and the A's new parent is B. Similarly, a node A is removed from B when the removing steps are invoked with A as the removedNode argument and B as the oldParent argument.

### Specifying Repitition Amount

{min,max} Matches min to max occurrences.
{min} Matches min occurrences
{min,} Matches min or more occurrences.

### Matching the Beginning and Ending

### Anchoring Metacharacters.

- ^ Anchors the match to the start of the line.
  = \$ Anchors the match to the end of the line

### Word Boundary Metacharacters.

The characters are referencing positions and not an acutal character

- \b Word-boundary= Pattern bounded by a non-word character
- \B Nonword boundary- Pattern bounded by a word character
- Word characters: \w or [a-zA-Z0=9]

### Writing Accuarate Regular Expressions

- When possible define the quanitity of regular expressions
  = Narrow the scope of repeated expressons. Intially we used wildcard but the scope could be every character and hence we use a character set.
- We specified starting and ending points using anchor tags.
- We need to test multiple data sets

### Exercise:

The contents file contains a string of text stored in a variable text1. This string of text is a statement that contains a day of the week as a part of the statement. Write a regular expression that will match any day of the week and then replace that day of the week in the string with Monday. Display the results of the console.

```javascript
The regular expression here would be /\b[m,t,w,f,s][a-z]{1,4}[n,s,i,r]day\b/igm
let newText=text.replace(reg, 'Monday');
```

### Specifying options using the pipe symbol

### Using Grouping

Let us match the date pattern using a regular expression (yyyy/mm/dd)

/^(\d\d\d\d)[-./](\d{1,2})[-./](\d{1,2})\$/

### Lookahead Groups

### Matching Domains ( in Regexpal)

- Pattern ---> /\w+(?=\.com)/g

### Matching Domains (in Javascript)

```javascript
let data="allthingsjavascript.com google.com youtube.com"
let reg=/\w+(?=\.com)/g,
```

### Understanding Capturing Groups

If we want to capture the word 'yoyo' with a pattern.
The pattern with groups would like this.
/(yo)\1/g

When we choose to repeate a group, it does not catch the pattern but the text for example, in the pattern below:

```javascript
/^(\d\d\d\d)([-./])(\d{1,2})\2(\d{1,2})$/g;
```

Here the pattern [-./] repeates twice in dates and hence I use the pattern \2 to capture that. These are called group backreferences.

Here is an example for matching a domain name. We want to force the pattern to match a '.com'

```javascript
let data = 'allthingsjavascript.com google.com youtube.com';
let reg = /w+(?=\.com)/g;
let arr = data.match(reg);

//["allthingsjavascript", "google","youtube"]
```

### Creating a regular expression for passwords

- Must contain atleast 8 or more characters. (Using a lookahead group means that the first group does not consume any characters.
- Must contain an uppercase, lowercase and a number

```javascript
/^(?=.{8,})(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9]).*$/g;
```

### Negative Lookahead Groups

In a lookahead group, the input should match each look ahead group. For example, a password should be atleast 8 or more characters, have a digit, have a lowercase character. In a negative lookahead group (?!), the group is not a match for the input. We are trying to make sure it does not include the following group.

### Exercise

Iterate through the data provided. Use a regular expression to store the names in a new array but change the order of the name so first name is listed first and the last name is last.

```javascript
let data = ['Jensen, Dale', 'Smith,Andrea'];

let reg = /(\w+),(\w+)/;

let newData = data.map(val => {
	let arr = reg.exec(val);
	if (arr !== null) {
		return arr[2] + '' + arr[1];
	} else return null;
});
```

### Applying Regular Expressions

1. Matching email addresses (/^[^\s@]+@[^\s@.]+\.[^\s@.]+\$/)
2. Matching twitter handles (/^@?(\w+)\$/)

### Using Replace with Regular Expressions

```javascript
// Create a new array that shows the names with the firstName and the surname.

let names = [
	'Smith, James',
	'Peterson, Alyssa',
	'Johnson, Lynette',
	'Lopez, Tony'
];

let newNames = names.map(function(name) {
	return name.replace(/(\w+),(\w+)/, '$2 $1');
});
```
