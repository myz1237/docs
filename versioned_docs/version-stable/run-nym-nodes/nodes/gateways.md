---
sidebar_label: "Gateways"
description: "Gateways provide a destination for mixnet packets. Most of the internet doesn't use encrypted Sphinx packets, so the gateway acts as a destination for Sphinx traffic."
hide_title: false
title: Gateways
---

:::note
The Nym gateway was built in the [building nym](/docs/stable/run-nym-nodes/build-nym/) section. If you haven't yet built Nym and want to run the code, go there first.
:::

Gateways provide a destination for mixnet packets. Most of the internet doesn't use encrypted Sphinx packets, so the gateway acts as a destination, sort of like a mailbox, for messages.

Nym clients connect to gateways. Messages are automatically piped to connected clients and deleted from the gateway's disk storage. If a client is offline when a message arrives, it will be stored for later retrieval. When the client connects, all messages will be delivered, and deleted from the gateway's disk. As of release 0.8.x gateways use end-to-end encryption, so they cannot see the content of what they're storing for users.

When it starts up, a client registers itself with a gateway, and the gateway returns an access token. The access token plus the gateway's IP can then be used as a form of addressing for delivering packets.

The default gateway implementation included in the Nym platform code holds packets for later retrieval. For many applications (such as simple chat), this is usable out of the box, as it provides a place that potentially offline clients can retrieve packets from. The access token allows clients to pull messages from the gateway node.

### Initialising your gateway

You can check that your binaries are properly compiled with:

```
./nym-gateway --help
```

Which should return:

```

      _ __  _   _ _ __ ___
     | '_ \| | | | '_ \ _ \
     | | | | |_| | | | | | |
     |_| |_|\__, |_| |_| |_|
            |___/

             (gateway - version 1.0.0)

    
nym-gateway 1.0.0
Nymtech

Build Timestamp:    2022-04-27T15:11:59.359957641+00:00
Build Version:      1.0.0
Commit SHA:         a2313a457cf168833c28de202349e55df0106996
Commit Date:        2022-04-27T15:05:38+00:00
Commit Branch:      release/1.0.0
rustc Version:      1.60.0
rustc Channel:      stable
cargo Profile:      release

USAGE:
    nym-gateway <SUBCOMMAND>

OPTIONS:
    -h, --help
            Print help information

    -V, --version
            Print version information

SUBCOMMANDS:
    help
            Print this message or the help of the given subcommand(s)
    init
            Initialise the gateway
    node-details
            Show details of this gateway
    run
            Starts the gateway
    sign
            Sign text to prove ownership of this mixnode
    upgrade
            Try to upgrade the gateway

```

To check available configuration options use:

```
 ./nym-gateway init --help
```
Which will return the following: 

```
      _ __  _   _ _ __ ___
     | '_ \| | | | '_ \ _ \
     | | | | |_| | | | | | |
     |_| |_|\__, |_| |_| |_|
            |___/

             (gateway - version 1.0.0)

    
nym-gateway-init 
Initialise the gateway

USAGE:
    nym-gateway init [OPTIONS] --id <ID> --host <HOST> --wallet-address <WALLET_ADDRESS> --mnemonic <MNEMONIC>

OPTIONS:
        --announce-host <ANNOUNCE_HOST>
            The host that will be reported to the directory server

        --clients-port <CLIENTS_PORT>
            The port on which the gateway will be listening for clients gateway-requests

        --datastore <DATASTORE>
            Path to sqlite database containing all gateway persistent data

    -h, --help
            Print help information

        --host <HOST>
            The custom host on which the gateway will be running for receiving sphinx packets

        --id <ID>
            Id of the gateway we want to create config for

        --mix-port <MIX_PORT>
            The port on which the gateway will be listening for sphinx packets

        --mnemonic <MNEMONIC>
            Cosmos wallet mnemonic needed for double spending protection

        --validator-apis <VALIDATOR_APIS>
            Comma separated list of endpoints of the validators APIs

        --wallet-address <WALLET_ADDRESS>
            The wallet address you will use to bond this gateway, e.g.
            nymt1z9egw0knv47nmur0p8vk4rcx59h9gg4zuxrrr9
```

:::note
Users who have built the repository with `eth` features enabled will see additional flags output in their console. 
:::

The following command returns a gateway on your current IP with the `id` of `supergateway`:

```
./nym-gateway init --id supergateway --host $(curl ifconfig.me) --wallet-address <WALLET_ADDRESS> --mnemonic <MNEMONIC> 
```

The `$(curl ifconfig.me)` command above returns your IP automatically using an external service. Alternatively, you can enter your IP manually wish. If you do this, remember to enter your IP **without** any port information.

Gateways **must** also be capable of addressing IPv6, which is something that is hard to come by with many ISPs. Running a gateway from behind your router will be tricky because of this, and we strongly recommend to run your gateway on a VPS. Additional to IPv6 connectivity, this will help maintain better uptime and connectivity.

Users who have `eth` features enabled will have to add several flags to this command in order to initialise a gateway: 

```
./nym-gateway init --id supergateway --host $(curl ifconfig.me) --wallet-address <WALLET_ADDRESS> --eth-endpoint <ETH_ENDPOINT> --mnemonic <MNEMONIC>
```

