# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

iOS アプリ **nokori（のこり）**（「3秒で記録できる、静かな家計簿」）の
ランディングページとプライバシーポリシーを配信する**静的サイト**。
アプリ本体のソースは別の非公開リポジトリで管理されている（ここには無い）。

## Architecture

ビルドツール・依存・パッケージマネージャは一切無い。素の HTML を直接編集する。

- `index.html` — ランディングページ（日本語）。CSS と JS をすべて `<style>` / `<script>` でインライン埋め込み。
- `privacy.html` — プライバシーポリシー（日本語 / English を 1 ファイル内に併記）。独立したインライン CSS を持つ。
- `assets/app-icon.png`、`assets/screenshots/*.png` — 画像。`index.html` から相対パスで参照。
- `.nojekyll` — GitHub Pages の Jekyll 処理を無効化（このファイルは消さない）。

2 ファイルはレイアウトを**共有していない**。デザイントークン（`--paper`, `--ink`, `--sage` などの
CSS 変数のカラーパレット）が両ファイルにそれぞれ重複定義されている。色やフォントを変えるときは
**両方の `:root` を揃えて編集する**こと。フォントは Google Fonts（Zen Old Mincho = 見出し、
Zen Kaku Gothic New = 本文）。

## Conventions

- **No-JS / reduced-motion を必ず壊さない。** `index.html` の `.reveal` アニメーションは
  `<html class="js">` が付いたときだけ要素を隠す設計。JS 無効環境では最初から表示される。
  `IntersectionObserver` 非対応や observer 未発火に備えた安全網（即時表示・2.5秒タイムアウト）も
  入っているので、reveal 周りを触るときはこのフォールバックを残す。
- 文言・トーンは「引き算」「静けさ」が軸。アプリの世界観（広告なし・完全オフライン・データ収集なし）に
  反する表現を足さない。
- 見出しの折返し制御に `<span class="nb">`（`white-space: nowrap`）を使用。日本語の不自然な改行を避ける用途。

## Develop / Preview

ローカルプレビューは任意の静的サーバで十分（ビルド不要）:

```bash
python3 -m http.server 8000   # → http://localhost:8000
```

## Deploy

GitHub Pages（`main` ブランチのルート）で自動公開。
`main` への push がそのまま本番反映 → https://mkontani.github.io/nokori-site/
別途のビルド・CI ステップは無い。
