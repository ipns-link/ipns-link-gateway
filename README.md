# IPNS-Link-gateway

![Activity](https://img.shields.io/badge/Expect%20major%20updates-yes-violet.svg) ![PR](https://img.shields.io/badge/PRs-Accepted-green) ![Platform](https://img.shields.io/badge/Platform-GNU%2fLinux-blue.svg) ![Lang](https://img.shields.io/badge/Lang-Bash-cyan.svg) ![UI](https://img.shields.io/badge/UI-Command%20line-orange.svg)

Prototype implementation of [IPNS-Link-gateway with resolution style: path](https://github.com/ipns-link/specs#ipns-link-gateway-specs).

### Hosting

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy) 

Or

**Self-host**:

```bash
PORT=<port> ipns-link-gateway
```

### Dependency

- `bash`
- [`go-ipfs-cli`](https://docs.ipfs.io/install/command-line/#linux) v0.9.0 or above
- `curl` and other standard GNU/Linux tools

### Environment variables

`PORT` : TCP port that the gateway listens to. Default: `8080`

`IPFS_PATH` : Path to local IPFS repo. Default: `~/.ipfs`

`MAX_CONNS` : Maximum number of clients that can be served concurrently. Default: `500`

# Acknowledgements

The `github-corner`s in [index.html](./index.html) is courtesy of [T. Holman](https://tholman.com/github-corners/).

