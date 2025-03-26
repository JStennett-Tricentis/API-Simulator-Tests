# ServiceFusion API Simulator Port Allocation

This document lists all ports used by simulators in this project to avoid conflicts.

| File | Service | Port |
|------|---------|------|
| `./Workspaces/workspaces.yml` | Tosca Cloud Workspaces | 20001 |
| `./Playlists/playlist-search.yml` | Tosca Cloud Playlists Search | 20002 |
| `./Executions/executions-search.yml` | Tosca Cloud Executions Search | 20003 |
| `./Playlists/playlist-recent-runs.yml` | Tosca Cloud Playlist Recent Runs | 20004 |
| `./Authorization/authorization.yml` | Tosca Cloud Authorization | 20005 |
| `./Tenants/tenants.yml` | Tosca Cloud Tenants | 20006 |
| `./Spaces/spaces.yml` | Tosca Cloud Spaces | 20007 |
| `./Playlists/playlist-runs.yml` | Tosca Cloud Playlist Runs | 20008 |
| `./Test cases/test-case-runs.yml` | Tosca Cloud Test Case Runs | 20009 |
| `./Test cases/test-case-search.yml` | Tosca Cloud Test Case Search | 20010 |
| `./Troubleshooting/troubleshooting-example.yml` | Troubleshooting Example | 20011 |
| `./Test cases/test-case-create.yml` | Tosca Cloud Test Case Creation | 20012 |
| `./Playlists/playlist-create.yml` | Tosca Cloud Playlist Creation | 20013 |
| `./Integration/test-case-playlist-integration.yml` | Test Case Playlist Integration | 20014 |
| `./Integration/product-catalog.yml` | Product Catalog Service | 21100 |

## Port Range Allocation Guidelines

For future development, please allocate ports within the following ranges based on the module type:

- **Async Services**: 13000-13999
- **Dynamic State**: 14000-14999
- **Delay Services**: 15000-15999
- **Buffers**: 16000-16999
- **SharedState**: 17000-17999
- **DataMagic**: 20000-29999

When creating a new simulator, please:

1. Check this document first to avoid port conflicts
2. Update this document when allocating a new port
3. Use the designated port range for the module type
4. Increment the last used port in the range by 1 when adding a new service
