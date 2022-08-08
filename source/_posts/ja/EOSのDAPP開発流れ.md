---
title: EOSのDAPP開発流れ
lang: ja
date: 2018-08-18 18:18:56
tags: Blockchain
category: Blockchain
---


#### 1. コードを実行し効果を調べる

● C++等言語を開発する

#### 2. ツールを通してコンパイルする

*「ツール」：cleos、keosd、nodeos、eosiocpp
① コントラクトファイルを作成する
● eosiocpp -n hello
（パス：/opt/eosio/bin/data-dir/contracts）
（チェック：docker exec -it docker_nodeosd_1 ls /hello）
② ファイルをコンパイルする
● eosio cpp -o hello.wast hello.cpp
● eosio cpp -g hello.abi hello.cpp
③ コントラクトをマイグレートする
● cleos set contract eosio /hello/ /hello/hello.wast /hello/hello.abi -p eosio@active
④ コントラクトを呼び出す
● cleos push action hello hi '["user"]' -p eosio@active 
● あるいは「eosjs」を通してコンパイルする（nodejsを使う）

#### 3. 関連するフロントエンド画面を完成する

● React.js/JavaScript/HTML/CSS

#### 4. EOSのメインWebサイトにデプロイする

● 「config.ini」ファイルを修正し、コントラクトをマイグレートする（RAMがかかる）

-------------------------------------
<center>![EOS_DAPP](/image/Blockchain/EOS/EOS_DAPP.jpg)</center> 

-------------------------------------
##### Githubプロジェクト（Github）

- [Github Link](https://github.com/hjtso/eos "Title") 

##### 参考資料（Reference）

- [EOS智能合约介绍](https://github.com/shanlusun/blockchain/tree/master/eos/05 "Title") 
- [EOS开发调试环境搭建](https://blog.csdn.net/caokun_8341/article/details/80713851 "Title") 
- [EOS源码学习系列](https://www.jianshu.com/p/24e1607ac7a2 "Title") 