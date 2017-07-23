# Linux

Assocaitive Array

    ```
    declare -A array
    for i in "${!array[@]}"
    do
      echo "key  : $i"
      echo "value: ${array[$i]}"
    done
    ```

Multiple lines

    ```
    tee filepath > /dev/null <<- EOF
    This
    Is 
    A Test
    EOF
    ```

