# ServiceFusion API Simulator Port Allocation

This document lists all ports used by simulators in this project to avoid conflicts.

|File | Service | Port |
|-----|---------|------|
| buffer.yml | Dynamic Buffer | 16111 |
| split-buffer.yml | Split Buffer | 16112 |
| Ssimple-state.yml | ShoppingCartService | 17079 |
| data-driven-magic.yml | MagicUsersService | 26415 |
| decisions/data-driven-where.yml | Decisions | 55513 |
| decisions/conditions-trigger.yml | Conditions Trigger | 22622 |
| delay-eval.yml | Delay Service | 15555 |
| dynamic-state.yml | Virtual Order Service | 14323 |
| v3 - CURL/Workspaces/workspaces.yml | Tosca Cloud Workspaces | 20001 |
| v3 - CURL/Playlists/playlist-search.yml | Tosca Cloud Playlists Search | 20002 |
| v3 - CURL/Executions/executions-search.yml | Tosca Cloud Executions Search | 20003 |
| v3 - CURL/Playlists/playlist-recent-runs.yml | Tosca Cloud Playlist Recent Runs | 20004 |
| v3 - CURL/Authorization/authorization.yml | Tosca Cloud Authorization | 20005 |
| v3 - CURL/Tenants/tenants.yml | Tosca Cloud Tenants | 20006 |
| v3 - CURL/Spaces/spaces.yml | Tosca Cloud Spaces | 20007 |
| v3 - CURL/Playlists/playlist-runs.yml | Tosca Cloud Playlist Runs | 20008 |
| v3 - CURL/Test cases/test-case-runs.yml | Tosca Cloud Test Case Runs | 20009 |
| v3 - CURL/Test cases/test-case-search.yml | Tosca Cloud Test Case Search | 20010 |
| v3 - CURL/Troubleshooting/troubleshooting-example.yml | Troubleshooting Example | 20011 |
| v3 - CURL/Test cases/test-case-create.yml | Tosca Cloud Test Case Creation | 20012 |
| v3 - CURL/Playlists/playlist-create.yml | Tosca Cloud Playlist Creation | 20013 |
| v3 - CURL/Integration/test-case-playlist-integration.yml | Test Case Playlist Integration | 20014 |
| v3 - CURL/Integration/product-catalog.yml | Product Catalog Service | 21100 |
| v2 - IRIS-to-Simulator/Organizations/organization.yml | Tosca Cloud Organization | 21001 |
| v2 - IRIS-to-Simulator/Executions/execution.yml | Tosca Cloud Executions | 21002 |
| v2 - IRIS-to-Simulator/Playlists/runs.yml | Tosca Cloud Runs | 21003 |
| v2 - IRIS-to-Simulator/Inventory/inventory.yml | Tosca Cloud Inventory | 21004 |
| v2 - IRIS-to-Simulator/Autorization/authorization.yml | Tosca Cloud Authorization | 21005 |

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
