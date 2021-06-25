---
title: Configuration
id: configuration
---

Bee is a very flexible program, and can be configured in a variety of
ways depending on your use case. This expanded section will cover
configuration in detail.

## Important Configuration Changes

:::important
Before starting Bee for the first time, there is *some* configuration
to do! Make sure you consider updating the following recommended
settings!
:::

### Full Node

By default, Bee runs as a [light
node](/docs/access-the-swarm/light-nodes). To fully participate in
the swarm, you must set the `--full-node` option to `true`.

### Swap Endpoint

In order to access the blockchain, your Bee must be connected to an XDAI blockchain node on the XDAI network. We recommend running your own XDAI Node, but if you prefer, you may also sign up to [getblock.io](https://getblock.io) API service and set your url to be `https://stake.getblock.io/mainnet/?api_key=your-api-key`

### NAT Address

To enable others to connect to your node, you must broadcast your
public IP and ensure Bee is accessible on the correct p2p port
(default `1634`). We recommend that you [manually configure your
external IP and check connectivity](/docs/installation/connectivity)
to ensure that your Bee is able to receive inbound connections from
other peers.

First determine your public IP address.

```bash
curl icanhazip.com
```

```bash
123.123.123.123
```

Then configure your node, including your p2p port (default `1634`).

```yaml
nat-addr: "123.123.123.123:1634"
```

### LevelDB Open File Descriptors Limit

Bee is designed to work on a lot of different hardware. To facilitate
the exploration of this, we have given node
operators access to leveldb's `--db-open-files-limit`. This influences
the speed with which Bee can read and write to its database, and
therefore its performance in forwarding and serving chunks. Some say
setting this to much more than the default 200 leads to a much
enhanced ability to participate in the swarm and get those BZZs!
Share your experience in the #node-operators channel of our [Discord
server](https://discord.gg/wdghaQsGq5) to help us make this process
more automated in the future!

### ENS Endpoint

The ENS domain resolution system is used to host websites on Bee, and in order to use this your Bee must be connected to an Ethereum blockchain node on the main network. If you would like to [browse the swarm](/docs/access-the-swarm/browse-the-swarm) We recommend you sign up to [Infura's](https://infura.io) API service and set your `--resolver-options=https://mainnet.infura.io/v3/your-api-key`.

## Specifying Configuration

### Configuration Priority

Configuration is processed in the following ascending order of preference:

1. Command Line Arguments
2. Environment Variables
3. Configuration File

### Command Line Arguments

Run `bee start --help` in your Terminal to get the list of available command line arguments.

### Environment variables

Bee config may also be passed using environment variables.

Environment variables are set as variables in your operating system's
session or systemd configuration file. To set an environment variable,
type the following in your terminal session.

```bash
export VARIABLE_NAME=variableValue
```

Verify if it is correctly set by running `echo $VARIABLE_NAME`.

All available configuration options are available as `BEE` prefixed,
capitalised, and underscored environment variables.

e.g. `--api-addr` becomes `BEE_API_ADDR`.


### Configuration file
Bee can also be configured by providing a YAML configuration file using the `--config` flag.

```bash
bee start --config /home/<user>/bee-config.yaml 
```

#### Automatically generate a config file

Configuration files can be easily generated by simply substituting the `start` command with `printconfig` when starting Bee using the command line.

```bash
bee printconfig &> bee-config.yaml
```

This produces the following file contents, showing the default
configuration of Bee:

```yaml
api-addr: :1633
block-hash: ""
block-time: "15"
bootnode: []
bootnode-mode: false
cache-capacity: "1000000"
clef-signer-enable: false
clef-signer-endpoint: ""
clef-signer-ethereum-address: ""
config: /home/user/.bee.yaml
cors-allowed-origins: []
data-dir: /home/user/.bee
db-block-cache-capacity: "33554432"
db-disable-seeks-compaction: false
db-open-files-limit: "200"
db-write-buffer-size: "33554432"
debug-api-addr: :1635
debug-api-enable: false
full-node: false
gateway-mode: false
global-pinning-enable: false
help: false
mainnet: false
nat-addr: ""
network-id: "10"
p2p-addr: :1634
p2p-quic-enable: false
p2p-ws-enable: false
password: ""
password-file: ""
payment-early: "10000000"
payment-threshold: "100000000"
payment-tolerance: "100000000"
postage-stamp-address: ""
price-oracle-address: ""
resolver-options: []
standalone: false
swap-deployment-gas-price: ""
swap-enable: true
swap-endpoint: ws://localhost:8546
swap-factory-address: ""
swap-initial-deposit: "10000000000000000"
swap-legacy-factory-addresses: []
tracing-enable: false
tracing-endpoint: 127.0.0.1:6831
tracing-service-name: bee
transaction: ""
verbosity: "5"
warmup-time: 20m0s
welcome-message: ""
```


## Configuring Bee Installed Using a Package Manager

Bee node's installed using package managers `apt-get` or `yum` are
configured using a configuration file which is automatically generated
during the installation process.

To alter Bee's configuration, simply edit the configuration file as
desired, and restart your Bee node using `systemctl`.

### Linux

```bash
sudo vi /etc/bee/bee.yaml
sudo systemctl restart bee
```

### MacOS

```bash
vi /usr/local/etc/swarm-bee/bee.yaml
brew services restart swarm-bee
```

## Configuration Options

Bee provides the following options to customise your node.

### Global

#### --config

*default* `/home/<user>/.bee.yaml`

The location of a YAML configuration file containing configuration options. See [configuration](#configuration-file).

### Start

#### --api-addr

*default* :1633

The IP and port the API will serve HTTP requests from. Omitting the IP
part of the address will cause the server to listen to all
interfaces. Argument values are of the form '132.132.132.132:1633'.

#### --block-time

*default* 15

The expected block time of the attached SWAP endpoint.

#### --bootnode

*default* `/dnsaddr/bootnode.ethswarm.org`

This is a [multiaddr](https://github.com/multiformats/multiaddr)
specifying the Bee bootnodes used for bootstrapping the network. It
can be multiple values.

By default a node connects to the Swarm mainnet.  When using a private or test network, network specific bootnodes must be set. 

Any Bee node in a network can act as a bootnode.

#### --cache-capacity

*default* `1000000`

The amount of disk space, in chunks, that is used for forwarding and uploading chunks.

#### --clef-signer-enable             

*default* `false`

Set this to true to enable signing using Ethereum's Clef external
signer. Clef is a new feature which requires a corresponding rules
files or running in advanced mode to allow for auto-signing of
handshakes and cheques.

#### --clef-signer-endpoint

*default* **default path for clef for each host operating system**

You may also specify a custom IPC file path for your Clef signer.

#### --clef-signer-ethereum-address

*default* **selects the clef address at index 0**

Use this command to specify which Bee Clef address is used if you have imported multiple keys into Bee Clef.

:::warning
If you have multiple addresses imported into your instance of Bee Clef, you must specify your address for each node, including the first one, as addresses may been re-ordered during import.
:::

#### --cors-allowed-origins

*default* `[]`

HTTP/WS origin domains or wildcards defining these, which the API will
allow responses to, e.g.

```bash
bee start --cors-allowed-origins="*"
bee start --cors-allowed-origins="https://website.ethswarm.org"
```

#### --data-dir

*default* `/home/<user>/.bee`

The location on your disk where Bee stores its data. Data in this
directory will be required to restore a node state using the same key.

This consists of the following three types of data.


##### 1. Chunk Data (localstore)

This consists of chunks and files that you have pinned locally, cached chunks you have requested, or chunks within your radius of responsibility which you are responsible for serving to your peers.

##### 2. State Data (statestore)

This is information about the local state of your Bee node and should be backed up.

##### 3. Keystore Data (keys)

These files contain encrypted versions of your private key and should be backed up and kept private.

:::danger
Keep the key files in your keystore data directory safe! 

They are the cryptographic proof of your network identity and cannot be recovered.
:::

*The next four options expose low-level configurations for
[LevelDB](https://pkg.go.dev/github.com/syndtr/goleveldb@v1.0.0/leveldb/opt#Options)'s
 [Openfile](https://pkg.go.dev/github.com/syndtr/goleveldb@v1.0.0/leveldb#OpenFile)
method. Please let us know how you get on with tweaking these settings
on your hardware in the
[#node-operators](https://discord.gg/X3ph5yGRFU) channel on our
[Discord server](https://discord.gg/wdghaQsGq5)*

#### --db-block-cache-capacity
*default* `33554432`

Corresponds to LevelDB `BlockCacheCapacity` (see above)

#### --db-disable-seeks-compaction
*default* `false`

Corresponds to LevelDB `DisableSeeksCompaction` (see above)

#### --db-open-files-limit
*default* `200`

:::info
To accomodate less powerful hardware and operating systems, the `db-open-files-limit` is set deliberately low. We recommend that you try to increase it to nearer to `10000` or more if this is possible when using your hardware. *Please let us know how you get on with tweaking these settings on your hardware in the [#node-operators](https://discord.gg/X3ph5yGRFU) channel on our [Discord server](https://discord.gg/wdghaQsGq5)*
:::

Corresponds to LevelDB `OpenFilesCacheCapacity` (see above)

#### --db-write-buffer-size
*default* `33554432`

Corresponds to LevelDB `WriteBuffer` (see above)

#### --debug-api-addr

*default* `:1635`

The IP and port the [Debug API](/docs/api-reference/api-reference)
will serve HTTP requests from.

Omitting the IP part of the address will cause the server to listen to
all requests.

`--debug-api-enable` must be set to `true`.

#### --debug-api-enable

*default* `false`

Set this to `true` to enable access to the [Debug API](/docs/api-reference/api-reference)

#### --full-node

*default* false

Enable this by setting it to `true` to fully participate in serving and forwarding chunks to the network.

#### --gateway-mode 

*default* `false`

Set this to `true` to disable a set of sensitive features in the API
to ensure that it is safe to expose your `api-addr` to the public
Internet.

#### --global-pinning-enable

*default* `false`

Enables the Global Pinning functionality when set to `true`.

#### --mainnet

*default* `false`

#### --nat-addr

*default* `""`

Sets the expected public IP. Normally this is generated automatically, but in certain circumstances it may be desirable to set it manually.

#### --network-id

*default* `10`

The network ID for which to accept new connections. Set to 1 for mainnet, 10 for testnet.

#### --p2p-addr

*default* `:1634`

The IP and port to listen for p2p protocol messages.

#### --p2p-quic-enable

*default* `false`

#### --p2p-ws-enable

*default* `false`

Enables web-sockets transport for p2p communications.

#### --password

*default* `""`

Password used to decrypt Swarm identity keys. 

:::danger
Passing passwords as command line arguments is insecure. Use a password file or environment variable in production environments.
:::

#### --password-file

*default* `""`

The path to a file that contains password for decrypting keys. The empty string assumes no file is presented.

#### --payment-early

*default* `1000000000000`

Amount in BZZ below the peers payment threshold which causes Bee to
initiate settlement.

#### --payment-threshold

*default* `10000000000000`

The threshold in BZZ where you expect to get paid from your peers.

#### --payment-tolerance

*default* `10000000000000`

The excess debt above payment threshold in gBZZ where you disconnect from your peer.

#### --postage-stamp-address

*default* *automatically configured depending on network*

The address of the postage stamp contract on the Ethereum blockchain, used for buying batches of stamps.

#### --resolver-options 

*default* eth:0x00000000000C2E074eC69A0dFb2997BA6C7d2e1e@localhost:8545

ENS API endpoint for a TLD, with contract address. Multiple values can be provided. 

Settings should be provided in the format `[tld:][contract-addr@]url`

A default top level domain and resolver contract address are provided, but an ENS/Geth endpoint must be provided to enable this functionality.

#### --standalone

*default* `false`

Set this flag if we would like the node not to try to connect to the network. Useful for development.

#### --swap-enable

*default* `true`

#### --swap-endpoint           

*default* `ws://localhost:8546`

SWAP ethereum blockchain endpoint. Must be equipped with websockets.

#### --swap-factory-address

*default* `anointed contract for the current blockchain id`

#### --swap-initial-deposit

*default* `10000000000000000`

#### --tracing-enable

*default* `false`

Send tracing spans to the tracing service. More information how to
configure and visualize tracing data is available
[here](/docs/bee-developers/useful-dev-info#tracing).

#### --tracing-endpoint

*default* `127.0.0.1:6831`

The URL where the tracing service listens for Thrift protocol UDP messages.

#### --tracing-service-name

*default* `bee`

Bee service identifier in tracing spans.

#### --transaction

*default* `""`

As a spam prevention measure, for nodes which deployed their
chequebook with v0.5.0 or before, specify `transaction` - [the
transaction hash of any Ethereum transaction on the XDAI
network](/docs/FAQ#how-can-i-find-a-transaction-hash-for-the---transaction-configuration-parameter)
sent from the Bee node's Ethereum address.

#### --verbosity

*default* `info`

0=silent, 1=error, 2=warn, 3=info, 4=debug, 5=trace

#### --welcome-message

*default* `""`

Custom welcome message to be displayed to peers on succesful connection.