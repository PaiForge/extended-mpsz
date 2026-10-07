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

2026 年 10 月時点の riichi-mahjong は旧仕様（1.x）を実装しており、2.0 には未対応です。具体的には次の差分があります。

- 方向注釈 `-` `=` `+`、加槓 `{...}`、`^` を解釈できない。鳴き元はチー=上家、ポン・大明槓=対面を固定で設定する
- `0` および字牌の範囲外の数字を黙って読み飛ばす（2.0 では拒否が必須）
- `[1m2m3m]` のような複数サフィックスのブロックを受理する
- 牌種 ID（`HaiKindId`）は 34 種のみで赤属性を持たないため、赤 5 の保持には牌 ID（`HaiId`）側の対応が必要
- 面子の型（`Furo`）に鳴いた牌・加槓牌のフィールドがない
- 正規形への変換は riichi-mahjong には無く、mahjong-scoring 側の直列化が独自に純手牌の整列を行っている

### 公開 API

解析関数（`parseMspz` `parseExtendedMspz`）は `neverthrow` の `Result` を返し、例外は投げません。判定関数（`isMspz` `isExtendedMspz`）は型ガードで、`boolean` を返します。

| 関数 | 説明 |
|---|---|
| `parseMspz(input: string): Result<Tehai, MspzParseError>` | 標準 MPSZ（面子ブロックなし）を解析し、全牌を `closed` に格納した `Tehai` を返す |
| `parseExtendedMspz(input: string): Result<Tehai, MspzParseError>` | 拡張 MPSZ を解析し、純手牌を `closed`、面子ブロックを `exposed` に格納した `Tehai` を返す |
| `isMspz(input: string): input is MspzString` | 標準 MPSZ として書式が正しいかを判定する |
| `isExtendedMspz(input: string): input is ExtendedMspzString` | `[` または `(` を含み、かつ拡張 MPSZ として書式が正しいかを判定する。括弧を含まない文字列は正しい MPSZ でも `false` |

```typescript
import { parseExtendedMspz } from "@pai-forge/riichi-mahjong";

const result = parseExtendedMspz("123m[456p]"); // 1.x 表記
if (result.isOk()) {
  result.value.closed;  // [1m, 2m, 3m]
  result.value.exposed; // [ { type: "Shuntsu", hais: [4p, 5p, 6p], furo: { type: "Chi", from: Kamicha } } ]
}
```

暗槓は `exposed` に `furo` を持たない `Kantsu` として格納されます。
この実装状況は riichi-mahjong 側の情報であり、2.0 対応が進んだ時点で riichi-mahjong の文書に移します。

## 関連する記法

- [kobalab/majiang-core](https://github.com/kobalab/majiang-core) の面子表記は、サフィックスを前置する `m123` 形式で、鳴き元の記号 `-`（上家）`=`（対面）`+`（下家）の意味は本記法と共通です。本記法は記号を鳴いた牌に付け、位置に意味を持たせない点が異なります。
- 天鳳の牌理などで使われる `123m456p` 形式（赤を `0` で表す）は、本記法の純手牌部分と互換です。

## 来歴

- 旧称は Extended MSPZ（拡張MSPZ）です。2026 年 10 月に Extended MPSZ へ改称しました。記法の内容は変わっていません。
- 以前は [PaiForge/docs](https://github.com/PaiForge/docs) の 1 ファイルとして管理していました。本リポジトリはその履歴を引き継いでいます。

## ライセンス

本リポジトリの文書は [Creative Commons Attribution 4.0 International（CC BY 4.0）](https://creativecommons.org/licenses/by/4.0/) の下で提供します。

© 2026 PaiForge
