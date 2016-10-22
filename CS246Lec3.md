## Shell scripts

- file containing a sequence of shell commands

  - execute as if it was a program

  - `#!/bin/bash` — tell the OS this is a shell script "she-bang"

    e.g.

    ```shell
    #!/bin/bash
    date
    whoami
    pwd
    ```

  - it's a text file with any file name

  - `./first-script` — tell the system in the current directory and look for the file named first-script

  - 1. she-bang
    2. make excitable
       - chmod u+x….

## Path variable

- list of directories
- where bash looks for commands
- never includes the current directory
- `./` to run from current directory

## Variables

- supported by bash
- `x=1` — set a variable(no spaces!)
- `echo $x` — get value of variable
- `echo ${x}` — alt syntax(better), avoids ambiguity
- variables are all strings
  - `dir=~` — `dir=/u4/j2avery(home directory)`
  - `dir=x` — * glowing Patter for "all files"
  - `echo $(dir)`
  - `cd $(dir)`

## Double quotes

- bash will expand a variable in double quotes
  - e.g. `"Path is ${PATH}"`

## Single quotes

- bash will not epand, will always get literal value

## Script Arguments

- `./myscript arg1, arg2`

  - `$1` — first argument from command line
  - `$2` — second argument from command line
  - `$0` — name of the script being run
  - `$#` — number of command line arguments

- e.g. check if a word is in the dictionary

  ```shell
  #!/bin/bash
  # use egrep to solve (this is a comment)
  egrep "^$1$" /usr/share/dict/word
  ```

- e..g password check

  ```shell
  #!/bin/bash
  egrep "^$1$" /usr/share/dict/words > /dev/null
  if[ $? -eq 0 ]; then
  	echo "bad password"
  else
  	echo "Good password"
  fi	
  ```

  - `egrep` return 0 means match
  - `$?` last return code

## Syntax

```shell
if[cond]; then
	...
else if[cond] then
	...
else ...
	...
```

- e..g verify arguments

  ```shell
  #!/bin/bash
  usage(){
    echo "Usage:$0 password"
  }
  if [ $# -ne 1 ]; then
  	usage
  	exit
  fi
  egrep "^$1$" /usr/share/dict/word > /dev/null
  ```

## Variables are strings

- operators/expression will tell bash to treat as a string or integer
- (integer)
  - `eq` equals
  - `ne` not equals
  - `lt` less than
  - `gt` greater than
- (string)
  - = equals
  - == equals
  - != not equals
  - \> greater than
  - \< less than

## Loops

- e.g. bring numbers from 1 to `$1`(i.e. command-line argument)

  ```shell
  #!/bin/bash
  x=1
  while [ $x -le $1 ]; do
  	echo $x
  	x=$((x+1))
  done
  ```

  - `(())` syntax for arithmetic operations

- e.g. iterate through a list

  - rename a bunch of files from .cpp to .cc

  ```shell
  #!/bin/bash
  for name in *.cpp; do
  	mv ${name} ${name%cpp}cc
  done
  ```

- list-string sep by whitespace

  - e.g. how many times does $1(word) occur in $2(file)

    ```shell
    #!/bin/bash
    x=0
    for word in $(cat $2); do
    	if [ word = $1 ]; then
    		x = $((x+1))
    	fi
    done
    echo $x
    ```