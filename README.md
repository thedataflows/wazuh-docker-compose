# Wazuh deployment with Docker Compose

This repo will automate the deployment of a single-node [Wazuh](https://wazuh.com) instance with Docker or Containerd using `docker-compose` or `nerdctl` respectively.

## Prerequisites

- [mikefarah/yq](https://github.com/mikefarah/yq)
- [go-task](https://taskfile.dev)
- Optional: [mise-en-place](https://mise.jdx.dev/)
  - [./mise.run](https://mise.run) to download and install mise in the user directory
  - `mise up` will install locally the tools specified in the [mise.toml](mise.toml) file
- `docker` with `docker-compose` or `containerd` with `nerdctl` (with docker symlinked to it), recommended in rootlesskit mode

## Environment

Create an `.env` file in the project root containing the following (the passwords are a sample):

```ini
INDEXER_PASSWORD=IndexerPass
API_PASSWORD=MustBeALongerPassword123#
DASHBOARD_PASSWORD=DashboardPass
```

Optionally, you can set the following variables:

```ini
## WAZUH_MANAGER_PORTS is an array of localport:containerport, comma separated
WAZUH_MANAGER_PORTS=["11514:1514","11515:1515","11514:514/udp","55000:55000"]
## WAZUH_DASHBOARD_PORTS is an array of localport:containerport, comma separated
WAZUH_DASHBOARD_PORTS=["11443:5601"]
```

## Run

- List all tasks: `task`
- If you have set different passwords than the default ones, you must run:
  - `task wazuh:change_passwords` - will stop and start the Wazuh containers
  - Wait for the indexer to become ready
  - `task wazuh:post_change_passwords`
- Deploy Wazuh (re-create the containers): `task wazuh:up`
- Destroy Wazuh (but keep the volumes): `task wazuh:down`
- Start Wazuh: `task wazuh:start`
- Stop Wazuh: `task wazuh:stop`
- Watch logs: `task wazuh:logs`
