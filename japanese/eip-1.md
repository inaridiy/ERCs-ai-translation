---
original: 264fba8fe128035a913064efbb74a66bb64d6a6d9f6deef620d3c47cc06d99e9
---

---
eip: 1
title: EIPの目的とガイドライン
status: Living
type: Meta
author: Martin Becze <mb@ethereum.org>、Hudson Jameson <hudson@ethereum.org>、他
created: 2015-10-27
---

## EIPとは何ですか?

EIPはEthereum Improvement Proposalの略で、Ethereumコミュニティに情報を提供したり、Ethereumの新機能や処理、環境についての設計書です。EIPには、その機能の簡潔な技術仕様と根拠が記載されています。EIPの著者は、コミュニティのコンセンサスを形成し、反対意見を文書化する責任があります。

## EIPの根拠

EIPは、新機能の提案、技術的な問題に関するコミュニティからの意見収集、Ethereumの設計決定の記録を主な目的としています。EIPはバージョン管理されたリポジトリ内のテキストファイルとして管理されるため、機能提案の履歴が記録されます。

Ethereumの実装者にとって、EIPはその実装の進捗を追跡する便利な方法です。理想的には、各実装のメンテナーがどのEIPを実装したかを一覧表示することで、ユーザーが特定の実装やライブラリの現在の状況を把握できるようになります。

## EIPの種類

EIPには3つのタイプがあります:

