Shell scripting is a powerful tool that allows you to automate tasks, manage files and directories, and interact with the operating system.

```bash

#!/bin/bash
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
# Checking if an argument is provided
if [ -z "$1" ]; then
  echo "No argument provided."
fi

#!/bin/bash
while getopts "a:b:c" opt; do
  case $opt in
    a)
      echo "Option -a with argument: $OPTARG"
      ;;
    b)
      echo "Option -b with argument: $OPTARG"
      ;;
    c)
      echo "Option -c"
      ;;
    \?)
      echo "Invalid option: -$OPTARG"
      ;;
  esac
done
# Running the script with options:
# ./script.sh -a argumentA -b argumentB -c argumentC

#!/bin/bash

# Nested if-else
if [ condition1 ]; then
  if [ condition2 ]; then
    echo "Condition 1 and Condition 2 met."
  else
    echo "Condition 1 met, but Condition 2 not met."
  fi
else
  echo "Condition 1 not met."
fi

# While loop with control statements
counter=0
while [ $counter -lt 10 ]; do
  echo "Counter: $counter"
  counter=$((counter + 1))
  if [ $counter -eq 5 ]; then
    break  # Exit the loop when counter reaches 5
  fi
done
```