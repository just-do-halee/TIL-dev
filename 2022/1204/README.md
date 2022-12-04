###### _by just-do-halee_

# _Today I learned_

## 2022-12-4

<br>

- Rust::IO

  - ::FileSystem

    ```rust
    use std::fs::File;
    
    // File::create(): create or overwrite with `write-only` mode.
    File::create("foo.txt")?.write_all(b"hello")?;
    // File::open(): open with `read-only` mode.
    _ = std::fs::File::open("foo.txt")?.read(&mut [0u8; 10]);
    ```

    ```rust
    use std::fs::OpenOptions;

    // OpenOptions::new(): a builder for file options

    // read & write & create mode.
    OpenOptions::new()
        .read(true)
        .write(true)
        .create(true)
        .open("foo.txt")?;

    // append mode. same as
    // {
    //     let mut file = std::fs::OpenOptions::new()
    //         .write(true)
    //         .open("foo.txt")?;
    //
    //     file.seek(SeekFrom::End(0))?;
    //     file.write_all(b"hello")?;
    // }
    OpenOptions::new()
        .append(true)
        .open("foo.txt")?
        .write_all(b"hello")?;
    ```

    ```rust
    use std::{
        fs,
        io::{self, Read},
    };

    // idiomatic way to read only

    let file = fs::File::open("foo.txt")?;
    let mut reader = io::BufReader::new(file); // for read() many times without flush

    // for no-alloc
    let mut buf = [0u8; 32];

    loop {
        match reader.read(&mut buf)? {
            0 => break,
            n => {
                let s = std::str::from_utf8(&buf[..n]).unwrap();
                print!("{}", s);
            }
        }
    }
    ```

    ```rust
    use std::fs;
    use os::unix::prelude::PermissionsExt;

    fs::rename("foo.txt", "bar.txt")?;
    fs::set_permissions("baz.txt", fs::Permissions::from_mode(0o777))?;
    fs::copy("bar.txt", "bar2.txt")?;
    fs::remove_file("bar2.txt")?;

    fs::create_dir("baz")?;
    fs::create_dir_all("baz/some/dir")?;

    fs::remove_dir("baz/some/dir")?;
    fs::remove_dir_all("baz/some")?;

    // walk through a directory
    fs::read_dir(".")?
        .filter_map(|entry| entry.ok())
        .for_each(|entry| println!("{:?}", entry.path()));
    ```

<br><br>

##### **_[`| Back to list |`](../../README.md)_**
