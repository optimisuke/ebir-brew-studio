# AGENTS.md

このリポジトリで作業する AI エージェント／開発者向けのガイドです。

## プロジェクト概要

**ÉBIR BREW STUDIO（エビールブリュースタジオ）** の公式サイトと、来店者向けの小さな Web アプリ。

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
│   └── index.html    # 来店者向けの localStorage アプリ（内容は検討中）
├── assets/           # ロゴ・画像・favicon など
│   └── logo.jpeg
├── README.md
├── AGENTS.md
└── CLAUDE.md         # @AGENTS.md のみ
```

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
