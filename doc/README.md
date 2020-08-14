IQCASH Core
=============

Setup
---------------------
[IQCASH Core](http://iq.cash/wallet) is the original IQCASH client and it builds the backbone of the network. However, it downloads and stores the entire history of IQCASH transactions; depending on the speed of your computer and network connection, the synchronization process can take anywhere from a few hours to a day or more. Thankfully you only have to do this once.

Running
---------------------
The following are some helpful notes on how to run IQCASH Core on your native platform.

### Unix

Unpack the files into a directory and run:

- `bin/iqcash-qt` (GUI) or
- `bin/iqcashd` (headless)

### Windows

Unpack the files into a directory, and then run iqcash-qt.exe.

### macOS

Drag IQCASH-Qt to your applications folder, and then run IQCASH-Qt.

### Need Help?

* See the documentation at the [IQCASH Wiki](https://github.com/IQCASH/IQCash/wiki)
for help and more information.
* Ask for help on [BitcoinTalk](https://bitcointalk.org/index.php?topic=4360591.0) or on the [IQCASH Forum](http://forum.iq.cash/).
* Join our Discord server [IQCash Discord](https://discord.gg/Ruc4K4V)

Building
---------------------
The following are developer notes on how to build IQCASH Core on your native platform. They are not complete guides, but include notes on the necessary libraries, compile flags, etc.

- [Dependencies](dependencies.md)
- [macOS Build Notes](build-osx.md)
- [Unix Build Notes](build-unix.md)
- [Windows Build Notes](build-windows.md)
- [Gitian Building Guide](gitian-building.md)

Development
---------------------
The IQCASH repo's [root README](/README.md) contains relevant information on the development process and automated testing.

- [Developer Notes](developer-notes.md)
- [Multiwallet Qt Development](multiwallet-qt.md)
- [Release Notes](release-notes.md)
- [Release Process](release-process.md)
- [Source Code Documentation (External Link)](https://www.fuzzbawls.pw/iqcash/doxygen/)
- [Translation Process](translation_process.md)
- [Unit Tests](unit-tests.md)
- [Unauthenticated REST Interface](REST-interface.md)
- [Dnsseed Policy](dnsseed-policy.md)

### Resources
* Discuss on the [BitcoinTalk](https://bitcointalk.org/index.php?topic=4360591.0) or the [IQCASH](http://forum.iq.cash/) forum.
* Join the [IQCash Discord](https://discord.gg/Ruc4K4V).

### Miscellaneous
- [Assets Attribution](assets-attribution.md)
- [Files](files.md)
- [Tor Support](tor.md)
- [Init Scripts (systemd/upstart/openrc)](init.md)

License
---------------------
Distributed under the [MIT software license](/COPYING).
This product includes software developed by the OpenSSL Project for use in the [OpenSSL Toolkit](https://www.openssl.org/). This product includes
cryptographic software written by Eric Young ([eay@cryptsoft.com](mailto:eay@cryptsoft.com)), and UPnP software written by Thomas Bernard.
