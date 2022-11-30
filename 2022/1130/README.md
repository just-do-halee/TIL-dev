###### _by just-do-halee_

# _Today I learned_

## 2022-11-30

<br>

- Rust::Alloc

  - ::Vec

    ```rust
    _ = vec![1,2,3,4,5];
    // same as Box::new([1,2,3,4,5]).into_vec();
    // so the capacity is 5 as .len()
    ```

    ```rust
    struct ClonableStruct;
    impl Clone for ClonableStruct {
        fn clone(&self) -> Self {
            eprintln!("cloned!");
            Self
        }
    }
    _ = vec![ClonableStruct; 10];
    // will print out "cloned!" 9 times
    // first one is just original
    ```

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
