# Firefly III

## TODO:

* Installation

    https://github.com/firefly-iii/firefly-iii


## FAQ

* What is the docker machine name for firefly?

    firefly-iii
    ```
    docker-machine start firefly-iii
    ```

* How do you start the firefly docker-machine

    ```
    eval $(docker-machine env firefly-iii)
    docker-compose -f docker-compose.yml up
    ```
