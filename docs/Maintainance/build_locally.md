#### Set up the environment

=== "Windows"

    ```bash
    python -m venv .venv
    .venv/Scripts/activate
    ```
=== "MacOS"

    ```bash
    python -m venv .venv
    source .venv/bin/activate
    ```

#### Push the code to the repository

```bash
git add .
git commit -m "commit message"
git push
```


## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.



## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

