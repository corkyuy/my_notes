# Linux

## How to...

* Check open ports

    ```
    apt-get update
    apt-get install net-tools
    netstat -tuplen
    ```

## Assocaitive Array

```
declare -A array
for i in "${!array[@]}"
do
  echo "key  : $i"
  echo "value: ${array[$i]}"
done
```

## Multiple lines

```
tee filepath > /dev/null <<- EOF
This
Is 
A Test
EOF
```

