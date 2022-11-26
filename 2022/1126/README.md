###### _by just-do-halee_

# _Today I learned_

## 2022-11-26

<br>

- Linux

  - ::ETC

    - Permission (drwxrwxrwx)

        ```shell
        drwxr-xr-x  35 just-do-halee  staff   1.1K Nov 26 13:46 2022
        -rw-r--r--   1 just-do-halee  staff   2.4K Oct  8 15:05 README.md
        -rw-r--r--   1 just-do-halee  staff   575B Sep  7 23:52 TEMPLATE.md
        -rwxrwxrwx   1 just-do-halee  staff   262B Oct  6 21:12 new
        ```

        **_`d(directory)`_** Directory or File
        
        **_`r(read)`_** Readable

        **_`w(write)`_** Writable

        **_`x(execute)`_** Executable

        ```shell
        d / rwx / rwx / rwx ...
        ```

        **_`_ / user owner / group owner / others`_** Permissions

        r = 4 , w = 2 , x = 1 , no permission = 0

        - Example

            ```shell
            -rwxr-xrw-  1  owner  staff  5B  Sep  7 21:12 Example

            = rwx(4+2+1)r-x(4+1)rw-(4+2) = 756

            = chmod 756 Example
            ```

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