- **Standards Track EIP**は、ネットワークプロトコルの変更、ブロックやトランザクションの有効性ルールの変更、アプリケーション標準/慣例の提案、相互運用性に影響する変更や追加など、ほとんどすべてのEthereum実装に影響する変更を記述するものです。Standards Track EIPは、設計書、実装、(必要に応じて)正式な仕様の更新の3つの部分から構成されます。さらに、Standards Track EIPは以下のカテゴリに分類されます:
  - **Core**: コンセンサスフォークを必要とする改善([EIP-5](./eip-5.md)、[EIP-101](./eip-101.md)など)、およびコンセンサスクリティカルではないが「コアデベロッパー」の議論に関連する可能性のある変更([EIP-90]、[EIP-86](./eip-86.md)のマイナー/ノードストラテジー変更2、3、4など)。
  - **Networking**: [devp2p](https://github.com/ethereum/devp2p/blob/readme-spec-links/rlpx.md)([EIP-8](./eip-8.md))や[Light Ethereum Subprotocol](https://ethereum.org/en/developers/docs/nodes-and-clients/#light-node)の改善、[whisper](https://github.com/ethereum/go-ethereum/issues/16013#issuecomment-364639309)や[swarm](https://github.com/ethereum/go-ethereum/pull/2959)のネットワークプロトコル仕様の提案改善などが含まれます。
  - **Interface**: メソッド名([EIP-6](./eip-6.md))や[contract ABI](https://docs.soliditylang.org/en/develop/abi-spec.html)などの言語レベルの標準の改善が含まれます。
  - **ERC**: トークン標準([ERC-20](./eip-20.md))、名称レジストリ([ERC-137](./eip-137.md))、URIスキーム、ライブラリ/パッケージフォーマット、ウォレットフォーマットなどのアプリケーションレベルの標準と慣例が含まれます。

- **Meta EIP**は、Ethereumに関するプロセスを記述したり、プロセスの変更を提案するものです。プロセスEIPはStandards Track EIPのようなものですが、Ethereumプロトコル自体ではなく、他の領域に適用されます。実装を提案することもできますが、Ethereumのコードベースではありません。通常はコミュニティのコンセンサスを必要とし、Informational EIPのように無視されることはありません。手順、ガイドライン、意思決定プロセスの変更、Ethereum開発に使用されるツールや環境の変更などが例として挙げられます。すべてのメタEIPはプロセスEIPとも見なされます。

- **Informational EIP**は、Ethereumの設計上の問題を説明したり、Ethereumコミュニティに一般的なガイドラインや情報を提供するものですが、新機能を提案するものではありません。Informational EIPはEthereum コミュニティのコンセンサスや推奨事項を必ずしも表すものではないため、ユーザーや実装者は自由にInformational EIPを無視したり従ったりすることができます。

単一のEIPには、単一の主要な提案や新しいアイデアを含めることが強く推奨されます。EIPが焦点を絞っているほど、成功する可能性が高くなります。1つのクライアントの変更にはEIPは必要ありませんが、複数のクライアントに影響する変更や、複数のアプリケーションが使用する標準を定義する場合にはEIPが必要です。

EIPは一定の最小基準を満たす必要があります。提案する機能の明確で完全な説明が必要です。その機能は全体として改善を表すものでなければなりません。該当する場合は、提案する実装が堅牢であり、プロトコルを過度に複雑化させないものでなければなりません。

### Coreカテゴリのための特別な要件

**Core** EIPがEVM(Ethereum Virtual Machine)に言及または変更を提案する場合は、ニーモニックによる命令を参照し、少なくともそれらのニーモニックのオペコードを定義する必要があります。推奨される方法は以下のとおりです:

```
REVERT (0xfe)
```

## EIPワークフロー

### EIPの世話

関係者は、あなた(EIPの提唱者または*EIP著者*)、[*EIPエディター*](#eip-editors)、[*Ethereumコアデベロッパー*](https://github.com/ethereum/pm)です。

正式なEIPを書く前に、あなたのアイデアを検討する必要があります。重複を避けるために、まずEthereum コミュニティにアイデアを提示し、オリジナリティを確認することをお勧めします。そのため、[Ethereum Magiciansフォーラム](https://ethereum-magicians.org/)で議論スレッドを開くことをお勧めします。

アイデアが承認されたら、次の責任は(EIPを通じて)そのアイデアをレビュー担当者とすべての関係者に提示し、エディター、デベロッパー、コミュニティにフィードバックを求めることです。あなたのEIPに対する関心が、実装に必要な作業量や、それに準拠する必要のある当事者の数に見合ったものであるかどうかを評価する必要があります。たとえば、CoreカテゴリのためのEIPの実装には、ERCカテゴリのEIPよりもはるかに多くの作業が必要で、Ethereumクライアントチームからの十分な関心が必要です。コミュニティからのネガティブなフィードバックは考慮され、EIPがDraft状態から進めなくなる可能性があります。

### Coreカテゴリのためのプロセス

Coreカテゴリのためのプロセスでは、クライアントの実装が**Final**状態(下記の「EIPプロセス」を参照)になるためには、あなたが実装を提供するか、クライアントに実装を行わせる必要があります。

クライアントの実装者にEIPをレビューしてもらうには、AllCoreDevsコールで発表するのが最良の方法です。[AllCoreDevsアジェンダのGitHubイシュー](https://github.com/ethereum/pm/issues)にコメントを投稿してリクエストすることができます。

AllCoreDevsコールには3つの目的があります。1つ目は、EIPの技術的な長所を議論すること。2つ目は、他のクライアントが何を実装するかを把握すること。3つ目は、ネットワークアップグレードのためのEIP実装を調整することです。

これらのコールでは通常、「大まかなコンセンサス」が得られます。この「大まかなコンセンサス」は、EIPが分裂を引き起こすほど論争的ではなく、技術的に健全であるという前提に基づいています。

:warning: EIPプロセスとAllCoreDevsコールは、論争的な非技術的な問題に対処するために設計されたものではありませんが、他の方法がないため、しばしばそれらに巻き込まれてしまいます。これにより、クライアントの実装者がコミュニティの意向を把握しようとする負担が増大し、EIPとAllCoreDevsコールの技術的な調整機能が阻害されます。EIPの世話をする際は、EIPに関するEthereum Magiciansフォーラムのスレッドに、可能な限り多くのコミュニティ議論を含めるか、リンクすることで、コンセンサスを形成するプロセスを容易にすることができます。

*要するに、あなたの役割は、以下の様式とフォーマットを使ってEIPを書き、適切なフォーラムでの議論を世話し、そのアイデアに関するコミュニティのコンセンサスを形成することです。*

### EIPプロセス

すべてのトラックのEIPについて、以下の標準化プロセスが適用されます:

![EIPステータスダイアグラム](../assets/eip-1/EIP-process-update.jpg)

**Idea** - 事前ドラフトの段階のアイデア。EIPリポジトリ内では追跡されません。

**Draft** - 正式に追跡されるEIPの最初の段階。EIPエディターが適切な形式で整えられた場合、EIPリポジトリにマージされます。

**Review** - EIP著者がピアレビューの準備ができ、リクエストしたことを示します。

**Last Call** - EIPを**Final**に移行する前の最終レビューウィンドウです。EIPエディターが`Last Call`ステータスを割り当て、通常14日後の`last-call-deadline`を設定します。

この期間に必要な規範的な変更がある場合は、EIPを`Review`に戻します。

**Final** - これはファイナルな標準を表します。FinalステータスのEIPは最終的な状態にあり、エラッタの修正と非規範的な明確化以外は更新されるべきではありません。

Last CallからFinalへの移行PRには、ステータスの更新以外の変更を含めるべきではありません。内容や編集上の提案変更は、このステータス更新PRとは別に事前にコミットされるべきです。

**Stagnant** - Draft、Review、Last CallのいずれかのステータスにあるEIPで、6か月以上非アクティブだった場合は`Stagnant`に移行されます。著者やEIPエディターがDraftまたはそれ以前のステータスに復活させることができます。復活されない場合、提案は永久にこのステータスにとどまります。

>*EIP著者は、EIPのステータスが自動的に変更された場合に通知されます*

**Withdrawn** - EIP著者(ら)がEIPの提案を取り下げました。この状態は最終的なものであり、このEIP番号を使って再び提案することはできません。後日アイデアが追求される場合は、新しい提案として扱われます。

**Living** - 最終的な状態に達せず、継続的に更新されるように設計されたEIPに割り当てられる特別なステータスです。EIP-1がその代表例です。

## 成功したEIPに含まれるべきもの

各EIPには以下の部分が含まれるべきです:

- 前文 - EIPの番号、簡潔な説明タイトル(最大44文字)、説明(最大140文字)、著者情報などのメタデータを含むRFC 822形式のヘッダー。カテゴリに関わらず、タイトルと説明にはEIP番号を含めないでください。詳細は[以下](./eip-1.md#eip-header-preamble)を参照してください。
- 概要 - 技術的な要約となる短い段落です。この仕様セクションの要点を簡潔かつ人間が読みやすい形で表現する必要があります。
- 動機 *(オプション)* - Ethereumプロトコルを変更するEIPには、動機セクションが重要です。既存のプロトコル仕様が、EIPが解決しようとする問題に不十分であることを明確に説明する必要があります。動機が明らかな場合は、このセクションを省略できます。
- 仕様 - 新機能の構文と意味論を詳細に記述する必要があります。現在のEthereum プラットフォーム(besu、erigon、ethereumjs、go-ethereum、nethermind、その他)で競合し、相互運用可能な実装が可能になるよう、仕様は十分に詳細でなければなりません。

- 根拠 - 仕様の詳細を説明し、設計の動機と特定の設計決定の理由を述べます。検討された代替案や関連する取り組みなどについて説明する必要があります。根拠では、EIPをめぐる議論で提起された重要な異論や懸念についても議論する必要があります。
- 下位互換性 *(オプション)* - 下位互換性を損なうすべてのEIPには、この非互換性とその影響を説明するセクションが含まれている必要があります。著者がこの非互換性にどのように対処するかを説明する必要があります。提案に下位互換性がない場合は省略できますが、下位互換性がある場合は必ず含める必要があります。
- テストケース *(オプション)* - コンセンサスの変更に影響するEIPには、実装のためのテストケースが必須です。テストは、EIP内にデータ(入力/期待出力ペアなど)として埋め込むか、`../assets/eip-###/<filename>`に含める必要があります。コアでない提案の場合は、このセクションを省略できます。
- 参考実装 *(オプション)* - この仕様の理解や実装を支援するための参考/サンプル実装を含む任意のセクションです。すべてのEIPについて省略できます。
- セキュリティ上の考慮事項 - すべてのEIPには、提案された変更に関連するセキュリティの影響/考慮事項を議論するセクションが含まれている必要があります。セキュリティに関連する設計上の決定、懸念、重要な議論、実装固有のガイダンスとピットフォール、脅威とリスクの概要、それらへの対処方法などの情報を含める必要があります。セキュリティ上の考慮事項のセクションがないEIPは却下されます。レビュー担当者によってセキュリティ上の考慮事項が十分であると判断されない限り、EIPは**Final**ステータスに進めません。
- 著作権放棄 - すべてのEIPは公有財産でなければなりません。著作権放棄は[CC0](../LICENSE.md)を参照し、以下の文言を使用する必要があります: `Copyright and related rights waived via [CC0](../LICENSE.md).`

## EIPのフォーマットとテンプレート

EIPは[markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)フォーマットで書く必要があります。[テンプレート](https://github.com/ethereum/EIPs/blob/master/eip-template.md)に従ってください。

## EIPヘッダー前文

各EIPは、3つのハイフン(`---`)で囲まれた[RFC 822](https://www.ietf.org/rfc/rfc822.txt)形式のヘッダー前文で始まる必要があります。このヘッダーは、Jekyllによる["front matter"](https://jekyllrb.com/docs/front-matter/)とも呼ばれています。ヘッダーは以下の順序で表示される必要があります。

`eip`: *EIP番号*

`title`: *EIPのタイトルは数語で、完全な文章ではありません*

`description`: *説明は1つの完全な(短い)文章です*

`author`: *著者の名前やユーザー名、または名前とメールアドレスのリスト。詳細は以下の通りです。*

`discussions-to`: *公式の議論スレッドを指すURL*

`status`: *Draft、Review、Last Call、Final、Stagnant、Withdrawn、Living*

`last-call-deadline`: *(オプションのフィールド、`Last Call`ステータスの場合のみ必要) Last Callの期限*

`type`: *`Standards Track`、`Meta`、または`Informational`のいずれか*

`category`: *`Core`、`Networking`、`Interface`、または`ERC`のいずれか* (オプションのフィールド、`Standards Track`EIPの場合のみ必要)

`created`: *EIPが作成された日付*

`requires`: *EIP番号* (オプションのフィールド)

`withdrawal-reason`: *(オプションのフィールド、`Withdrawn`ステータスの場合のみ必要) EIPが取り下げられた理由の文章*

リストを許可するヘッダーは、要素をカンマで区切る必要があります。

日付を要求するヘッダーは常にISO 8601形式(yyyy-mm-dd)で行う必要があります。

### `author`ヘッダー

`author`ヘッダーには、EIPの著者/所有者の名前、メールアドレス、またはユーザー名が記載されます。匿名を希望する場合は、ユーザー名のみ、または名前とユーザー名を使用できます。`author`ヘッダーの値の形式は以下のようになります:

> Random J. User &lt;address@dom.ain&gt;

または

> Random J. User (@username)

または

> Random J. User (@username) &lt;address@dom.ain&gt;

メールアドレスやGitHubユーザー名が含まれる場合、および

> Random J. User

メールアドレスもGitHubユーザー名も提供されない場合。

少なくとも1人の著者がGitHubユーザー名を使う必要があります。これにより、変更リクエストの通知を受け取り、それらを承認または拒否する機能を持つことができます。

### `discussions-to`ヘッダー

EIPがドラフト段階にある間は、`discussions-to`ヘッダーにEIPが議論されているURLが示されます。

推奨される議論URLは[Ethereum Magicians](https://ethereum-magicians.org/)のトピックです。GitHubのプルリクエスト、一時的なURL、時間とともにロックされる可能性のあるURLを指すことはできません。

### `type`ヘッダー

`type`ヘッダーは、EIPのタイプ(Standards Track、Meta、Informational)を指定します。Standardsトラックの場合は、サブカテゴリ(core、networking、interface、ERC)も含める必要があります。

### `category`ヘッダー

`category`ヘッダーは、EIPのカテゴリを指定します。これはStandards Trackのみに必要です。

### `created`ヘッダー

`created`ヘッダーには、EIPに番号が割り当てられた日付が記録されます。両方のヘッダーはyyyy-mm-dd形式で記述する必要があります。

### `requires`ヘッダー

EIPには`requires`ヘッダーが含まれる場合があり、このEIPが依存するEIP番号を示します。そのような依存関係がある場合は、このフィールドが必要です。

`requires`の依存関係は、現在のEIPを理解または実装するためには、別のEIPからの概念や技術的要素が必要な場合に作成されます。単に別のEIPに言及しただけでは、必ずしも依存関係が生まれるわけではありません。

## 外部リソースへのリンク

以下に記載された特定の例外を除いて、外部リソースへのリンクを**含めるべきではありません**。外部リソースは予期せず消失、移動、または変更される可能性があります。

許可された外部リソースを管理するプロセスは、[EIP-5757](./eip-5757.md)で説明されています。

### 実行クライアント仕様

通常のMarkdownの構文を使って、Ethereum実行クライアント仕様へのリンクを含めることができます。例:

```markdown
[Ethereum Execution Client Specifications](https://github.com/ethereum/execution-specs/blob/9a1f22311f517401fed6c939a159b55600c454af/README.md)
```

これは以下のように表示されます:

[Ethereum Execution Client Specifications](https://github.com/ethereum/execution-specs/blob/9a1f22311f517401fed6c939a159b55600c454af/README.md)

許可されるExecutionクライアント仕様のURLは、特定のコミットにアンカーされている必要があり、次の正規表現に一致する必要があります:

```regex
^(https://github.com/ethereum/execution-specs/(blob|commit)/[0-9a-f]{40}/.*|https://github.com/ethereum/execution-specs/tree/[0-9a-f]{40}/.*)$
```

### コンセンサスレイヤー仕様

通常のMarkdownの構文を使って、Ethereum コンセンサスレイヤー仕様の特定のコミットへのリンクを含めることができます。例:

```markdown
[Beacon Chain](https://github.com/ethereum/consensus-specs/blob/26695a9fdb747ecbe4f0bb9812fedbc402e5e18c/specs/sharding/beacon-chain.md)
```

これは以下のように表示されます:

[Beacon Chain](https://github.com/ethereum/consensus-specs/blob/26695a9fdb747ecbe4f0bb9812fedbc402e5e18c/specs/sharding/beacon-chain.md)

許可されるコンセンサスレイヤー仕様のURLは、特定のコミットにアンカーされている必要があり、次の正規表現に一致する必要があります:

```regex
^https://github.com/ethereum/consensus-specs/(blob|commit)/[0-9a-f]{40}/.*$
```

### ネットワーキング仕様

通常のMarkdownの構文を使って、Ethereumネットワーキング仕様の特定のコミットへのリンクを含めることができます。例:

```markdown
[Ethereum Wire Protocol](https://github.com/ethereum/devp2p/blob/40ab248bf7e017e83cc9812a4e048446709623e8/caps/eth.md)
```

これは以下のように表示されます:

[Ethereum Wire Protocol](https://github.com/ethereum/devp2p/blob/40ab248bf7e017e83cc9812a4e048446709623e8/caps/eth.md)

許可されるネットワーキング仕様のURLは、特定のコミットにアンカーされている必要があり、次の正規表現に一致する必要があります:

```regex
^https://github.com/ethereum/devp2p/(blob|commit)/[0-9a-f]{40}/.*$
```

### World Wide Web Consortium (W3C)

W3C「Recommendation」ステータスの仕様へのリンクは、通常のMarkdownの構文を使って含めることができます。例えば、以下のリンクは許可されます:

```markdown
[Secure Contexts](https://www.w3.org/TR/2021/CRD-secure-contexts-20210918/)
```

これは以下のように表示されます:

[Secure Contexts](https://www.w3.org/TR/2021/CRD-secure-contexts-20210918/)

許可されるW3C推奨仕様のURLは、技術レポートの名前空間にある日付付きの仕様を指す必要があり、次の正規表現に一致する必要があります:

```regex
^https://www\.w3\.org/TR/[0-9][0-9][0-9][0-9]/.*$
```

### Web Hypertext Application Technology Working Group (WHATWG)

通常のMarkdownの構文を使って、WHATWG仕様へのリンクを含めることができます。例:

```markdown
[HTML](https://html.spec.whatwg.org/commit-snapshots/578def68a9735a1e36610a6789245ddfc13d24e0/)
```

これは以下のように表示されます:

[HTML](https://html.spec.whatwg.org/commit-snapshots/578def68a9735a1e36610a6789245ddfc13d24e0/)

許可されるWHATWG仕様のURLは、`spec`サブドメインで定義された仕様(アイデア仕様は許可されない)に、特定のコミットスナップショットにアンカーされている必要があり、次の正規表現に一致する必要があります:

```regex
^https:\/\/[a-z]*\.spec\.whatwg\.org/commit-snapshots/[0-9a-f]{40}/$
```

WHATWGによって推奨されていませんが、EIPは特定のコミットにアンカーされる必要があります。これにより、EIPが最終化された時点での生きた標準のバージョンを読者が参照できるようになります。これにより、EIPが参照するバージョンと現在の生きた標準との互換性を維持することができます。

### Internet Engineering Task Force (IETF)

通常のMarkdownの構文を使って、IETF Request For Comment (RFC)仕様へのリンクを含めることができます。例:

```markdown
[RFC 8446](https://www.rfc-editor.org/rfc/rfc8446)
```

これは以下のように表示されます:

[RFC 8446](https://www.rfc-editor.org/rfc/rfc8446)

許可されるIETF仕様のURLは、RFCナンバーが割り当てられた仕様(インターネットドラフトは許可されない)を指す必要があり、次の正規表現に一致する必要があります:

```regex
^https:\/\/www.rfc-editor.org\/rfc\/.*$
```

### Bitcoin Improvement Proposal

通常のMarkdownの構文を使って、Bitcoin Improvement Proposalへのリンクを含めることができます。例:

```markdown
[BIP 38](https://github.com/bitcoin/bips/blob/3db736243cd01389a4dfd98738204df1856dc5b9/bip-0038.mediawiki)
```

これは以下のように表示されます:


[BIP 38](https://github.com/bitcoin/bips/blob/3db736243cd01389a4dfd98738204df1856dc5b9/bip-0038.mediawiki)

許可されるBitcoin Improvement ProposalのURLは、特定のコミットにアンカーされている必要があり、次の正規表現に一致する必要があります:

```regex
^(https://github.com/bitcoin/bips/blob/[0-9a-f]{40}/bip-[0-9]+\.mediawiki)$
```

### National Vulnerability Database (NVD)

National Institute of Standards and Technology (NIST)が公開する Common Vulnerabilities and Exposures (CVE)システムへのリンクは、最新の変更日付で修飾して含めることができます。以下の構文を使用します:

```markdown
[CVE-2023-29638 (2023-10-17T10:14:15)](https://nvd.nist.gov/vuln/detail/CVE-2023-29638)
```

これは以下のように表示されます:

[CVE-2023-29638 (2023-10-17T10:14:15)](https://nvd.nist.gov/vuln/detail/CVE-2023-29638)

### Digital Object Identifier System

Digital Object Identifier (DOI)で修飾されたリンクは、以下の構文を使って含めることができます:

````markdown
これは脚注付きの文章です。[^1]

[^1]:
    ```csl-json
    {
      "type": "article",
      "id": 1,
      "author": [
        {
          "family": "Jameson",
          "given": "Hudson"
        }
      ],
      "DOI": "00.0000/a00000-000-0000-y",
      "title": "An Interesting Article",
      "original-date": {
        "date-parts": [
          [2022, 12, 31]
        ]
      },
      "URL": "https://sly-hub.invalid/00.0000/a00000-000-0000-y",
      "custom": {
        "additional-urls": [
          "https://example.com/an-interesting-article.pdf"
        ]
      }
    }
    ```
````

これは以下のように表示されます:

<!-- markdownlint-capture -->
<!-- markdownlint-disable code-block-style -->

これは脚注付きの文章です。[^1]

[^1]:
    ```csl-json
    {
      "type": "article",
      "id": 1,
      "author": [
        {
          "family": "Jameson",
          "given": "Hudson"
        }
      ],
      "DOI": "00.0000/a00000-000-0000-y",
      "title": "An Interesting Article",
      "original-date": {
        "date-parts": [
          [2022, 12, 31]
        ]
      },
      "URL": "https://sly-hub.invalid/00.0000/a00000-000-0000-y",
      "custom": {
        "additional-urls": [
          "https://example.com/an-interesting-article.pdf"
        ]
      }
    }
    ```

<!-- markdownlint-restore -->

[Citation Style Language Schema](https://resource.citationstyles.org/schema/v1.0/input/json/csl-data.json)でサポートされているフィールドを参照してください。スキーマに対する検証に加えて、参考文献にはDOIと少なくとも1つのURLが含まれている必要があります。

トップレベルのURLフィールドは、無料で閲覧できる参照文書のコピーにリンクする必要があります。`additional-urls`の下の値も、参照文書のコピーにリンクする必要がありますが、料金を請求することができます。

## 他のEIPへのリンク

他のEIPへの参照は、参照しているEIP番号`EIP-N`の形式で行う必要があります。EIPを参照する際は、最初の参照時に必ず相対Markdownリンクを付ける必要があり、後続の参照でも付けることができます。リンクは必ず相対パスで行い、このGitHubリポジトリ、そのフォーク、メインのEIPサイト、メインEIPサイトのミラーなどで機能するようにする必要があります。例えば、このEIPへのリンクは`./eip-1.md`のようになります。

## 補助ファイル

画像、図、補助ファイルは、そのEIPの`assets/eip-N`サブディレクトリ(NはそのファイルのあるEIP番号)に含める必要があります。EIP内でイメージにリンクする際は、`../assets/eip-N/image.png`のような相対リンクを使用してください。

## EIPの所有権の移転

時折、EIPの所有権を新しいチャンピオンに移譲する必要が生じます。一般的に、元の著者を移譲後のEIPの共著者として残したいと思いますが、これは元の著者次第です。EIPの所有権を移譲する良い理由は、元の著者がもはや更新する時間や興味がなくなった場合、または連絡が取れなくなった(つまり、メールに返事がない)場合です。EIPの方向性に同意しないからEIPの所有権を移譲するのは良い理由ではありません。EIPに関するコンセンサスを形成するよう努力しますが、それが不可能な場合は、常に競合するEIPを提出することができます。

EIPの所有権を引き継ぐことに興味がある場合は、元の著者とEIPエディターの両方に所有権の引き継ぎを要求するメッセージを送ってください。元の著者がタイムリーにメールに返事しない場合、EIPエディターが一方的な判断を下します(そのような決定は取り消すことができないわけではありません:)).

## EIPエディター

現在のEIPエディターは以下の通りです:

- Alex Beregszaszi (@axic)
- Greg Colvin (@gcolvin)
- Matt Garnett (@lightclient)
- Sam Wilson (@SamWilsn)
- Zainan Victor Zhou (@xinbenlv)
- Gajinder Singh (@g11tech)

EIPエディターの名誉職は以下の通りです:

- Casey Detrio (@cdetrio)
- Gavin John (@Pandapip1)
- Hudson Jameson (@Souptacular)
- Martin Becze (@wanderer)
- Micah Zoltu (@MicahZoltu)
- Nick Johnson (@arachnid)
- Nick Savers (@nicksavers)
- Vitalik Buterin (@vbuterin)

EIPエディターになりたい場合は、[EIP-5069](./eip-5069.md)を確認してください。

## EIPエディターの責任

新しいEIPが提出されると、エディターは以下のことを行います:

- EIPが準備できているかどうかを確認します: 健全で完全であること。アイデアは技術的に意味があるはずですが、最終ステータスになる可能性が低いかもしれません。
- タイトルは内容を正確に説明している必要があります。
- 言語(スペリング、文法、文章構造など)、マークアップ(GitHub Flavored Markdown)、コードスタイルをチェックします。

EIPが準備できていない場合、エディターは具体的な指示とともに、修正のためにそれを著者に返送します。

EIPがリポジトリに準備できたら、EIPエディターは以下のことを行います:

- EIP番号を割り当てます(通常は順次; エディターは番号の奪い合いが疑われる場合は再割り当てできます)
- 対応する[プルリクエスト](https://github.com/ethereum/EIPs/pulls)をマージします
- EIP著者に次のステップを伝えるメッセージを送ります。

多くのEIPは、Ethereumコードベースへの書き込みアクセスを持つ開発者によって書かれ、維持されています。EIPエディターはEIPの変更を監視し、構造、文法、スペリング、マークアップの間違いを修正します。

エディターはEIPに対して判断を下すわけではありません。私たちは管理・編集の部分を行うだけです。

## スタイルガイド

### タイトル

前文の`title`フィールド:

- 「standard」や同様の言葉を含めるべきではありません。
- EIPの番号を含めるべきではありません。

### 説明

前文の`description`フィールド:

- 「standard」や同様の言葉を含めるべきではありません。
- EIPの番号を含めるべきではありません。

### EIP番号

`category`が`ERC`のEIPを参照する場合は、`ERC-X`の形式(XはそのEIPの割り当て番号)で記述する必要があります。他のカテゴリのEIPを参照する場合は、`EIP-X`の形式(Xはそのエイピーの割り当て番号)で記述する必要があります。

### RFC 2119およびRFC 8174

EIPでは、[RFC 2119](https://www.ietf.org/rfc/rfc2119.html)および[RFC 8174](https://www.ietf.org/rfc/rfc8174.html)の用語法に従うことが推奨されており、仕様セクションの冒頭に以下を挿入する必要があります:

> The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

## 履歴

このドキュメントは、[Bitcoin's BIP-0001](https://github.com/bitcoin/bips)を大幅に参考にしており、その多くはそのままコピーされています。BIP-0001はAmir Taakiによって書かれ、それ自体は[Python's PEP-0001](https://peps.python.org/)に由来しています。多くの箇所でテキストがそのまま複製されています。PEP-0001のテキストはBarry Warsaw、Jeremy Hylton、David Goodgerによって書かれましたが、彼らはEthereum Improvement Processでの使用について責任を負うものではなく、Ethereumや EIPに関する技術的な質問については相談されるべきではありません。すべてのコメントはEIPエディターに向けてください。

## 著作権

Copyright and related rights waived via [CC0](../LICENSE.md).