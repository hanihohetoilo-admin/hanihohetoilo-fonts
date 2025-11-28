# フォントCDN - Cloudflare Pages用

このディレクトリには、Cloudflare Pagesにホスティングするための最適化された日本語フォントファイルが含まれています。

## ファイル構成

```
fonts-cdn/
├── fonts.css                                          # メインCSSファイル
├── m-plus-1-code-japanese-400-normal.woff2            # 598KB
├── m-plus-1-code-japanese-500-normal.woff2            # 619KB
├── m-plus-1-code-japanese-700-normal.woff2            # 600KB
└── mochiy-pop-one-japanese-400-normal.woff2           # 969KB
```

**合計サイズ**: 約2.7MB

## メリット

✅ Azureの帯域を消費しない（フォントは別のCDNから配信）
✅ Google Fontsよりも最適化（必要なファイルのみ）
✅ PageSpeed Insightsのスコア改善
✅ 完全無料（Cloudflare Pagesは無制限）
✅ 日本語全文字対応（コンテンツ追加時も再生成不要）

## Cloudflare Pagesへのデプロイ手順

### 方法1: Cloudflare Dashboard（簡単）

1. **GitHubリポジトリを作成**
   ```bash
   cd fonts-cdn
   git init
   git add .
   git commit -m "Add optimized Japanese fonts"
   gh repo create hanihohetoilo-fonts --public --source=. --push
   ```

2. **Cloudflare Pagesに接続**
   - https://dash.cloudflare.com/login にadmin@hanihohetoilo.orgでログイン
   - 「Pages」→「Create a project」
   - 「Connect to Git」→ GitHubリポジトリを選択
   - Build settings:
     - Build command: (空欄)
     - Build output directory: `/`
   - 「Save and Deploy」

3. **デプロイ完了**
   - `https://hanihohetoilo-fonts.pages.dev` のようなURLが発行されます

### 方法2: Wrangler CLI（上級者向け）

```bash
npm install -g wrangler
wrangler login
wrangler pages project create hanihohetoilo-fonts
wrangler pages publish fonts-cdn --project-name=hanihohetoilo-fonts
```

## HTMLファイルの変更

デプロイ後、すべてのHTMLファイル（10ファイル）の`<head>`セクションを以下のように変更:

### 変更前
```html
<!-- Preload Google Fonts CSS for faster rendering -->
<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=M+PLUS+1+Code:wght@400;500;700&family=Mochiy+Pop+One&display=swap">
<link href="https://fonts.googleapis.com/css2?family=M+PLUS+1+Code:wght@400;500;700&family=Mochiy+Pop+One&display=swap" rel="stylesheet">
```

### 変更後
```html
<!-- Optimized Japanese fonts from Cloudflare Pages -->
<link rel="preload" as="style" href="https://hanihohetoilo-fonts.pages.dev/fonts.css">
<link rel="stylesheet" href="https://hanihohetoilo-fonts.pages.dev/fonts.css">
```

## 性能比較

| 項目 | Google Fonts | Cloudflare自前ホスティング |
|------|--------------|---------------------------|
| 初回読込 | 117KB | 2.7MB |
| レンダリングブロック | あり | なし（preload済） |
| Azure帯域消費 | 0MB | 0MB |
| 文字範囲 | 全日本語 | 全日本語 |
| 更新作業 | 不要 | 不要 |
| PageSpeed | ⚠️ 警告あり | ✅ 最適化 |

## カスタムドメイン（オプション）

Cloudflare Pagesでカスタムドメインを設定できます:

1. Pages設定 → Custom domains
2. `fonts.hanihohetoilo.org` を追加
3. DNSレコードが自動設定されます
4. HTMLで `https://fonts.hanihohetoilo.org/fonts.css` を使用

## キャッシュ設定

Cloudflare Pagesは自動的に:
- ブラウザキャッシュ: 1年
- CDNキャッシュ: 永続
- Brotli/Gzip圧縮: 自動

追加設定は不要です。
