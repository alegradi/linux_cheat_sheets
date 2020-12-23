# Shell scripting basics

$1  (first agrument)
```
FILE=$1
if [[ -z $FILE ]]; then  # Check if it is defined
    echo "Please specify file to operate on"
    exit 1
fi
```

