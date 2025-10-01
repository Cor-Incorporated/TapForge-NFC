# TapForge NFC - ランディングページ

TapForge NFC カード印刷サービスのランディングページ

## 概要

送料別 300 円から NFC 名刺を提供するサービスのランディングページです。Blackspike Astro LP テンプレートをベースに構築されています。

## 技術スタック

- **Astro 5.x** - 静的サイトジェネレーター（ハイブリッドモード）
- **Tailwind CSS 4.x** - スタイリング
- **Swiper 11.x** - カルーセル
- **Stripe** - 決済統合
- **Cloudflare Pages** - デプロイ先

## セットアップ

### 依存関係のインストール

```bash
npm install
```

### 環境変数の設定

プロジェクトルートに `.env` ファイルを作成し、以下の環境変数を設定してください：

```env
# Stripe API Keys
PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
```

### 開発サーバーの起動

```bash
npm run dev
```

ブラウザで `http://localhost:4321` を開いてください。

### ビルド

```bash
npm run build
```

## プロジェクト構造

```
/
├── public/              # 静的ファイル
├── src/
│   ├── assets/         # 画像、フォント、CSS
│   ├── components/     # Astroコンポーネント
│   ├── data/          # JSONデータファイル
│   ├── layouts/       # レイアウトコンポーネント
│   └── pages/         # ページファイル
│       ├── index.astro       # トップページ
│       └── api/
│           └── create-checkout.astro  # Stripe API
├── astro.config.mjs   # Astro設定
└── package.json
```

## セクション構成

1. **Hero** - 100vh、青色背景、メインメッセージ
2. **Visual Showcase** - Swiper カルーセルでサービス紹介
3. **仕様・メリット** - NFC カードの仕様を表示
4. **Pricing** - 3 つの料金プラン（10 枚、50 枚、100 枚以上）
5. **FAQ** - よくある質問（アコーディオン形式）
6. **CTA** - 注文への誘導

## Stripe 統合

### Checkout セッションの作成

```javascript
const response = await fetch("/api/create-checkout", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify({
    priceId: "price_...", // Stripe Price ID
    quantity: 1,
  }),
});

const { url } = await response.json();
window.location.href = url; // Stripe Checkoutへリダイレクト
```

## デプロイ

### Cloudflare Pages へのデプロイ

1. Cloudflare Pages プロジェクトを作成
2. GitHub リポジトリと連携
3. ビルド設定：
   - **Build command**: `npm run build`
   - **Output directory**: `dist`
4. 環境変数を設定：
   - `STRIPE_SECRET_KEY`
   - `PUBLIC_STRIPE_PUBLISHABLE_KEY`

## カスタマイズ

### データの更新

`src/data/` フォルダ内の JSON ファイルを編集してコンテンツを変更できます：

- `global_settings.json` - サイト全体の設定
- `home.json` - トップページのタイトルと説明
- `services.json` - サービス紹介（3 つ）
- `pricing.json` - 料金プラン
- `faq.json` - よくある質問
- `testimonials.json` - お客様の声

### スタイルの変更

- `src/assets/css/` - カスタム CSS
- テーマカラー: `#1E40AF` (青) と `#10B981` (緑)

## ライセンス

MIT License

## クレジット

- Blackspike Astro LP Template
- Astro
- Tailwind CSS
- Swiper
