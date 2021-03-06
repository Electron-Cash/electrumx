===========
 ChangeLog
===========

.. note:: It is strongly recommended you upgrade to Python 3.7, which
   fixes bugs in asyncio that caused an ever-growing open file count
   and memory consumption whilst serving clients.  Those problems
   should not occur with Python 3.7.

.. note:: The primary difference between this and the official ElectrumX
   is that this version supports any block ordering (including the lexically-
   ordered blocks that can be created/followed by several Bitcoin Cash node
   implementations: ABC, BU, XT, bchd, bcash, Bitprim, and others.). We also
   employ a different anti-phishing strategy than they do, based on a
   community-maintained blacklist.json file as well as manual banning
   to supplement that via the banip command. We also have slightly more
   generous limits to things like MAX_SEND and MAX_SUBS than the defaults
   of ElectrumX.  We also don't implement their newest cost control logic which
   we feel is too restrictive for normal BCH usage of the server.


Version 1.10.1 (21 April 2019)
==============================

* Added PEER_DISCOVERY_TOR environment variable (default off). If on, then
  ElectronX will behave as before this version and always forward/discover
  *.onion peers.  If off, then it will completely ignore *.onion peers and
  never forward them.  This policy change has been implemented because of the
  ease with which sybil attacks are possible if PEER_DISCOVER_TOR is enabled.
  (cculianu)
* Added more stuff to `getenv` rpc command display (cculinau)
* Optimized the ban handling a little bit to refuse peers earlier in the
  lifecycle of peer_discovery if a host is banned. (cculianu)

Version 1.10.0 (14 April 2019)
==============================

* Added banip, unbanip, banhost, unbanhost, listbanned, getenv, and
  session_ip_counts rpc commands (cculianu)
* Disallow multiple servers from same IP to appear in peers (doesn't apply
  to Tor, however) (cculianu)
* Limit client connections per IP to 50 by default (MAX_SESSIONS_PER_IP)
  to prevent the "clients exhausting FDs on server" attack vector (doesn't
  apply to Tor or localhost clients).
  (cculianu)
* Limit client connections per IP for Tor to 1000 by default (MAX_SESSIONS_TOR)
  Note that this limit also applies to clients coming from localhost even if
  not using Tor. (cculianu)
* Added blacklist.json mechanism for downloading a community maintained
  list of bad peers as an anti-phishing, anti-sybil attack measure.
  Use BLACKLIST_URL="" in env to disable, but it is recommended you leave
  it enabled.  We use our own file format for this but the code for this
  subsystem also understand the ElectrumX 1.10.0 format as well. (cculianu)
* Set MAX_SEND to 4000000 (4MB) by default (was originally 1MB which fails
  on some BCH wallets). (cculianu)
* Raised server limit from 250,000 subs to 1,000,000 subs by default
  (corresponds to env var MAX_SUBS) (cculianu)
* Raised default BANDWIDTH_LIMIT from 2MB to 8MB (8000000 bytes).  This is
  because very busy BCH wallets with huge histories would need a higher limit.
  Note the limit is a soft limit in bytes per hour, and it is not catastrophic
  to send a client 8MB in 1 hour. (cculianu)


Version 1.9.4 (7 Feb 2019)
============================
* Synch up to ElectrumX codebase (pulling in relevant & useful commits)
  (cculianu)
* coin additions / updates: SmartCash (rc125), NIX (phamels), Minexcoin
  (joesixpack), BitcoinCash (mblunderburg), Dash (zebra-lucky),
  BitcoinCashRegtest (ezegom), AXE (slowdive), NOR (flo071),
  BitcoinPlus (bushsolo), Myriadcoin (cryptapus), Trezarcoin (ChekaZ), Bitcoin
  Diamond (John Shine), NMC (jeremyrand), Dash (zebra-lucky), PeerCoin
  (peerchemist), BCH testnet (Mark Lundeberg, cculianu), Unitus (ChekaZ)
* close ElectrumX issues: `#554`_, `#653`_, `#655`_, `#684`_, `#713`_
* other minor tweaks (Michael Schmoock, Michael Taborsky)
* increase default MAX_SEND for AuxPow Chains.  Truncate AuxPow for block
  heights covered by a checkpoint.  (jeremyrand)
* tighter RPC param checking (ghost43)
* require aiorpcX 0.10.3

Version 1.8.12 (10 Nov 2018)
============================

* bug fix

Version 1.8.11 (07 Nov 2018)
============================

