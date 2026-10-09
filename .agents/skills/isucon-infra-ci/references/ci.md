# CI操作と結果の照合

## 事前確認

workflowの名前・入力・trigger・ref、GitHub認証、対象AWS、アプリSHA、操作種別、結果保存先を調べる。既存workflowの入力名を使い、存在しないdeploy/benchmark APIを仮定しない。workflow_dispatchはdefault branch上にworkflowが必要。変更対象ブランチのコードを確実にcheckoutする契約を確認する。

分析のみの依頼でpush・dispatchをしない。許可済みの反映範囲内なら毎回の確認は不要だが、環境・コスト・削除などの拡張は別の権限が必要。CI未構築や本番の起動手段未提供ならその部分は未実施と報告する。

## 起動・待機・結果

以下は調査・照合用の例。WORKFLOW、BRANCH、SHA、RUN_IDは確認済みの値へ置換する。

```sh
gh workflow view WORKFLOW
gh run list --workflow WORKFLOW --branch BRANCH --commit SHA --json databaseId,headSha,status,conclusion,createdAt,url
gh run view RUN_ID --json headSha,status,conclusion,url,jobs
gh run view RUN_ID --log-failed
```

操作の入力と起動前時刻をrun.mdに記録し、既存のworkflow契約に沿ってdispatchする。dispatch応答からrun IDが得られるとは仮定しない。workflow・SHA・時刻・実験識別子で新規runを照合する。最新run一件だけを無条件選択しない。候補が曖昧なら状態を確認し、再dispatchしない。

workflowのheadShaと、入力で指定してcheckoutするアプリSHAが異なる構成もある。両者を別々に記録し、runの検索にはworkflowのSHA、計測対象の確認には実際に反映されたアプリSHAを使う。headShaだけをアプリの反映証明にしない。

run IDを記録したら、そのrunを完了まで追う。短い状態確認・利用可能な待機手段を使い、待機中も進捗を伝える。通信断や中断から復帰した場合は同じrunを確認する。cancelled/timeoutでも途中反映され得るため対象状態を調べる。

CIの成功とベンチの有効結果は別。公式起動が非同期なら公式ジョブIDで完了・採点結果まで確認する。対象SHA、ベンチvalidity、エラー、スコア、条件、ログ・プロファイル原本を記録する。取得する成果物に秘密情報を含めない。

## 排他・復旧

同一AWS環境のTerraform・反映・再起動・ベンチは共通の環境単位で排他する。workflow名やブランチ単位だけのconcurrencyでは、別workflowからの干渉を防げない。

連続処理は一つの排他区間で反映→検証→ベンチを行うか、連携した環境ロックを維持する。分離workflowの間に別反映が入らない契約を確認する。稼働中のapply・deploy・benchを新runで自動cancelしない。pendingの置換・順序を保証とみなさず、完了SHAを毎回確認する。

性能退行は候補だけをコードでrevertし、CIで反映・復旧検証する。must修正は保持する。schema復旧でデータを消す操作は自動選択しない。再起動追試では初期化がデータを消す前に、ベンチ中の書き込みが取得できることを確認する。

## 実行場所

練習：サービスと計測対象はAWS。負荷生成も対象から分離したAWS環境で行う。本番：許可された競技サーバーと公式ベンチマーカー。GitHub-hosted runnerはビルド・制御・結果取得に使い、採点用サービス処理や練習の採点負荷生成を載せない。診断の静的解析はローカルでも可能。
