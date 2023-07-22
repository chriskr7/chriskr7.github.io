---
title:  "간단히 알아보는 대표적인 블록체인 동의 알고리즘"
categories: tech
tags:
  - blockchain
  - bitcoin
  - ethereum
  - consensus algorithm
  - proof of work
  - proof of stake
  - pow
  - pos
toc: false
comments: true
---
블록체인의 동의 알고리즘은 블록체인의 다음 블록이 오직 하나의 진정한 블록임을
확정시키고 시스템을 변조하거나 전복시키려는 적들에게서 보호하고 성공적으로 포크를
이루어내는 역할을 한다. 즉 변이를 막으면서 체인의 다음 블록의 Winner를 정하는
알고리즘이라고 말할 수 있다.

### Proof of Work

Proof of Work 합의 알고리즘은 어쩌면 독자에게 친숙한 알고리즘일 수 있다. 왜냐하면
비트코인이 해당 알고리즘을 사용하고 있기 때문이다.

Proof of Work에서는 다수의 Miner들은 매우 어려운 암호 퍼즐을 푸는 형식으로 체인의
다음 블록을 추가하기 위해 경쟁한다. 해당 퍼즐을 먼저 풀어 경쟁에서 이긴 Miner는
6.25(23년 6월 기준)의 새로운 minted 비트코인과 약간의 거래 수수료를 얻는다.

Proof of Work에 대한 공통적인 비판은 막대한 컴퓨팅 리소스를 요구하는 데다가 거래에
대한 확인이 10분에서 60분 소요되며 확장이 잘 되지 않고 채굴이 주로 전기료가 싼 지역에
집중된다는 것이다. (중국)

+ **Proof of Work Coins**:
  + Bitcoin(BTC)
  + Ethereum Classic(ETC)
  + Bitcoin Cash(BCH)
  + ZCash(ZEC)
  + RobotEra
  + Tamadoge(TAMA)
  + LuckyBlock(LBlock)
{: .notice--info}

### Proof of Stake

이 합의 알고리즘은 블록을 채굴하기 위해 비싼 컴퓨터 장비들에게 투자하는 대신에
validator가 코인 시스템에 투자한다. validator란 단어에 주목하길 바란다. 이
알고리즘에서는 새로운 코인을 만들지 않고 코인의 수량은 발행 첫날에 정해지고
validator들은 해당 코인의 수량을 사거나 배당받고 알고리즘에 따라 거래 수수료를 받는다.

이 합의 알고리즘에서는 validators가 가진 coin의 수량에 따라 선택된다.
Proof of Work에서는 어려운 암호 퍼즐을 푸는 것을 요구하지만 Proof of Stake에서는
각각의 validators들이 다음 블록의 권리(거래 수수료를 받을 수 있는 권리)를 주장한다.
다음 블록의 승자(Bitcoin의 Miner)는 validators들 중 무작위로 선출되며 확률은
각 validator들이 가진 코인의 수에 비례한다. (2 코인을 가진 validator는 1 코인을 가진
validator보다 다음 블록의 승자로 선출될 확률이 2배이다.)

Validator로 참가하기 위해서는 코인 소유자는 최소 32 ETH을 보증 계좌를 예금해야한다.
(물론 더 적은 ETH로도 validator 풀에 참여하면 Validator로 참가할 수 있다.)
추가되는 블록들은 여러 validator들에 의해 검증되며 특정 수 이상의 validator들이
해당 블록들에 대해 정확성을 인정하면 해당 블록의 추가는 마무리된다.

+ **Proof of Stake Coins**:
  + Ethereum 2.0(ETH)
  + Binance(BNB)
  + Polygon
  + Tezos
  + Polkadot
  + Solana
  + BitDAO
{: .notice--info}

### Proof of Work 와 Proof of Stake 비교

|---|---|---|
|구분|Proof of Work|Proof of Stake|
|블록 생성자|Miner|Validator|
|블록생성 필요조건|GPU, 파워, 컴퓨팅 기기|Coins or Tokens|
|주요 보상|블록 생성 보상|거래 수수료|

----

### Reference

+ [What Does Proof-of-Stake (PoS) Mean in Crypto?](https://www.investopedia.com/terms/p/proof-stake-pos.asp)
+ [A (Short) Guide to Blockchain Consensus Protocols](https://www.coindesk.com/markets/2017/03/04/a-short-guide-to-blockchain-consensus-protocols/)