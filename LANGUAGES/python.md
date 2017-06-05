# =============================================
## Installing Python
**Python comes preinstalled on OS X with developer tools installed**
brew install python
curl https://bootstrap.pypa.io/ez_setup.py -o - | sudo python
sudo easy_install pip
Check your /usr/bin and /usr/local/bin for easy_install installations and remove any old script:
sudo rm -f /usr/bin/easy_install*
sudo rm -f /usr/local/bin/easy_install*
Download and run distribute:
curl -O https://svn.apache.org/repos/asf/oodt/tools/oodtsite.publisher/trunk/distribute_setup.py

sudo python distribute_setup.py
sudo rm distribute_setup.py
Try again, and enjoy. E.g.:
sudo easy_install pip

# =============================================
## Requirements File (pip)
Location: ./requirements.txt 
Purpose:  Development dependencies.
A Pip requirements file should be placed at the root of the repository. It should specify the dependencies required to contribute to the project: testing, building, and generating documentation.

If your project has no development dependencies, or you prefer development environment setup via setup.py, this file may be unnecessary.


## Python setuptools
Setuptools is a package development process library designed to facilitate packaging Python projects by enhancing the Python standard library distutils (distribution utilities). It includes: Python package and module definitions.

## Setup.py
Location: ./setup.py 
Purpose:  Package and distribution management.
If your module package is at the root of your repository, this should obviously be at the root as well.

# =============================================
## Makefile
Location: ./Makefile 
Purpose:  Generic management tasks.
If you look at most of my projects or any Pocoo project, you'll notice a Makefile laying around. Why? These projects aren't written in C... In short, make is a incredibly useful tool for defining generic and platform agnostic tasks for your project.
Sample Makefile:

init:
pip install -r requirements.txt

test:
py.test tests

# =============================================
# Python
Python is a general-purpose interpreted, interactive, object-oriented, and high-level programming language. It was created by Guido van Rossum during 1985- 1990. Like Perl, Python source code is also available under the GNU General Public License (GPL). This tutorial gives enough understanding on Python programming language.
**Python is Interpreted**: Python is processed at runtime by the interpreter. You do not need to compile your program before executing it. This is similar to PERL and PHP.

**Python is Interactive**: You can actually sit at a Python prompt and interact with the interpreter directly to write your programs.

**Python is Object-Oriented**: Python supports Object-Oriented style or technique of programming that encapsulates code within objects.

**Python is a Beginner's Language**: Python is a great language for the beginner-level programmers and supports the development of a wide range of applications from simple text processing to WWW browsers to games.


###### Interactive Mode Programming
Invoking the interpreter without passing a script file as a parameter brings up the following prompt −
```
$ python
Python 2.4.3 (#1, Nov 11 2010, 13:34:43)
[GCC 4.1.2 20080704 (Red Hat 4.1.2-48)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>
Type the following text at the Python prompt and press the Enter:

>>> print "Hello, Python!"
```

###### Script Mode Programming
Invoking the interpreter with a script parameter begins execution of the script and continues until the script is finished. When the script is finished, the interpreter is no longer active.

Let us write a simple Python program in a script. Python files have extension .py. Type the following source code in a test.py file:
print "Hello, Python!"
We assume that you have Python interpreter set in PATH variable. Now, try to run this program as follows −
```
$ python test.py
This produces the following result:
Hello, Python!
```
Let us try another way to execute a Python script. Here is the modified test.py file −
```
print "Hello, Python!"
```
We assume that you have Python interpreter available in /usr/bin directory. Now, try to run this program as follows −
```
$ chmod +x test.py     # This is to make file executable
$./test.py
```
This produces the following result −
Hello, Python!

###### Identifiers
A Python identifier is a name used to identify a variable, function, class, module or other object. An identifier starts with a letter A to Z or a to z or an underscore (_) followed by zero or more letters, underscores and digits (0 to 9).

Python does not allow punctuation characters such as @, $, and % within identifiers. Python is a case sensitive programming language. Thus, Manpower and manpower are two different identifiers in Python.

Here are naming conventions for Python identifiers −
* Class names start with an uppercase letter. All other identifiers start with a lowercase letter.
* Starting an identifier with a single leading underscore indicates that the identifier is private.
* Starting an identifier with two leading underscores indicates a strongly private identifier.
* If the identifier also ends with two trailing underscores, the identifier is a language-defined special name.

###### Waiting for user input
The following line of the program displays the prompt, the statement saying “Press the enter key to exit”, and waits for the user to take action −
```
raw_input("\n\nPress the enter key to exit.")
```

###### Multiple statements on a single line
The semicolon ( ; ) allows multiple statements on the single line given that neither statement starts a new code block. Here is a sample snip using the semicolon −
```
import sys; x = 'foo'; sys.stdout.write(x + '\n')
```

