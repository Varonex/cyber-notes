# Bash scripting

When you want to automate commands in bash, you can create a script.
The script must have the execution permission.

## Shebang

The shebang is here to precise the executable to run the script with. In Bash, it is `/bin/bash`. Contrary to comments that you write with `#`, the shebang is at the very top and is prefixed with `#!` :

```bash
#!/bin/bash
# Shebang to precise the shell.
```

## Variables & Functions

### Variables

You can create variables with different datatypes. Those variables can be accessed via `$` and `${}`.
The different syntax work :

```bash
# Definition.
myInt=1
myString="goodnight"
mySlice=${myString: 0:3} # Display "goo". Fabricates a slice of the string thanks to : start:length. Needs a space between the first ":" and the start ! Can start with a negative integer to go backwards (: -1:3).

# String concatenation.
myString+=" gday"

# Assignation.
myAssignation=$PATH # If PATH is null, stores null. Can be unsafe depending on the context.
myInterpolation="$PATH" # Safe variable assignation, defaults with an empty string in case PATH is empty.
myLitteral='$PATH' # Simple quotes don't allow for interpolation.

# Defaulting.
var=""
echo "${var:-default}" # Displays "default" if var is empty.
echo "${var:=default}" # Assigns "default" if var is vide.
echo "${var:-$PATH}" # Variables can be used inside the defaulting clause.

# Deletion.
unset "var" # Need to use the variable name, not $var.
```

### Arrays

You can also define arrays to store elements as is. The syntax goes as follow :

```bash
# Definition.
myArr=("a" "b" "c") # Delimiters are '()' and separators are just whitespaces.

# Assignation.
myArr[3]="d"
myArr+=("e" "f") # Append the right-side arry into the left-side one.

# Indexing.
elm0=${myArr[0]} # Element at index 0.
elm0=$myArr # Also element 0.
allTheArr=${myArr[@]} # Polls all the elements in a single, iterable string.
slice="${myArray[@]: 1:2}" # Slicing, from 1 to 1+2 (excluded).

# Size.
size=${#myArr[@]} # Array size.

# Deletion.
unset "myArr[0]" # Deletes the first element without shifting.
unset "myArr" # Deletes the whole array.
```

### Dictionaries

Key-value dictionaries can be created :

```bash
# Definition.
declare -A myDict

# Assignation.
myDict["goodnight"]="a"

# Indexing.
myVal=${myDict["goodnight"]}
keys=${!myDict[@]} # Fetches all the keys in an iterable string.
values=${myDict[@]} # Fetches all the values in an iterable string.

# Size.
size=${#myDict[@]}

# Deletion.
unset "myDict['bonsoir']" # Deletes a single key.
unset "myDict" # Deletes the whole dictionary.
```

### Arguments

Arguments are variables that are referenced while invoking the script. Their identifiers are literal numbers, corresponding to the index of the argument in the invocation string :

```bash
# Example command : "./script.sh a 1 b 2 c 3"

# Indexing.
echo "$0" # $0 stores the invokation path. "./script.sh" in the example.
arg1="$1" # Assigns the first argument. "a" in the example.
arg10="${10}" # {} for complex identifiers. Undefined in the example.
allArgs="$*" # Takes all the args to wrap them in a single string with $IFS as separator. In a loop, does NOT allow iteration (iterates over the huge string only once).
allArgsSep="$@" # Same as $*, but in a loop, iterates on all the separated elements.

# Size.
nbArg="$#" # Amount of arguments ($1 to $n). 6 in the example ($0 does not count).

# Shift the parameters from $1 to $n ($1 est overriden by $2 etc... $0 remains intact).
shift
```

### Temporary assignations

If a command is written at the end of a series of assignations, then those variables will get reset after the command has ended :

```bash
IFS=" " read -r l

k=1 sh -c 'echo $k' # 1. Here we need to NOT interpolate $k, and use sh -c to clone the environment and then output. If we used echo directly, the variable wouldn't have gotten changed in time.
echo "$k" # Empty.

a=1 b=2 c=3 sh -c 'echo $a $b $c' # We can chain a variety of variables.
```

### Functions

Functions act like commands. Arguments work exactly the same as the script's arguments. You can return values with STDOUT and also return a code (0-255) :

```bash
myFunc() {
	a=1 # Global assignation.
	local b=2 # Local assignation. Like in lua !
	
	echo "$1" # Argument $1 is local to the function here. Displays the first function-wide argument.
	# Everything else works as in globalscope.
	
	# Returned values should be done via echo.
	return 0 # Return codes (0-255) are returned via the return keyword.
}
```

You can call a function like a command :

```bash
# Call.
myFunc 4 # Displays 4.

# Assignation (subfunction).
myResult=$(myFunc 5) # Stores 5.
myResult2=`myFunc 5` # Same as above, old syntax.

# Error code.
lastCode="$?" # Fetches the last return code. Returns 0 as the last function is myFunc.
```

