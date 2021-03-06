# Boostware

**A simple client for speeding up file downloads**

# How to install

```
$ git clone git@github.com:un33k/boostware.git
$ cd boostware
$ yarn install | npm install
```

# Setup typescript and ts-node

```
# global scope
npm install -g ts-node
npm install -g typescript
```

# How to compile (transpile)

```bash
$ yarn prepare | npm run prepare

# Javascript and Typescritp definition files will be in `/lib`
```

# Options and params

```bash
# Run typescript
$ yarn multiget --help

# Run javascript
$ node lib/command.js --help
```

# Options (detailed)

```
node lib/command.js --help
Usage: command <url> [-o] [-b [-msc]] [-f] [-v]

Options:
  -V, --version                output the version number
  -o, --output <n>             path to save file to
  -b, --boost                  boost download multiple concurrent chunck fetch
  -m, --max-chunks <n>         total number of chunks to fetch
  -s, --chunk-size <n>         size of each chunk to fetch
  -c, --concurrent-chunks <n>  number of chunks fetched concurrently
  -f, --force                  overwrite if file already exists
  -v, --verbosity <n>          log verbosity (1,2,3)
  -h, --help                   output usage information
```

# Example
```
yarn multiget http://neekware.com/data/test/a-z.txt -v 3 -f -m 1 -s 3 -c 3

File created (a-z.txt)
# .... async batch & fetch
Fetching chunk range: [9, 11]
Fetching chunk range: [3, 5]
Fetching chunk range: [0, 2]
Fetching chunk range: [6, 8]
Wrote [0, 2] to file.
Wrote [3, 5] to file.
Wrote [6, 8] to file.
Fetching chunk range: [21, 23]
Fetching chunk range: [18, 20]
Fetching chunk range: [24, 26]
Fetching chunk range: [15, 17]
Fetching chunk range: [12, 14]
Wrote [18, 20] to file.
Wrote [21, 23] to file.
Wrote [24, 26] to file.
Wrote [9, 11] to file.
Wrote [12, 14] to file.
Wrote [15, 17] to file.

```

# How it works

## boost (default: false)
If the `-b,--boost` option is not selected, the file will be downloaded in one request and all other concurrency options are ignored.

If the `-b --boost` is selected, then the command prepares itself for concurrent multi-chunk download.

The `boost` options can be combined to optimize one or more of `memory`, `cpu`, `network`, `io`, etc.

## concurrent fetch groups (default: 4)
For slower networks, `-c` option ensures that only a `fixed` number of async requests are made to the server at a time.

The chunk fetcher works on the `all or none` premise.
With `-c` option, up-to `<n>` number of chunks are scheduled to be fetched in parallel. Each request individually retries `3` times to compensate for jittery networks. Each retry is pushed-off with increasing delay time to ensure the network bandwidth is not affected with multiple retries.

## chunk size (default: 1MB)
The `-s` option can be set for optimal chunk download speed.

## example
To download a 400 MB file, we can have the `-s` set to 2MB chunk each. Then we set `-c` to 4 concurrent chunks to be fetched at a time.  This way, the client, schedules the first 4 chunks of 2 MB in parallel. Once all those requests are on their ways, the second batch of 4 is scheduled and so on ..., till the full file has been requested in multiple chunks.  Storing the chunks are fully `async`. First come first `saved` respecting the position and ordering of the chunks.

## Future (roadmap)
1. Tests
2. Code coverage
3. Continuous Integrations (CI)
4. Available on npm.js
5. Support for more http(s) methods (PUT,POST,DELETE ..)
5. More examples

# License

Released under a ([MIT](https://github.com/un33k/boostware/blob/master/LICENSE)) license.

# Version

X.Y.Z Version

    `MAJOR` version -- making incompatible API changes
    `MINOR` version -- adding functionality in a backwards-compatible manner
    `PATCH` version -- making backwards-compatible bug fixes
