# sorg [![Build Status](https://github.com/brandur/sorg/workflows/sorg%20CI/badge.svg)](https://github.com/brandur/sorg/actions)

A Go-based build script that compiles my [personal website][brandur]. This is
the site's second incarnation, with the original being [a Ruby/Sintra
stack][org] (sorg = "static org").

The site deploys automatically from its CI build with GitHub Actions as changes
are committed to the master branch.

## Clone

``` sh
git clone https://github.com/brandur/sorg.git
```

## Build

Install Go 1.9+, [direnv][direnv] set up and run [blackswan][blackswan], then:

``` sh
cp .envrc.sample .envrc

direnv allow

# Used to run the test suite.
createdb sorg-test

# Compile Go executables.
make install

# Run an initial build of the site, look for build output in public/.
make build

# Note that when watching for changes, this project has unfortunately gotten
# large enough that we bypass Mac OS' per-process limit for file descriptors,
# so that needs to be increased before the command below becomes runnable. This
# line can be put in an RC file:
ulimit -n 4096

# Watch for changes in Go files and/or content and recompile and rebuild when
# one occurs.
make loop
```

The project can be deployed to s3 using:

``` sh
pip install awscli

export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...
export S3_BUCKET=...
make deploy
```

Cached photos can be fetched using:

``` sh
make photographs-download
```

## Development

Run the entire lifecycle like in CI:

``` sh
make
```

Run the test suite:

``` sh
createdb sorg-test
make test
```

Run a single package's test suite or single test:

``` sh
go test ./markdown
go test ./markdown -run TestCollapseHTML
```

Get more verbose output while running tests:

```
go test -v ./markdown
```

[blackswan]: https://github.com/brandur/blackswan
[brandur]: https://brandur.org
[direnv]: https://direnv.net/
[org]: https://github.com/brandur/org

<!--
# vim: set tw=79:
-->