###### Comments
A hash sign (#) that is not inside a string literal begins a comment. All characters after the # and up to the end of the physical line are part of the comment and the Python interpreter ignores them.
```
# First comment
print "Hello, Python!" # second comment
```

###### Lines and Indentation


###### Arithmetic Operators
+ Addition	Adds values on either side of the operator.	a + b = 30
- Subtraction	Subtracts right hand operand from left hand operand.	a – b = -10
* Multiplication	Multiplies values on either side of the operator	a * b = 200
  / Division	Divides left hand operand by right hand operand	b / a = 2
  % Modulus	Divides left hand operand by right hand operand and returns remainder	b % a = 0
  ** Exponent	Performs exponential (power) calculation on operators	a**b =10 to the power 20
  //	Floor Division - The division of operands where the result is the quotient in which the digits after the decimal point are removed. But if one of the operands is negative, the result is floored, i.e., rounded away from zero (towards negative infinity):	9//2 = 4 and 9.0//2.0 = 4.0, -11//3 = -4, -11.0//3 = -4.0


###### Comparison Operators
==	If the values of two operands are equal, then the condition becomes true.	(a == b) is not true.
!=	If values of two operands are not equal, then condition becomes true.
>	If the value of left operand is greater than the value of right operand, then condition becomes true.	(a > b) is not true.
><	If the value of left operand is less than the value of right operand, then condition becomes true.	(a < b) is true.
>=	If the value of left operand is greater than or equal to the value of right operand, then condition becomes true.	(a >= b) is not true.
><=	If the value of left operand is less than or equal to the value of right operand, then condition becomes true.	(a <= b) is true.

# =============================================
# Variables
Variables are nothing but reserved memory locations to store values. This means that when you create a variable you reserve some space in memory.

Based on the data type of a variable, the interpreter allocates memory and decides what can be stored in the reserved memory. Therefore, by assigning different data types to variables, you can store integers, decimals or characters in these variables.

###### Multiple Assignment
Python allows you to assign a single value to several variables simultaneously. For example −
```
a = b = c = 1
```
Here, an integer object is created with the value 1, and all three variables are assigned to the same memory location. You can also assign multiple objects to multiple variables. For example −
```
a, b, c = 1, 2, "john"
```

###### Deleting Variable
del varA

# =============================================
# Decision Making

```
if expression:
   statement(s)
else:
   statement(s)

if var2:
   print "2 - Got a true expression value"
   print var2
else:
   print "2 - Got a false expression value"
   print var2
```


```
var = 100
if var < 200:
   print "Expression value is less than 200"
   if var == 150:
      print "Which is 150"
   elif var == 100:
      print "Which is 100"
   elif var == 50:
      print "Which is 50"
elif var < 50:
   print "Expression value is less than 50"
else:
   print "Could not find true expression"

print "Good bye!"
```



# =============================================
# Loops
###### for
```
for letter in 'Python':     # First Example
   print 'Current Letter :', letter

fruits = ['banana', 'apple',  'mango']
for fruit in fruits:        # Second Example
   print 'Current fruit :', fruit
print "Good bye!"

fruits = ['banana', 'apple',  'mango']
for index in range(len(fruits)):
   print 'Current fruit :', fruits[index]
print "Good bye!"

for num in range(10,20):  #to iterate between 10 to 20
   for i in range(2,num): #to iterate on the factors of the number
      if num%i == 0:      #to determine the first factor
         j=num/i          #to calculate the second factor
         print '%d equals %d * %d' % (num,i,j)
         break #to move to the next number, the #first FOR
   else:                  # else part of the loop
      print num, 'is a prime number'
```

###### while
A while loop statement in Python programming language repeatedly executes a target statement as long as a given condition is true.
```
count = 0
while (count < 9):
   print 'The count is:', count
   count = count + 1
print "Good bye!"
```

```
count = 0
while count < 5:
   print count, " is  less than 5"
   count = count + 1
else:
   print count, " is not less than 5"
```
When the above code is executed, it produces the following result −
```
0 is less than 5
1 is less than 5
2 is less than 5
3 is less than 5
4 is less than 5
5 is not less than 5
```

###### break
It terminates the current loop and resumes execution at the next statement, just like the traditional break statement in C.

```
for letter in 'Python':     # First Example
   if letter == 'h':
      break
   print 'Current Letter :', letter

Current Letter : P
Current Letter : y
Current Letter : t
```

###### continue
It returns the control to the beginning of the while loop.. The continue statement rejects all the remaining statements in the current iteration of the loop and moves the control back to the top of the loop.
```
for letter in 'Python':     # First Example
   if letter == 'h':
      continue
   print 'Current Letter :', letter
```

###### pass
It is used when a statement is required syntactically but you do not want any command or code to execute. It is like a TODO.
The pass statement is a null operation; nothing happens when it executes. The pass is also useful in places where your code will eventually go, but has not been written yet

# =============================================
# Type Conversion
* Type int(x) to convert x to a plain integer.
* Type long(x) to convert x to a long integer.
* Type float(x) to convert x to a floating-point number.
* Type complex(x) to convert x to a complex number with real part x and imaginary part zero.
* Type complex(x, y) to convert x and y to a complex number with real part x and imaginary part y. x and y are numeric expressions
* str(x) - Converts object x to a string representation.
* repr(x) - Converts object x to an expression string.
* eval(str) - Evaluates a string and returns an object.
* tuple(s)- Converts s to a tuple.
* list(s) - Converts s to a list.
* set(s) - Converts s to a set.
* dict(d) - Creates a dictionary. d must be a sequence of (key,value) tuples.
* frozenset(s) - Converts s to a frozen set.
* chr(x) - Converts an integer to a character.
* unichr(x) - Converts an integer to a Unicode character.
* ord(x) - Converts a single character to its integer value.
* hex(x) - Converts an integer to a hexadecimal string.
* oct(x) - Converts an integer to an octal string.

# =============================================
# Numbers
* int (signed integers): They are often called just integers or ints, are positive or negative whole numbers with no decimal point.
* long (long integers ): Also called longs, they are integers of unlimited size, written like integers and followed by an uppercase or lowercase L.
* float (floating point real values) : Also called floats, they represent real numbers and are written with a decimal point dividing the integer and fractional parts. Floats may also be in scientific notation, with E or e indicating the power of 10 (2.5e2 = 2.5 x 102 = 250).
* complex (complex numbers) : are of the form a + bJ, where a and b are floats and J (or j) represents the square root of -1 (which is an imaginary number). The real part of the number is a, and the imaginary part is b. Complex numbers are not used much in Python programming.


###### Mathematical Functions
**abs(x)** - The absolute value of x: the (positive) distance between x and zero.
**ceil(x)** - The ceiling of x: the smallest integer not less than x
**cmp(x, y)** - -1 if x < y, 0 if x == y, or 1 if x > y
**exp(x)** - The exponential of x: ex
**fabs(x)** - The absolute value of x.
**floor(x)** - The floor of x: the largest integer not greater than x
**log(x)** - The natural logarithm of x, for x> 0
**log10(x)** - The base-10 logarithm of x for x> 0 .
**max(x1, x2,...)** - The largest of its arguments: the value closest to positive infinity
**min(x1, x2,...)** - The smallest of its arguments: the value closest to negative infinity
**modf(x)** - The fractional and integer parts of x in a two-item tuple. Both parts have the same sign as x. The integer part is returned as a float.
**pow(x, y)** - The value of x**y.
**round(x [,n])** - x rounded to n digits from the decimal point. Python rounds away from zero as a tie-breaker: round(0.5) is 1.0 and round(-0.5) is -1.0.
**sqrt(x)** - The square root of x for x > 0

###### Random Number Functions
choice(seq) - A random item from a list, tuple, or string.
randrange ([start,] stop [,step]) - A randomly selected element from range(start, stop, step)
random() - A random float r, such that 0 is less than or equal to r and r is less than 1
seed([x]) - Sets the integer starting value used in generating random numbers. Call this function before calling any other random module function. Returns None.
shuffle(lst) - Randomizes the items of a list in place. Returns None.
uniform(x, y) - A random float r, such that x is less than or equal to r and r is less than y

###### Trigonometric Functions
acos(x) - Return the arc cosine of x, in radians.
asin(x) - Return the arc sine of x, in radians.
atan(x) - Return the arc tangent of x, in radians.
atan2(y, x) - Return atan(y / x), in radians.
cos(x) - Return the cosine of x radians.
hypot(x, y) - Return the Euclidean norm, sqrt(x*x + y*y).
sin(x) - Return the sine of x radians.
tan(x) - Return the tangent of x radians.
degrees(x) - Converts angle x from radians to degrees.
radians(x) - Converts angle x from degrees to radians.


# =============================================
# Lists
The most basic data structure in Python is the sequence. Each element of a sequence is assigned a number - its position or index. The first index is zero, the second index is one, and so forth.

list1 = ['physics', 'chemistry', 1997, 2000];
list2 = [1, 2, 3, 4, 5 ];
list3 = ["a", "b", "c", "d"]

list1 = ['physics', 'chemistry', 1997, 2000];
list2 = [1, 2, 3, 4, 5, 6, 7 ];

print "list1[0]: ", list1[0]
print "list2[1:5]: ", list2[1:5]

Updating Values
list = ['physics', 'chemistry', 1997, 2000];

print "Value available at index 2 : "
print list[2]
list[2] = 2001;
print "New value available at index 2 : "
print list[2]

Deleting List elements
list1 = ['physics', 'chemistry', 1997, 2000];

print list1
del list1[2];
print "After deleting value at index 2 : "
print list1

Basic Operations
len([1, 2, 3])	3	Length
[1, 2, 3] + [4, 5, 6]	[1, 2, 3, 4, 5, 6]	Concatenation
['Hi!'] * 4	['Hi!', 'Hi!', 'Hi!', 'Hi!']	Repetition
3 in [1, 2, 3]	True	Membership
for x in [1, 2, 3]: print x,	1 2 3	Iteration

L = ['spam', 'Spam', 'SPAM!']
L[2]	'SPAM!'	Offsets start at zero
L[-2]	'Spam'	Negative: count from the right
L[1:]	['Spam', 'SPAM!']	Slicing fetches sections

###### Functions
cmp(list1, list2) - Compares elements of both lists.
len(list) - Gives the total length of the list.
max(list) - Returns item from the list with max value.
min(list) - Returns item from the list with min value.
list(seq) - Converts a tuple into list.

###### Methods
list.append(obj) - Appends object obj to list
list.count(obj) - Returns count of how many times obj occurs in list
list.extend(seq) - Appends the contents of seq to list
list.index(obj) - Returns the lowest index in list that obj appears
list.insert(index, obj) - Inserts object obj into list at offset index
list.pop(obj=list[-1]) - Removes and returns last object or obj from list
list.remove(obj) - Removes object obj from list
list.reverse() - Reverses objects of list in place
list.sort([func]) - Sorts objects of list, use compare func if given

# =============================================
# Sets
mylist = [u'nowplaying', u'PBS', u'PBS', u'nowplaying', u'job', u'debate', u'thenandnow']
myset = set(mylist)
print myset

> > > d = set()
> > > type(d)
> > > <type 'set'>
> > > d.update({1})
> > > d.add(2)
> > > d.update([3,3,3])
> > > d
> > > set([1, 2, 3])

# =============================================
# Tuples
A tuple is a sequence of immutable Python objects. Tuples are sequences, just like lists. The differences between tuples and lists are, the tuples cannot be changed unlike lists and tuples use parentheses, whereas lists use square brackets.
```
tup1 = ('physics', 'chemistry', 1997, 2000);
tup2 = (1, 2, 3, 4, 5 );
tup3 = "a", "b", "c", "d";

The empty tuple is written as two parentheses containing nothing −
tup1 = ();
```

###### Accessing Values
To access values in tuple, use the square brackets for slicing along with the index or indices to obtain value available at that index. For example −
```
tup1 = ('physics', 'chemistry', 1997, 2000);
tup2 = (1, 2, 3, 4, 5, 6, 7 );

print "tup1[0]: ", tup1[0]
print "tup2[1:5]: ", tup2[1:5]
```

###### Updating Tuples
Tuples are immutable which means you cannot update or change the values of tuple elements. 
```
tup1 = (12, 34.56);
tup2 = ('abc', 'xyz');

# Following action is not valid for tuples
# tup1[0] = 100;

# So let's create a new tuple as follows
tup3 = tup1 + tup2;
print tup3
```

###### Deleting tuple elements
Removing individual tuple elements is not possible. There is, of course, nothing wrong with putting together another tuple with the undesired elements discarded.
this is valid
del tup1 

###### Basic Operations
len((1, 2, 3))	3	Length
(1, 2, 3) + (4, 5, 6)	(1, 2, 3, 4, 5, 6)	Concatenation
('Hi!',) * 4	('Hi!', 'Hi!', 'Hi!', 'Hi!')	Repetition
3 in (1, 2, 3)	True	Membership
for x in (1, 2, 3): print x,	1 2 3	Iteration

###### Default is tuple
Any set of multiple objects, comma-separated, written without identifying symbols, i.e., brackets for lists, parentheses for tuples, etc., default to tuples
a = 1,2
is equal to
a = (1,2)

###### Functions
**cmp(tuple1, tuple2) -** Compares elements of both tuples.
**len(tuple) -** Gives the total length of the tuple.
**max(tuple) -** Returns item from the tuple with max value.
**min(tuple) -** Returns item from the tuple with min value.
**tuple(seq) -** Converts a list into tuple.

# =============================================
# Dictionary
Each key is separated from its value by a colon (:), the items are separated by commas, and the whole thing is enclosed in curly braces. An empty dictionary without any items is written with just two curly braces, like this: {}.
Keys are unique within a dictionary while values may not be. The values of a dictionary can be of any type, but the keys must be of an immutable data type such as strings, numbers, or tuples. If we attempt to access a data item with a key, which is not part of the dictionary, we get an error 
```
dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First', 2: "c"}
dict = {}
dict['one'] = "This is one"
dict[2]     = "This is two"
print "dict['Name']: ", dict['Name']
print "dict['Age']: ", dict['Age']
```
**Adding and Updating dict**
```
dict['Age'] = 8; # update existing entry
dict['School'] = "DPS School"; # Add new entry
```
**Deleting keys and dict**
```
dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'}
del dict['Name']; # remove entry with key 'Name'
dict.clear();     # remove all entries in dict
del dict ;        # delete entire dictionary
```

###### Dictionary Functions
**cmp(dict1, dict2)** - Compares elements of both dict.
**len(dict)** - Gives the total length of the dictionary. This would be equal to the number of items in the dictionary.
**str(dict)** - Produces a printable string representation of a dictionary
**type(variable)** - Returns the type of the passed variable. If passed variable is dictionary, then it would return a dictionary type.

###### Dictionary Methods
**dict.clear()** - Removes all elements of dictionary dict
**dict.copy()** - Returns a shallow copy of dictionary dict
**dict.fromkeys()** - Create a new dictionary with keys from seq and values set to value.
**dict.get(key, default=None)** - For key key, returns value or default if key not in dictionary
**dict.has_key(key)** - Returns true if key in dictionary dict, false otherwise
**dict.items()** - Returns a list of dict's (key, value) tuple pairs
**dict.keys()** - Returns list of dictionary dict's keys
**dict.setdefault(key, default=None)** - Similar to get(), but will set **dict[key]=default** if key is not already in dict
**dict.update(dict2)** - Adds dictionary dict2's key-values pairs to dict
**dict.values()** - Returns list of dictionary dict's values

**Methods**
Methods in python are generally reffered to as instance mehtods/functions.

**Functions**
Functions are generally the normal functions or class functions.

More than one entry per key not allowed. Which means no duplicate key is allowed. When duplicate keys encountered during assignment, the last assignment wins. For example −

dict = {'Name': 'Zara', 'Age': 7, 'Name': 'Manni'}

print "dict['Name']: ", dict['Name']
When the above code is executed, it produces the following result −

dict['Name']:  Manni


Built in dictionary functions and methods

# =============================================
# Strings
Strings are amongst the most popular types in Python. We can create them simply by enclosing characters in quotes. Python treats single quotes the same as double quotes. Python does not support a character type; these are treated as strings of length one, thus also considered a substring.
```
var1 = 'Hello World!'
var2 = "Python Programming"
print "var1[0]: ", var1[0]
print "var2[1:5]: ", var2[1:5]
```

###### Updating strings
```
var1 = 'Hello World!'
print "Updated String :- ", var1[:6] + 'Python'
```

String special Operators
+ **Concatenation** - Adds values on either side of the operator	a + b will give HelloPython
* **Repetition** - Creates new strings, concatenating multiple copies of the same string	a*2 will give -HelloHello
  []	**Slice** - Gives the character from the given index	a[1] will give e
  [ : ]	**Range Slice** - Gives the characters from the given range	a[1:4] will give ell
  in	**Membership** - Returns true if a character exists in the given string	H in a will give 1
  not in	Membership - Returns true if a character does not exist in the given string	M not in a will give 1
  **r/R	Raw String** - Suppresses actual meaning of Escape characters. The syntax for raw strings is exactly the same as for normal strings with the exception of the raw string operator, the letter "r," which precedes the quotation marks. The "r" can be lowercase (r) or uppercase (R) and must be placed immediately preceding the first quote mark.	print r'\n' prints \n and print R'\n'prints \n
  **%	Format** - Performs String formatting	See at next section

###### Escape Charachters

###### String Formatting Operator
print "My name is %s and weight is %d kg!" % ('Zara', 21) 

###### Triple Quotes

###### String Methods
**capitalize() -** Capitalizes first letter of string
**center(width, fillchar) -** Returns a space-padded string with the original string centered to a total of width columns.
**count(str, beg= 0,end=len(string)) -** Counts how many times str occurs in string or in a substring of string if starting index beg and ending index end are given.
**decode(encoding='UTF-8',errors='strict') -** Decodes the string using the codec registered for encoding. encoding defaults to the default string encoding.
**encode(encoding='UTF-8',errors='strict') -** Returns encoded string version of string; on error, default is to raise a ValueError unless errors is given with 'ignore' or 'replace'.
**endswith(suffix, beg=0, end=len(string)) -** Determines if string or a substring of string (if starting index beg and ending index end are given) ends with suffix; returns true if so and false otherwise.
**expandtabs(tabsize=8) -** Expands tabs in string to multiple spaces; defaults to 8 spaces per tab if tabsize not provided.
**find(str, beg=0 end=len(string)) -** Determine if str occurs in string or in a substring of string if starting index beg and ending index end are given returns index if found and -1 otherwise.
**index(str, beg=0, end=len(string))** - Same as find(), but raises an exception if str not found.
**isalnum() -** Returns true if string has at least 1 character and all characters are alphanumeric and false otherwise.
**isalpha() -** Returns true if string has at least 1 character and all characters are alphabetic and false otherwise.
**isdigit() -** Returns true if string contains only digits and false otherwise.
**islower() -** Returns true if string has at least 1 cased character and all cased characters are in lowercase and false otherwise.
**isnumeric() -** Returns true if a unicode string contains only numeric characters and false otherwise.
**isspace() -** Returns true if string contains only whitespace characters and false otherwise.
**istitle() -** Returns true if string is properly "titlecased" and false otherwise.
**isupper() -** Returns true if string has at least one cased character and all cased characters are in uppercase and false otherwise.
**join(seq) -** Merges (concatenates) the string representations of elements in sequence seq into a string, with separator string.
**len(string) -** Returns the length of the string
**ljust(width[, fillchar]) -** Returns a space-padded string with the original string left-justified to a total of width columns.
**lower() -** Converts all uppercase letters in string to lowercase.
**lstrip()** - Removes all leading whitespace in string.
**maketrans()** - Returns a translation table to be used in translate function.
**max(str)** - Returns the max alphabetical character from the string str.
**min(str)** - Returns the min alphabetical character from the string str.
replace(old, new [, max]) - Replaces all occurrences of old in string with new or at most max occurrences if max given.
**rfind(str, beg=0,end=len(string))** - Same as find(), but search backwards in string.
**rindex( str, beg=0, end=len(string))** - Same as index(), but search backwards in string.
**rjust(width,[, fillchar])** - Returns a space-padded string with the original string right-justified to a total of width columns.
**rstrip() -** Removes all trailing whitespace of string.
**split(str="", num=string.count(str))** - Splits string according to delimiter str (space if not provided) and returns list of substrings; split into at most num substrings if given.
**splitlines( num=string.count('\n'))** - Splits string at all (or num) NEWLINEs and returns a list of each line with NEWLINEs removed.
**startswith(str, beg=0,end=len(string))** - Determines if string or a substring of string (if starting index beg and ending index end are given) starts with substring str; returns true if so and false otherwise.
**strip([chars])** - Performs both lstrip() and rstrip() on string
**swapcase()** - Inverts case for all letters in string.
**title()** - Returns "titlecased" version of string, that is, all words begin with uppercase and the rest are lowercase.
**translate(table, deletechars="")** - Translates string according to translation table str(256 chars), removing those in the del string.
**upper()** - Converts lowercase letters in string to uppercase.
**zfill (width)** - Returns original string leftpadded with zeros to a total of width characters; intended for numbers, zfill() retains any sign given (less one zero).
**isdecimal()** - Returns true if a unicode string contains only decimal characters and false otherwise.

# =============================================
# Functions
A function is a block of organized, reusable code that is used to perform a single, related action. Functions provide better modularity for your application and a high degree of code reusing.
As you already know, Python gives you many built-in functions like print(), etc. but you can also create your own functions. These functions are called user-defined functions.

* Function blocks begin with the keyword def followed by the function name and parentheses ( ( ) ).
* Any input parameters or arguments should be placed within these parentheses. You can also define parameters inside these parentheses.
* The first statement of a function can be an optional statement - the documentation string of the function or docstring.
* The code block within every function starts with a colon (:) and is indented.
* The statement return [expression] exits a function, optionally passing back an expression to the caller. A return statement with no arguments is the same as return None.

```
def functionname( parameters ):
   "function_docstring"
   function_suite
   return [expression]

def printme( str ):
   "This prints a passed string into this function"
   print str
   return
```
###### Calling a function
```
printme("Again second call to the same function")
```

###### Function definition is here
```
def changeme( mylist ):
   "This changes a passed list into this function"
   mylist.append([1,2,3,4]);
   print "Values inside the function: ", mylist
   return
```

```
#Now you can call changeme function
mylist = [10,20,30];
changeme(mylist);
print "Values outside the function: ", mylist

#Function definition is here
def changeme(mylist):
   "This changes a passed list into this function"
   mylist = [1,2,3,4]; # This would assig new reference in mylist
   print "Values inside the function: ", mylist
   return

#Now you can call changeme function
mylist = [10,20,30];
changeme( mylist );
print "Values outside the function: ", mylist
```

###### Required Arguments
Error is produced if the number of arguments dont match.
```
# Function definition is here
def printme(str):
   "This prints a passed string into this function"
   print str
   return;

# Now you can call printme function
# error is produced
printme()
```

###### Keyword Arguments
Keyword arguments are related to the function calls. When you use keyword arguments in a function call, the caller identifies the arguments by the parameter name.
This allows you to skip arguments or place them out of order because the Python interpreter is able to use the keywords provided to match the values with parameters. 

```
# Function definition is here
def printme( str ):
   "This prints a passed string into this function"
   print str
   return;

# Now you can call printme function
printme( str = "My string")

# Function definition is here
def printinfo( name, age ):
   "This prints a passed info into this function"
   print "Name: ", name
   print "Age ", age
   return;

# Now you can call printinfo function. Order can be changed and the variables are automatically mapped
printinfo( age=50, name="miki" )
```

###### Default Arguments
A default argument is an argument that assumes a default value if a value is not provided in the function call for that argument. 
```
# Function definition is here
def printinfo( name, age = 35 ):
   "This prints a passed info into this function"
   print "Name: ", name
   print "Age ", age
   return;

# Now you can call printinfo function
printinfo("some")
printinfo(adsf="some") # produces error
printinfo( name="miki" ) # works
printinfo( age=50, name="miki" ) # order can be changed if arguments are named
```

###### Variable Length Arguments
You may need to process a function for more arguments than you specified while defining the function. These arguments are called variable-length arguments and are not named in the function definition, unlike required and default arguments.
```
# Function definition is here
def printinfo( arg1, *vartuple ):
   "This prints a variable passed arguments"
   print "Output is: "
   print arg1
   for var in vartuple:
      print var
   return;

# Now you can call printinfo function
printinfo( 10 )
printinfo( 70, 60, 50 )
```

###### Global and Local Variables
Variables that are defined inside a function body have a local scope, and those defined outside have a global scope.
Normal variables are not passed as reference.
```
total = 0; # This is global variable.
# Function definition is here
def sum( arg1, arg2 ):
   # Add both the parameters and return them."
   total = arg1 + arg2; # Here total is local variable.
   print "Inside the function local total : ", total
   return total;

# Now you can call sum function
sum( 10, 20 );
# prints 30
print "Outside the function global total : ", total   
# prints 0
```

Functions within Functions
```
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)
```

Where to define Functions in Script
Generator functions
A function or method which uses the yield statement (see section The yield statement) is called a generator function. Such a function, when called, always returns an iterator object which can be used to execute the body of the function: calling the iterator’s iterator.***next***() method will cause the function to execute until it provides a value using the yield statement. When the function executes a return statement or falls off the end, a StopIteration exception is raised and the iterator will have reached the end of the set of values to be returned.

Coroutine functions
A function or method which is defined using async def is called a coroutine function. Such a function, when called, returns a coroutine object. It may contain await expressions, as well as async with and async for statements. See also the Coroutine Objects section.

Asynchronous generator functions
A function or method which is defined using async def and which uses the yield statement is called a asynchronous generator function. Such a function, when called, returns an asynchronous iterator object which can be used in an async for statement to execute the body of the function.

Calling the asynchronous iterator’s aiterator.***anext***() method will return an awaitable which when awaited will execute until it provides a value using the yield expression. When the function executes an empty return statement or falls off the end, a StopAsyncIteration exception is raised and the asynchronous iterator will have reached the end of the set of values to be yielded.
	
# =============================================
# Python Modules
Python modules are one of the main abstraction layers available and probably the most natural one. Abstraction layers allow separating code into parts holding related data and functionality. 
A module allows you to logically organize your Python code. Grouping related code into a module makes the code easier to understand and use. A module is a Python object with arbitrarily named attributes that you can bind and reference.

###### Defining module. Any python source file is a module
You can use any Python source file as a module by executing an import statement in some other Python source file. The import has the following syntax. A module is loaded only once, regardless of the number of times it is imported. This prevents the module execution from happening over and over again if multiple imports occur.
When the interpreter encounters an import statement, it imports the module if the module is present in the search path. A search path is a list of directories that the interpreter searches before importing a module. For example, to import the module support.py, you need to put the following command at the top of the script

```
# Import module support
import support

# Now you can call defined function that module as follows
support.print_func("Zara")

# If there is no module present then it throws an error
import abc
```
When a module is imported then the code in it gets run too. Therefore it is adviced to have only function definitions in the module and then call them in the file importing them.

###### from..import statement
Python's from statement lets you import specific attributes from a module into the current namespace. The from...import has the following syntax
For example, to import the function fibonacci from the module fib, use the following statement. This statement does not import the entire module fib into the current namespace; it just introduces the item fibonacci from the module fib into the global symbol table of the importing module.
```
from fib import fibonacci
```

###### from..import * statement
It is also possible to import all names from a module into the current namespace by using the following import statement. Also in this case we dont require to add prefix of the module name in calling the function present in that module.
```
from modname import *
# use this 
some_func()
# instead of this. (this wont work)
modname.some_func()
```
This provides an easy way to import all the items from a module into the current namespace; however, this statement should be used sparingly.

###### dir()
The dir() built-in function returns a sorted list of strings containing the names defined by a module.
The list contains the names of all the modules, variables and functions that are defined in a module. 
```
import math
content = dir(math)

# example
math.pi
math.tan(10)

print content
```

###### locals() and globals()
The **globals()** and **locals()** functions can be used to return the names in the global and local namespaces depending on the location from where they are called.
If **locals()** is called from within a function, it will return all the names that can be accessed locally from that function.
If **globals()** is called from within a function, it will return all the names that can be accessed globally from that function.
The return type of both these functions is dictionary. Therefore, names can be extracted using the keys() function.

###### Locating Modules
When you import a module, the Python interpreter searches for the module in the following sequences −
* The current directory.
* If the module isn't found, Python then searches each directory in the shell variable PYTHONPATH.
* If all else fails, Python checks the default path. On UNIX, this default path is normally /usr/local/lib/python/.
  The module search path is stored in the system module sys as the **sys.path** variable. The sys.path variable contains the current directory, PYTHONPATH, and the installation-dependent default.

###### Pip Packages
Pip is a package manager. It installs packages and not modules. Packages are collectkon of modules that can be used. Pip package path are already included in sys.path.
```
pip when used with virtualenv will generally install packages in the path <virtualenv_name>/lib/<python_ver>/site-packages.
```
For example the django package is installed in
```
venv_test/lib/python2.7/site-packages/django
```

Namespacing in a folder

# =============================================
# Python Packages
*A Python module is simply a Python source file, which can expose classes, functions and global variables.s
When imported from another Python source file, the file name is treated as a namespace.
A Python package is simply a directory of Python module(s).*

Python provides a very straightforward packaging system, which is simply an extension of the module mechanism to a directory.

Any directory with an ***init*****.py** file is considered a Python package. The different modules in the package are imported in a similar manner as plain modules, but with a special behavior for the ***init***.py file, which is used to gather all package-wide definitions.

A file modu.py in the directory pack/ is imported with the statement import pack.modu. This statement will look for an ***init***.py file in pack, execute all of its top-level statements. Then it will look for a file named pack/modu.py and execute all of its top-level statements. After these operations, any variable, function, or class defined in modu.py is available in the pack.modu namespace.

###### Code in init.py
A commonly seen issue is to add too much code to ***init***.py files. When the project complexity grows, there may be sub-packages and sub-sub-packages in a deep directory structure. In this case, importing a single item from a sub-sub-package will require executing all ***init***.py files met while traversing the tree.

Leaving an ***init***.py file empty is considered normal and even a good practice, if the package’s modules and sub-packages do not need to share any code.

###### importing packages
Lastly, a convenient syntax is available for importing deeply nested packages: import very.deep.module as mod. This allows you to use mod in place of the verbose repetition of very.deep.module.
```
mypackage/__init__.py <-- this is what tells Python to treat this directory as a package
mypackage/mymodule.py
```
So then you would do:
```
import mypackage.mymodule
from mypackage.mymodule import myclass
```

###### Naming convention
**modules (filenames)** should have short, all-lowercase names, and they can contain underscores;
**packages (directories)** should have short, all-lowercase names, preferably without underscores;
**classes** should use the CapWords convention.

###### Another Example
Consider a file Pots.py available in Phone directory. This file has following line of source code −
```
def Pots():
   print "I'm Pots Phone"
```
Similar way, we have another two files having different functions with the same name as above −

Phone/Isdn.py file having function Isdn()
Phone/G3.py file having function G3()

Now, create one more file ***init***.py in Phone directory −
Phone/***init***.py
To make all of your functions available when you've imported Phone, you need to put explicit import statements in ***init***.py as follows −
```
from Pots import Pots
from Isdn import Isdn
from G3 import G3
```
After you add these lines to ***init***.py, you have all of these classes available when you import the Phone package.

```
# Now import your Phone Package.
import Phone
Phone.Pots()
Phone.Isdn()
Phone.G3()
```

###### Advanced
Every module has a name and statements in a module can find out the name of its module. This is especially handy in one particular situation - As mentioned previously, when a module is imported for the first time, the main block in that module is run. What if we want to run the block only if the program was used by itself and not when it was imported from another module? This can be achieved using the ***name*** attribute of the module.
```
if __name__ == '__main__':
	print 'This program is being run by itself'
else:
	print 'I am being imported from another module'
```
Every Python module has it's ***name*** defined and if this is '***main***', it implies that the module is being run standalone by the user and we can do corresponding appropriate actions.

If a module is imported does the code in that run?
Yes it runs
And the variables/functions that are defined in that module can be used like so
someModule.somevariable
someModule.someFunction()


Exiting a python program
sys.exit() raises a SystemExit exception in the current thread. 
Good way to exit
Exit from Python. This is implemented by raising the  SystemExit exception, so cleanup actions specified by finally clauses of try statements are honored, and it is possible to intercept the exit attempt at an outer level.
sys documentation - https://docs.python.org/2/library/sys.html

sys.exit("some error message")

Since exit() ultimately “only” raises an exception, it will only exit the process when called from the main thread, and the exception is not intercepted.

The optional argument arg can be an integer giving the exit status (**defaulting to zero**), **or another type of object**. If it is an integer, zero is considered “successful termination” **and any nonzero value is considered “abnormal termination” by shells and the like**.


# =============================================
# Error Handling
Python provides two very important features to handle any unexpected error in your Python programs and to add debugging capabilities in them −
* Exception Handling: This would be covered in this tutorial. Here is a list standard Exceptions available in Python: Standard Exceptions.
* Assertions: This would be covered in Assertions in Python

Exception - Base class of all exceptions

###### Try..Catch
```
try:
   You do your operations here;
   ......................
except ExceptionI:
   If there is ExceptionI, then execute this block.
except ExceptionII:
   If there is ExceptionII, then execute this block.
   ......................
else:
   If there is no exception then execute this block. 
```

Nested try catch
https://docs.python.org/3/tutorial/errors.html

# =============================================
# Python OOP
In python do not use getter/setter methods. Instead just access the attribute itself, or, if you need code to be run every time the attribute is accessed or set, use properties.
```
class C:
    def __init__(self):
        self._x = None

    def getx(self):
        return self._x

    def setx(self, value):
        self._x = value

    def delx(self):
        del self._x

    x = property(getx, setx, delx, "I'm the 'x' property.")
```
    
###### Terms
* **Class**: A user-defined prototype for an object that defines a set of attributes that characterize any object of the class. The attributes are data members (class variables and instance variables) and methods, accessed via dot notation.
* **Class variable**: A variable that is shared by all instances of a class. Class variables are defined within a class but outside any of the class's methods. Class variables are not used as frequently as instance variables are.
* **Data member**: A class variable or instance variable that holds data associated with a class and its objects.
* **Function overloading**: The assignment of more than one behavior to a particular function. The operation performed varies by the types of objects or arguments involved.
* **Instance variable**: A variable that is defined inside a method and belongs only to the current instance of a class.
* **Inheritance**: The transfer of the characteristics of a class to other classes that are derived from it.
* **Instance**: An individual object of a certain class. An object obj that belongs to a class Circle, for example, is an instance of the class Circle.
* **Instantiation**: The creation of an instance of a class.
* Method : A special kind of function that is defined in a class definition.
* **Object**: A unique instance of a data structure that's defined by its class. An object comprises both data members (class variables and instance variables) and methods.
* **Operator overloading**: The assignment of more than one function to a particular operator.

###### init method
The first method ***init***() is a special method, which is called class constructor or initialization method that Python calls when you create a new instance of this class.

###### Adding self as argument
You declare other class methods like normal functions with the exception that the first argument to each method is self. Python adds the self argument to the list for you; you do not need to include it when you call the methods.

###### Class Objects
Class Objects are of 2 types:
1. Attribute references
2. Instantiation

```
class MyClass:
    """A simple example class"""
    i = 12345

    def f(self):
        return 'hello world'
```
**Class Attribute References** uses the standard syntax for attribute reference (obj.something)
```
MyClass.i
MyClass.f
```
**Class instantiation** uses function notation. Just pretend that the class object is a parameterless function that returns a new instance of the class. 
```
x = MyClass()
```

**Order Matters**
```
class Hello:
	def __init__(self):
		return
	
	def some():
		return ""
	
	def some(self):
		return "some"
	
	def some(self, some=""):
		return

	some = ""

# some is the variable defined at last	
Hello.some
""

class Hello:
	some = ""
	def __init__(self):
		return
	
	def some():
		return ""
	
	def some(self):
		return "some"
	
	def some(self, some=""):
		return

# Here the instance method is the last. It is unbound because it is being called on class 
# rather than its instance
Hello.some
<unbound method Hello.some>
```


###### Class and instance variables
Here empCount is a class variable.
```
class Employee:
   'Common base class for all employees'
   empCount = 0

   def __init__(self, name, salary):
      self.name = name
      self.salary = salary
      Employee.empCount += 1
   
   def displayCount(self):
     print "Total Employee %d" % Employee.empCount

   def displayEmployee(self):
      print "Name : ", self.name,  ", Salary: ", self.salary
```

```
class Employee:
   'Common base class for all employees'
   empCount = 0

   def __init__(self, name, salary):
      self.name = name
      self.salary = salary
      Employee.empCount += 1
   
   def displayCount(self):
     print "Total Employee %d" % Employee.empCount

   def displayEmployee(self):
      print "Name : ", self.name,  ", Salary: ", self.salary

"This would create first object of Employee class"
emp1 = Employee("Zara", 2000)
"This would create second object of Employee class"
emp2 = Employee("Manni", 5000)
emp1.displayEmployee()
emp2.displayEmployee()
print "Total Employee %d" % Employee.empCount
```

Generally speaking, instance variables are for data unique to each instance and class variables are for attributes and methods shared by all instances of the class:

```
class Dog:

    kind = 'canine'         # class variable shared by all instances

    def __init__(self, name):
        self.name = name    # instance variable unique to each instance

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.kind                  # shared by all dogs
'canine'
>>> e.kind                  # shared by all dogs
'canine'
>>> d.name                  # unique to d
'Fido'
>>> e.name                  # unique to e
'Buddy'
```
As discussed in A Word About Names and Objects, shared data can have possibly surprising effects with involving mutable objects such as lists and dictionaries. For example, the tricks list in the following code should not be used as a class variable because just a single list would be shared by all Dog instances:
```
class Dog:

    tricks = []             # mistaken use of a class variable

    def __init__(self, name):
        self.name = name

    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks                # unexpectedly shared by all dogs
['roll over', 'play dead']
```
Correct design of the class should use an instance variable instead:

```
class Dog:

    def __init__(self, name):
        self.name = name
        self.tricks = []    # creates a new empty list for each dog

    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks
['roll over']
>>> e.tricks
['play dead']
```

###### Built-in class attributes/variables
Every Python class keeps following built-in attributes and they can be accessed using dot operator like any other attribute −
* ***dict***: Dictionary containing the class's namespace.
* ***doc***: Class documentation string or none, if undefined.
* ***name***: Class name.
* ***module***: Module name in which the class is defined. This attribute is "***main***" in interactive mode.
* ***bases***: A possibly empty tuple containing the base classes, in the order of their occurrence in the base class list.

```
Employee.__doc__: Common base class for all employees
Employee.__name__: Employee
Employee.__module__: __main__
Employee.__bases__: ()
Employee.__dict__: {'__module__': '__main__', 'displayCount':
<function displayCount at 0xb7c84994>, 'empCount': 2, 
'displayEmployee': <function displayEmployee at 0xb7c8441c>, 
'__doc__': 'Common base class for all employees', 
'__init__': <function __init__ at 0xb7c846bc>}
```

###### Static Methods
Static methods are a special case of methods. Sometimes, you'll write code that belongs to a class, but that doesn't use the object itself at all. 
Here **staticmethod** is a decorator.
For example:
```
class Pizza(object):
    @staticmethod
    def mix_ingredients(x, y):
        return x + y
 
    def cook(self):
        return self.mix_ingredients(self.cheese, self.vegetables)
```
Static methods improves readability of the code using descriptor
It allows us to override the mix_ingredients method in a subclass. If we used a function mix_ingredients defined at the top-level of our module, a class inheriting from Pizza wouldn't be able to change the way we mix ingredients for our pizza without overriding cook itself.
    
###### Class Methods
Class methods are methods that are not bound to an object, but to… a class!
```
>>> class Pizza(object):
...     radius = 42
...     @classmethod
...     def get_radius(cls):
...         return cls.radius
... 
>>> 
>>> Pizza.get_radius
<bound method type.get_radius of <class '__main__.Pizza'>>
>>> Pizza().get_radius
<bound method type.get_radius of <class '__main__.Pizza'>>
>>> Pizza.get_radius is Pizza().get_radius
True
>>> Pizza.get_radius()
42
>>> Pizza().get_radius()
42
```

###### Functions
All the double underscore methods are defined in the classes.
```
def some():
	return "some"

# Str class is of class "type"
str.__class__
<type 'type'>

# Creating a new instance of str
str()
""

# class of string
"".__class__
<type 'str'>

# class of class
"".__class__.__class__
<type 'type'>

# Class of String
abc = " "
abc.__class__
<type 'str'>

# this returns function object
some
<function some at 0x10053c230>

# Class to which functions belong
some.__class__
<type 'function'>

# Functions accepted
dir(some)
['__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__doc__', '__format__', '__get__', '__getattribute__', '__globals__', '__hash__', '__init__', '__module__', '__name__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'func_closure', 'func_code', 'func_defaults', 'func_dict', 'func_doc', 'func_globals', 'func_name']
```
* ***self*** is the class instance object
* ***func*** is the function object
* ***doc*** is the method’s documentation 
* ***name*** is the method name

###### Summary
* **_single_leading_underscore:** weak "internal use" indicator.  E.g. "from M import *" does not import objects whose name starts with an underscore.
* **single_trailing_underscore_:** used by convention to avoid conflicts with Python keyword, e.g. Tkinter.Toplevel(master, class_='ClassName')
* **__double_leading_underscore:** when naming a class attribute, invokes name mangling (inside class FooBar, __boo becomes _FooBar__boo; see below).
* ***double_leading_and_trailing_underscore*****:** "magic" objects or attributes that live in user-controlled namespaces.  E.g. ***init***, ***import*** or ***file***.  Never invent such names; only use them as documented.

###### Private Instance Variables (_x Single Underscore)
Use _one_underline to **mark you methods as not part of the API**.
Python **doesn't have real private methods**, so one underline in the beginning of a method or attribute means you shouldn't access this method, because it's not part of the API. 
“Private” instance variables that cannot be accessed except from inside an object don’t exist in Python. However, there is a convention that is followed by most Python code: a name prefixed with an underscore 
It's very common when using properties:
```
def _get_errors(self):
    if self._errors is None:
        self.full_clean()
    return self._errors
```

###### Base Methods/Variables or Magic Objects (***x*** Double underscore methods)
When you see a method like ***this***, the rule is simple: don't call it. Why? Because it means it's a method python calls, not you.
**These generraly are used by python to perform specific operations.**
You can always override your parent class methods. One reason for overriding parent's methods is because you may want special or different functionality in your subclass.
* ***init*** ( self [,args...] ): Constructor (with any optional arguments)
  * Sample Call : obj = className(args)
* ***del***( self ): Destructor, deletes an object
  * Sample Call : del obj
* ***repr***( self ): Evaluatable string representation
  * Sample Call : repr(obj)
* ***str***( self ): Printable string representation
  * Sample Call : str(obj)
* ***cmp*** ( self, x ): Object comparison
  * Sample Call : cmp(obj, x)
    **Overloading base methods**
```
class Vector:
   def __init__(self, a, b):
      self.a = a
      self.b = b

   def __str__(self):
      return 'Vector (%d, %d)' % (self.a, self.b)
   
   def __add__(self,other):
      return Vector(self.a + other.a, self.b + other.b)

v1 = Vector(2,10)
v2 = Vector(5,-2)
print v1 + v2
# When the above code is executed, it produces the following result −
Vector(7,8)
```

###### Non Overridable Variables/Methods (__x double underscore variables)
An object's attributes may or may not be visible outside the class definition. 
So when you create a method starting with __ you're saying that you don't want anybody to override it, it will be accessible just from inside the own class. 
**name mangling**
It should not be used to mark a method as private, the goal here is to avoid your method to be overridden by a subclass. It is not private as it can still be accessed by using _ClassName__x
You need to name attributes with a double underscore prefix, and those attributes then are not be directly visible to outsiders.
```
class JustCounter:
   __secretCount = 0
  
   def count(self):
      self.__secretCount += 1
      print self.__secretCount

counter = JustCounter()
counter.count()
counter.count()
print counter.__secretCount
```
When the above code is executed, it produces the following result −
```
1
2
Traceback (most recent call last):
  File "test.py", line 12, in <module>
    print counter.__secretCount
AttributeError: JustCounter instance has no attribute '__secretCount'
```
Python protects those members by internally changing the name to include the class name. You can access such attributes as object._className__attrName. If you would replace your last line as following, then it works for you −
```
print counter._JustCounter__secretCount
```

###### Constructor and Destructor Magic Methods of a class
We can customize these methods. For example in the below example, ***del***() destructor prints the class name of an instance that is about to be destroyed
```
class Point:
   def __init( self, x=0, y=0):
      self.x = x
      self.y = y
   def __del__(self):
      class_name = self.__class__.__name__
      print class_name, "destroyed"
```
You normally will not notice when the garbage collector destroys an orphaned instance and reclaims its space. But a class can implement the special method ***del***(), called a destructor, that is invoked when the instance is about to be destroyed. This method might be used to clean up any non memory resources used by an instance.

###### Attribute Access


###### Getter and Setter methods
One underline in beginning
Python doesn't have real private methods, so one underline in the beginning of a method or attribute means you shouldn't access this method, because it's not part of the API. It's very common when using properties:

###### Decorators

###### Properties
These are class_object.width
Used when we want special variables that are computed on the fly
```
    @property
    def width(self):
        return self.bbox[2] - self.bbox[0]

    @property
    def height(self):
        return self.bbox[3] - self.bbox[1]

    @property
    def layout(self):
        if hasattr(self, "_layout"): return self._layout
        self._layout = self.pdf.process_page(self.page_obj)
        return self._layout

    @property
    def objects(self):
        if hasattr(self, "_objects"): return self._objects
        self._objects = self.parse_objects()
        return self._objects
```
        

Inheritance
Multiple Inheritance
# =============================================
# Python Encoding
Unicode
- Unicode contains a far larger set of strings
- Converting it into ASCII which is the one interpreted by most programming languages is a must.
> > > a = u'hello'
> > > a
> > > u'hello'
> > > str(a)
> > > 'hello'

A = u'\u2014'
str(A)
*** UnicodeEncodeError: 'ascii' codec can't encode character u'\u2014' in position 0: ordinal not in range(128)

u'hello world'.encode('utf-8')
'hello world'

UTF-8 is an encoding scheme. Other encoding schemes include UTF-16 (with two different byte orders) and UTF-32. (For some confusion, a UTF-16 scheme is called “Unicode” in Microsoft software.)

ASCII defines codepoint values (they were not called codepoints until Unicode came along) 0-127, but it does not define their encodings. All language encodings use the same values as ASCII for their first 128 characters. UTF-8, ISO encodings, Latin encodings, etc are all 8bit encodings that support ASCII values. UTF-16 and UTF-32 are 16/32bit encodings that also support ASCII values. Codepoint values and their encoded Codeunit values within a given encoding are two separate things.

> > > u'hello world'.encode('utf-8')
> > > 'hello world'

> > > > u'hellò world'.encode('utf-8')
> > > > 'hell\xc3\xb2 world'

> > > u'hellò world'.encode('latin-1')
> > > 'hell\xf2 world'

text = u'\u2014'
text.encode("utf-8")
'\xe2\x80\x94'

http://stackoverflow.com/questions/147741/character-reading-from-file-in-python

When printing it prints the unicode
print text
—

If taking example of the dash above (-), its a special unicode dash. There can be a mapping between that and the readable version of the dash. 

If it is not there then we need tp 


# =============================================
# Python Tools/Utilities
The standard library comes with a number of modules that can be used both as modules and as command-line utilities.

# =============================================
# Python Tools/Utilities - pdb
The pdb module is the standard Python debugger. It is based on the bdb debugger framework.

# =============================================
# Python Tools/Utilities - profile module
cProfile.py sum.py

# =============================================
# Python Tools/Utilities - tabnanny module
The tabnanny module checks Python source files for ambiguous indentation. If a file mixes tabs and spaces in a way that throws off indentation, no matter what tab size you're using, the nanny complains
tabnanny.py -v sum.py

# =============================================
# Python Tools/Utilities - Sys module
This is also a standard that is already avaialble with python. So it doesnt need to be mentioned in pip.

import sys
Get the path of python
print(sys.path)

# =============================================
# Python Tools/Utilities - Os module
import os
os.environ['PYTHONPATH'].split(os.pathsep)


###### Itertools
Remove elements from list using selectors. This method means you only generate the selectors once (and itertools.compress() should be nice and fast).

a=["a", "b", "c", "d", "e"]
b=[1, 2, None, 3, 4]
selectors = [x is not None for x in b]
list(itertools.compress(a, selectors))
['a', 'b', 'd', 'e']
list(itertools.compress(b, selectors))
[1, 2, 3, 4]


dict([(2,1),(3,1),(2,2)])
{2: 2, 3: 1}

This is for list
[ [ (val, i) for val in value_cluster ] for i, value_cluster in enumerate(clusters) ]

This is for tuples
( something for some in some )

###### chain()
p, q, ...	p0, p1, ... plast, q0, q1, ...	
chain('ABC', 'DEF') --> A B C D E F

###### compress()
data, selectors	(d[0] if s[0]), (d[1] if s[1]), ...
compress('ABCDEF', [1,0,1,0,1,1]) --> A C E F

dropwhile()	pred, seq	seq[n], seq[n+1], starting when pred fails	dropwhile(lambda x: x<5, [1,4,6,4,1]) --> 6 4 1

###### groupby()
iterable[, keyfunc]	
sub-iterators grouped by value of keyfunc(v)	 
```
things = [("animal", "bear"), ("animal", "duck"), ("plant", "cactus"), ("vehicle", "speed boat"), ("vehicle", "school bus")]

for key, group in groupby(things, lambda x: x[0]):
    for thing in group:
        print "A %s is a %s." % (thing[1], key)
    print " "
```


Let’s say we wanted to ‘group’ together all the animals that are the same. Instead of looping through all the elements and keep temporary lists, let’s use itertools.groupby.
```
animals = ['cow', 'cow', 'bird', 'pony', 'pony', 'pony', 'fish', 'cow']
for key, group in itertools.groupby(animals):
    print key, group
We effectively get :
cow <itertools._grouper object at 0x7faa6266af50>
bird <itertools._grouper object at 0x7faa6266af90>
pony <itertools._grouper object at 0x7faa6266af50>
fish <itertools._grouper object at 0x7faa6266af90>
cow <itertools._grouper object at 0x7faa6266af50>
```

###### ifilter()
pred, seq	
elements of seq where pred(elem) is true
ifilter(lambda x: x%2, range(10)) --> 1 3 5 7 9

###### ifilterfalse()
pred, seq	
elements of seq where pred(elem) is false	
ifilterfalse(lambda x: x%2, range(10)) --> 0 2 4 6 8

###### islice()
seq, [start,] stop [, step]	elements from seq[start:stop:step]islice('ABCDEFG', 2, None) --> C D E F G

###### imap()
func, p, q, ...	func(p0, q0), func(p1, q1), ...	
imap(pow, (2,3,10), (5,2,3)) --> 32 9 1000

###### starmap()
func, seq, func(*seq[0]), func(*seq[1]), ...	
starmap(pow, [(2,5), (3,2), (10,3)]) --> 32 9 1000

###### tee()
it, n	it1, it2, ... itn splits one iterator into n	 
###### takewhile()
pred, seq	seq[0], seq[1], until pred fails	takewhile(lambda x: x<5, [1,4,6,4,1]) --> 1 4

izip()	
p, q, ...	(p[0], q[0]), (p[1], q[1]), ...	
izip('ABCD', 'xy') --> Ax By

izip_longest()	
p, q, ...	(p[0], q[0]), (p[1], q[1]), ...	
izip_longest('ABCD', 'xy', fillvalue='-') --> Ax By C- D-

Sort according to value
chars_sorted = sorted(chars, key=itemgetter("x0"))

###### attr_getter
attr_getter = itemgetter(attr)
b = {"a": "b"}
b.get("a")

###### Map
Map applies a function to all the items in an input_list. Here is the blueprint:
map(function_to_apply, list_of_inputs)

```
items = [1, 2, 3, 4, 5]
squared = []
for i in items:
    squared.append(i**2)

items = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, items))
```

Most of the times we use lambdas with map so I did the same. Instead of a list of inputs we can even have a list of functions!
```
def multiply(x):
    return (x*x)
def add(x):
    return (x+x)

funcs = [multiply, add]
for i in range(5):
    value = list(map(lambda x: x(i), funcs))
    print(value)

# x(i) means pass i to the variable
# Output:
# [0, 0]
# [1, 2]
# [4, 4]
# [9, 6]
# [16, 8]
```


###### Filter
As the name suggests, filter creates a list of elements for which a function returns true. Here is a short and concise example:

```
number_list = range(-5, 5)
less_than_zero = list(filter(lambda x: x < 0, number_list))
print(less_than_zero)
# Output: [-5, -4, -3, -2, -1]
```

###### Reduce
Reduce is a really useful function for performing some computation on a list and returning the result. For example, if you wanted to compute the product of a list of integers.

from functools import reduce
product = reduce((lambda x, y: x * y), [1, 2, 3, 4])

# Output: 24

###### sorted
A simple ascending sort is very easy -- just call the sorted() function. It returns a new sorted list:
```
sorted([5, 2, 3, 1, 4])
[1, 2, 3, 4, 5]
```

You can also use the list.sort() method of a list. It modifies the list in-place (and returns None to avoid confusion). Usually it's less convenient than sorted() - but if you don't need the original list, it's slightly more efficient.
```
a = [5, 2, 3, 1, 4]
a.sort()
a
[1, 2, 3, 4, 5]
```

Another difference is that the list.sort() method is only defined for lists. In contrast, the sorted() function accepts any iterable.
```
sorted({1: 'D', 2: 'B', 3: 'B', 4: 'E', 5: 'A'})
[1, 2, 3, 4, 5]
```

list.sort() and sorted() added a key parameter to specify a function to be called on each list element prior to making comparisons.

For example, here's a case-insensitive string comparison:
```
sorted("This is a test string from Andrew".split(), key=str.lower)
['a', 'Andrew', 'from', 'is', 'string', 'test', 'This']
```

The value of the key parameter should be a function that takes a single argument and returns a key to use for sorting purposes. This technique is fast because the key function is called exactly once for each input record.

A common pattern is to sort complex objects using some of the object's indices as a key. For example:
```
student_tuples = [
        ('john', 'A', 15),
        ('jane', 'B', 12),
        ('dave', 'B', 10),
]
sorted(student_tuples, key=lambda student: student[2])   # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```
The same technique works for objects with named attributes. For example:

```
class Student:
        def __init__(self, name, grade, age):
                self.name = name
                self.grade = grade
                self.age = age
        def __repr__(self):
                return repr((self.name, self.grade, self.age))
        def weighted_grade(self):
                return 'CBA'.index(self.grade) / float(self.age)

student_objects = [
        Student('john', 'A', 15),
        Student('jane', 'B', 12),
        Student('dave', 'B', 10),
]
sorted(student_objects, key=lambda student: student.age)   # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```
###### enumerate
A new built-in function, enumerate(), will make certain loops a bit clearer. enumerate(thing), where thing is either an iterator or a sequence, returns a iterator that will return (0, thing[0]), (1, thing[1]), (2, thing[2]), and so forth.

A common idiom to change every element of a list looks like this:
```
for i in range(len(L)):
    item = L[i]
    # ... compute some result based on item ...
    L[i] = result

#This can be rewritten using enumerate() as:

for i, item in enumerate(L):
    # ... compute some result based on item ...
    L[i] = result
```


###### isinstance
Specifying multiple instances
isinstance(some_var, (str, int)):

# Pandas
In [1]: import pandas as pd
In [2]: import numpy as np
In [3]: import matplotlib.pyplot as plt

"a".***class***

###### repr
The result of repr('foo') is the string 'foo'. In your Python shell, the result of the expression is expressed as a representation too, so you're essentially seeing repr(repr('foo')).

###### eval
eval calculates the result of an expression. The result is always a value (such as a number, a string, or an object). Multiple variables can refer to the same value, as in: