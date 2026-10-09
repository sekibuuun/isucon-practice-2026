---
name: isucon-db
description: ISUCONのGo・MySQLでスロークエリ、N+1、実行計画、接続待ちを計測し改善する。
---

# DB改善

最初に [ルール](../isucon-workflow/references/regulations.md) と [共通ループ](../isucon-workflow/references/loop.md) を読む。[DB判断](references/database.md) を参照する。

1. MySQL版、schema、SQL呼び出し経路、slow log、DBStats、初期化手順を確認する。
2. 総時間・回数・走査行数から支配的なクエリを選ぶ。EXPLAINと現実のデータ分布で仮説を確認する。
3. インデックス、N+1のバッチ化、取得列削減、接続再利用を一つの仮説単位で変更する。
4. DDL・設定・初期化互換性はコードにし、確認済みの操作経路で反映する。直接SQLで対象環境の状態だけ変更しない。
5. 結果の件数・順序・NULL・権限・トランザクション・永続性をセルフレビューする。対象環境のベンチでスコア、SQL総時間・回数を比較し採否を記録する。
