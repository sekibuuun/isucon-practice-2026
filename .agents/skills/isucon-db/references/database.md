# DB判断


## SQL・インデックス

slow logを同じベンチ時間窓で集計する。一件の最大時間だけでなく総時間と頻度を優先する。返却行数に対して走査が多ければ、WHERE・JOIN・ORDER BY・LIMITに沿う複合インデックスを検討する。選択度・左端列・書き込みコスト・容量も見る。

EXPLAINの推定と実測は分ける。EXPLAIN ANALYZEは実際にクエリを実行するため、副作用・高負荷・対応バージョンを確認してから許可範囲内で使う。FORCE INDEXやSTRAIGHT_JOINは実行計画とデータ分布に根拠がある場合だけ使う。

N+1はJOIN・IN・一括取得・集計で減らすが、親ごとのLIMIT、重複、順序、ページング、削除済みデータ、認可を変えない。JOIN後のLIMITで元の件数が変わらないか確認する。SELECT *が大きな画像等を運ぶなら必要列だけ取得する。

## Go接続・永続性

sql.DBを再利用する。DBStatsのWaitCount/WaitDuration等を同期間の差分で観測し、全アプリ台数分の最大接続、DB上限、メモリを合わせて考える。SetMaxOpenConns/SetMaxIdleConns等は実測から値を選ぶ。rowsのclose・走査エラー・tx終了・contextを確認する。

ドライバーのinterpolateParamsやprepare方針は実際の版、文字コード、互換性、コストを確認して選ぶ。無条件で切り替えない。

migrationと初期化でschema・索引が再現されることを確認する。性能のための耐久性設定変更も、受理済み書き込みが再起動後に取得できる条件を満たす必要がある。DDLのロック・途中失敗・再実行と、アプリとの反映順をCIに記録する。旧schemaへ戻すとデータを失う操作を自動で選ばない。
