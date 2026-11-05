# Using awk as a file parser

Handles multiple files at the same time in order to extract intel from them, using a separator. When providing several files, awk will concatenate all of them and manipulate them all at once. A `BEGIN` phase and `END` phase are provided to handle the lifecycle of the algorithm.
- `-F` : Change the field separator.
- `-f` : Reads awk code from a file.
- `-v` : Inject a variable to the awk program (`-v var=val)`.
```bash
# Comment.
	
# Function declaration.
function square(x)
{
	return x*x
}

# Code run at the very beginning (before file parsing).
BEGIN
{
	# We cannot access file-related environment variables here yet.
		
	# FS is the variable that stores the field separator. Overriding it will cut each line differently. When invoking awk from the terminal, defined via -F flag.
	print FS
		
	# Print field separator. Can be overridden. Space by default.
	print OFS
}
	
# The following code is executed for each row.
{
	# $0 = the entire line.
	print $0
		
	# Displays $0 (if nothing is provided).
	print
		
	# $i = i-th element. We can litterally index "$i" in a for-loop to go through every argument.
	print $1
		
	# NF displays the amount of fields in the row, and $NF gives the value of the last field. This is logical since $NF would be $5 (same as $i in for loops).
	print NF, $NF
		
	 # NR outputs the row number AFTER ALL FILES ARE CONCATENATED, and FNR displays the row number FOR EACH FILE.
	print NR, FNR
}
	
# Conditional statement. Each line that respects this condition will get fed in the following code.
$1 == "Hi"
{
	# C-style ifs.
	if ($1 != "Hi")
	{
		print "Cannot happen"
	}
	else if ($1 == "Hi")
	{
		print "Always happens"
	}
	else
	{
		print "Impossible to reach"
	}

	# C-style for loop.
	for (i = 1; i <= NF; i++)
	{
		# $i accesses field i.
		print "Column " i " : " $i
	}
		
	# C-style while.
	i = 1
	while (i <= NF)
	{
		print "Column " i " : " $i
		i++
			
		if (i == 0)
		{
			break
		}
	}
	
	# C-style do/while.	
	i = 1
	do
	{
		print "Column " i " : " $i
		i++
		continue
	}
	while (i <= NF)
}

# Same as above, but done with a regex.
$1 ~ /aeiouy/
{
	# Skip line.
	next
}

# Code that is read after parsing all the lines.
END
{
	# Arrays. Indices should always start at 1 in awk.
	t[1] = 0
		
	# Dictionaries.
	t["night"] = "hey"
	
	# Key presence test.
	if ("night" in t)
	{
		print "Found key \"night\" in the dictionary, with value \"" t["night"] "\" !"
	}
		
	# Array & dictionary iterator.
	for (cle in t)
	{...}
		
	# Removes the value at the provided key.
	delete t[1]
		
	# Deletes all the structure.
	delete t
		
	# Multidimension arrays don't exist. They are done by concatenating each key via a separator.
	SUBSEP = ":" # Field separator for key access.
		
	i = 1
	j = 3
	t[i, j] = "a"
		
	# Accessing t[i, j] is equivalent to concatenating i and j via SUBSEP and accessing the value.
	print t[i, j], t[i SUBSEP j]
	
	# Function call.
	print square(2)
}
```

```bash
# Call by file.
awk -F ";" -f file.awk ...files

# Alternatively, you can define the entirety of the script in between ''
awk -F ";" '$2 ~ "hi" {print $1}' ...files
```

!!! warning "String Indices"
	String indices start at 1, **not 0** !

- `length(x)` : Length of a string..
- `substr(x, pos, len)` : Extracts a subtring from string x, starting at `pos` for up to `len` characters. Returns the substring.
- `tolower(x)`, `toupper(x)` : Capitalization.
- `split(x, out, sep)` : Splits `x` using separator `sep`. Puts the result in `out` and returns the amount of elements assigned in `out`.
