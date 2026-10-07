# Extended MPSZ

リーチ麻雀の 1 人の手牌（純手牌と成立済みの面子）を、単一の ASCII 文字列で表す記法の仕様です。`123m456p789s11z` 形式の MPSZ 記法を基底に、副露 `[...]`・加槓 `{...}`・暗槓 `(...)` の面子ブロックと、鳴き元・鳴いた牌・加槓牌を示す注釈、赤ドラを表す `0` を加えています。

```
123m789s11z[40-6p]{7=777^z}
```

- 純手牌: 1m 2m 3m 7s 8s 9s 東 東
- `[40-6p]`: 赤 5p を上家からチーした順子
- `{7=777^z}`: 中を対面からポンした刻子に、中を加槓した槓子

## 文書

| ファイル | 内容 |
|---|---|
| [SPEC.md](SPEC.md) | 仕様本文。バージョン 2.0、状態は草案（Draft） |
| [CHANGELOG.md](CHANGELOG.md) | 仕様の変更履歴 |
| [LICENSE](LICENSE) | CC BY 4.0 の全文 |

## バージョンと状態

仕様はセマンティックバージョニングに従います。対象は「正当な文字列の集合」「各文字列の意味」「正規形」の 3 つです。現在は草案（Draft）で、破壊的変更を MAJOR を上げずに行うことがあります。規則の詳細と将来の拡張の方式は SPEC.md の第 11 節を参照してください。

## 参照実装

参照実装は [PaiForge/riichi-mahjong](https://github.com/PaiForge/riichi-mahjong)（TypeScript、npm の `@pai-forge/riichi-mahjong`）です。

| riichi-mahjong | 対応する仕様 |
|---|---|
| 0.12.0 以降 | 2.0（Draft） |
| 0.11.x 以前 | 1.x |

実装状況の詳細は riichi-mahjong 側の文書（README の「対応仕様」、CHANGELOG）を参照してください。

## 関連する記法

- [kobalab/majiang-core](https://github.com/kobalab/majiang-core) の面子表記は、サフィックスを前置する `m123` 形式で、鳴き元の記号 `-`（上家）`=`（対面）`+`（下家）の意味は本記法と共通です。本記法は記号を鳴いた牌に付け、位置に意味を持たせない点が異なります。
- 天鳳の牌理などで使われる `123m456p` 形式（赤を `0` で表す）は、本記法の純手牌部分と互換です。

## 来歴

- 旧称は Extended MSPZ（拡張MSPZ）です。2026 年 10 月に Extended MPSZ へ改称しました。記法の内容は変わっていません。
- 以前は [PaiForge/docs](https://github.com/PaiForge/docs) の 1 ファイルとして管理していました。本リポジトリはその履歴を引き継いでいます。

## ライセンス

本リポジトリの文書は [Creative Commons Attribution 4.0 International（CC BY 4.0）](https://creativecommons.org/licenses/by/4.0/) の下で提供します。

© 2026 PaiForge
