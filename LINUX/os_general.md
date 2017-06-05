###### /etc/profile
It says that the /etc/profile file sets the environment variables at startup of the Bash shell. The /etc/profile.d directory contains other scripts that contain application-specific startup files, which are also executed at startup time by the shell.
Why are these files not a part of /etc/profile if they are also critical to Bash startup ?
If you mean, "Why are they not just combined into one giant script?", the answer is:

Because that would be a maintenance nightmare for the people who are responsible for the scripts.
Because having the scripts loaded as independent modules makes the whole system more dynamically adjustable -- individual scripts can be added and removed without affecting the others. Etc.
Because they are loaded via /etc/profile which makes them a part of the bash "profile" in the same way anyway.
http://unix.stackexchange.com/questions/64258/what-do-the-scripts-in-etc-profile-d-do

Conditions
https://linuxacademy.com/blog/linux/conditions-in-bash-scripting-if-statements/

###### Use of $
Variables can be created with or wihout $.
$var and var is same. However lots of places $ has to be put.

###### Double parentheses (( ))
**Important:** Variables used inside double parentheses do not need to be prefixed with '$'. 
Double parentheses (( )) are used for arithmetic:
```
((var++))
((var = 3))
for ((i = 0; i < VAL; i++))
echo $((var + 2))
```
To print you have to add $ otherwise it doesnt work
```
var=3; ((var++)); echo ((var))
-bash: syntax error near unexpected token `('
```
To print you have to add $.
```
var=3; ((var++)); echo $((var))
4

var=3; ((var++)); echo $var
4

var=3; ((var++)); echo $var++
4++

var=3; ((var++)); echo $((var+1))
5

var=3; ((var++)); echo $((var++))
4

var=3; ((var++)); echo $((++var))
5
```

###### Square brackets []
**Important:** Variables need to prefixed with $. Square brackets [] are used for test construct
```
$ VAR=2
$ if [ $VAR -eq 2 ]
> then
> echo 'yes'
> fi
> yes
```

###### Double square brackets
**Important:** Variables need to prefixed with $. Double square brackets [[]]offer extended functionality to single square brackets, useful for the regular expression operator =~
```
$ VAR='some string'
$ if [[ $VAR =~ [a-z] ]]; then
> echo 'is alphabetic'
> fi
> is alphabetic
```

###### Curly braces
**Important:** $ must not be used inside. $ must come before.****
Curly braces are also used for parameter expansion
Usage of the $ like ${HOME} gives the value of HOME. Usage of the $ like $(echo foo) means run whatever is inside the parentheses in a subshell and return that as the value. In my example, you would get foo since echo will write foo to standard out
```
$ var="abcdefg"; echo ${var%d*}
abc
```
Followng throws error
```
var="abcdefg"; echo $((var%d*))
-bash: var%d*: division by 0 (error token is "*")
```
In the following var++ does not work. And echo var prints var.
```
var=3; var++; echo var
> -bash: var++: command not found
> var
```
To make this correct
```
var=3; ((var++)); echo ${var}
> var
> var=3; ((var++)); echo var
```

The $ expression is evaulated and run
```
var=1
$var
-bash: 1: command not found
```

###### Common $ variables
$1, $2, $3, ... are the positional parameters.
"$@" is an array-like construct of all positional parameters, {$1, $2, $3 ...}.
$# number of arguments passed to current script
$* / $@ list of arguments passed to script as string / delimited list

```
#!/bin/bash
for ((i=0; i<=$#; i++)); do
  echo "parameter $i --> ${!i}"
done
$ ./myparams.sh "hello" "how are you" "i am fine"
parameter 0 --> myparams.sh
parameter 1 --> hello
parameter 2 --> how are you
parameter 3 --> i am fine
```

$- current options set for the shell.
```
echo $-
himBH
```

$$ pid of the current shell (not subshell).
```
echo $$
20389
```

$_ most recent parameter (or the abs path of the command to start the current shell immediately after startup).
```
echo $_
echo
```

$IFS is the (input) field separator.

$? is the most recent foreground pipeline exit status.
```
echo $?
0
```

$! is the PID of the most recent background command.
```
echo $!
20483
```

$0 is the name of the shell or shell script.
```
echo $0
-bash
```


###### Command Status
A 0 status indicated success.
A non zero status indicates error.

sdfas
-bash: sdfas: command not found
echo $?
127
pwd
/Users/saurabh/projects/pro.de/common/templates
echo $?
0

###### Return statement of a command
Every shell command returns false or true. Here the first cd command if failed will move on to the second or which fails and inturn exits.
Clubbing with Or
```
cd "<%= fetch(:deploy_to) %>" || (
  echo "! ERROR: not set up."
  echo "The path '<%= fetch(:deploy_to) %>' is not accessible on the server."
  echo "You may need to run 'mina setup' first."
  false
) || exit 15
```


Check if program exists
Avoid which. Not only is it an external process you're launching for doing very little (meaning builtins like hash, type or command are way cheaper), you can also rely on the builtins to actually do what you want, while the effects of external commands can easily vary from system to system.
So, don't use which. Instead use one of these:

$ command -v foo >/dev/null 2>&1 || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
$ type foo >/dev/null 2>&1 || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
$ hash foo 2>/dev/null || { echo >&2 "I require foo but it's not installed.  Aborting."; exit 1; }
(Minor side-note: some will suggest 2>&- is the same 2>/dev/null but shorter – this is untrue. 2>&- closes FD 2 which causes an error in the program when it tries to write to stderr, which is very different from successfully writing to it and discarding the output (and dangerous!))

Doesnt print anythinf
sudo sh -c echo some
> 
Now it prints
sudo sh -c 'echo some'
some



Quotes
in ruby every string gets converted to double quotes. And double quotes insinde are back slashed.
Wont Complete
"""

string = ''
""
string = '''
"'"
string = """
" /" "
Also single quoting does not insert variable in rails
'#{var}' will not work
Howver %{#{var}} works
str = %q[ruby 'on rails"] # Single quoting
str2 = %Q[quoting with #{str}] # will insert variable



saurabh@Amber:~ $ var=3; echo "$var"
3
saurabh@Amber:~ $ var=3; echo "var"
var
saurabh@Amber:~ $ var=3; echo '$var'
$var


echo "I am \"Finding\" difficult to write this to file" > file.txt echo
echo "I can \"write\" without double quotes" >> file.txt
The same holds true if you i.e. also want to write the \ itself, as it may cause side effects. So you have to use \\

Another option would be to use The `'' instead of quotes.

echo 'I am "Finding" difficult to write this to file' > file.txt echo
echo 'I can "write" without double quotes' >> file.txt
However in this case variable substition doesn't work, so if you want to use variables you have to put them outside.

echo "This is a test to write $PATH in my file" >> file.txt
echo 'This is a test to write '"$PATH"' in my file' >> file.txt


###### Yum
**Search Command**
If you do not know the name of the package, use the search or provides options. Alternatively, use wild cards or regular expressions with any yum search option to broaden the search critieria.

The search option checks the names, descriptions, summaries and listed package maintainers of all of the available packages to find those that match. For example, to search for all packages that relate to PalmPilots, type:

su -c 'yum search PalmPilot'
To search for all packages that include files called libneon, type:

su -c 'yum provides libneon'
To search for all packages that either provide a MTA (Mail Transport Agent) service, or include files with mta in their name:

su -c 'yum provides MTA'

**List Command**
To list all packages with names that begin with tsc, type:

su -c 'yum list tsc\*'

To search for a specific package by name, use the list function. To search for the package tsclient, use the command:

su -c 'yum list tsclient'
Enter the password for the root account when prompted.

To make your queries more precise, specify packages with a name that include other attributes, such as version or hardware architecture. To search for version 0.132 of the application, use the command:

su -c 'yum list tsclient-0.132'

' cant be escaped
echo 'abc'\''abc'
abc'abc


echo "abc"\""abc"
abc"abc