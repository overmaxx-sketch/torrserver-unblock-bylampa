# TorrServer Bylampa Unlock

An unofficial [TorrServer](https://github.com/YouROK/TorrServer) fork with a targeted compatibility patch for Bylampa.online. This repository publishes its own builds and releases; use the downloads from this repository, not the upstream release page.

> This project is not affiliated with or endorsed by the TorrServer upstream maintainers or Bylampa.online. The patch changes TorrServer's WAF `Referer` handling and cannot guarantee that third-party services remain available or compatible.

## What this fork changes

- **Bylampa compatibility patch:** removes the Bylampa-related WAF referer restriction from `server/web/waf/referer.go` during release preparation.
- **Automated upstream sync:** tracks stable upstream `MatriX.*` releases starting at `MatriX.144.1`.
- **Separate release tags:** each fork release uses the upstream version plus the `-bylampa` suffix, for example `MatriX.144.1-bylampa`.
- **Fork-specific builds:** release assets are built and published through this repository's GitHub Actions workflows.

The rest of the code is based on the upstream TorrServer project. This fork does not represent a separate TorrServer implementation.

## Download

Open the [Releases page](https://github.com/overmaxx-sketch/torrserver-unblock-bylampa/releases) and select a release with a tag ending in `-bylampa`. Download the asset that matches your operating system and CPU architecture. The files attached to a specific release are the source of truth for what is available in that version.

Do not use assets from the upstream TorrServer Releases page if you specifically need the Bylampa patch.

## Quick start

### Windows

1. Download the matching `TorrServer-windows-*.exe` asset from this repository's Releases page.
2. Run the downloaded executable.
3. On the same device, open [http://127.0.0.1:8090](http://127.0.0.1:8090) to access the web interface.

### Linux

Download the binary matching your CPU architecture. For example, for Linux on amd64:

```bash
chmod +x ./TorrServer-linux-amd64
./TorrServer-linux-amd64
```

Replace the filename with the exact asset you downloaded. If your distribution or architecture provides an AppImage release asset, you can use that instead.

### macOS

Download the `TorrServer-darwin-amd64` build for Intel Macs or `TorrServer-darwin-arm64` for Apple Silicon, then run the binary from Terminal. You may need to grant execute permission first:

```bash
chmod +x ./TorrServer-darwin-arm64
./TorrServer-darwin-arm64
```

Use the filename matching the downloaded asset. By default, the web interface is available at [http://127.0.0.1:8090](http://127.0.0.1:8090) on the device running TorrServer.

### Docker

This repository's Docker workflow builds images for `linux/amd64`, `linux/arm64`, and `linux/arm/v7` and publishes release-tagged images to GitHub Container Registry when the workflow succeeds.

1. Choose a real `-bylampa` tag from the [Releases page](https://github.com/overmaxx-sketch/torrserver-unblock-bylampa/releases).
2. Set `TAG` below to that exact tag, then run the commands:

```bash
# Replace this example with an existing tag from the Releases page.
TAG="MatriX.144.1-bylampa"
mkdir -p "$HOME/torrserver-data"
docker pull "ghcr.io/overmaxx-sketch/torrserver-unblock-bylampa:${TAG}"
docker run -d \
  --name torrserver \
  --restart unless-stopped \
  -p 8090:8090 \
  -v "$HOME/torrserver-data:/opt/ts" \
  "ghcr.io/overmaxx-sketch/torrserver-unblock-bylampa:${TAG}"
```

The example tag is illustrative; use a tag that actually appears on the Releases page and confirm that its container image has been published. Open [http://127.0.0.1:8090](http://127.0.0.1:8090) after the container starts. Keep the data volume when replacing the container so your persistent data is preserved.

Do not expose TorrServer directly to the public internet without appropriate authentication and network protection.

## Release assets and platforms

The workflows define builds for these targets. The exact files attached to each release may vary by version:

- **Windows:** amd64 and 386; a GStreamer-enabled amd64 build.
- **Linux:** amd64, arm64, ARMv7, ARMv5, 386, MIPS, MIPSel, MIPS64, MIPS64el, and RISC-V 64; AppImage builds are also produced for amd64, arm64, and ARMv7. GStreamer-enabled builds are produced for amd64 and arm64.
- **macOS:** Intel amd64 and Apple Silicon arm64; GStreamer-enabled builds for both architectures.
- **FreeBSD:** amd64 and ARMv7.
- **Android:** ARMv7, arm64, 386, and amd64 binaries for compatible environments.
- **iOS:** an XCFramework intended for embedding in a host application; it is not a standalone iOS app.
- **Docker:** `linux/amd64`, `linux/arm64`, and `linux/arm/v7`.

Some GStreamer-enabled builds have additional runtime requirements. Read the notes attached to the particular release before choosing one.

## Versioning and updates

Fork releases are based on upstream stable `MatriX.*` releases starting from `MatriX.144.1`. The fork's release tag adds `-bylampa`, for example `MatriX.144.1-bylampa`. The synchronization workflow applies the patch and publishes a separate Git tag and GitHub Release in this repository.

To update a direct installation, download and run the desired newer asset from this repository. For Docker, pull the new image tag and recreate the container while reusing the same data volume.

## Troubleshooting

- **The web interface does not open:** confirm the process or container is running and that port `8090` is available; open the address on the same device where TorrServer is running.
- **The binary will not start on Linux or macOS:** check that you selected the correct architecture and that the file has execute permission.
- **A release does not contain the asset you expect:** check the assets and release notes for that exact tag; not every target is necessarily published in every release.
- **Bylampa still does not work:** this patch only changes the matching TorrServer WAF referer restriction. It does not control Bylampa.online, network access, or other third-party requirements.

## Source and license

This fork is based on [YouROK/TorrServer](https://github.com/YouROK/TorrServer). See this repository's [LICENSE](https://github.com/overmaxx-sketch/torrserver-unblock-bylampa/blob/master/LICENSE) for licensing terms.
