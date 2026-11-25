# newsdesk

好きなトピック(タグ)の最新ニュースをまとめて見る、個人用のニュースデスクです。Claude のアーティファクトとして動く、単一の HTML ファイルです。

## 機能
- 左メニューのタグ(AI・スポーツ・ゲーム・ガジェット)と、自分で追加するタグ
- タグごとのウォッチワード・ミュートワード(「共通」の設定も可)
- 「↻ 最新を取得」で、選んだタグのニュースを Claude が検索して追加(取得期間は3日〜1か月から選択)
- 設定とニュースの JSON 書き出し・取り込み
- 設定はブラウザのローカル保存、タグとニュースはアーティファクトのデータベースに保存

## 構成
- `index.html` — ページ本体(HTML/CSS/JS を1ファイルに収録)
- `docs/collector-prompt.md` — 「最新を取得」で起動する定期タスクのプロンプト

## 動作条件
`index.html` は Claude アーティファクトの実行環境(`window.claude`)を前提にしています。必要な capability は次のとおりです。

- `db` — タグ・ニュース・取得状況の保存
- `user` — 書き込み権限の確認
- `mcp` — コネクタ `Claude Code Remote` の `fire_trigger` で取得タスクを起動
- `downloads` — JSON のファイル保存

`TRIGGER_ID`(`index.html` 冒頭付近)は自分の定期タスクの ID に置き換えてください。これらが無い環境でも、埋め込みの初期データの閲覧、ワード設定、JSON のコピーは使えます。

## データ
- `tags/{id}`: `{label, query, createdAt}`
- `news/{id}`: `{tag, date, sub, kind, title, summary, url, src, fetchedAt, flag?, caveat?}`
- `meta/refresh`: `{status, scope, tags[], days, requestedAt, startedAt?, finishedAt?, added?, message?}`
