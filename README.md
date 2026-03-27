# csv-to-json

A simple, fast, and cross-platform CLI tool to convert CSV data to JSON, written in Go.

## Features

- Convert CSV to a JSON array.
- Reads from a file or `stdin`.
- Single binary, no external dependencies.
- Cross-platform support (macOS amd64/arm64, Linux amd64/arm64, Windows amd64).

## Installation

1.  Go to the [Releases page](https://github.com/nlink-jp/csv-to-json/releases).
2.  Download the appropriate archive for your operating system and architecture (e.g., `csv-to-json-v1.0.2-darwin-arm64.zip`).
3.  Extract the archive.
4.  Move the `csv-to-json` binary to a directory in your `$PATH` (e.g., `/usr/local/bin` on macOS/Linux).

## Usage

### Basic Conversion

Provide a path to a CSV file as an argument:

```sh
csv-to-json data.csv > output.json
```

### From `stdin`

Pipe data directly to the command:

```sh
cat data.csv | csv-to-json > output.json
```

### Options

-   `-v`, `--version`: Print the version information.

    ```sh
    $ csv-to-json --version
    csv-to-json version v0.1.0 
    ```

## Building

### Prerequisites

-   Go (1.18 or later)
-   Make
-   Git

### Building

1.  Clone the repository:

    ```sh
    git clone https://github.com/nlink-jp/csv-to-json.git
    cd csv-to-json
    ```

2.  Create a git tag to embed version info (optional, defaults to commit hash):

    ```sh
    git tag v0.1.0
    ```

3.  Run `make` to build binaries and archives:

    ```sh
    # Cross-compile for all platforms and create .zip archives
    make package

    # Or, to build just for your local machine
    make build
    ```

    The build artifacts will be located in the `dist/` directory.