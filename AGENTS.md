# AGENTS.md

このリポジトリで作業する AI エージェント／開発者向けのガイドです。

## プロジェクト概要

**ÉBIR BREW STUDIO（エビールブリュースタジオ）** のサイトと、来店者向けの小さな Web アプリ。

- 神戸・住吉のまちに根ざした小さなブリュワリーパブ（海老原夫妻が運営）
- サイトはブランド紹介・ビール紹介・店舗情報が中心。物販は既存の Square ショップに誘導
- コンテンツ（写真・お知らせ）は基本 Instagram で管理し、サイトには埋め込みで反映

## 技術スタック / 制約

- **HTML + 素の JavaScript + Tailwind CSS** のみ。フレームワーク・ビルドステップなし
- Tailwind は CDN（Play CDN）で読み込む。npm / バンドラは使わない
- **サーバーなし**。ホスティングは GitHub Pages（または他の静的配信）を想定
- 各ユーザーのデータは **localStorage** に保存（バックエンド・DB なし）
- **スマホファースト**で実装し、PC でも崩れないようにする

## ディレクトリ構成

```
/
├── index.html        # トップ（LP）: ヒーロー / ブランド / ビール / アクセス / Instagram
├── app/
│   ├── index.html    # 来店者向けアプリ（タップリスト + 飲んだ記録 / SPA）
│   └── qr.html       # 店舗向け: QR ページ（スマホ画面に収まる1ページ。スクショ印刷想定 / noindex）
├── assets/           # ロゴ・画像・favicon など
│   ├── logo.jpeg
│   └── vendor/
│       └── qrcode.min.js  # オフライン QR 生成（qr.html 用にベンダリング）
├── robots.txt        # クローラ許可 + sitemap の場所
├── sitemap.xml       # クロール対象 URL 一覧（トップ / app）
├── TODO.md           # 今後の作業メモ（独自ドメイン設定 など）
├── README.md
├── AGENTS.md
└── CLAUDE.md         # @AGENTS.md のみ
```

## アプリ（app/）

来店者向けの「タップリスト + 飲んだ記録」アプリ。店内 QR から来店客がアクセスする想定。**飲みながら片手で使えるシンプルさ**を最優先し、すべて1ページ（タブ・画面遷移なし）に収める。サーバー・DB なし、記録は端末の **localStorage** のみ。

- **構成**: `app/index.html` 1ファイル・1ページ。`render()` がページ全体を描画し、ビールをタップすると下からスライドする記録シート（`openCheckin()`）が開く。ルーティング・タブ・別画面は持たない
- **ビールの単一情報源**: JS の `BEERS` 配列（`id` / `name` / `style` / `abv` / `ibu` / `img` or `color` / `desc` / `active` / `new`）。`active:false` は一覧から外れる（記録と隠しバッジ「伝説の一杯」には残る）、`new:true` は隠しバッジ「新作ハンター」の対象
- **ページ構成（上から）**: コンプ率バー（スリム1行）→ ラインナップ（現行銘柄の横スワイプカルーセル＋未飲フィルタ。見出しは「ラインナップ」、内部的にはタップリスト）→ きろく（記録一覧）→ バッジ
- **チェックイン**: ビールをタップ → 記録シートで「お気に入り指数」（★・任意）＋メモ（任意）を入れて記録。同一銘柄に複数回記録可（カードに杯数バッジ）。新しくバッジを獲得すると画面上部にトーストで通知、コンプ率100%で金色演出
- **きろく**: 今日の記録は展開表示、昨日より前は日付ごとに折りたたみ（タップで開閉）。各記録は「日付 時刻」つき。記録カードをタップで編集シート（すき度・メモの修正、削除。日時は保持）
- **コンプ率**: 現行（`active`）銘柄の制覇率。100%到達時に金色演出（`celebrate()`、達成済みは `ebir.celebrated` フラグで再演出を抑止）
- **バッジ**: `BADGES` 配列＋`evaluateBadges()` で記録とタイムスタンプから純粋関数で判定。通常バッジ（はじめの一杯／常連 5・10・25／コンプリート）と隠しバッジ（未獲得は「???」: 夜更かし／おかわり／きき酒師／新作ハンター／伝説の一杯／アーメットの騎士）
- **ストレージとマイグレーション**: 記録は `ebir.records`、スキーマ版数は `ebir.schema`。起動時に `runMigrations()` が `MIGRATIONS[]` を逐次適用。データ形を変えるときは1ステップ足して `CURRENT_SCHEMA` を上げる（旧 `ebir.mybeers.v1` からの取り込みを含む）
- **QR ページ**: `app/qr.html`（店舗向け・`noindex`）。`assets/vendor/qrcode.min.js` でアプリ URL の QR をオフライン生成。**アプリと共通ヘッダーの、スマホ画面に収まる1ページ**（印刷ボタンやカード枠は持たず、スクショして印刷する想定）。「20歳未満の飲酒禁止」を明記

