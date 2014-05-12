node-cryptonote-pool
====================

Mining pool for CryptoNote based coins such as Bytecoin and Monero

#### TODO

* Flooding detection
* IP banning for low-diff shares (prevent CPU overload with low-diff share attacks)
* Collecting stats and exposing via ajax/rest API
  * Currency network/block difficulty
  * Current block height
  * Network hashrate
  * Pool hashrate
  * Each miners' hashrate
  * Blocks found (pending, confirmed, and orphaned)
  * Total paid out
* Worker login validation (make sure miners are uing proper wallet addresses for mining)
* Light-weight front-end using API to display pool data
* Sending payments 
  * Add pool fee percent to config
  * Use redis data and wallet API to send out payments


Usage
===

#### Requirements
* Coin daemon(s) (find the coin's repo and build latest version from source)
* [Node.js](http://nodejs.org/) v0.10+ ([follow these installation instructions](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager))
* [Redis](http://redis.io/) key-value store v2.6+ ([follow these instructions](http://redis.io/topics/quickstart))

##### Seriously
Those are legitimate requirements. If you use old versions of Node.js or Redis that may come with your system package manager then you will have problems. Follow the linked instructions to get the last stable versions.


#### 1) Downloading & Installing

[**Redis security warning**](http://redis.io/topics/security): be sure firewall access to redis - an easy way is to include `bind 127.0.0.1` in your `redis.conf` file

Clone the repository and run `npm update` for all the dependencies to be installed:

```bash
git clone https://github.com/zone117x/node-cryptonote-pool.git pool
cd pool
npm update
```

#### 2) Configuration

Explanation for each field:
````javascript
{
    "coin": "monero",

    "poolPort": 5555,

    /* Initial difficulty miners are set to. */
    "difficulty": 10,

    /* Variable difficulty is a feature that will automatically adjust difficulty for
       individual miners based on their hashrate in order to lower networking and CPU
       overhead. */
    "varDiff": {
        "minDiff": 2, //Minimum difficulty
        "maxDiff": 512,
        "targetTime": 15, //Try to get 1 share per this many seconds
        "retargetTime": 30, //Check to see if we should retarget every this many seconds
        "variancePercent": 30 //Allow time to very this % from target without retargeting
    },

    /* Set to "auto" by default which will spawn one process/fork/worker for each CPU
       core in your system. Each of these workers will run a separate instance of your pool(s),
       and the kernel will load balance miners using these forks. Optionally, the 'forks' field
       can be a number for how many forks will be spawned. */
    "clusterForks": "auto",

    /* Specifies the level of log output verbosity. Anything more severe than the level specified
       will also be logged. */
    "logLevel": "debug", //or "warn", "error"

    /* By default the pool logs to console and gives pretty colors. If you direct that output to a
       log file then disable this feature to avoid nasty characters in your log file. */
    "logColors": true,

    /* Disabled by default as simpleminer doesn't yet support it. */
    "useLongPolling": false,


    /* Poll RPC daemons for new blocks every this many milliseconds. */
    "blockRefreshInterval": 1000,

    /* Coin daemon connection details. */
    "daemon": {
        "host": "127.0.0.1",
        "port": 18081
    },

    /* Wallet daemon connection details and address */
    "wallet": {
        "host": "127.0.0.1",
        "port": 8082,
        "address": "4AsBy39rpUMTmgTUARGq2bFQWhDhdQNekK5v4uaLU699NPAnx9CubEJ82AkvD5ScoAZNYRwBxybayainhyThHAZWCdKmPYn"
    },

    /* How many seconds until we consider a miner disconnected. */
    "minerTimeout": 600,

    /* Redis connection into. */
    "redis": {
        "host": "127.0.0.1",
        "port": 6379
    }
}
````


#### 3) Start the pool

```bash
node init.js
```



License
-------
Released under the GNU General Public License v2

http://www.gnu.org/licenses/gpl-2.0.html
