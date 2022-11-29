###### _by just-do-halee_

# _Today I learned_

## 2022-11-29

<br>

- Linux

  - ::ETC

    - Output Redirection

        - **_`>`_** is the output redirection operator used for overwriting files that already exist in the directory or a new will be created.

        ```sh
        ls > test.txt  # creates test.txt that has ls information.
        echo "some string" > test.txt  # overwrites test.txt with 'some string'
        cat test.txt  # it will be 
        # files...
        # folders...
        # some string
        ```

        - **_`>>`_** is an output operator as well, but, it appends the data provided to the file.

        ```sh
        echo "hello" >> test2.txt  # creates test2.txt with 'hello'
        echo "world" >> test2.txt  # appends 'world' to the test2.txt
        cat test2.txt  # it will be
        # hello
        # world
        ```


<br><br>

##### **_[`| Back to list |`](../../README.md)_**
