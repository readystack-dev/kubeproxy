# kube-proxy: ReadyStack build kit

The offline build kit for the Docker image `readystack/kubeproxy:v1.36.6-CE-ubuntu24.04-r5`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v1.36.6-CE-ubuntu24.04-r5](https://github.com/readystack-dev/kubeproxy/releases/tag/v1.36.6-CE-ubuntu24.04-r5)
- Image: [`docker.io/readystack/kubeproxy:v1.36.6-CE-ubuntu24.04-r5`](https://hub.docker.com/r/readystack/kubeproxy/tags), manifest digest `sha256:10fd4b76e3a44002c77d6983893dc6caac9c127d47791b9cbe458dffe20dc319`
- Product page: https://readystack.dev/products/kubeproxy
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=kubeproxy

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

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/kubeproxy/recipe. The complete kit is the release download. Each release is a tag here.
