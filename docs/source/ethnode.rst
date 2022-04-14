Ethereum node 
=============

Steps to run an Ethereum node on WSL 2 that stores
its blockchain data on a Windows mounted disk. 

Items
-----

* 1TB SSD
* USB 3.0 to 2.5" SATA cable
* WSL 2
* geth
* Web3.py

geth
----

In wsl follow geth's installation guide for Ubuntu.

.. code-block:: console
   
   Enable geth's personal package archive: 
   $ sudo add-apt-repository -y ppa:ethereum/ethereum

   $ sudo apt-get update
   $ sudo apt-get install ethereum

``geth`` the go-ethereum command line interface is now available on your 
system in ``/usr/bin/`` along with

* ``abigen``   -- package smart contracts
* ``bootnode`` -- find peers in private networks
* ``clef``     -- manage account operations
* ``evm``      -- debug EVM opcodes
* ``puppeth``  -- assemble and maintain private networks
* ``rlpdump``  -- recursive length prefix utility

More `here <https://github.com/ethereum/go-ethereum#executables>`_.

/mnt/e
-------

Configure ``geth`` to store blockchain data on the formatted SSD.
The disk is located in the ``/mnt/`` directory from wsl.

.. code-block:: console
   
   
   Move your node's data directory from wsl to windows. 
   $ mv ~/.ethereum/ /mnt/e/

   Create a symlink in ~ targeting the new data dir on windows.
   $ ln -s /mnt/e/.ethereum ~/.ethereum

   Update geth's config file.
   $ geth dumpconfig > mainnet.toml
   $ vi mainnet.toml

   Under [Node] edit: 
   Datadir = "/mnt/e/.ethereum"

Ready
-----

Start syncing your node to the Ethereum mainnet.

.. code-block:: console
   
   $ geth --config mainnet.toml --ipcdisable --http

Without specifying the sync mode our node is the default type: Snap. 
Ethereum nodes communicate by JSON-RPC APIs, and geth supports multiple
endpoints to transport RPC calls.

* HTTP
* WebSocket
* Unix Domain Socket

For our node, we disabled the IPC server (Unix socket) and use an
HTTP server listening on localhost:8545 (default) to talk to the
node and, when synced, effectively the whole Ethereum network.

Final sync stats
^^^^^^^^^^^^^^^^

There be no graphs or metrics here. I only synced in the evenings, 
starting and stopping the process by hand. 509 GB of block data (and counting)
later we are synced. Power consumption for the initial download was ~30 watts
greater than running the node while synced. 

=======      ======
State        Watts*
=======      ====== 
Syncing       160
Synced        130
Idle PC        80
=======      ======

*\*Estimates are from a P3 Kill A Watt meter.*

PGE's monthly energy rates for my location: 

============      =========
Charge            $ per kWh
============      ========= 
Usage              $0.06690
Transmission       $0.00243
Distrubtion        $0.04694
TOTAL              $0.11627
============      =========

Assuming continuous uptime: 1 month * 30 days * 24 hours * 130 watts * 1 kWh / 1000 watts * $0.11627 = $10.88

Include a list of adjustments that add ~7.59% to the final amount.

$10.88 + $10.88 * 0.0759 = **$11.71 monthly cost to run a node** 

Calls with Web3.py
^^^^^^^^^^^^^^^^^^
Coming soon 

Acknowledgments
^^^^^^^^^^^^^^^

"Btw I'm using the 1tb ssd you gave me. Much thx I owe u one (tb) ;)" 

"Yeee! Glad it's of good use now. 💾" 

