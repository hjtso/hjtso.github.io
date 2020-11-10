
---
title: ETHのDAPP開発流れ
lang: ja
date: 2018-07-07 18:18:56
tags: Blockchain
---

#### 1. コードをRemixに実行し効果を調べる

● Solidity或はSerpent、Vyper等言語を開発する

#### 2. Truffleを通してコンパイルする

● turffle compile
● turffle migrate (ganache-cli)
● turffle console (web3.jsとtruffle box)
● turffle test (JavaScriptとSolidity)

#### 3. 関連するフロントエンド画面を完成する

● React.js/JavaScript/HTML/CSS

#### 4. EthereumのメインWebサイトにデプロイする

● Remix + MetaMask + MyEtherWallet (Ropsten Test Network)
● Truffle + Infura (turffle migrate --reset --network ropsten)
● Truffle + Ethereum Full-node (gethとparity)（GASがかかる）

-------------------------------------
<center>![ETH_DAPP](/image/Blockchain/ETH/ETH_DAPP.jpg)</center> 

-------------------------------------
##### Githubプロジェクト（Github）

- [Github Link](https://github.com/HJTSO/Team_C "Title") 

##### 参考資料（Reference）

- [Solidity学習ヒント集](https://qiita.com/Yukiya025/items/b0b67c44b2878998015c "Title") 
- [これからEthereumでDApps開発する人にオススメサイト](https://qiita.com/yukatou/items/05652ba149266f81d4fe "Title") 