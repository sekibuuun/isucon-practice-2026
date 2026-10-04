# サーバー設定・Terraform


## nginx・Linux

nginxの圧縮、buffering、upstream keepalive、worker・接続数は観測から選ぶ。巨大ファイル、ストリーミング、アプリ占有時間、CPU・帯域を合わせて評価する。対象版のデフォルトを調べ、実測から設定を選ぶ。

静的配信は内容・Content-Type・URI・認可を維持する。画像の正本と再生成可能なファイルcacheを分ける。欠損時のアプリfallback、更新・initialize、複数台・再起動での保持を確認する。CDNは一般的な手法だが競技の許可外資源へ処理を委譲しない。

FD、接続キュー、ポート、I/O、CPU steal等を測る。sysctlを闇雲に増やさない。変更は設定ファイル・systemd等に残し、CI反映と再起動後の読み込みを確認する。systemd起動依存と再試行、必要なmount・権限・環境変数を確認する。nginx -t等の対象環境検証もCIで実行する。

## AWS・Terraform

既存のprovider版、state/backend、workspace、region、AMI、許可されたinstance種別・台数・networkを調べる。競技開始後に指定されるAMI等を事前に決めない。変更はTerraformへ記述し、fmt -check、validate、planを使い分ける。

applyはCI経由。planの差分を確認し、対象・許可条件・置換/削除・ディスク・データ保持への影響をレビューする。レビューした保存planとapplyを対応付け、state lockingを利用する。state・planには秘密が含まれ得るため非公開で扱い、Gitへ登録しない。AWS認証は既存方式を使い、コードや記録へ資格情報を貼らない。

user_dataは既存instanceの再起動ごとに設定を再適用する保証ではない。初回構築と継続的設定反映を区別し、既存CIの反映手順に接続する。TerraformでAWSを管理しても、nginxやsystemdの実体が反映されたことは別途確認する。
