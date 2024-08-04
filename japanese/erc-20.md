---
original: 50d0bdc535ed8987ac5c124cb5f9c39606806c97e909aae66ef4b747dd9c93dd
---

---
eip: 20
title: トークン標準
author: Fabian Vogelsteller <fabian@ethereum.org>, Vitalik Buterin <vitalik.buterin@ethereum.org>
type: Standards Track
category: ERC
status: Final
created: 2015-11-19
---

## 簡単な要約

トークンの標準インターフェースです。


## 概要

以下の標準は、スマートコントラクト内のトークンの標準APIの実装を可能にします。
この標準は、トークンの転送や、他のオンチェーンの第三者によるトークンの使用を承認するための基本的な機能を提供します。


## 動機

標準インターフェースにより、Ethereumのあらゆるトークンを他のアプリケーション(ウォレットや分散型取引所など)で再利用できるようになります。


## 仕様

## トークン
### メソッド

**注意事項**:
 - 以下の仕様では、Solidity `0.4.17` (以降)の構文を使用しています
 - コールする側は、`returns (bool success)` からの `false` を処理する必要があります。コールする側は `false` が返されることはないと想定してはいけません!


#### name

トークンの名称を返します - 例: `"MyToken"`。

オプション - このメソッドは利便性の向上に使用できますが、インターフェースや他のコントラクトはこれらの値が存在することを前提としてはいけません。


``` js
function name() public view returns (string)
```


#### symbol

トークンのシンボルを返します。例: "HIX"。

オプション - このメソッドは利便性の向上に使用できますが、インターフェースや他のコントラクトはこれらの値が存在することを前提としてはいけません。

``` js
function symbol() public view returns (string)
```



#### decimals

トークンの小数点以下の桁数を返します - 例: `8` は、トークンの金額を `100000000` で割ると、ユーザー表示になります。

オプション - このメソッドは利便性の向上に使用できますが、インターフェースや他のコントラクトはこれらの値が存在することを前提としてはいけません。

``` js
function decimals() public view returns (uint8)
```


#### totalSupply

トークルの総供給量を返します。

``` js
function totalSupply() public view returns (uint256)
```



#### balanceOf

アドレス `_owner` のアカウント残高を返します。

``` js
function balanceOf(address _owner) public view returns (uint256 balance)
```



#### transfer

`_value` 数のトークンをアドレス `_to` に転送し、`Transfer` イベントを発火させます。
メッセージ呼び出し元のアカウント残高が転送に必要な分のトークンを持っていない場合は、関数が `throw` する必要があります。

*注意* 0 値の転送も通常の転送として扱い、`Transfer` イベントを発火させる必要があります。

``` js
function transfer(address _to, uint256 _value) public returns (bool success)
```



#### transferFrom

アドレス `_from` からアドレス `_to` に `_value` 数のトークンを転送し、`Transfer` イベントを発火させます。

`transferFrom` メソッドは引き出しワークフローに使用され、コントラクトがユーザーに代わってトークンを転送できるようにします。
これにより、コントラクトがユーザーに代わってトークンを転送したり、サブ通貨で手数料を徴収したりできるようになります。
関数は、`_from` アカウントが何らかの方法で送信者を明示的に承認していない限り、`throw` する必要があります。

*注意* 0 値の転送も通常の転送として扱い、`Transfer` イベントを発火させる必要があります。

``` js
function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
```



#### approve

`_spender` に、最大 `_value` 数のトークンを複数回引き出す権限を与えます。この関数が再度呼び出された場合は、現在の許可を `_value` で上書きします。

**注意**: [ここで説明されている](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/)攻撃ベクトルや、[ここで議論されている](https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729)ものを防ぐために、クライアントは同じ送信者に対して許可を最初に `0` に設定してから別の値に設定するようなユーザーインターフェースを作成する必要があります。
ただし、コントラクト自体はこれを強制してはいけません。これは後方互換性を維持するためです。

``` js
function approve(address _spender, uint256 _value) public returns (bool success)
```


#### allowance

`_owner` が `_spender` に許可している残高を返します。

``` js
function allowance(address _owner, address _spender) public view returns (uint256 remaining)
```



### イベント


#### Transfer

トークンが転送されたときに必ず発火させる必要があります。ゼロ値の転送も含みます。

新しいトークンを作成するトークンコントラクトは、トークンの作成時に `_from` アドレスを `0x0` に設定して `Transfer` イベントを発火させる必要があります。

``` js
event Transfer(address indexed _from, address indexed _to, uint256 _value)
```



#### Approval

`approve(address _spender, uint256 _value)` への成功呼び出しで必ず発火させる必要があります。

``` js
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```



## 実装

Ethereumネットワーク上には既に多くのERC20準拠のトークンが展開されています。
ガス節約や安全性の向上など、さまざまなトレードオフを持つ実装が各チームによって書かれています。

#### 実装例は以下で入手可能です
- [OpenZeppelinの実装](../assets/eip-20/OpenZeppelin-ERC20.sol)
- [ConsenSysの実装](../assets/eip-20/Consensys-EIP20.sol)


## 履歴

この標準に関連する過去のリンク:

- Vitalik Buterinによる元の提案: https://github.com/ethereum/wiki/wiki/Standardized_Contract_APIs/499c882f3ec123537fc2fccd57eaa29e6032fe4a
- Redditでの議論: https://www.reddit.com/r/ethereum/comments/3n8fkn/lets_talk_about_the_coin_standard/
- 元のIssue #20: https://github.com/ethereum/EIPs/issues/20



## 著作権
著作権およびそれに関連する権利は[CC0](../LICENSE.md)によって放棄されています。