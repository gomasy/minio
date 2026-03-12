> [!IMPORTANT]
> **This is a community-maintained fork of [minio/minio](https://github.com/minio/minio), maintained by [Pigsty](https://pigsty.io).**
> This project is **not** affiliated with, endorsed by, or sponsored by MinIO, Inc.
> "MinIO" is a trademark of MinIO, Inc., used here solely to identify the upstream project.
>
> Changes from upstream are minimal:
> - Restored the embedded management console version reference
> - Updated documentation links and Go module paths to point to this repository
>
> Distributed under the original [GNU AGPLv3](LICENSE) license.

# MinIO Quickstart Guide

[![Website: minio.pigsty.io](https://img.shields.io/badge/website-minio.pigsty.io-slategray?style=flat&logo=cilium&logoColor=white)](https://pigsty.io) [![Slack](https://slack.min.io/slack?type=svg)](https://slack.min.io) [![license](https://img.shields.io/badge/license-AGPL%20V3-blue)](https://github.com/pgsty/minio/blob/master/LICENSE)

MinIO is a high-performance, S3-compatible object storage solution released under the GNU AGPL v3.0 license.
Designed for speed and scalability, it powers AI/ML, analytics, and data-intensive workloads with industry-leading performance.

- S3 API Compatible – Seamless integration with existing S3 tools
- Built for AI & Analytics – Optimized for large-scale data pipelines
- High Performance – Ideal for demanding storage workloads.

This README provides instructions for building MinIO from source and deploying onto baremetal hardware.
Use the [MinIO Documentation](https://github.com/minio/docs) project to build and host a local copy of the documentation.

## Install from Source

Use the following commands to compile and run a standalone MinIO server from source.
If you do not have a working Golang environment, please follow [How to install Golang](https://golang.org/doc/install). Minimum version required is [go1.24](https://golang.org/dl/#stable)

```sh
go install github.com/pgsty/minio@latest
```

You can alternatively run `go build` and use the `GOOS` and `GOARCH` environment variables to control the OS and architecture target.
For example:

```
env GOOS=linux GOARCH=arm64 go build
```

Start MinIO by running `minio server PATH` where `PATH` is any empty folder on your local filesystem.

The MinIO deployment starts using default root credentials `minioadmin:minioadmin`.
You can test the deployment using the MinIO Console, an embedded web-based object browser built into MinIO Server.
Point a web browser running on the host machine to <http://127.0.0.1:9000> and log in with the root credentials.
You can use the Browser to create buckets, upload objects, and browse the contents of the MinIO server.

You can also connect using any S3-compatible tool, such as the MinIO Client `mc` commandline tool:

```sh
mc alias set local http://localhost:9000 minioadmin minioadmin
mc admin info local
```

See [Test using MinIO Client `mc`](#test-using-minio-client-mc) for more information on using the `mc` commandline tool.
For application developers, see <https://min.io/docs/minio/linux/developers/minio-drivers.html> to view MinIO SDKs for supported languages.

## Build Docker Image

You can use the `docker build .` command to build a Docker image on your local host machine.
You must first [build MinIO](#install-from-source) and ensure the `minio` binary exists in the project root.

The following command builds the Docker image using the default `Dockerfile` in the root project directory with the repository and image tag `myminio:minio`

```sh
docker build -t myminio:minio .
```

Use `docker image ls` to confirm the image exists in your local repository.
You can run the server using standard Docker invocation:

```sh
docker run -p 9000:9000 -p 9001:9001 myminio:minio server /tmp/minio --console-address :9001
```

Complete documentation for building Docker containers, managing custom images, or loading images into orchestration platforms is out of scope for this documentation.
You can modify the `Dockerfile` and `dockerscripts/docker-entrypoint.sh` as-needed to reflect your specific image requirements.

## Install using Helm Charts

There are two paths for installing MinIO onto Kubernetes infrastructure:

- Use the [MinIO Operator](https://github.com/minio/operator)
- Use the community-maintained [Helm charts](https://github.com/pgsty/minio/tree/master/helm/minio)

The Community Helm chart has instructions in the folder-level README.

## Test MinIO Connectivity

### Test using MinIO Console

MinIO Server comes with an embedded web based object browser.
Point your web browser to <http://127.0.0.1:9000> to ensure your server has started successfully.

> [!NOTE]
> MinIO runs console on random port by default, if you wish to choose a specific port use `--console-address` to pick a specific interface and port.

### Test using MinIO Client `mc`

`mc` provides a modern alternative to UNIX commands like ls, cat, cp, mirror, diff etc. It supports filesystems and Amazon S3 compatible cloud storage services.

The following commands set a local alias, validate the server information, create a bucket, copy data to that bucket, and list the contents of the bucket.

```sh
mc alias set local http://localhost:9000 minioadmin minioadmin
mc admin info
mc mb data
mc cp ~/Downloads/mydata data/
mc ls data/
```

Follow the MinIO Client [Quickstart Guide](https://min.io/docs/minio/linux/reference/minio-mc.html#quickstart) for further instructions.

## Explore Further

- [MinIO Erasure Code Overview](https://min.io/docs/minio/linux/operations/concepts/erasure-coding.html)
- [Use `mc` with MinIO Server](https://min.io/docs/minio/linux/reference/minio-mc.html)
- [Use `minio-go` SDK with MinIO Server](https://min.io/docs/minio/linux/developers/go/minio-go.html)
- [MinIO V3 Metrics Reference](docs/metrics/v3.md)

## Contribute

Please follow MinIO [Contributor's Guide](https://github.com/minio/minio/blob/master/CONTRIBUTING.md) for guidance on making new contributions to the repository.

## License

- MinIO source is licensed under the [GNU AGPLv3](LICENSE).
- MinIO [documentation](docs/) is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
- [License Compliance](COMPLIANCE.md)
