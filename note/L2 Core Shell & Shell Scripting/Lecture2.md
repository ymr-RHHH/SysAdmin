# WHY should I Learn to script?

- You're a SysAdmin

- You have to run some command all the time. But you don't want to repeat yourself. 

  So we can describe our task as a step-by-step set of instructions so that a computer can do it for you !



# Bash

In this course, the professor demonstrated operations by Bash. (But I like Zsh......)

- It's a shell
- Also a programming language

## How to run a Shell Script?

- `bash /path/to/script` (usually `.sh` file)

- `chmod +x /path/to/script`

  `/path/to/script`  （Requires shebang(at the start of the file)）

## Shebang

Just a special comment, specifies that the file is a script and calls a certain interpreter (i.e. Bash, Sh, Zsh, Python).

Shebang is written on the top of the file like this:

-  `#!/bin/bash`
- `#!/bin/sh`
- `#!/bin/python`

It's very important to include this otherwise the terminal is not going to know what to do in order to run the script. 

## Comments

Just like this:

`# this is a comment`

<br>



# Variables

Like any good scripting language, sometimes you need variables in order to not repeat yourself, that's the whole point of bash scripting.

The shell variable are a little funky......

## Shell variables

- Whitespace Matters, you should pay attention to use Whitespace. Make sure that all the spaces where they're supposed to be quotes.

  ```shell
  NAME = "1234"  #false
  NAME="1234"    #true
  ```

- Use `$VAR` to output the value of variable `VAR`

- Variables in Shell are untyped.

- Operations are contextual.

- Everything is string.

  ```shell
  FOO=1
  $FOO + 1
  #error! FOO is a string, can't perform mathematical operations.
  ```

- `expr` command  (expression)

  when we want to calculate values, we can use `expr`. 

  ```shell
  FOO=1
  expr $FOO+1
  #2
  ```

## User input

We can use `read` command to get user input

- In Bash:

  ```shell
  read -p "send: " FOO  # -p is for the optional prompt
  #type something and enter
  echo "sent: $FOO"
  ```

- In Zsh:

  ```shell
  read "FOO?send: "  # Zsh 特有的语法
  echo "sent: $FOO"
  ```

- Help for `read`

  If we just use `man read` , the system will tell us it's a systemCall. But we just want to know how to use the "read" program. In fact, `read`'s usage in Bash and Zsh is different

  - Bash

    ```shell
    # Bash
    help read
    
    # 或者
    man bash | less -p "^ *read \["
    ```

  - Zsh

    ```shell
    # Zsh
    run-help read
    
    # 或者
    man zshbuiltins | less -p "^ *read \["
    ```

## Subshell

- `$(cmd)` evaluates the command `cmd` inside, and substitutes the output into the script.

  ```shell
  FOO=$(expr 1+1)
  echo $FOO
  ```



# Conditions

Conditions are similar to initials to any other programming languages,  but it has a few quirks that you'll be mindful of. So you have initial checks which evaluate the expression.

- `test`

  conditional checks

- Evaluates an expression Also synonymous with `[]`

- Shell use negative logic :

  - 0 (true)
  - 1 (false)

- If you like arguments, you can supply the test command with:

  - -eq `==`

  - -ne `!=`

  - -gt `>`

  - -ge `>=`

  - -lt `<`

  - -le `<=`

```shell
test zero = zero; echo $?

test zero = one; echo $?
```

> - **`;` - Command Separator **
>
>   The semicolon `;` is a **command separator** in shell. It allows you to run multiple commands on the same line, one after another.
> 	```shell
> 	# Runs command1, then command2, then command3
> 	command1; command2; command3
> 	```
>
> -  **`$?` - Exit Status Variable**
>
>   `$?` is a **special shell variable** that holds the **exit status** (return code) of the last command executed.
>
>   ```shell
>   [ 0 -lt 1 ] && [ 0 -gt 1 ]  # This evaluates to FALSE (1 > 0 is false)
>   echo $?                     # Prints "1" (the exit status)
>   ```

## "Boolean" ops

- `&&` and `||` for shell
- `-a` and `-o` for `test` / `[]` 

```shell
[0 -lt 1] && [0 -gt 1]; echo $?

[0 -lt 1 -o 0 -gt 1]; echo $?
```

## if

### if

```shell
if ["$1" -eq 79];  # ; is necessary here! This is a requirement of the shell syntax.
then
	echo "nice"
fi
```

### if-else
```shell
if ["$1" -eq 79]; 
then
	echo "nice"
else
	echo "darn"
fi
```

### elif

...And what ifn't but if

```shell
if ["$1" -eq 79]; 
then
	echo "nice"
elif ["$1" -eq 79];
then
	echo "the answer"
else
	echo "WTF!!"
fi
```

## case

```bash
#! /bin/bash
read -p "are you 21?" ANSWER
case "$ANSWER" in 
	"yes")
	echo "yes!!!!!!!";;
	"no")
	echo "okkkkkk";;
	*)
	echo "Answer my question!!!"
esac
```



# Loops

## for loops

- for all your stuff in stuffs
	```shell
	#!/bin/bash
	SHEEP=("one" "dos" "tre")
	
	for S in $SHEEP
	do
	  echo "$S sheep..."
	done
	```

	echo：
	
	```shell
	┌──(kali㉿kali-vbox)-[~/SysAdmin/Lecture2]
	└─$ ./test.sh  
	one sheep...
	dos sheep...
	tre sheep...
	```
	
	<br>
	
	```shell
	#!/bin/bash
	SHEEP=("one" "dos" "tre")
	
	for S in ${SHEEP[*]}
	do
	  echo "$S sheep..."
	done
	```
	
	- :warning: 
	
	  if we just write this
	
	  ```shell
	  SHEEP=("one" "dos" "tre")
	  # 数组包含：SHEEP[0]="one", SHEEP[1]="dos", SHEEP[2]="tre"
	  
	  for S in $SHEEP  # 相当于 for S in "one"
	  do
	    echo "$S sheep..."
	  done
	  ```
	
- also supports ranges

  ```shell
  n=0
  for x in {1..10}
  do 
  	n=$(expr $x + $n)
  done
  echo $n
  ```

## while loops

```shell
while true
do
	echo "2233"
done
```



> # Exercise 
>
> Let's write a script that copies files in our current directory into new files with "new" prepended to the contents.
>
> etc.
>
> ```shell
> > ls
> a.txt b.txt c.txt 
> > ./mycoolscript.sh 
> > ls
> a.txt b.txt c.txt new_a.txt new_b.txt new_c.txt
> ```
>
> ```shell
> #! /bin/bash
> 
> FILES=$(ls *.txt)
> for FILE in $FILES
> do
> 	cp $FILE new$FILE
> done
> ```



























