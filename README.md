# Mustang Marketplace README

This project is now split into:

- `main.cpp` + `ApiClient.*`: presentation tier (ncurses client)
- `server_main.cpp` + domain entities: application tier (network API server)
- `Database.sql` + SQLite access in server: data tier

## Prerequisites

This project is built with:

- a C++17 compiler
- `make`
- SQLite3 development libraries
- OpenSSL / libcrypto development libraries
- ncurses development libraries
- `cpp-httplib`
- `nlohmann/json`

### Ubuntu / Debian

Refresh package metadata and install the required build dependencies:

```bash
sudo apt update
sudo apt install -y build-essential make pkg-config \
    libsqlite3-dev libssl-dev libncurses-dev libcpp-httplib-dev \
    nlohmann-json3-dev
```

If your system already has the packages but you want the latest available
versions from your configured repositories, run:

```bash
sudo apt update
sudo apt upgrade
```

### macOS

Install the Xcode Command Line Tools first:

```bash
xcode-select --install
```

Then install the project dependencies with Homebrew:

```bash
brew update
brew install openssl sqlite ncurses cpp-httplib nlohmann-json
```

If you want to upgrade already-installed Homebrew packages first:

```bash
brew update
brew upgrade
```

On some macOS systems, Homebrew libraries are installed outside the default
compiler search path. If `make all` fails with missing headers or linker errors,
build with explicit Homebrew include and library paths:

Intel Mac:

```bash
make all \
  CXX=clang++ \
  CXXFLAGS="-std=c++17 -Wall -Wextra -g -I. -I/usr/local/include" \
  LIBS_CLIENT="-L/usr/local/lib -lncurses -lcpp-httplib" \
  LIBS_SERVER="-L/usr/local/lib -lsqlite3 -lcrypto -lcpp-httplib" \
  LIBS_TEST="-L/usr/local/lib -lsqlite3 -lcrypto"
```

Apple Silicon Mac:

```bash
make all \
  CXX=clang++ \
  CXXFLAGS="-std=c++17 -Wall -Wextra -g -I. -I/opt/homebrew/include" \
  LIBS_CLIENT="-L/opt/homebrew/lib -lncurses -lcpp-httplib" \
  LIBS_SERVER="-L/opt/homebrew/lib -lsqlite3 -lcrypto -lcpp-httplib" \
  LIBS_TEST="-L/opt/homebrew/lib -lsqlite3 -lcrypto"
```

## Build

```bash
make all
```

## Run

Terminal 1:

```bash
./marketplace_server
```

Terminal 2:

```bash
./marketplace_client
```

The client connects to `127.0.0.1:9090` over plain TCP (one request line per connection).

## Acceptance Tests

The project includes grouped acceptance-test binaries for the user stories.
To build and run all of them:

```bash
make test_accept_all
```

## Optional: Connect to a remote server machine

You can point the client at another machine on your network using environment variables:

```bash
MARKETPLACE_HOST=<server-lan-ip> MARKETPLACE_PORT=9090 ./marketplace_client
```

If unset, defaults are:

- `MARKETPLACE_HOST=127.0.0.1`
- `MARKETPLACE_PORT=9090`
