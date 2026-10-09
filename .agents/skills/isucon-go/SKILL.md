---
name: isucon-go
description: ISUCONのGoアプリで計測されたCPU・割り当て・ロック・HTTP処理のボトルネックを改善する。
---

# Goアプリ改善

最初に [ルール](../isucon-workflow/references/regulations.md) と [共通ループ](../isucon-workflow/references/loop.md) を読む。[Goの改善判断](references/go.md) を必要な箇所だけ参照する。

- go.mod、実行Goバージョン、起動方式、ホットパスとpprofを確認する。計測がなければisucon-measureで根拠を取る。
- 毎回のテンプレート解析・外部コマンド・重複計算・割り当て・不要な転送など、観測されたコストを減らす。形式・認証・順序・エラー応答は維持する。
- SQLや接続待ちが主因ならisucon-db、重複計算の保持ならisucon-cache、配信経路ならisucon-infra-ciを読む。
- 実装後は関連テスト、必要ならrace検査、セルフレビューを行う。診断ビルドやrace有効ビルドを性能採点に使わない。
- microbenchmarkは補助として使い、対象環境へ反映後の有効スコアで採否を決める。run・reviewに結果を残す。
