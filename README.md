# Cloudanix Helm charts

Helm repository for the Cloudanix container protection chart (`cloudanix`), served at
`https://cloudanix.github.io/charts`.

## Install

```bash
helm repo add cloudanix https://cloudanix.github.io/charts
helm repo update
helm search repo cloudanix

helm upgrade --install cloudanix cloudanix/cloudanix \
  --namespace cloudanix --create-namespace \
  --set authToken="<auth_token>" \
  --set accountId="<account_id>" \
  --set clusterIdentifier="<cluster_identifier>" \
  --set clusterName="<cluster_name>" \
  --set clusterDomain="<cluster_domain>"
```

The Cloudanix console shows this command with your values filled in. The chart's README
(`helm show readme cloudanix/cloudanix`) covers capabilities, custom labels, and the Falco and
Cilium options.

## Verify the chart

Each chart version has a SHA256 checksum file, `cloudanix-<version>.tgz.sha256`, next to the
chart in this repository. The same file is attached as `SHA256SUMS` to the
[release](https://github.com/Cloudanix/charts/releases) named `cloudanix-<version>`. Check the
file you install against it:

```bash
VERSION=2.3.1
helm pull cloudanix/cloudanix --version "$VERSION"
curl -fsSLO "https://github.com/Cloudanix/charts/releases/download/cloudanix-$VERSION/SHA256SUMS"
sha256sum -c SHA256SUMS          # macOS: shasum -a 256 -c SHA256SUMS
# cloudanix-2.3.1.tgz: OK

helm install cloudanix ./cloudanix-"$VERSION".tgz --namespace cloudanix --create-namespace -f values.yaml
```

- From this site instead of the release:
  `curl -fsSLO https://cloudanix.github.io/charts/cloudanix-$VERSION.tgz.sha256 && sha256sum -c cloudanix-$VERSION.tgz.sha256`.
  It sits on the same host as the chart, as does the `digest` in
  [`index.yaml`](https://cloudanix.github.io/charts/index.yaml), so it shows an intact download.
  The release is the copy from a second place.
- Air-gapped installs: mirror the `.tgz` and its `.sha256` together, and verify after the copy.
- A published version is never rebuilt or replaced. If the check fails, do not install it;
  contact Cloudanix support.
- Checksum releases start at 2.2.0. For an earlier version, compare against its `index.yaml`
  digest, or ask us to publish its checksum.
