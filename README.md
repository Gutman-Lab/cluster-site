# cluster.drgutman.org

Static site for the GutmanLab compute and storage clusters: the main
`pmoxprime` Proxmox/Ceph cluster, the `kubertpmox` DR cluster, and the hardware
benchmark archive.

## Structure

```
index.html              Landing page: two clusters, headline results, field notes
proxmox.html            Proxmox primer + pmoxprime hardware, network, Ceph layout
benchmarks.html         Ceph rados bench + TrueNAS fio + Icarus tuning + caveats
kubertpmox.html         The nine-node all-flash DR cluster
notes/
  index.html            Note index
  ceph-benchmarks-deep-dive.html
  icarus-optimization.html
assets/site.css         Shared styling (no build step)
```

Plain HTML and CSS. No JavaScript, no framework, no build. Open `index.html`
directly or serve the directory:

```sh
python3 -m http.server 8080
```

## Content provenance

The benchmark and cluster material is adapted from the lab's internal
`GutmanLabDocumentation` repository (`public/frankencluster/` and
`src/pages/blog/`). Numbers are published as measured in September 2026 and are
labelled as a snapshot, not live telemetry. Current cluster facts (November
2026) are called out separately.

Do not add credentials, internal IP addresses, tenant data, or case metadata to
this repository. The site is public.

## Deployment

Target: `https://cluster.drgutman.org`, served from the lab gateway as a static
document root behind the existing Nginx Proxy Manager instance. The intended
host layout mirrors the `info.bdsa.io` pattern:

- document root: `/srv/cluster.drgutman.org/current` (atomic symlink)
- releases: `/srv/cluster.drgutman.org/releases/<UTC stamp>/`
- a small read-only `nginx:alpine` container on the Docker bridge
- one NPM proxy host for `cluster.drgutman.org`, HTTP→HTTPS, per-host Let's
  Encrypt certificate

Deployment is owned by DevOps/SysAdmin. A site deploy only replaces the static
tree; it does not touch other proxy hosts or the shared wildcard certificate.
