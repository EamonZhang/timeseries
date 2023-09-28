# Drycc Postgres
[![Build Status](https://woodpecker.drycc.cc/api/badges/drycc/timeseries/status.svg)](https://woodpecker.drycc.cc/drycc/timeseries)

Drycc (pronounced DAY-iss) Workflow is an open source Platform as a Service (PaaS) that adds a developer-friendly layer to any [Kubernetes](http://kubernetes.io) cluster, making it easy to deploy and manage applications on your own servers.

For more information about the Drycc Workflow, please visit the main project page at https://github.com/drycc/workflow.

We welcome your input! If you have feedback, please submit an [issue][issues]. If you'd like to participate in development, please read the "Development" section below and submit a [pull request][prs].

# About

This component is a Timescaledb for use in Kubernetes. While it's intended for use inside of the Drycc Workflow open source [PaaS](https://en.wikipedia.org/wiki/Platform_as_a_service), it's flexible enough to be used as a standalone pod on any Kubernetes cluster or even as a standalone container.

# Development

The Drycc project welcomes contributions from all developers. The high level process for development matches many other open source projects. See below for an outline.

- Fork this repository
- Make your changes
- Submit a [pull request][prs] (PR) to this repository with your changes, and unit tests whenever possible
- If your PR fixes any [issues][issues], make sure you write Fixes #1234 in your PR description (where #1234 is the number of the issue you're closing)
- The Drycc core contributors will review your code. After each of them sign off on your code, they'll label your PR with LGTM1 and LGTM2 (respectively). Once that happens, a contributor will merge it

## Prerequisites

In order to develop and test this component in a Drycc cluster, you'll need the following:

* [GNU Make](https://www.gnu.org/software/make/)
* [Podman](https://podman.io/) installed, configured and running
* A working Kubernetes cluster and `kubectl` installed and configured to talk to the cluster
 * If you don't have this setup, please see [the Kubernetes documentation][k8s-docs]

## Testing Your Code

Once you have all the aforementioned prerequisites, you are ready to start writing code. Once you've finished building a new feature or fixed a bug, please write a unit or integration test for it if possible. See [an existing test](https://github.com/drycc/postgres/blob/main/contrib/ci/test.sh) for an example test.

If your feature or bugfix doesn't easily lend itself to unit/integration testing, you may need to add tests at a higher level. Please consider adding a test to our [end-to-end test suite](https://github.com/drycc/workflow-e2e) in that case. If you do, please reference the end-to-end test pull request in your pull request for this repository.

### Dogfooding

Finally, we encourage you to [dogfood](https://en.wikipedia.org/wiki/Eating_your_own_dog_food) this component while you're writing code on it. To do so, you'll need to build and push Container images with your changes.

This project has a [Makefile](https://github.com/drycc/timeseries/blob/main/Makefile) that makes these tasks significantly easier. It requires the following environment variables to be set:

* `DRYCC_REGISTRY` - A Container registry that you have push access to and your Kubernetes cluster can pull from
  * If this is [Docker Hub](https://hub.docker.com/), leave this variable empty
  * Otherwise, ensure it has a trailing `/`
* `IMAGE_PREFIX` - The organization in the Container repository. This defaults to `drycc`, but if you don't have access to that organization, set this to one you have push access to.
* `SHORT_NAME` (optional) - The name of the image. This defaults to `timeseries`
* `VERSION` (optional) - The tag of the Container image. This defaults to the current Git SHA (the output of `git rev-parse --short HEAD`)

Assuming you have these variables set correctly, run `make podman-build` to build the new image, and `make podman-push` to push it. Here is an example command that would push to `example.com/arschles/timeseries:devel`:

```console
export DRYCC_REGISTRY=example.com/
export IMAGE_PREFIX=arschles
export VERSION=devel
make podman-build podman-push
```

Note that you'll have to push your image to a Container repository (`make podman-push`) in order for your Kubernetes cluster to pull the image. This is important for testing in your cluster.


[issues]: https://github.com/drycc/timeseries/issues
[k8s-docs]: http://kubernetes.io/docs
[prs]: https://github.com/drycc/timeseries/pulls