* require aiorpcX 0.10.1

Version 1.8.10 (05 Nov 2018)
============================

* require aiorpcX 0.10.0
* fix `#632`_
* coin additions / updates: ZelCash (TheTrunk)

Version 1.8.9 (02 Nov 2018)
===========================

* fix `#630`_

Version 1.8.8 (01 Nov 2018)
===========================

* require aiorpcX 0.9.0
* coin additions / updates: decred (dajohi, bolapara), zcash (erasmospunk),
  namecoin (JeremyRand),CivX (turcol), NewYorkCoin (erasmospunk)
* fix `#603`_, `#608`_
* other minor fixes and changes: FMCorz

Version 1.8.7 (13 Sep 2018)
===========================

* require aiorpcX 0.8.1
* fix reorg bug loading blocks from disk (erasmospunk)

Version 1.8.6 (12 Sep 2018)
===========================

* require aiorpcX 0.8.0
* suppress socket.send() errors
* new coin TokenPay (samfiragabriel)
* minor fix: wakiyamap

Version 1.8.5 (18 Aug 2018)
===========================

* require aiorpcX 0.7.3 which contains a couple of bugfixes
* fix `#552`_, `#577`_
* fixed a session limiting bug reported by ghost43
* coin additions / updates: PIVX and Decred Testnets, BitcoinGreen (cunhasb)
  Monacoin (wakayamap)
* proper generation input handling for various altcoins (erasmospunk) fixing
  `#570`_

Version 1.8.4 (14 Aug 2018)
===========================

* improved notification handling and efficiency
* improved daemon handling with minor fixes; full tests for Daemon class
* remove chain_state class
* various internal cleanups and improvements (erasmospunk)
* add PIVX support (erasmospunk) - mempool handling WIP
* fix protocol 1.3 handling of blockchain.block.header RPC (ghost43)

Version 1.8.3 (11 Aug 2018)
===========================

* separate the DB and the BlockProcessor objects
* comprehensive mempool tests
* fix `#521`_, `#565`_, `#567`_

Version 1.8.2 (09 Aug 2018)
===========================

* require aiorpcX 0.7.1 which along with an ElectrumX change restores clean
  shutdown and flush functionality, particularly during initial sync
* fix `#564`_

Version 1.8.1 (08 Aug 2018)
===========================

* require aiorpcX 0.7.0 which fixes a bug causing silent shutdown of ElectrumX
* fix `#557`_, `#559`_
* tweaks related to log spew (I think mostly occurring with old versions
  of Python)

Version 1.8  (06 Aug 2018)
==========================

* require aiorpcX 0.6.2
* fix query.py; move to contrib.  Add :ref:`query <query>` function to RPC
* rewrite :command:`electrumx_rpc` so that proper command-line help is provided
* per-coin tx hash functions (erasmospunk)
* coin additions / updates: Groestlcoin (Kefkius, erasmospunk),
  Decred (erasmonpsunk)
* other minor (smmalis37)

Version 1.7.3  (01 Aug 2018)
============================

* fix `#538`_

Version 1.7.2  (29 Jul 2018)
============================

* require aiorpcX 0.5.9; 0.5.8 didn't work on Python 3.7

Version 1.7.1  (28 Jul 2018)
============================

* switch to aiorpcX 0.5.8 which implements some curio task management
  primitives on top of asyncio that make writing correct async code
  much easier, as well as making it simpler to reason about
* use those primitives to restructure the peer manager, which is now
  fully concurrent again, as well as the block processor and
  controller
* fix `#534`_ introduced in 1.7
* minor coin tweaks (ghost43, cipig)

Version 1.7  (25 Jul 2018)
==========================

* completely overhauled mempool and address notifications
  implementation.  Cleaner and a lot more efficient, especially for
  initial synchronization of the mempool.  Mempool handling is fully
  asynchronous and doesn't hinder client responses or block
  processing.
* peer discovery cleaned up, more work remains
* cleaner shutdown process with clear guarantees
* aiohttp min version requirement raised to 2.0
* onion peers are ignored if no tor proxy is available
* add Motion coin (ocruzv), MinexCoin (joesixpack)

Version 1.6  (19 July 2018)
===========================

* implement :ref:`version 1.4` of the protocol, with benefit for light
  clients, particularly mobile
