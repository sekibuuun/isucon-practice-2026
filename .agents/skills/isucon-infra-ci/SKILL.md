---
name: isucon-infra-ci
description: ISUCONのnginx・Linux設定とTerraform変更をコード管理し、GitHub Actions経由でAWSへ反映・再起動・ベンチ起動する。
---

# インフラ・CI

最初に [ルール](../isucon-workflow/references/regulations.md) と [共通ループ](../isucon-workflow/references/loop.md) を読む。操作前に [CI手順](references/ci.md)、サーバー変更時に [サーバー・Terraform](references/server.md) を読む。

- 既存構成・Terraform・workflow・認証・対象環境を調べる。構築が依頼されていなければ不足を報告し、勝手にAWSやCIを作らない。
- アプリ・SQL・nginx・systemd・Linux設定をコード化する。AWS設定はTerraform。直接SSH・SSM・手動SQLで状態だけ書き換えない。
- ローカルは編集・静的検証、CIは制御、性能計測はAWS対象環境。本番の負荷生成は公式ベンチマーカーの正式手段を使う。
- 設定の検証・セルフレビュー後、許可範囲内でCIを起動し、runと実際の対象SHA、結果を照合する。
- 再起動・データ保持・起動順・性能再現まで記録する。CI成功や単純health checkだけで競技の完了にしない。
