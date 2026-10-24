# BojaMiner

NVIDIA GPU miner for Linux x86_64, WSL2 and compatible HiveOS systems.

## Algorithms

| Algorithm | Coin | Fee |
| --- | --- | --- |
| `pearlhash` | Pearl (PRL) | 1% |
| `poseidon2b` | Parano1d (NOID) | 1% |
| `poseidon2-goldilocks` | Quantus (QTC) | 1% |

## Requirements

Linux x86_64, **glibc 2.34+**, cgroup v2, and a compatible NVIDIA driver with
`nvidia-smi` available on `PATH`. Run with `sudo`; mining workers drop privileges.
Python and the CUDA toolkit are not required. WSL2 needs NVIDIA GPU support and
cgroup v2. Includes native kernels and PTX fallbacks for NVIDIA 10-50 series and compatible
enterprise GPUs. Untested models are not performance-qualified.

## Run

Extract the archive, then replace `POOL:PORT` and `COIN_ADDRESS` with your pool
and the address for the selected coin:

```bash
tar -xzf BojaMiner_0.4.0-rc8-source9_linux.tar.gz
chmod +x BojaMiner
sudo ./BojaMiner -a pearlhash --pool POOL:PORT --address COIN_ADDRESS --worker Rig1
```

For account-based pools, supply the mining username through `--address`.
Use `--password PASSWORD` if needed; the default is `x`. Stop with `Ctrl+C`.
Selected GPUs initialize automatically, and existing state is retained.

## Arguments

| Argument | Description |
| --- | --- |
| `-a`, `--algorithm` | Algorithm from the table above; default `pearlhash` |
| `--pool HOST:PORT` | Pool; accepts `stratum+tcp://` or `stratum+ssl://` |
| `--address ADDRESS` | Coin payout address or pool mining username |
| `--password PASSWORD` | Pool password; default `x` |
| `--worker NAME` | Worker name; default is the hostname |
| `--id 0,1,2` | Physical `nvidia-smi` GPU indices; default is all available GPUs |
| `--list-devices` | Show GPU indices, UUIDs and CUDA mapping |
| `--no-tui` | Use scrolling text output |
| `--api-bind-http IP:PORT` | Enable the read-only monitoring API |
| `--api-allow IP/CIDR` | Allow a monitoring client; repeatable |
| `--help` | Show usage |
| `--licenses` | Print bundled third-party notices |

## Monitoring

Enable local statistics with `--api-bind-http 127.0.0.1:4067`.
For a remote manager, bind the rig's LAN address and allow only the manager's
address with `--api-allow`. Use a trusted network; the API is read-only HTTP.

Awesome Miner uses a user-defined BojaMiner entry with API-only compatibility
and the compatible `/summary` protocol. HiveOS uses the separate custom-miner
adapter archive and requires Bash, `jq` and `curl`.

## Build from source

This repository distributes binaries, not source. Source builds require the
separately supplied BojaMiner source/build package and its pinned native inputs.
The Linux build uses Python 3.12, PyInstaller, GCC/binutils and Rust; changing
GPU kernels also requires a compatible CUDA toolkit. Follow the build package's
instructions; the release archive alone cannot rebuild the miner.

## Licenses

Third-party components retain their respective licenses and copyright notices.
Run `./BojaMiner --licenses` to read the bundled notices.
