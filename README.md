<h1 align="center">aac-webgrab</h1>

<p align="center">Self-hosted deployment definition for WebGrab+Plus, an EPG (electronic programme guide) data grabber.</p>

<p align="center">
  <a href="https://webgrabplus.com"><img alt="Upstream" src="https://img.shields.io/badge/upstream-webgrabplus.com-blue"></a>
  <img alt="Image" src="https://img.shields.io/badge/image-lscr.io%2Flinuxserver%2Fwebgrabplus%3A5.5.3--ls289-2496ED?logo=docker&logoColor=white">
  <img alt="License" src="https://img.shields.io/badge/license-proprietary%20freeware-lightgrey">
</p>

## About

WebGrab+Plus (WG++) is an EPG data grabber. It downloads television programme information
from configured web sources and writes the result to data files, which other tools can then
import. This repository does not contain the application; it describes how to run the
LinuxServer.io container image of WebGrab+Plus with App-as-Code.

Key points:

- Runs as a container with separate volumes for configuration and generated data
- Configurable user/group IDs and time zone
- Debug logging can be switched on through an environment variable

**License notice:** WebGrab+Plus is distributed by its developer as *freeware, closed source*,
without published source code (see the upstream FAQ). Its licence terms are set by the
upstream developer, and some features require a paid donator status. The software is
not open source and is not covered by any open-source licence. The license of this repository
covers only the deployment definition.

## Upstream

- Project and download: <https://webgrabplus.com>
- Container image: `lscr.io/linuxserver/webgrabplus:5.5.3-ls289` (packaged by LinuxServer.io)
- Image documentation: <https://docs.linuxserver.io/images/docker-webgrabplus/>
- License: proprietary freeware, closed source (upstream developer)

## Quick start (Docker Compose)

The Compose file is rendered from `service.yml` by the App-as-Code template engine. For a
manual run, start the container:

```bash
docker compose up -d
```

If the service is routed through Traefik, the external Docker networks used by your reverse
proxy must exist first (the reference deployments use `services-secured` and `services-exposed`).

WebGrab+Plus stores its configuration in the `config` volume. Configure the grabber through
the files there, following the upstream documentation.

## Configuration

Environment variables (defaults from `service.yml`). No secrets are defined for this service.

| Variable | Default | Description |
|---|---|---|
| `PUID` | `1000` | User ID the container runs as |
| `PGID` | `1000` | Group ID the container runs as |
| `TZ` | `Europe/Berlin` | Time zone |
| `WEBGRABPLUS_DEBUG` | `true` | Enable debug output of the grabber |

Ports:

| Port | Protocol | Purpose |
|---|---|---|
| `80` | TCP | Declared in `service.yml` as `web`. The upstream image documentation does not state a web interface port |

Volumes:

| Mount | Description |
|---|---|
| `config` -> `/config` | WebGrab+Plus configuration files |
| `data` -> `/data` | Output directory for generated EPG data |

## Deploy with App-as-Code

1. Edit `service.yml`. It is the single source of truth for image, ports, environment,
   volumes and per-stage overrides. Do not edit the rendered output.
2. Pushing to `main` (or a configured stage branch) runs the service pipeline, which renders
   the manifests with `aac-template-engine` and triggers the deployment.
3. Releases of this definition are published under <https://github.com/app-as-code/aac-webgrab>.

## Credits and license

WebGrab+Plus is developed by its upstream developer and is proprietary freeware with closed
source. This repository only contains the deployment definition and is not a fork of the
application. No icon is shipped with this repository because no suitable, verifiably licensed
logo was found in the icon collections.
