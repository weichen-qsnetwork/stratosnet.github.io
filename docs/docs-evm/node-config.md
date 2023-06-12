---
id: node-config
title: Node config
description: Stratos project description and whitepaper.
keywords:
  - docs
  - stratos
---

## Node configuration to enable EVM

In order to enable EVM feature on stratos chain node (fullnode/validator), edit your `<path_to_your_node>/config/app.toml` with the following:

```
[json-rpc]

# Enable defines if the gRPC server should be enabled.
enable = true
```