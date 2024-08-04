---
original: 54a9b25ffcd12966c60d6abd6584734d2793734f266fbe41118e6ee52b05dc9e
---

---
eip: 55
title: 大文字小文字混在のチェックサム付きアドレスエンコーディング
author: Vitalik Buterin <vitalik.buterin@ethereum.org>, Alex Van de Sande <avsa@ethereum.org>
discussions-to: https://github.com/ethereum/eips/issues/55
type: Standards Track
category: ERC
status: Final
created: 2016-01-14
---

# 仕様

コード:

``` python
import eth_utils


def checksum_encode(addr): # 20バイトのバイナリアドレスを入力として受け取る
    hex_addr = addr.hex()
    checksummed_buffer = ""

    # ハッシュ化のためにhex addressをascii/utf-8として扱う
    hashed_address = eth_utils.keccak(text=hex_addr).hex()

    # hex addressの各文字を反復処理する
    for nibble_index, character in enumerate(hex_addr):

        if character in "0123456789":
            # 10進数の数字は大文字にできない
            checksummed_buffer += character
        elif character in "abcdef":
            # ハッシュの対応するニブルが8以上かどうかを確認する
            hashed_address_nibble = int(hashed_address[nibble_index], 16)
            if hashed_address_nibble > 7:
                checksummed_buffer += character.upper()
            else:
                checksummed_buffer += character
        else:
            raise eth_utils.ValidationError(
                f"認識できないhex文字 {character!r} position {nibble_index}"
            )

    return "0x" + checksummed_buffer


def test(addr_str):
    addr_bytes = eth_utils.to_bytes(hexstr=addr_str)
    checksum_encoded = checksum_encode(addr_bytes)
    assert checksum_encoded == addr_str, f"{checksum_encoded} != expected {addr_str}"


test("0x5aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed")
test("0xfB6916095ca1df60bB79Ce92cE3Ea74c37c5d359")
test("0xdbF03B407c01E7cD3CBea99509d93f8DDDC8C6FB")
test("0xD1220A0cf47c7B9Be7A2E6BA89F429762e7b9aDb")

```

英語では、アドレスをhexに変換しますが、`i`番目の桁が文字(つまり`abcdef`のいずれか)の場合は、小文字のhexアドレスのハッシュの`4*i`ビット目が1であれば大文字で、そうでなければ小文字で出力します。

# 根拠

メリット:
- 大文字小文字混在を許容する多くのhexパーサーと下位互換性があり、徐々に導入できる
- 長さは40文字のままである
- 平均して1アドレスあたり15ビットのチェックが行われ、ランダムに生成されたアドレスが誤って入力されてもチェックを通過する確率は約0.0247%である。これはICAP方式の約50倍の改善であるが、4バイトのチェックコードほど良くはない。

# 実装

JavaScriptでの実装:

```js
const createKeccakHash = require('keccak')

function toChecksumAddress (address) {
  address = address.toLowerCase().replace('0x', '')
  var hash = createKeccakHash('keccak256').update(address).digest('hex')
  var ret = '0x'

  for (var i = 0; i < address.length; i++) {
    if (parseInt(hash[i], 16) >= 8) {
      ret += address[i].toUpperCase()
    } else {
      ret += address[i]
    }
  }

  return ret
}
```

```
> toChecksumAddress('0xfb6916095ca1df60bb79ce92ce3ea74c37c5d359')
'0xfB6916095ca1df60bB79Ce92cE3Ea74c37c5d359'
```

Keccak256ハッシュの入力は小文字の16進数文字列(つまり、ASCIIでエンコードされたhexアドレス)であることに注意してください:

```
    var hash = createKeccakHash('keccak256').update(Buffer.from(address.toLowerCase(), 'ascii')).digest()
```

# テストケース

```
# 全て大文字
0x52908400098527886E0F7030069857D2E4169EE7
0x8617E340B3D01FA5F11F306F4090FD50E238070D
# 全て小文字
0xde709f2102306220921060314715629080e2fb77
0x27b1fdb04752bbc536007a920d24acb045561c26
# 通常
0x5aAeb6053F3E94C9b9A09f33669435E7Ef1BeAed
0xfB6916095ca1df60bB79Ce92cE3Ea74c37c5d359
0xdbF03B407c01E7cD3CBea99509d93f8DDDC8C6FB
0xD1220A0cf47c7B9Be7A2E6BA89F429762e7b9aDb
```