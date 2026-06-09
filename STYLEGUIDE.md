# STYLEGUIDE.md

実装レベルのスタイル仕様（デザイントークンとコンポーネント）。方針は [DESIGN.md](./DESIGN.md) を参照。
スタイルは Tailwind CSS（Play CDN）で、配色・フォントは各ページ冒頭の `tailwind.config` に定義。

## カラートークン

| 名前 | HEX | 用途 |
|---|---|---|
| `ink` | `#141414` | 本文・見出し・Story/フッターの背景・主要ボーダー |
| `paper` | `#ffffff` | ベース背景・暗色面上のテキスト |
| `cream` | `#FBF7EF` | あたたかい白。Instagram 節の背景、画像の下地 |
| `gold` | `#E2A52A` | 差し色。CTA・見出しラベル・星評価・リンク hover・スペック表記 |
| `goldDark` | `#C0871C` | ゴールドの hover／押下時 |

透明度は Tailwind の `/` 表記で調整（例: `text-ink/70`, `border-ink/10`, `bg-paper/90`）。

### ゴールドの使いどころ
- 主ボタン「ビールを見る」: `bg-gold text-ink`（hover `bg-goldDark text-paper`）
- ヘッダー「Shop」ピル: `bg-gold text-ink`
- セクションラベル（eyebrow）: `text-gold`
- ビールのスタイル表記（例 `SOUR ・ 5.0% ・ IBU 7`）: `text-gold`
- リンク／フッターナビ hover: `hover:text-gold`
- アプリ: 「記録する」`bg-gold`、星評価 `text-gold`

## タイポグラフィ

- フォント: `Inter, system-ui, -apple-system, "Hiragino Kaku Gothic ProN", "Noto Sans JP", sans-serif`
- ウェイト: 400 / 500 / 600 / 700
- h1: `text-3xl sm:text-5xl font-bold tracking-tight`
- h2: `text-2xl sm:text-3xl font-bold`
- eyebrow ラベル: `text-xs font-semibold uppercase` + `letter-spacing: 0.25em`（`.section-label`）+ `text-gold`
- 本文: `text-base sm:text-lg`（Story は `leading-loose`、他は `leading-relaxed`）
- 注記: `text-xs text-ink/50`

## レイアウト

- コンテナ幅: メイン `max-w-5xl`、読み物系 `max-w-3xl` / `max-w-2xl`、左右 `px-5`
- セクション縦余白: `py-20 sm:py-28`
- 角丸: カード `rounded-3xl` / `rounded-2xl`、ボタン `rounded-full`、入力 `rounded-xl`
- 影は控えめ（`shadow-sm` / hover `shadow-md`）。境界は `border-ink/10` の細線が基本

## コンポーネント

### ヘッダー（共通）
- `sticky top-0`、`bg-paper/90 backdrop-blur`、下境界 `border-ink/10`、`h-16`
- 左: ロゴ（`assets/logo.jpeg`, `h-9`）＋ サイト名（＋ `β` タグ）／ 右: ナビ（sm 以上）と「Shop」ピルを 1 グループにまとめて右寄せ
- ナビの「My Beers」と各 APP 導線にはゴールドの `APP` バッジを付与

### ヒーロー（トップ）
- `grid lg:grid-cols-2`。**DOM 順は「コピー → 画像」**
  - スマホ: コピー（ロゴ→`ÉBIR BREW STUDIO`→`エビールブリュースタジオ`→ボタン）が先、下にキービジュアル
  - PC: 左コピー／右キービジュアル
- ロゴ `h-14`、サブタイトル `tracking-widest text-ink/50`
- ボタン: 主＝ゴールド塗り、副＝`border-ink` 枠線
- キービジュアル（`assets/key-visual.jpg`）: `rounded-3xl ring-1 ring-ink/10`。スマホ `max-w-[220px]`、PC は列いっぱい
- 動き（CSSのみ・ビールらしさ）:
  - **泡**: `.bubbles` レイヤー（`section` を `relative overflow-hidden`、コンテンツは `z-10`）。ゴールドの小円が `@keyframes rise` で立ちのぼる
  - **入場**: `.fade-up`（コピー、キービジュアルは `delay-2` でスタッガー）
  - **ゆらぎ**: キービジュアルに `.floaty`（ゆっくり上下）
  - `prefers-reduced-motion` 時はいずれも無効化
