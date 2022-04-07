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

Sleeping on it TBD


Calls with Web3.py
^^^^^^^^^^^^^^^^^^
Coming soon


Acknowledgments
^^^^^^^^^^^^^^^

"Btw I'm using the 1tb ssd you gave me. Much thx I owe u one (tb) ;)" 

"Yeee! Glad it's of good use now. 💾" 

