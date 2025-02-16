# Wormhole v2 Mainnet

Wormhole mainnet connects the following chains:

- Solana [Mainnet Beta](https://docs.solana.com/clusters#mainnet-beta).

- Ethereum Mainnet.

- Terra Columbus mainnet.

- Binance Smart Chain mainnet.

It runs the [v2.5.0](https://github.com/certusone/wormhole/releases/tag/v2.5.0) guardiand reference implementation.

## Network parameters

Gossip network name:

    /wormhole/mainnet/2

Gossip bootstrap node:

    /dns4/wormhole-mainnet-v2-bootstrap.certus.one/udp/8999/quic/p2p/12D3KooWL6xoqY8yU2xR2K6cP6jix4LnGSrRh94HCKiK371qUFeU

Connected chain contracts:

| Network                       | Wormhole core contract address                 |
|-------------------------------|------------------------------------------------|
| Ethereum Mainnet (Core)     | [`0x98f3c9e6E3fAce36bAAd05FE09d375Ef1464288B`](https://etherscan.io/address/0x98f3c9e6E3fAce36bAAd05FE09d375Ef1464288B)  |
| Ethereum Mainnet (Impl)       | [`0x736d2a394f7810c17b3c6fed017d5bc7d60c077d`](https://etherscan.io/address/0x736d2a394f7810c17b3c6fed017d5bc7d60c077d)  |
| Binance Smart Chain (Core)  | [`0x98f3c9e6E3fAce36bAAd05FE09d375Ef1464288B`](https://bscscan.com/address/0x98f3c9e6E3fAce36bAAd05FE09d375Ef1464288B)   |
| Binance Smart Chain (Impl)    | [`0x736d2a394f7810c17b3c6fed017d5bc7d60c077d`](https://bscscan.com/address/0x736d2a394f7810c17b3c6fed017d5bc7d60c077d)   |
| Solana Mainnet                | [`worm2ZoG2kUd4vFXhvjh93UUH596ayRfgQ2MgjNMTth`](https://explorer.solana.com/address/worm2ZoG2kUd4vFXhvjh93UUH596ayRfgQ2MgjNMTth) |
| Terra Columbus-5              | [`terra1dq03ugtd40zu9hcgdzrsq6z2z4hwhc9tqk2uy5`](https://finder.terra.money/columbus-5/address/terra1dq03ugtd40zu9hcgdzrsq6z2z4hwhc9tqk2uy5) |
| Polygon Mainnet (Core)        | [`0x7A4B5a56256163F07b2C80A7cA55aBE66c4ec4d7`](https://polygonscan.com/address/0x7A4B5a56256163F07b2C80A7cA55aBE66c4ec4d7#code) |
| Polygon Mainnet (Impl)        | [`0xd2F50cAFfCaf238a0C3e78Ce81c5BF7D9f7f3350`](https://polygonscan.com/address/0xd2F50cAFfCaf238a0C3e78Ce81c5BF7D9f7f3350#code) |

Eth and BSC use the same deployer key, leading to identical addresses. This key has no privileges.

## Guardian set

Current generation: **1** (see [v1.prototext](guardianset/v1.prototxt)).

## Example command line options

Refer to the [operations guide](https://github.com/certusone/wormhole/blob/dev.v2/docs/operations.md) on how to set up a node.
Example systemd unit file:

```
[Unit]
Description=wormhole v2 guardiand
Requires=network.target
After=network.target

[Service]
User=wormhole
Group=wormhole
ExecStart=/opt/wormhole/wormhole/build/bin/guardiand node \
    --bootstrap "/dns4/wormhole-mainnet-v2-bootstrap.certus.one/udp/8999/quic/p2p/12D3KooWL6xoqY8yU2xR2K6cP6jix4LnGSrRh94HCKiK371qUFeU" \
    --network "/wormhole/mainnet/2" \
    --ethContract "0x98f3c9e6E3fAce36bAAd05FE09d375Ef1464288B" \
    --bscContract "0x98f3c9e6E3fAce36bAAd05FE09d375Ef1464288B" \
    --polygonContract "0x7A4B5a56256163F07b2C80A7cA55aBE66c4ec4d7" \
    --solanaContract "worm2ZoG2kUd4vFXhvjh93UUH596ayRfgQ2MgjNMTth" \
    --terraContract "terra1dq03ugtd40zu9hcgdzrsq6z2z4hwhc9tqk2uy5" \
    --adminSocket /run/guardiand/admin.socket \
    --dataDir /opt/wormhole/data \
    --nodeName "<your name>" \                                  # <-- your node's name (for network explorer usage)
    --nodeKey "/opt/wormhole/keys/wormhole-node.key" \          # <-- node key (auto-generated if not present)
    --guardianKey "/opt/wormhole/keys/wormhole-guardian.key" \  # <-- your guardian key generated by "guardiand keygen"
    --ethRPC "wss://your-eth-node" \                            # <-- your ETH full/light node websocket URI (ws:// or wss://)
    --bscRPC "wss://bsc-node.example.com" \                     # <-- your BSC full/light node websocket URI
    --polygonRPC "wss://polygon-node.example.com" \             # <-- your Polygon Bor full/light node websocket URI
    --solanaRPC "http://solana-node:8899" \                     # <-- Solana RPC URI
    --solanaWS "ws://solana-node:8900" \                        # <-- Solana WS URI (typically RPC +1)
    --terraWS "ws://terra-node/websocket" \                     # <-- Terra node websocket URI
    --terraLCD "http://terra-node:1317" \                       # <-- Terra LCD server HTTP URI
    --statusAddr=[::]:6060                                      # <-- exposes Prometheus metrics - firewall recommended
RuntimeDirectory=guardiand
RuntimeDirectoryMode=700
RuntimeDirectoryPreserve=yes
PermissionsStartOnly=yes
PrivateTmp=yes
PrivateDevices=yes
SecureBits=keep-caps
AmbientCapabilities=CAP_IPC_LOCK
CapabilityBoundingSet=CAP_IPC_LOCK
NoNewPrivileges=yes
Restart=on-failure
RestartSec=5s
LimitNOFILE=65536
LimitMEMLOCK=infinity

[Install]
WantedBy=multi-user.target
```