Remember to bond your node via the Nym wallet, which can be downloaded [here](https://github.com/nymtech/nym/releases/). This is required for the blockchain to recognize your node and its software version, and include your gateway in the mixnet. 

### Running your gateway

The `run` command runs the gateway.
<!--Shall we use code tag in this command?-->
Example:

`./nym-gateway run --id supergateway`

Results in:

```
      _ __  _   _ _ __ ___
     | '_ \| | | | '_ \ _ \
     | | | | |_| | | | | | |
     |_| |_|\__, |_| |_| |_|
            |___/

             (gateway - version 1.0.0)

    
Starting gateway supergateway...

To bond your gateway you will need to install the Nym wallet, go to https://nymtech.net/get-involved and select the Download button.
Select the correct version and install it to your machine. You will need to provide the following: 
 
Identity Key: 6jWSJZsQ888jwzi1CBfkHefiDdUEjgwfeMfJU4RNhDuk
Sphinx Key: HbqYJwjmtzDi4WzGp7ehj8Ns394sRvJnxtanX28upon
Owner Signature: wRKxr1CnoyBB9AYPSaXgE4dCP757ffMz5gkja8EKaYR82a63FK9HYV3HXZnLcSaNXkzN3CJnxG2FREv1ZE9xwvx
Host: 62.240.134.46 (bind address: 62.240.134.46)
Version: 1.0.0
Mix Port: 1789, Clients port: 9000
Data store is at: "/home/mx/.nym/gateways/supergateway/data/db.sqlite"
 2022-04-27T16:04:33.831Z INFO  nym_gateway::node > Starting nym gateway!
 2022-04-27T16:04:34.268Z INFO  nym_gateway::node > Starting mix packet forwarder...
 2022-04-27T16:04:34.269Z INFO  nym_gateway::node > Starting mix socket listener...
 2022-04-27T16:04:34.269Z INFO  nym_gateway::node::mixnet_handling::receiver::listener > Running mix listener on "62.240.134.46:1789"
 2022-04-27T16:04:34.269Z INFO  nym_gateway::node                                      > Starting client [web]socket listener...
 2022-04-27T16:04:34.269Z INFO  nym_gateway::node                                      > Finished nym gateway startup procedure - it should now be able to receive mix and client traffic!
```

If you ever want to check the version details of your node, run:  

```
./nym-gateway --version 
```

This prints various bits of information about your node: 

```
      _ __  _   _ _ __ ___
     | '_ \| | | | '_ \ _ \
     | | | | |_| | | | | | |
     |_| |_|\__, |_| |_| |_|
            |___/

             (gateway - version 1.0.0)

    
nym-gateway 1.0.0

```

#### Configure your firewall

Although your gateway is now ready to receive traffic, your server may not be - the following commands will allow you to set up a properly configured firewall using `ufw`:

```
# check if you have ufw installed
ufw version
# if it is not installed, install with
sudo apt install ufw -y
# enable ufw
sudo ufw enable
# check the status of the firewall
sudo ufw status
```

Finally open your gateway's p2p port, as well as ports for ssh and incoming traffic connections:

```
sudo ufw allow 1789,22,9000/tcp
# check the status of the firewall
sudo ufw status
```

For more information about your gateway's port configuration, check the [gateway port reference table](#gateway-port-reference) below.

### Automating your gateway with systemd

Although it's not totally necessary, it's useful to have the gateway automatically start at system boot time. Here's a systemd service file to do that:

```ini
[Unit]
Description=Nym Gateway (1.0.0-rc.1)
StartLimitInterval=350
StartLimitBurst=10

[Service]
User=nym
LimitNOFILE=65536
ExecStart=/home/nym/nym-gateway run --id supergateway
KillSignal=SIGINT
Restart=on-failure
RestartSec=30

[Install]
WantedBy=multi-user.target
```

Put the above file onto your system at `/etc/systemd/system/nym-gateway.service`.

Change the path in `ExecStart` to point at your gateway binary (`nym-gateway`), and the `User` so it is the user you are running as.

If you have built nym on your server, and your username is `jetpanther`, then the start command might look like this:

`ExecStart=/home/jetpanther/nym/target/release/nym-gateway run --id your-id`. Basically, you want the full `/path/to/nym-gateway run --id whatever-your-node-id-is`

Then run:

```
systemctl enable nym-gateway.service
```

Start your node:

```
service nym-gateway start
```

This will cause your node to start at system boot time. If you restart your machine, the node will come back up automatically.

You can also do `service nym-gateway stop` or `service nym-gateway restart`.

Note: if you make any changes to your systemd script after you've enabled it, you will need to run:

```
systemctl daemon-reload
```

This lets your operating system know it's ok to reload the service configuration.

### Metrics 
This is currently only one metrics endpoint for the gateway. It can be accessed via `curl` like this: 

```
# For gateways on the Sandbox testnet
curl https://sandbox-validator.nymtech.net/api/v1/status/gateway/<GATEWAY_ID>/core-status-count
# For gateways on the Mainnet
curl https://validator.nymtech.net/api/v1/status/gateway/<GATEWAY_ID>/core-status-count
```

This endpoint returns the number of times that the gateway has been selected from the rewarded set and had 1000 packets sent to it, before being used by the network monitor to test the rest of the network. 

- `identity`: the identity key of the gateway. 
- `count`: the number of times it has been used for network testing. 

### Gateway port reference

All gateway-specific port configuration can be found in `$HOME/.nym/gateways/<your-id>/config/config.toml`. If you do edit any port configs, remember to restart your gateway.

| Default port | Use                       |
|--------------|---------------------------|
| 1789         | Listen for Mixnet traffic |
| 9000         | Listen for Client traffic |

