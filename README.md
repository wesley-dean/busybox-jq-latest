# busybox-jq-latest

## What this is

This is a daily-built (via cron job) of the latest and greatest BusyBox with JQ.

## How to use this

The 'jq' binary is set as the entrypoint for the image, so it can be run by piping
JSON content to Docker Run:

```sh
cat file.json | docker run -it --rm wesleydean/busybox-jq '.'
```

Alternatively, I've included a script, 'busybox-jq' that does the above (plus
the requisite 'tty' magic), so this will accomplish the same thing:

```sh
cat file.json | ./busybox-jq '.'
```

### How this is built

The build will:

1. download the latest from the 'master' branch of 'stedolan/jq' from GitHub
2. compile, link, and strip a static 'jq' executable
3. build a Docker image based on BusyBox with 'jq' installed as the entrypoint

Because the base image is BusyBox and the executable is stripped, the
resulting image is pretty small -- roughly 6.24mb at the time I wrote this.

The included .travis.yml file may be used to perform the build using Travis-CI
and push the resulting image up to Docker Hub.  That said, I've daily builds
setup so you're welcome to use mine:

[https://hub.docker.com/r/wesleydeanflexion/busybox-jq]

## Why this exists

This is a convenient (to me) way to use 'jq' on systems where jq is not installed
but Docker is (and we can pull / run Docker images).

## Dependency maintenance

Routine GitHub Actions dependency updates are managed by the centrally hosted
Jenkins Renovate job, with repository enrollment in root `renovate.json`.
The existing Dependabot configuration disables scheduled version-update PRs
but retains independent Dependabot security-update handling and its bot-scoped
auto-merge workflow.  Required review and CI protections are not changed.

The historical `.github/renovate.js` was a standalone Renovate runner
configuration referencing the former `wesley-dean-flexion` repository owner,
including auto-merge settings.  It has been retired in favor of centralized
runner configuration.  One unverified `publish_container` action SHA has
been replaced by the former `v1.1.0` version reference for Renovate to
resolve and pin, which temporarily reduces supply-chain immutability;
review the ensuing Renovate update before merging it.

## Who wrote this

My role in this is just to build and slap in 'jq'; the fine authors of BusyBox
and JQ did the real heavy lifting.

* [busybox](https://github.com/mirror/busybox)
* [jq](https://github.com/stedolan/jq)

Me, I'm a DevSecOps Engineer.

* [wesleydean.com](https://wesleydean.com/)
