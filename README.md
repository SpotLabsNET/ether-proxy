# ether-proxy

Ethereum mining proxy with web-interface.

**Proxy feature list:**

* Rigs availability monitoring
* Keep track of accepts, rejects, blocks stats
* Easy detection of sick rigs
* Daemon failover list

![Demo](https://raw.githubusercontent.com/sammy007/ether-proxy/master/proxy.png)

### Building on Linux

Dependencies:

  * go >= 1.4
  * geth

Export GOPATH:

    export GOPATH=$HOME/go

Install required packages:

    go get github.com/ethereum/ethash
    go get github.com/ethereum/go-ethereum/common
    go get github.com/goji/httpauth
    go get github.com/gorilla/mux
    go get github.com/yvasiyarov/gorelic

Compile:

    go build -o ether-proxy main.go

### Building on Windows

Follow [this wiki paragraph](https://github.com/ethereum/go-ethereum/wiki/Installation-instructions-for-Windows#building-from-source) in order to prepare your environment.
Install required packages (look at Linux install guide above). Then compile:

    go build -o ether-proxy.exe main.go

### Building on Mac OS X

If you didn't install [Brew](http://brew.sh/), do it. Then install Golang:

    brew install go

And follow Linux installation instructions because they are the same for OS X.

### Configuration

Configuration is self-describing, just copy *config.example.json* to *config.json* and specify endpoint URL and upstream URLs.

#### Example upstream section

```javascript
"upstream": [
  {
    "pool": true,
    "name": "NiceHash as primary pool",
    "url": "http://ethereum.eu.nicehash.com:3500/n1c3-1CzrFvvieNaZg5aMHkb8eAPCSeVVUfUpax.myproxy/250",
    "timeout": "10s"
  },
  {
    "pool": true,
    "name": "SuprNova as secondary pool",
    "url": "http://eth-mine.suprnova.cc:3000/myusername.myproxy/250",
    "timeout": "10s"
  },
  {
    "name": "local geth wallet as backup",
    "url": "http://127.0.0.1:8545",
    "timeout": "10s"
  }
],
```

In this example we specified [NiceHash](https://www.nicehash.com) mining pool as main primary mining target. Additionally we specified [suprnova.cc](https://eth.suprnova.cc) and a local geth node as backup.

With <code>"submitHashrate": true|false</code> proxy will forward <code>eth_submitHashrate</code> requests to upstream.

#### Running

    ./ether-proxy config.json

#### Connecting and mining with ethminer to the proxy

The syntax is:

    ethminer -G -F http://x.x.x.x:8546/miner/N/X

where N = desired difficulty (approximately expected rig hashrate, divided by 2) and X is the name of the rig.

Examples for one AMD and one NVIDIA rig:

    ethminer -G -F http://x.x.x.x:8546/miner/50/myamdrig
    ethminer -U -F http://x.x.x.x:8546/miner/30/mynvidiarig

### Pools that work with this proxy

* [NiceHash](https://www.nicehash.com) NiceHash Ethereum pool with auto-convert to Bitcoin
* [suprnova.cc](https://eth.suprnova.cc) suprnova.cc Ethereum pool

### TODO

* Report block numbers
* Report luck per rig
* Maybe add more stats
* Maybe add charts

### Acknowlegments

Forked from [sammy007's ethere-proxy](https://github.com/sammy007/ether-proxy), all credits goes to sammy007.

### License

The MIT License (MIT).