## SEO

- 公開 URL は `https://optimisuke.github.io/ebir-brew-studio/`（GitHub Pages のデフォルト）。`canonical` / OGP / sitemap はこの絶対 URL を基準にする。独自ドメイン（CNAME）を設定したら全ファイルの URL を差し替える
- 各ページの `<head>` に `canonical` / OGP / Twitter カード / `robots` を記述。OGP 画像は `assets/key-visual.jpg`
- トップの `<head>` に JSON-LD 構造化データ（`Brewery`＋`BarOrPub` と `WebSite`）を記述。所在地・営業時間・SNS（Instagram / Square）を反映する。**営業時間や所在地を変えたら JSON-LD も同時に更新する**
- 電話・メールは非公開のため構造化データにも含めない（公開する場合は `telephone` 等を追加）

## デザイン指針

方針は [DESIGN.md](./DESIGN.md)、実装値（配色・フォント・コンポーネント）は [STYLEGUIDE.md](./STYLEGUIDE.md) を参照。

参照: https://ebirbrewstudio.square.site/

- ミニマル＆クラフト。白基調（背景 `#ffffff`）、文字・アクセントはチャコール／ブラック寄り
- フォントは **Inter**（CDN）。日本語は端末標準のゴシックにフォールバック
- ロゴは黒い騎士のヘルメット（アーメット）マーク（`assets/logo.jpeg`）。favicon にも流用
- 装飾は控えめに。余白を広く取り、写真で見せる

## ブランド情報（サイト本文のソース）

- 名称: ÉBIR BREW STUDIO / エビールブリュースタジオ
- 所在地: 〒658-0052 兵庫県神戸市東灘区住吉東町3丁目7−12
- TEL: 078-600-0314 / Mail: ebeer.kobe@gmail.com
- 営業時間: 月〜金 17:00–23:00 / 土・日 12:00–23:00
- Instagram: https://www.instagram.com/ebirbrewstudio/
- オンラインショップ（Square）: https://ebirbrewstudio.square.site/
- Google マップ: https://maps.app.goo.gl/kdRLTrfPLH8FiyMM8

ブランドステートメントやビール一覧などの長文は `index.html` 内に直接記述する（CMS なし）。

## 開発・確認

ビルド不要。ローカル確認は任意の静的サーバーで:

```sh
python3 -m http.server 8000
# → http://localhost:8000
```

## デプロイ

GitHub Pages（`main` ブランチのルート、または `docs/`）で配信する想定。
静的ファイルのみなのでビルド設定は不要。

## ドキュメントの更新

サイトやアプリに変更を加えたら、関連ドキュメントも必ず同じ変更内で更新する（コードとドキュメントを乖離させない）。

- デザイン方針が変わったら → [DESIGN.md](./DESIGN.md)
- 配色・フォント・コンポーネント・画像などの実装が変わったら → [STYLEGUIDE.md](./STYLEGUIDE.md)
- 構成・スタック・店舗情報・運用ルールが変わったら → このファイル（AGENTS.md）／[README.md](./README.md)

## 約束ごと

- 20歳未満の飲酒禁止の表記をサイトに残す（酒類サイトの必須事項）
- 個人情報・決済はこのサイトで扱わない（物販は Square 側に任せる）
- localStorage 以外の永続化（Cookie / 外部送信）は行わない