### Special variables


- `IFS` : Internal field separator.
- `$$` : Current PID.
- `$!` : PID of the last asynchronous process ran with &.
- `$-` : Current active shell options.
- `$_` : Last argument of the last command.

## Control flow

### If conditions

IF control flow. Supports `if/elif/else`.

```bash
if <cond>; then
	echo "Always runs, 1 is true."

elif <cond>; then
	echo "Does not run."
else
	echo "Doesn't run either."
fi
```

#### Legacy comparison

There is the legacy comparison syntax :

```bash
# Old comparison syntax.
if [ -z "$0" -a -f "$1" ]; then
	...
fi

# Supports flags to do or, and, >=, <=, ...
# Very primitive.
[ -f -z -a -o ! -lt -gt -eq -ne ... ]
```

#### String-based comparison

There is string-based comparison. More advanced, and supports regex. Supports all the same operators as the legacy comparison.

!!! warning "Comparisons"
	Comparisons do not use the numeric order here ! It uses the lexicographic order, meaning `2 > 10` is true.

```bash
if [[ "$0" == "" && -f "$1" || -f *.txt ]]; then
	...
fi

# Supports same as [] but also string-oriented syntax.
[[ -f ... && || == != >= <= > < * =~ ]]
```

#### Arithmetic comparison

The arithmetic comparison uses the C-style syntax to do things. You can do litterally everything asq in C.

```bash
if ((i == 21)); then
	...
fi

# Supports every operator in C. Variables don't use $ as a prefix.
((i ++ -- / * + - >= <= > < != == && || & | > < \<< >>))
```

### Case

Cases are like ifs and uses the same logic you can see in switches. You can make keys composed of different values :

```bash
val="hi"
case "$val" in
	a)
		# code
		;;
	
	b|c)
		# code
		;;
	
	*)
		echo "Aucune valeur, retour au défaut"
		;;
esac
```

### While loops

While loop works as expected. It stops whenever the condition is met :

```bash
# Outputs 0 1 2.
i=0
while ((i < 3)); do
	echo "$i"
	((i++))
done

# Line-by-line read. Uses one-time command assignation to alter the IFS variable, which is the special variable used to define field separator.
while IFS=" " read -r line; do
	echo "Ligne : $line"
	
	if [[ "$line" == "" ]]; then
		continue
	fi
	
	# Or simply...
	[[ "$line" == "" ]] && continue
done

# Feed the content in the while directly.
while IFS=" " read -r line; do
	echo "Ligne : $line"
done <<< "Je peux feed ici des choses."
```

### Until loop

This loop is the opposite of `while`. While loops until a condition is met, whereas until loops until a condition is no longer met. All the techniques available with while are also available with until.

```bash
i=0
until ((i >= 5)); do
	echo "$i"
	((i++))
done
```

### For loop

For loops can be used to iterate a certain amount of time, or over values :

```bash
for i in {1..10}; do
	echo "$i" # from 1 to 10.
done

# C-style.
for ((i=0; i<5; i++)); do
	echo "$i" # From 0 to 4.
done

# Over an array.
arr=("a" "b" "c")

for val in ${arr[@]}; do
	echo "$val"
done

# Over a dictionary.
declare -A dict
dict["a"]=1

for key in ${!dict[@]}; do
	echo "myDict[$key] = ${myDict[$key]}"
done

# Lists elements in the current working directory.
for item in *; do
	echo "$item" # file/directory.
done

# Over a subcommand's output.
for item in $(ls); do
	echo "$item"
done
```

## Blocks & Scopes

Code blocks have various jobs. Whether it is to lanch a sub-environment, or just contain code somewhere, they have their own utility.

### Standard block

Block of code like in any other language. Captures and overwrites upper-scoped variables. However, all the declared variables are NOT imprisoned in the scope and freed later :

```bash
x=3
cd /
{
	cd /home
	x=1
	z=3
	
	pwd # Displays /home.
	echo "$x $z" # Displays "1 3".
}
pwd # Displays /home.
echo "$x $z" # Displays "1 3". Z exists.
```

### Arithmetic block

Same as the upper type of block, but it also uses the arithmetic syntax of the arithmetic comparison syntax :

```bash
res=1
((
	res = res & x
	res = res + 1
	res++, res-- # Oneline uses "," as instruction separator.
	
	res = (x > 0) ? res : x # Ternary operator.
))
echo "$res"
```

### Subshells

Subshells are isolated environments. They do capture the upper scope, but it is ran in a separated process so there is no variable overwrite. The `$()` and \`\` syntax are typical subshells that redirect STDOUT to the upper shell. Here is how to use them :

```bash
x=1
cd /
(
	# Subshell context.
	cd /home
	x=2
	y=1
	
	pwd # Displays /home.
	echo "$x $y" # Displays "2 1".
)
pwd # Displays /.
echo "$x $y" # Displays "1  ". Y is empty.
```
