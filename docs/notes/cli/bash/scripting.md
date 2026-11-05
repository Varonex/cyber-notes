```bash
#!/bin/bash
# Shebang to precise the shell.

#
## VARIABLES
#
myInt=1
myString="goodnight"
mySlice=${myString: 0:3} # Display "goo". Fabricates a slice of the string thanks to :start:length. Needs a space between the first ":" and the start ! Can start with a negative integer to go backwards (: -1:3).
myString+=" gday" # Concatenation.
myInterpolation="$PATH"
myLitteral='$PATH' # Simple quotes don't allow for interpolation.

#
## ARRAYS
#
myArr=("a" "b" "c")

elm0=${myArr[0]} # Element at index 0.
elm0=$myArr # Also element 0.
allTheArr=${myArr[@]} # Polls all the elements in a single, iterable string.

size=${#myArr[@]} # Array size.
myArr+=("d") # Append the right-side arry into the left-side one.
slice="${myArray[@]: 1:2}" # Slicing, from 1 to 1+2 (excluded).

unset "myArr[0]" # Works by name. Deletes the first element without shifting.
unset "myArr" # Deletes the whole array.

#
## DICTIONARIES
#

declare -A myDict
myDict["goodnight"]="a"
myVal=${myDict["goodnight"]}
keys=${!myDict[@]}
values=${myDict[@]}
size=${#myDict[@]}

#
## ARGUMENTS
#
arg0=$0 # Unsafe (can be null depending on the variable. Here is is guaranteed to exist). $0 refers to the binary invocation.
arg1="$1" # Safe.
arg10="${10}" # {} for complex identifiers.
nbArg="$#" # Amount of arguments ($1 to $n).
allArgs="$*" # Takes all the args to wrap them in a single string with $IFS as separator. In a loop, does NOT allow iteration (iterates over the huge string only once).
allArgsSep="$@" # Same as $*, but in a loop, iterates on all the separated elements.

# Shift the parameters from $1 to $n ($1 est overriden by $2 etc... $0 remains intact).
shift

#
## VARIABLE EXPANSION
#
var=""
echo "${var:-default}" # Displays "default" if var is empty.
echo "${var:=default}" # Assigns "default" if var is vide.

#
## FUNCTIONS
#
myFunc() {
	a=1 # Global assignation.
	local b=2 # Local assignation. Like in lua !
	
	echo "$1" # Argument $1 is local to the function here. Displays the first function-wide argument.
	# Everything else works as in globalscope.
	
	# Returned values should be done via echo.
	return 0 # Return codes (0-255) are returned via the return keyword.
}

#
## FUNCTION CALLS
#
myFunc 4 # Displays 4.
myResult=$(myFunc 5) # Stores 5.
myResult2=`myFunc 5` # Same as above, old syntax.
lastCode="$?" # Fetches the last return code. Returns 0 as the last function is myFunc.

#
## IF
#
if [ 1 ]; then
	echo "Code"

elif [ 2 ]; then

else

fi

## Old comparison syntax.
if [ -z "$0" -a -f "$1" ]; then
fi

[ -f -z -a -o ! -lt -gt -eq -ne ... ]

# More modern. Comparisons are string-oriented. So "2" > "10" as it uses the lexicographical order. We can still use -eq, -lt, -le, ...
if [[ "$0" == "" && -f "$1" || -f *.txt ]]; then
fi

[[ -f ... && || == != >= <= > < * =~ ]]

# Numeric-oriented comparisons. Supports the entirety of C-style syntax operators.
if ((i == 21)); then
fi

((i ++ -- / * + - >= <= > < != == && || & | > < \<< >>))

#
## WHILE
#
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

#
## UNTIL
#
until ((i >= 5)); do
	echo "$i"
	((i++))
done

#
## FOR
#
for i in {1..10}; do
done

# C-style.
for ((i=0; i<5; i++)); do
done

for item in *; do
done

#
## CASE
#
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

#
## RANGES
#
{1..10} # from 1 to 10.
{01..010} # from 01 to 010 (padding).
{1..10..2} # from 1 to 10, step of 2.
{10..1} # from 10 to 1.
{a..e} # from a to e.

#
## TEMPORARY ASSIGNATIONS
#

# If a command is written at the end of a series of assignations, then those variables will get reset after the command has ended.
IFS=" " read -r l

k=1 sh -c 'echo $k' # 1. Here we need to NOT interpolate $k, and use sh -c to clone the environment and then output. If we used echo directly, the variable wouldn't have gotten changed in time.
echo "$k" # Empty.

a=1 b=2 c=3 echo "$a $b $c" # We can chain a variety of variables.

#
## BLOCK
#

# Block of code like in any other language. Captures and overwrites. All the declared variables are NOT imprisoned in the scope.
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

#
## ARITHMETIC/LEGA-C BLOCK.
#

# Same as above with blocks, but uses arithmetics behaviors.
res=$x
((
	res = res & x
	res = res + 1
	res++, res-- # Oneline uses "," as instruction separator.
	
	res = (x > 0) ? res : x # Ternary operator.
))
echo "$res"

#
## SUBSHELL
#

# Invoke a subshell which starts a dedicated environment. Captures variables, but cannot outsource a variable. $() and `` are subshells.
cd /
(
	# Subshell.
	cd /home
	x=2
	y=1
	
	pwd # Displays /home.
	echo "$x $y" # Displays "2 1".
)
pwd # Displays /.
echo "$x $y" # Displays "1  ". Y is empty.

#
## SPECIAL VARIABLES
#

IFS # Internal field separator.
$$ # Current PID.
$! # PID of the last asynchronous process ran with &.
$- # Current active shell options.
$_ # Last argument of the last command.
```
