# File Scanner

[![Build Status](https://travis-ci.org/jlprince21/file_scanner.svg?branch=master)](https://travis-ci.org/jlprince21/file_scanner)

Written in Rust, this program collects file metadata and stores it in a PostgreSQL
database. Some things it gathers include:

1. File name
2. Path
3. Size
4. XxHash checksum

In addition to collecting data about files, the program assists in indexing files
with tools such as file tagging, search, and more. Development on these features
is underway... stay tuned!

# Getting Started

To run the project, you will need a PostgreSQL database setup and configure the
`.env` file in this project to point to your database. See *Configuration* section
below.

Next, you will need to use [diesel](http://diesel.rs/) to run the database migrations
necessary to create tables needed for the project.

```
diesel migration run
```

Next, set any remaining configuration values as detailed in *Configuration*.

Once the prerequisites are met, you may run or build the project with:

```
# To see help
cargo run -- --help

# To run
cargo run

# To build for release
cargo build --release

```

See *Arguments* section for details on the arguments this program accepts.

# Arguments

This app takes a minimum of one command line argument before it will perform any
action beyond simply terminating. This section is divided into commands  subcommands.

## Commands

### Action

You can specify one of several actions to use via the `-a` or `--action` command
flags followed by an action name. For now configuration beyond selecting an
action to perform is handled in the `.env` file, see *Configuration*. Valid
actions are:

1. duplicates - finds duplicate files within database via **matching** hashes.
2. hash - computes hashes, file size, etc and stores results in database.
3. orphans - iterates *all* database entries computed by hash action and does
a simple check to see if files are still present. If a file is not present, its
entry in the database will be removed.

Examples:

```
# Start hashing files:
cargo run -- --action hash
```

## Subcommands

### Tagging a Listing

To aid in searching for any given file, you can apply tags to a listing ID which
in the future will be used as a search mechanism. For example, you could search
for all files containing the tag `vacation` and viola :violin:, all files with
the tag applied are returned!

To tag a listing, whose ID is _123_, with tags of `summer`, `beach`, and `vacation`:

```
cargo run -- tag 123 -- summer beach vacation
```

# Configuration

`.env` configuration setting include:

1. DATABASE_URL - PostgreSQL connection string.
2. DIRECTORY_TO_SCAN - root directory location to start scanning files from.

# License

License is MIT. See LICENSE.