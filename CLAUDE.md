# 仙台 旅のしおり（sendai-guide）— Claude Code 起動時ガイド

> 毎セッション自動で読み込まれるブートストラップ。まず本ファイルを読んでから作業する。

## これは何
- 「中小企業家同友会 青年経営者全国交流会 in 仙台」（**2026/10/1(木)〜2(金)**）に参加する、名古屋青年同友会 第9支部の経営者 **約25名**向けの旅のしおりWebサイト。幹事（かずき）が情報をまとめて共有する用途。スマホ閲覧が主。
- **素の静的HTML**（フレームワークなし・外部API/DBなし）。デザインは作り込み済み（Zen Old Mincho 明朝＋Zen Kaku Gothic、ゴールド×クリーム×インク配色、ノイズテクスチャ、ヒーロー＋カウントダウン）。
- Claudeチャットで開発 → **2026-07-16 に Claude Code / C:\dev 運用へ移行**。実体は最初からGitHub接続の自動デプロイで、他ツールと同じ構成だった。

## 公開・デプロイ（重要）
- 公開URL：https://sendai.apppro-web.com/ （参加者に共有する公開サイト）
- 配信：Cloudflare Workers プロジェクト **`sendai-guide`**（Static Assets 配信）。独自ドメイン `sendai.apppro-web.com`（Domains: 2、workers.dev サブドメイン含む）。
- **GitHub `k-ugawa/sendai-guide` の `main` に push → Cloudflare Workers Builds が自動デプロイ**（数十秒）。手動アップロード不要。
- wrangler設定は main には無く、Cloudflare側の自動構成（origin の `cloudflare/workers-autoconfig` ブランチにCloudflareが生成）で配信している。ローカルに `wrangler.toml` は置いていない。
- **運用ルール**：`main` は本番＝参加者が見る公開サイト。文言・情報の微修正は main 直行可。レイアウトを触る大きめ改修は `preview/<topic>` ブランチに push → Workers Builds のプレビューURLで確認 → OKで main にマージ、が安全。

## ファイル構成（単一HTML主体）
- **`index.html`（約59KB）** = 本体。1ファイルに CSS/JS/データ全部入り。セクションは5つ：
  `#schedule`（スケジュール案）/ `#gourmet`（グルメ&楽しみ方）/ `#access`（移動方法）/ `#hotel`（宿泊候補）/ `#area`（仙台の街のイメージ）。`<script>` は1つ（カウントダウン等の軽微なJS）。
- **`sendai-report.html`（約12KB）** = 報告資料（QRコード入り・別ページ）。
- データは全てHTMLにベタ書き。改修は該当セクションを直接編集する。

## 改修時の注意
- **現状デザインを大きく崩さない**のが方針（ユーザー明言）。配色変数は `:root`（`--ink/--cream/--gold/--accent/--teal`）で一元管理。
- 検証は claude-in-chrome（ユーザーのChrome「ASUS NotePC」がCloudflareにログインしてMCP接続）で本番URLを直接見るのが確実。このPCのlocalhostへは自動ブラウザが到達しにくい。

## 関連資料の所在
- **Dropbox**「Claude Cowork/アプリ開発/【青全交】旅のしおりLP/」：
  - `sendai-shiori-handoff.md` = チャットが作った**React作り直し案**（懇親会の多数決投票・フライト時刻表・ナイトライフ・ディープスポット・持ち物リスト等、現行より**コンテンツが豊富**）。本番には未採用。**現行デザインへ機能・情報を足す際の“ネタ元”として参照**する。
  - `PDF資料/第54回青年経営者in宮城.pdf`（イベント公式資料）、`2026年度_あいち報告会チラシ.pdf`。
- 書類はDropbox、コードはGitHub（`k-ugawa/sendai-guide`）という他プロジェクトと同じ分担。
