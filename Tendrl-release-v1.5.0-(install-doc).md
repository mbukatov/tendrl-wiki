- **Latest packages**: https://copr.fedorainfracloud.org/coprs/tendrl/release/packages/
- **Latest dependencies**: https://copr.fedorainfracloud.org/coprs/tendrl/dependencies/packages/
- **Install**: https://github.com/Tendrl/documentation/wiki/Tendrl-Package-Installation-Reference(Revised)


**Pre-requisites (mandatory):**
- tendrl-release v1.5.0 is not backward compatible with central store for tendrl-release v1.4.x or v1.3.x, please use a new central store (etcd) instance, old data will not be migrated.
- The Node hosting tendrl-api/central_store should have minimum 12 GB of memory and 4 VCPUs (or equivalent)(due to alerts, logs being stored on this node)