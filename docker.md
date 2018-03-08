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

* I don't seem to be able to access the exposed port for my docker instance in
  OSX (docker). What to do?

    2 ways to deal with this:

    1. Get the docker-machine ip and connect there directly to the exposed port

    ```
    docker-machine ip <machine>
    ```

    1. Port forward from virtual box





