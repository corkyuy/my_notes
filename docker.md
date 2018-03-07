# Docker

## FAQ

* How do you start / create a new machine?

    ```
    docker-machine create <machine>
    docker-machine start <machine>
    ```

* How to remove a machine?

    ```
    docker-machine rm <machine>
    ```

* How to load docker machine environment

    ```
    eval $(docker-machine env <machine>)
    ```

* How to bash into a docker container instance?

    ```
    docker exec -it <container id> /bin/bash
    ```