* implement header proofs and merkle caches
* implement :func:`blockchain.transaction.id_from_pos` (ghost43)
* large refactoring of session and controller classes
* recent blocks are now stored on disk.  When backing up in a reorg
  ElectrumX uses these rather than asking the daemon for the blocks --
  some daemons cannot correctly handle orphaned block requests after
  a reorg.  Fixes `#258`_, `#315`_, `#479`_
* minor fixes: nijel

Version 1.5.2
=============

* package renamed from elctrumX-kyuupichan to electrumX
* split merkle logic out into lib/merkle.py
* fix `#523`_ for daemons based on older releases of core

Version 1.5.1
=============

Fixes a couple of issues found in 1.5 after release:

* update peer discovery code for :ref:`version 1.3` of the protocol
* setup.py would not run in a clean environment (e.g. virtualenv)
* logging via aiorpcX didn't work with the logging hierarchy updates
* log Python interpreter version on startup

Version 1.5
===========

.. note:: The two main scripts, :file:`electrumx_server` and
   :file:`electrumx_rpc` were renamed to drop the `.py` suffix.  You
   will probably need to update your run script accordingly.

* support :ref:`version 1.3` of the protocol
* increase minimum supported protocol version to :ref:`version 1.1`
* split out history handling in preparation for new DB format
* force close stubborn connections that refuse to close gracefully
* RPC getinfo returns server version (erasmospunk)
* add new masternode methods; document them all (elmora-do)
* make electrumx a Python package (eukreign)
* hierarchical logging, Env to take a coin class directly,
  server_listening event (eukreign)
* decred coin removed as mainnet does not sync
* issues fixed: `#414`_, `#443`_, `#455`_, `#480`_, `#485`_, `#502`_,
  `#506`_, `#519`_ (wakiyamap)
* new or updated coins: Feathercoin (lclc), NewYorkCoin Testnet(nicovs),
  BitZeny (wakiyamap), UFO (bushstar), GAME (cipig), MAC (nico205),
  Xuez (ddude), ZCash (wo01), PAC (elmora-do), Koto Testnet (wo01),
  Dash Testnet (ser), BTG all nets (wilsonmeier), Polis + ColossusXT +
  GoByte + Monoeci (cronos-polis), BitcoinCash Regtest (eukreign)
* minor tweaks: romanz, you21979, SuBPaR42, sangaman, wakiyamap, DaShak


**Neil Booth**  kyuupichan@gmail.com  https://github.com/kyuupichan

bitcoincash:qzxpdlt8ehu9ehftw6rqsy2jgfq4nsltxvhrdmdfpn

.. _#258: https://github.com/kyuupichan/electrumx/issues/258
.. _#315: https://github.com/kyuupichan/electrumx/issues/315
.. _#414: https://github.com/kyuupichan/electrumx/issues/414
.. _#443: https://github.com/kyuupichan/electrumx/issues/443
.. _#455: https://github.com/kyuupichan/electrumx/issues/455
.. _#479: https://github.com/kyuupichan/electrumx/issues/479
.. _#480: https://github.com/kyuupichan/electrumx/issues/480
.. _#485: https://github.com/kyuupichan/electrumx/issues/485
.. _#502: https://github.com/kyuupichan/electrumx/issues/50
.. _#506: https://github.com/kyuupichan/electrumx/issues/506
.. _#519: https://github.com/kyuupichan/electrumx/issues/519
.. _#521: https://github.com/kyuupichan/electrumx/issues/521
.. _#523: https://github.com/kyuupichan/electrumx/issues/523
.. _#534: https://github.com/kyuupichan/electrumx/issues/534
.. _#538: https://github.com/kyuupichan/electrumx/issues/538
.. _#552: https://github.com/kyuupichan/electrumx/issues/552
.. _#557: https://github.com/kyuupichan/electrumx/issues/557
.. _#559: https://github.com/kyuupichan/electrumx/issues/559
.. _#564: https://github.com/kyuupichan/electrumx/issues/564
.. _#565: https://github.com/kyuupichan/electrumx/issues/565
.. _#567: https://github.com/kyuupichan/electrumx/issues/567
.. _#570: https://github.com/kyuupichan/electrumx/issues/570
.. _#577: https://github.com/kyuupichan/electrumx/issues/577
.. _#603: https://github.com/kyuupichan/electrumx/issues/603
.. _#608: https://github.com/kyuupichan/electrumx/issues/608
.. _#630: https://github.com/kyuupichan/electrumx/issues/630
.. _#632: https://github.com/kyuupichan/electrumx/issues/630
