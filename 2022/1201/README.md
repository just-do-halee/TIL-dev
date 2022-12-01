###### _by just-do-halee_

# _Today I learned_

## 2022-12-1

<br>

- Rust::Function

  - fn(..) -> .. Type Function

    ```rust
    // higher order function
    fn hof(f: fn(i32) -> f32) { println!("{}", f(5)); }

    fn main() {
        let pure_closure = |x| x as f32;
        hof(pure_closure);
        fn pure_function(x: i32) -> f32 { x as f32 }
        hof(pure_function);

        // can be pure closure or a function.
        // this is function signature pointing actual fn. 

        // let t = 1;
        // let captured_closure = |x| {
        //    (x + t) as f32
        // };
        // this cannot be possible.
        // fn(..) -> .. only can be a no captured closure
        // or a function.
        // hof(captured_closure); 
    }
    ```

  <br>

  - Fn..(..) -> .. Trait Function

    - **_`Fn`_** can run any number of times, only closing over immutable bindings.
        
        ```rust
        // cannot capture mutable variables.
        fn hof(f: impl Fn(i32) -> f32) { println!("{}", f(5)); }

        fn main() {
            let t = 1;
            // let v = vec![1];
            let borrowed_captured = |x| {
                // let v2 = v; // impossible to take an ownership for capturing.
                (x + t) as f32
                // copy or borrowing only.
                // let v2 = &v; // possible.
            };
            // callable multiple times.
            hof(borrowed_captured);
            hof(borrowed_captured);
            hof(borrowed_captured);

            fn pure_function(x: i32) -> f32 { x as f32 }
            hof(pure_function);
            // can be immutable captured closure or
            // a function.
        }
        ```

    <br>

    - **_`FnMut`_** can run any number of times, closing over mutable and immutable bindings.
        
        ```rust
        // can capture mutable variables.
        fn hof(f: impl FnMut(i32) -> f32) { println!("{}", f(5)); }

        fn main() {
            let mut t = 0;
            let mut_borrowed_captured = |x| {
                t += 1;
                (x + t) as f32
            };
            // callable multiple times.
            hof(mut_borrowed_captured); // x + 1
            hof(mut_borrowed_captured); // x + 2
            hof(mut_borrowed_captured); // x + 3

            fn pure_function(x: i32) -> f32 { x as f32 }
            hof(pure_function);
            // can be mutable captured closure or
            // immutable captured closure, a function.
        }
        ```

    - **_`FnOnce`_** can run once, taking ownership of captured bindings.
        
        ```rust
        // can capture mutable variables.
        fn hof(f: impl FnOnce(i32) -> f32) { println!("{}", f(5)); }

        fn main() {
            let v = vec![1];
            let once_closure = |x| {
                let a = v;
                (x + a[0]) as f32
            };
            // callable just one time.
            // because of the closure taking
            // v(vec![1]) ownership
            hof(once_closure);
            // so hof() takes once_closure ownership itself.
            // hof(once_closure); // error: once_closure is already moved

            let v2 = vec![2];
            let borrowed_closure = |x| {
                (x + v2[0]) as f32
            };
            hof(borrowed_closure);
            hof(borrowed_closure);
            // but it can run multiple times because of only borrowing

            fn pure_function(x: i32) -> f32 { x as f32 }
            hof(pure_function);
            hof(pure_function);
            hof(pure_function);
            // can take ownership of captured variables and runs once, or
            // muttable.. immutable.. closure, a function mutiple times.
        }
        ```
    
<br><br>

##### **_[`| Back to list |`](../../README.md)_**
