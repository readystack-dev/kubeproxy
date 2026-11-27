[![ReadyStack](https://readystack.dev/logo.svg)](https://readystack.dev)

**Stable, Open-source Releases, Automated** · [readystack.dev](https://readystack.dev)

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-readystack%2Fkubeproxy-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/readystack/kubeproxy) [![Docker pulls](https://img.shields.io/docker/pulls/readystack/kubeproxy)](https://hub.docker.com/r/readystack/kubeproxy) [![Grade at release](https://img.shields.io/badge/grade%20at%20release-A%20%2894%25%29-2ea44f)](https://readystack.dev/products/kubeproxy) [![Signed: cosign](https://img.shields.io/badge/signed-cosign-2ea44f)](https://readystack.dev/keys/cosign.pub) ![SBOM: SPDX](https://img.shields.io/badge/SBOM-SPDX-2ea44f) ![Provenance: SLSA v1](https://img.shields.io/badge/provenance-SLSA%20v1-2ea44f) ![Offline rebuild: verified](https://img.shields.io/badge/offline%20rebuild-verified-2ea44f)

# kube-proxy: ReadyStack build kit

The offline build kit for the Docker image `readystack/kubeproxy:v1.36.6-CE-ubuntu24.04-r5`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v1.36.6-CE-ubuntu24.04-r5](https://github.com/readystack-dev/kubeproxy/releases/tag/v1.36.6-CE-ubuntu24.04-r5)
- Image: [`docker.io/readystack/kubeproxy:v1.36.6-CE-ubuntu24.04-r5`](https://hub.docker.com/r/readystack/kubeproxy/tags), manifest digest `sha256:10fd4b76e3a44002c77d6983893dc6caac9c127d47791b9cbe458dffe20dc319`
- Product page: https://readystack.dev/products/kubeproxy
- Source and build recipe: https://readystack.dev/products/kubeproxy/recipe
- Build trail: https://readystack.dev/products/kubeproxy/trail/v1.36.6-CE-ubuntu24.04-r5
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=kubeproxy

## The image

A DaemonSet in kube-system, one Pod per node, with the host network and privileged: true, as kubeadm deploys it; in the Kubernetes VM Stack it is our own copy of kubeadm's DaemonSet (install/controlplane/kube-proxy.yaml) with runAsUser: 0, applied after kubeadm init (which skips its own kube-proxy addon). The image's ENTRYPOINT is /go-runner; the DaemonSet runs /usr/local/bin/kube-proxy --config=/var/lib/kube-proxy/config.conf --hostname-override=$(NODE_NAME).

## Who uses kube-proxy and why

Cluster administrators who run Kubernetes nodes.

kube-proxy runs on each node and maintains the network rules that make Kubernetes Services work, forwarding traffic sent to a Service to one of the pods behind it.

A typical use: A web front end calls a database through a Kubernetes Service. kube-proxy on every node keeps rules that send traffic for that Service to one of the database pods, wherever they run, and updates the rules as pods are replaced.

From the upstream project's own description: https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/, https://kubernetes.io/docs/concepts/overview/components/.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-21 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 99 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`999:999`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-21; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/kubeproxy.

kube-proxy is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v1.36.6-CE-ubuntu24.04-r5 release](https://github.com/readystack-dev/kubeproxy/releases/tag/v1.36.6-CE-ubuntu24.04-r5): `ReadyStack-kubeproxy-v1.36.6-CE-ubuntu24.04-r5.tar.gz` (502.7 MB), SHA-256 `7d75e4cbbe85a43684e71eef6778c6b3d20fcceac19a8551f45cef1dbe745e4a`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-kubeproxy-v1.36.6-CE-ubuntu24.04-r5.rebuild.json.sigstore ReadyStack-kubeproxy-v1.36.6-CE-ubuntu24.04-r5.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-kubeproxy-v1.36.6-CE-ubuntu24.04-r5.tar.gz
cd v1.36.6-CE-ubuntu24.04-r5
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-kubeproxy-v1.36.6-CE-ubuntu24.04-r5.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/kubeproxy/recipe. The complete kit is the release download. Each release is a tag here. How to report a vulnerability: [SECURITY.md](SECURITY.md).

## About ReadyStack

**Stable, Open-source Releases, Automated.** ReadyStack releases popular open-source apps every week with every available critical and high security fix applied, plus full documentation, reference data, tests and a build recipe that recreates each release bit-for-bit. Each release is rebuilt from its pinned inputs with the network off, checked against its published digest, signed, and scanned and graded again every day. The images and build kits are free, and ReadyStack is soon to be a nonprofit.

[readystack.dev](https://readystack.dev) · [All products](https://readystack.dev/products.html) · [Build kits on GitHub](https://github.com/readystack-dev) · [Defect log](https://readystack.dev/defects.html) · contact@readystack.dev

— ReadyStack · https://readystack.dev
