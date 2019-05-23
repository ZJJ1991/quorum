# The technology behind the private transaction
* Transactions can be marked private. Private transactions go on the same public blockchain, but contain nothing but a SHA-3-512 digest of encrypted payloads that were exchanged between the relevant peers. Only the peers who have and can decrypt that payload will execute the real contracts of the transaction, while others will ignore it. This achieves the interesting property that transaction contents are still sealed in the blockchain’s history, but are known only to the relevant peers.
* Because not every peer executes every private transaction, a separate, private `state` Merkle Patricia trie was added. Mutations caused by private transactions affect the state in the private trie, while public transactions affect the public trie. The public state continues to be validated as expected, but the private state has no validation. (In the future, we’d like to have the peers verify shared private contract states.)<br>
中文解释
* 似有交易是可以在公链中传播的，但是只有加密过的SHA-3-512 digest payload。只有拥有paylad（也就是发似有交易的节点）和能解密payload的节点能执行真正的交易合约，而其他人可以忽略。这样可以保证交易仍然封装在区块链历史里，但是只有相关的节点才知道。
* 因为不是每一个节点都执行每一个私有交易，单独的私有`状态树`，私有交易的变化只造成私有交易树的变化，而公有交易影响公有交易树。公有状态会被验证，而私有状态没有验证过程。

# Constellation Istructions
## Introduction
Constellation, written in Haskell, is a daemon which implements a simple peer-to-peer network. Nodes on the network are each the authoritative destination for some number of public keys.

## Installation Envirionment
---
> Ubuntu 18.04 (other Linux Distributions may also apply)

## Official Site Instructions
---
Follow the official site for details. Basically the installation are classified into 4 procedures. Refer to the [official site](https://github.com/jpmorganchase/constellation) for details. The following paragraph will recap the main procedures it should take to finalize the installation process.
* Prerequisites

This procedure installs the prerequired dependicies. 

* Downloading precompiled binaries

This procedure downloads the compiled binaries.

* Installation from source
This procedure installs the Haskell associated environemnt into your hardware.

## problems you may encounter
---
Well, if you started to leveraging the `Constellation` toolkit to set up quorum chain environemnt, you may probabaly encounter a problem  as follows:

> error while loading shared libraries: libsodium.so.18: cannot open shared object file: No such file or directory<br>

This is because either the libsodium you installed is not the right version or you do not have it installed at your hardware at all.
* If you have an incorrect version, simply change the `libsodium.so` file into `libsodium.so.18`, which is located at `/usr/lib/x86_64-linux-gnu` directory. 
* If you do not have it installed at your machine, you have to download and install libsodium of version `1.0.11`. Refer to the following 2 links for instructions:
https://www.jianshu.com/p/cb0f6372cf93<br>
https://blog.csdn.net/hanshihao1336295654/article/details/79850584<br>

Note that the corresponding commands in the above mentioned link may probably need to be changed as follows depending on your machine:<br>
`sudo sh -c "echo '/usr/lib/x86_64-linux-gnu' >> /etc/ld.so.conf"`<br>
`sudo ldconfig`<br>
Finally, if you are really not able to install the version `1.0.11`, just change the `libsodium.so.23` or `libsodium.so` or any other versions located at `/usr/lib/x86_64-linux-gnu` directory into `libsodium.so.18`.

# Use Constellation in Practice
* Generate a key pair for your local node.
> ./constellation-node --workdir=`TheWorKDirectory`   --generatekeys=`TheNameOfYourKeyPair` <br>

* Build a private connection<br>
Assume you have a node running on localhost:30303 and localhost:30304 respectively.
> ./constellation-node --url=https://localhost:30303 --port=30303 --workdir=data --socket=constellation.ipc --othernodes=https://localhost:30304 --publickeys=validator1.pub --privatekeys=validator1.key<br>

Note that the validator1.pub and validator1.key should be the actual validator node's public and private key respectively. Manually create those in your `data` directory **instead of** generating those using this toolkit.
