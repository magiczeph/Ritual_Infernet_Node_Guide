<div align="center">

#  👨🏻‍💻 **Ritual Infernet Node Guide** 👨🏻‍💻

</div>


# -Device/System Requirements 💻

![image](https://github.com/user-attachments/assets/76006807-e83d-4c33-9d9d-8f7c95c8cdeb)


# Pre-Requirements 🛠

-Docker Desktop

-Evm wallet with minimum 5$ of BASE ETH

-Base Mainnet Rpc URL from ALCHEMY 

-Installations of Apt packages

```
sudo apt update && sudo apt upgrade -y
```

```
sudo apt -qy install curl git jq lz4 build-essential screen
```

# Cloning The Starter Repository

```
git clone https://github.com/ritual-net/infernet-container-starter
```

Navigate to the Repository

```
cd infernet-container-starter
```


# Running hello-world container

🔺before applying this command start the Docker in background

```
project=hello-world make deploy-container
```

![image](https://github.com/user-attachments/assets/82572d5f-86f9-4cca-aa85-164b8313c5ac)


# Edit Node Configuration Files

1️⃣)- Will change config for this file - `~/infernet-container-starter/deploy/config.json` 

1.1) Run this command 

```
rm ~/infernet-container-starter/deploy/config.json && nano ~/infernet-container-starter/deploy/config.json
```

1.2) Paste this full code given below

1.3) Replace `ENTER_API_KEY_FROM_ALCHEMY` with your actual API key!

1.4) Replace `ENTER_YOUR_PVT_KEYS` with your actual EVM wallet PVT key.. add (0x) before the key!

1.5) `ctrl+x` , `Y` + `Enter` to save this!

```
{
    "log_path": "infernet_node.log",
    "server": {
        "port": 4000,
        "rate_limit": {
            "num_requests": 100,
            "period": 100
        }
    },
    "chain": {
        "enabled": true,
        "trail_head_blocks": 3,
        "rpc_url": "ENTER_API_KEY_FROM_ALCHEMY",
        "registry_address": "0x3B1554f346DFe5c482Bb4BA31b880c1C18412170",
        "wallet": {
          "max_gas_limit": 4000000,
          "private_key": "ENTER_YOUR_PVT_KEYS",
          "allowed_sim_errors": []
        },
        "snapshot_sync": {
          "sleep": 3,
          "batch_size": 500,
          "starting_sub_id": 240000,
          "sync_period": 30
        }
    },
    "startup_wait": 1.0,
    
    "redis": {
        "host": "redis",
        "port": 6379
    },
    "forward_stats": true,
    "containers": [
        {
            "id": "hello-world",
            "image": "ritualnetwork/hello-world-infernet:latest",
            "external": true,
            "port": "3000",
            "allowed_delegate_addresses": [],
            "allowed_addresses": [],
            "allowed_ips": [],
            "command": "--bind=0.0.0.0:3000 --workers=2",
            "env": {},
            "volumes": [],
            "accepted_payments": {},
            "generates_proofs": false
        }
    ]
}
```



2️⃣)- Will change config for this file - `~/infernet-container-starter/projects/hello-world/container/config.json` 

2.1) Run this command 

```
rm ~/infernet-container-starter/projects/hello-world/container/config.json && nano ~/infernet-container-starter/projects/hello-world/container/config.json
```

2.2) Paste this full code given below

2.3) Replace `ENTER_API_KEY_FROM_ALCHEMY` with your actual API key!

2.4) Replace `ENTER_YOUR_PVT_KEYS` with your actual EVM wallet PVT key.. add (0x) before the key!

2.5) `ctrl+x` , `Y` + `Enter` to save this!



```
{
    "log_path": "infernet_node.log",
    "server": {
        "port": 4000,
        "rate_limit": {
            "num_requests": 100,
            "period": 100
        }
    },
    "chain": {
        "enabled": true,
        "trail_head_blocks": 3,
        "rpc_url": "ENTER_API_KEY_FROM_ALCHEMY",
        "registry_address": "0x3B1554f346DFe5c482Bb4BA31b880c1C18412170",
        "wallet": {
          "max_gas_limit": 4000000,
          "private_key": "ENTER_YOUR_PVT_KEYS",
          "allowed_sim_errors": []
        },
        "snapshot_sync": {
          "sleep": 3,
          "batch_size": 500,
          "starting_sub_id": 240000,
          "sync_period": 30
        }
    },
    "startup_wait": 1.0,
   
    "redis": {
        "host": "redis",
        "port": 6379
    },
    "forward_stats": true,
    "containers": [
        {
            "id": "hello-world",
            "image": "ritualnetwork/hello-world-infernet:latest",
            "external": true,
            "port": "3000",
            "allowed_delegate_addresses": [],
            "allowed_addresses": [],
            "allowed_ips": [],
            "command": "--bind=0.0.0.0:3000 --workers=2",
            "env": {},
            "volumes": [],
            "accepted_payments": {},
            "generates_proofs": false
        }
    ]
}
```



3️⃣)- Will change config for this file - `~/infernet-container-starter/projects/hello-world/contracts/script/Deploy.s.sol` 

3.1) Run this command 

```
rm ~/infernet-container-starter/projects/hello-world/contracts/script/Deploy.s.sol && nano ~/infernet-container-starter/projects/hello-world/contracts/script/Deploy.s.sol
```

3.2) Paste this full code given below

3.3) `ctrl+x` , `Y` + `Enter` to save this!


```
// SPDX-License-Identifier: BSD-3-Clause-Clear
pragma solidity ^0.8.13;

import {Script, console2} from "forge-std/Script.sol";
import {SaysGM} from "../src/SaysGM.sol";

contract Deploy is Script {
    function run() public {
        // Setup wallet
        uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");
        vm.startBroadcast(deployerPrivateKey);

        // Log address
        address deployerAddress = vm.addr(deployerPrivateKey);
        console2.log("Loaded deployer: ", deployerAddress);

        address registry = 0x3B1554f346DFe5c482Bb4BA31b880c1C18412170;
        // Create consumer
        SaysGM saysGm = new SaysGM(registry);
        console2.log("Deployed SaysHello: ", address(saysGm));

        // Execute
        vm.stopBroadcast();
        vm.broadcast();
    }
}
```




4️⃣)- Will edit Makefile - `~/infernet-container-starter/projects/hello-world/contracts/Makefile` 

4.1) Run this command 

```
rm ~/infernet-container-starter/projects/hello-world/contracts/Makefile && nano ~/infernet-container-starter/projects/hello-world/contracts/Makefile
```

4.2) Paste this full code given below

4.3) Replace `ENTER_YOUR_PVT_KEYS` with your actual EVM wallet PVT key.. add (0x) before the key!

4.4) Replace `ENTER_API_KEY_FROM_ALCHEMY` with your actual API key!

4.5) `ctrl+x` , `Y` + `Enter` to save this!


```
# phony targets are targets that don't actually create a file
.phony: deploy

# anvil's third default address
sender := ENTER_YOUR_PVT_KEYS
RPC_URL := ENTER_API_KEY_FROM_ALCHEMY

# deploying the contract
deploy:
	@PRIVATE_KEY=$(sender) forge script script/Deploy.s.sol:Deploy --broadcast --rpc-url $(RPC_URL)

# calling sayGM()
call-contract:
	@PRIVATE_KEY=$(sender) forge script script/CallContract.s.sol:CallContract --broadcast --rpc-url $(RPC_URL)
```









