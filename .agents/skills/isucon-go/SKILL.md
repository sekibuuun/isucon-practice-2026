---
name: isucon-go
description: ISUCONのGoアプリで計測されたCPU・割り当て・ロック・HTTP処理のボトルネックを改善する。
---

# Goアプリ改善

単独利用では [ルール](../isucon-workflow/references/regulations.md) と [共通ループ](../isucon-workflow/references/loop.md) を読む。workflowから利用中なら再読込しない。[Goの改善判断](references/go.md) は必要な箇所だけ参照する。

- go.mod、実行Goバージョン、起動方式、ホットパスとpprofを確認する。計測がなければisucon-measureで根拠を取る。
- 毎回のテンプレート解析・外部コマンド・重複計算・割り当て・不要な転送など、観測されたコストを減らす。形式・認証・順序・エラー応答は維持する。
- SQLや接続待ちが主因ならisucon-db、重複計算の保持ならisucon-cache、配信経路ならisucon-infra-ciを読む。
- 実装後は関連テスト、必要ならrace検査、セルフレビューを行う。診断ビルドやrace有効ビルドを性能採点に使わない。
- microbenchmarkは補助とし、対象環境での修正前後の有効スコアをrun・reviewに残して人間の採否判断を待つ。
