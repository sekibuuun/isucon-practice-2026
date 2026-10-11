---
name: isucon-db
description: ISUCONのGo・MySQLでスロークエリ、N+1、実行計画、接続待ちを計測し改善する。
---

# DB改善

単独利用では [ルール](../isucon-workflow/references/regulations.md) と [共通ループ](../isucon-workflow/references/loop.md) を読む。workflowから利用中なら再読込しない。[DB判断](references/database.md) は必要な箇所だけ参照する。

1. MySQL版、schema、SQL呼び出し経路、slow log、DBStats、初期化手順を確認する。
2. 総時間・回数・走査行数から支配的なクエリを選ぶ。EXPLAINと現実のデータ分布で仮説を確認する。
3. 同じボトルネックの候補から一つ選び、インデックス、N+1のバッチ化、取得列削減、接続再利用などを一つの仮説単位で変更する。
4. DDL・設定・初期化互換性はコードにし、確認済みの操作経路で反映する。直接SQLで対象環境の状態だけ変更しない。
5. 結果の件数・順序・NULL・権限・トランザクション・永続性をセルフレビューする。対象環境のベンチで修正前後のスコア、SQL総時間・回数を比較し、人間の採否判断を待つ。
