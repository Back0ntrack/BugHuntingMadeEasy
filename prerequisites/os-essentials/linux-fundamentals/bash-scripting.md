# Bash Scripting

```bash
#! /bin/bash
echo "Hello World!"
uname -a
whoami

```

## Variables

```bash
#!/bin/bash
name="Abhishek"
dept="IT"
sem=5

echo "Here is the student details"
echo "Name: $name"
echo "Department: $dept"
echo "Semester: $sem"
```

**Output**

<figure><img src="../../../.gitbook/assets/image (872).png" alt=""><figcaption></figcaption></figure>

We can export the variable in the `~/.bashrc` file to use it whenever we want:

### Permanent variables

```bash
export message="Hello World"
```

**Variable Naming Convention:**

* `Variable names should start with a letter or an underscore(_)`
* `Variable names can contain letters, numbers, and underscores (_)`
* `Variable names are case-sensitive`
* `Variable names should not contain spaces or special characters`
* `Avoid using reserved words, such as if, then, else, fi`

### Execute commands in echo

```bash
echo "Today is" `date`
```

Note that the is used to run another command.

<figure><img src="../../../.gitbook/assets/image (873).png" alt=""><figcaption></figcaption></figure>

### Special Variables

Special variables use the [Internal Field Separator](https://bash.cyberciti.biz/guide/$IFS) (`IFS`) to identify when an argument ends and the next begins.

| **IFS** | **Description**                                                                                                                                                         |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$#`    | This variable holds the number of arguments passed to the script.                                                                                                       |
| `$@`    | This variable can be used to retrieve the list of command-line arguments.                                                                                               |
| `$n`    | Each command-line argument can be selectively retrieved using its position. For example, the first argument is found at `$1`.                                           |
| `$$`    | The process ID of the currently executing process.                                                                                                                      |
| `$?`    | The exit status of the script. This variable is useful to determine a command's success. The value 0 represents successful execution, while 1 is a result of a failure. |

<figure><img src="../../../.gitbook/assets/image (874).png" alt=""><figcaption></figcaption></figure>

## Arguments

We can pass up to 9 arguments (`$0` to `$9`). The first argument means `$0`.

```bash
AbhishekPawar@htb[/htb]$ ./script.sh ARG1 ARG2 ARG3 ... ARG9
       ASSIGNMENTS:       $0      $1   $2   $3 ...   $9

```

## Arrays

```bash
#! /bin/bash
domains=(zero first second third fourth fifth sixth)
echo ${domains[0]}

# ==> Output
zero

```

## Comparison Operators

### String Operators

| **Operator** | **Description**                             |
| ------------ | ------------------------------------------- |
| `==`         | is equal to                                 |
| `!=`         | is not equal to                             |
| `<`          | is less than in ASCII alphabetical order    |
| `>`          | is greater than in ASCII alphabetical order |
| `-z`         | if the string is empty (null)               |
| `-n`         | if the string is not null                   |

```bash
#!/bin/bash
if [ "$1" != 'hackme']
then
	echo "hackme required as argument"
else
	echo "Hackme supplied"
fi

```

### Integer Operators

| **Operator** | **Description**             |
| ------------ | --------------------------- |
| `-eq`        | is equal to                 |
| `-ne`        | is not equal to             |
| `-lt`        | is less than                |
| `-le`        | is less than or equal to    |
| `-gt`        | is greater than             |
| `-ge`        | is greater than or equal to |

```bash
#!/bin/bash

if [ $1 -lt 18 ]
then
	echo "Not eligible to vote"
	exit 1
elif [ $1 -eq 18 ]
then
	echo "Wait 1 more year"
	exit 1
else
	echo "Eligible to vote"
	exit 1
fi
```

<figure><img src="../../../.gitbook/assets/image (875).png" alt=""><figcaption></figcaption></figure>

### File Operators

| Operator | Description                                            |
| -------- | ------------------------------------------------------ |
| `-e`     | if the file exist                                      |
| `-f`     | tests if it is a file                                  |
| `-d`     | tests if it is a directory                             |
| `-L`     | tests if it is if a symbolic link                      |
| `-N`     | checks if the file was modified after it was last read |
| `-O`     | if the current user owns the file                      |
| `-G`     | if the file’s group id matches the current user’s      |
| `-s`     | tests if the file has a size greater than 0            |
| `-r`     | tests if the file has read permission                  |
| `-w`     | tests if the file has write permission                 |
| `-x`     | tests if the file has execute permission               |

```bash
#!/bin/bash

# Check if the specified file exists
if [ -e "$1" ]
then
	echo -e "The file exists."
	exit 0

else
	echo -e "The file does not exist."
	exit 2
fi

```

### Logical Operators

| **Operator** | **Description**        |
| ------------ | ---------------------- |
| `!`          | logical negotation NOT |
| `&&`         | logical AND            |
| `\\|`        | logical OR             |

## Arithmetic

| **Operator** | **Description**                         |
| ------------ | --------------------------------------- |
| `+`          | Addition                                |
| `-`          | Substraction                            |
| `*`          | Multiplication                          |
| `/`          | Division                                |
| `%`          | Modulus                                 |
| `variable++` | Increase the value of the variable by 1 |
| `variable--` | Decrease the value of the variable by 1 |

```bash
#!/bin/bash

increase=1
decrease=1

echo "Addition: 10 + 10 = $((10 + 10))"
echo "Substraction: 10 - 10 = $((10 - 10))"
echo "Multiplication: 10 * 10 = $((10 * 10))"
echo "Division: 10 / 10 = $((10 / 10))"
echo "Modulus: 10 % 4 = $((10 % 4))"

((increase++))
echo "Increase Variable: $increase"

((decrease--))
echo "Decrease Variable: $decrease"

```

```bash
AbhishekPawar@htb[/htb]$ ./Arithmetic.sh

Addition: 10 + 10 = 20
Substraction: 10 - 10 = 0
Multiplication: 10 * 10 = 100
Division: 10 / 10 = 1
Modulus: 10 % 4 = 2
Increase Variable: 2
Decrease Variable: 0

```

### VarLength

```bash
#!/bin/bash
htb="HackTheBox"
echo ${#htb}

```

## IO Control

```bash
#!/bin/bash

read -p "Enter your age: " age

echo "Your age is: $age"

```

```bash
#!/bin/bash
echo -en "Enter your age:"
read age
echo "Your age is: $age"

```

## Flow Control

### For Loop

```bash
for variable in 1 2 3 4
do
	echo $variable
done

```

Use in single line:

```bash
for ip in 10.10.10.1 10.10.10.2 10.10.10.3;do ping -c 1 $ip; done

```

### While Loop

```bash
#!/bin/bash
read -p "Enter a number: " num
count=1

while [ $count -ne 10]
do
	echo "$num x $count = " $(($num*$count))
	((count++))

```

### Until Loop

```bash
#!/bin/bash

counter=0

until [ $counter -eq 10 ]
do
  # Increase $counter by 1
  ((counter++))
  echo "Counter: $counter"
done

```

## Case Statement

**Syntax**

```bash
case <expression> in
	pattern_1 ) statements ;;
	pattern_2 ) statements ;;
	pattern_3 ) statements ;;
esac

```

```bash
#!/bin/bash
read -p "Enter number of your birthday month:" num

case $num in
	"1") echo "January" ;;
	"2") echo "February" ;;
	"3") echo "March" ;;
	<SNIP>
	"*") exit 0;;   #Not Mandatory
esac

```

## Functions

**Syntax**`Method 1`

```bash
function name {
	<commands>
}
```

`Method 2`

```bash
name () {
	<commands>
}
```

**Function execution**

```bash
name
```

```bash
#!/bin/bash
# Function to print name of the user

function print_name {
	read -p "Enter your name: " name
	echo "Hii $name!, Glad to meet you"
}

```

### Pass parameter

```bash
#!/bin/bash
function print_pars {
	echo $1 $2 $3
}

one="First"
two="Second"
third="Third"

print_pars "$one" "$two" "$three"

```

### Return values

| **Return Code** | **Description**                |
| --------------- | ------------------------------ |
| `1`             | General errors                 |
| `2`             | Misuse of shell builtins       |
| `126`           | Command invoked cannot execute |
| `127`           | Command not found              |
| `128`           | Invalid argument to exit       |
| `128+n`         | Fatal error signal "`n`"       |
| `130`           | Script terminated by Control-C |
| `255\\\\*`      | Exit status out of range       |

## Verbose Debugging

```bash
bash -x -v <script>.sh
```

## [Script.sh](http://script.sh/)

```bash

RED='\\\\033[0;31m'
NC='\\\\033[0m' # No Color

function print_text {
    local text="$1"
    echo -e "${RED}${text}${NC}"
}

ip_regex="^([0-9]{1,3}\\\\.){3}[0-9]{1,3}$"

function make_dir {
  mkdir -p scanning/hosts scanning/ports
}

function execute_command {
  if [[ $1 == "hosts" ]]
  then
    print_text "$(cat alive_hosts.txt)"
  elif [[ $1 =~ $ip_regex ]]
  then
    ip=$1
   print_text "$(cat ./scanning/ports/${ip%.*}.0/${ip}.nmap)"
  fi

}

function take_commands {
  count=1
  while [[ count -lt 2 ]]
  do
    read -p "[Pentester@Parrot]-[~]# " command
    if [ "$command" == "exit" ]
    then
      ((count++))
    else
      execute_command "$command"
    fi
  done
}

function host_scan {
  ip_address=$(echo $1 | cut -d'/' -f1)
  sudo nmap -PE -sn -n  $1 -oN ./scanning/hosts/sub_$ip_address.nmap | grep "for" | cut -d " " -f5 > hosts.txt
  echo "List of alive hosts of $ip_address" >> alive_hosts.txt
  cat hosts.txt >> alive_hosts.txt
  hosts=$(cat hosts.txt)
  for i in $hosts
  do
    port_scan $i
  done
}

function port_scan {
  mkdir -p scanning/ports/$ip_address
  sudo nmap -sS -Pn -n $1 -oN ./scanning/ports/$ip_address/$1.nmap
}

function main {
  array=()

  make_dir
  for arg in "$@"
  do
      array+=("$arg")
  done

  for subnet in "${array[@]}"
  do
    echo "coming here"
    host_scan $subnet
  done
  take_commands
}

main "$@"

```