- リロード時は常に最上部から表示（`history.scrollRestoration = 'manual'`）

### Story
- `bg-ink text-paper` の反転セクション。ラベル `text-gold`、本文 `leading-loose text-paper/90`、段落間 `space-y-6`

### Beers
- 導入（ラベル＋見出し＋注記「ラインアップは随時変わります。最新は Instagram で」）
- 注目2種（AURA DE NADA / BRAGGOT WHISPER）: 写真付きカード
  - 画像枠 `aspect-[4/5] bg-cream object-cover`、hover で `scale-[1.03]`
  - 名前 → ゴールドのスタイル表記 → 説明
- ラインアップ: 集合写真（`assets/bottles-lineup.jpg`）＋ その他ビール一覧（確定スペックのみ表記。FORTY OF HOPE / IT BRUT は名前中心）
- 末尾に「オンラインショップで購入」ボタン（枠線）

### Instagram
- `bg-cream` の節。フォロー導線（ink ボタン＋Instagram アイコン、hover `goldDark`）
- ※プロフィールのフィード埋め込みは未実装（公式の簡易手段がないためフォロー導線で代替）

### Access
- 2カラム（情報 dl ＋ Google マップ iframe）
- 営業時間は固定表記＋注記「※お休みや営業時間の変更は Instagram をご確認ください。」
- 電話・メールはリンク（hover `text-gold`）

### フッター
- `bg-ink text-paper/70`。ロゴ＋サイト名、ナビ（hover `text-gold`）
- 「20歳未満の方の飲酒は法律で禁止されています。」の注記を必ず表示

### アプリ（マイビール記録 / `app/`）
- トップと同じ配色・フォント。コンテナ `max-w-2xl`
- ヘッダー: ロゴ＋「My Beers」＋ゴールドの `APP` バッジ（アプリだと分かるように）
- 入力は 2 ステップ構成
  - **1. ビールを選ぶ** — 横スクロールのカルーセル（`flex overflow-x-auto snap-x`, `scrollbar-hide`）。カード幅 `w-32`、画像枠 `aspect-[3/4] rounded-2xl`。選択中は `ring-2 ring-gold`
  - **2. 評価とメモ** — 星評価（タップ選択・再タップ解除, `text-gold`）＋メモ。`記録する` はゴールド塗り、ビール未選択時は `disabled`
- ビール一覧は **固定リスト**（JS の `BEERS` 配列が単一の情報源）。写真があれば写真、なければ `color` のビール色スウォッチ（泡→ビールの縦グラデーション）で表示
- 一覧: カード（新しい順）、左にサムネ（写真／スウォッチ）、件数表示、削除（確認ダイアログ）、空状態は破線カード
- 保存データ（localStorage）には表示用に `name` / `img` / `color` / `rating` / `note` / `date` を持たせ、リスト変更後も記録の見た目を保つ
- 入力フォーカス: `focus:border-ink focus:ring-2 focus:ring-ink/10`

## 画像（assets/）

| ファイル | 内容 | 使用箇所 |
|---|---|---|
| `logo.jpeg` | ロゴマーク | ヘッダー／ヒーロー／フッター／favicon |
| `key-visual.jpg` | ブランドアート（青い女性） | ヒーロー |
| `aura-de-nada.jpg` | AURA DE NADA ボトル＋グラス | Beers カード |
| `braggot-whisper.jpg` | BRAGGOT WHISPER ボトル＋グラス | Beers カード |
| `bottles-lineup.jpg` | FORTY OF HOPE / IT BRUT のボトル | Beers ラインアップ |
| `aura-de-nada-art.jpg` | AURA DE NADA ラベルアート | 予備（未使用） |
| `braggot-whisper-art.jpg` | BRAGGOT WHISPER ラベルアート | 予備（未使用） |

- Web 用に最適化済み（JPEG・最大辺 ~1100–1200px・各 40–200KB 程度）
- 下地は `bg-cream`、角丸 `object-cover` でトリミング

## アクセシビリティ
- 画像 `alt` は意味に応じて付与（装飾は空 `alt=""`）
- ゴールド塗りボタンは濃い `ink` テキストでコントラスト確保
