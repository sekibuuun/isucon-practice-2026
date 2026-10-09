---
name: isucon-cache
description: ISUCONで重い読み取り・重複計算のキャッシュを、更新整合性・初期化・再起動要件を守って設計する。
---

# キャッシュ改善

最初に [ルール](../isucon-workflow/references/regulations.md) と [共通ループ](../isucon-workflow/references/loop.md) を読む。[キャッシュ判断](references/cache.md) を参照する。

- SQL・取得列・重複処理の改善で足りないか確認し、保存・無効化のコストより効果が大きい対象を選ぶ。
- 正本、keyに必要なユーザー・権限・パラメータ、更新・削除・初期化経路、鮮度要件をspecへ書く。
- 許可されたサーバー上のメモリ・KVSを使う。揮発性キャッシュを書き込みデータの唯一の保存先にしない。
- miss、同時生成、更新との競合、初期化後の旧値、複数台、再起動の動作を検証・セルフレビューする。
- CIのcold/warm計測で有効スコアとmiss率・生成コスト・メモリを比較し採否を記録する。
