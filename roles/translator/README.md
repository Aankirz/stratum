# SV1 to SV2 Translator Proxy
This proxy is designed to sit in between a SV1 Downstream role (most typically Mining Device(s)
running SV1 firmware) and a SV2 Upstream role (most typically a SV2 Pool Server with Extended
Channel support).

The most typical high level configuration is:

```
<--- Most Downstream ----------------------------------------- Most Upstream --->

+---------------------------------------------------+  +------------------------+
|                     Mining Farm                   |  |      Remote Pool       |
|                                                   |  |                        |
|  +-------------------+     +------------------+   |  |   +-----------------+  |
|  | SV1 Mining Device | <-> | Translator Proxy | <------> | SV2 Pool Server |  |
|  +-------------------+     +------------------+   |  |   +-----------------+  |
|                                                   |  |                        |
+---------------------------------------------------+  +------------------------+

```

## Setup
### Configuration File
The `proxy-config.toml` is modified by the party that is running the Translator Proxy (most
typically the mining farm/miner hobbyist).

The configuration file contains the following information:

1. The SV2 Upstream connection information which includes the SV2 Pool authority public key 
   (`upstream_authority_pubkey`) and the SV2 Pool connection address (`upstream_address`) and port
   (`upstream_port`).
1. The SV1 Downstream connection information which includes the SV1 Mining Device address
   (`downstream_address`) and port (`downstream_port`).
1. The maximum and minimum SV2 versions (`max_supported_version` and `min_supported_version`) that
   the Translator Proxy implementer wants to support. Currently the only available version is `2`.
1. The desired minimum `extranonce2` size that the Translator Proxy implementer wants to use
   (`min_extranonce2_size`). The `extranonce2` size is ultimately decided by the SV2 Upstream role,
   but if the specified size meets the SV2 Upstream role's requirements, the size specified in this
   configuration file should be favored.
1. (NOT YET IMPLEMENTED) The estimated aggregate hashrate of all SV1 Downstream roles (Mining
   Devices) (`est_aggregate_hashrate`), which is communicated to the SV2 Upstream to help it decide
   a proper difficulty target.

### Run
1. Setup the `proxy-config.toml`.
1. Point the SV1 Downstream Mining Device(s) to the Translator Proxy IP address and port.
1. Run the Translator Proxy:

```
% cd roles/translator
% cargo run -p translator
```
