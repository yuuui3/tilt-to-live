---
name: slide-creation
description: GRAVIX(本リポジトリのHTML5傾きゲーム)に関するスライド資料を、ゲーム本体と同じ単一HTMLファイル形式で制作するためのスキル。プレゼン、ピッチ、ゲーム紹介、開発記録、技術解説などのスライド作成に使用する。「スライド作って」「プレゼン資料」「発表資料」「ピッチ資料」などのリクエストで起動する。
---

# GRAVIX スライド制作スキル

GRAVIX(`tilt-to-live.html`)と同じく **単一HTMLファイル** で完結するスライドデッキを制作する。
ゲーム本体のビジュアル(黒背景・ネオンカラー・Orbitron書体)と統一感のあるトーンで仕上げる。

## いつ使うか

ユーザーが以下のような依頼をしたとき:
- 「GRAVIX のスライド/プレゼン/紹介資料を作って」
- 「ゲーム説明スライドを作って」
- 「開発記録/振り返りスライドを作って」
- 「ピッチ資料/発表資料を作って」

## 設計原則

1. **単一HTML**: 外部ビルド不要。ブラウザで開けばそのまま動く。プロジェクト本体の哲学に揃える。
2. **デザイン統一**: ゲームと同じ配色とフォント(Orbitron)を使用。
   - 背景: `#000`
   - アクセント: シアン `#00E5FF`、マゼンタ `#FF00FF`、イエロー `#FFDD00`
   - グロー効果(`text-shadow` / `box-shadow`)で発光感を出す
3. **キーボード操作**: 矢印キー / Space / PageUp/Down でスライド送り。`F` でフルスクリーン。
4. **レスポンシブ**: 16:9 ベース。`vw/vh` を使い画面サイズに追従。
5. **アニメーション控えめ**: 入場時のフェード程度。読み手の集中を妨げない。
6. **日本語前提**: `lang="ja"`、本文は日本語。

## ワークフロー

### 1. 目的とアウトラインの確認

最初にユーザーに以下を確認する(明示されていない場合のみ):
- スライドの **目的**(誰に見せるか: 投資家/プレイヤー/開発者仲間/学校発表 等)
- **枚数の目安**(指定なければ 8〜12 枚)
- 含めたい **トピック**(ゲームプレイ/技術スタック/開発の苦労 等)

確認は1回にまとめる。決め打ちで進める場合はその旨告げる。

### 2. アウトライン作成

典型的な構成例(用途に応じて取捨選択):

| # | スライド          | 内容                                    |
|---|------------------|----------------------------------------|
| 1 | タイトル          | GRAVIX / サブタイトル / 作者           |
| 2 | コンセプト        | 一言で何のゲームか                     |
| 3 | 操作              | スマホを傾けて操作                     |
| 4 | ゲームルール      | 敵を避けてスコアを稼ぐ                 |
| 5 | 武器・コンボ      | オーブ拾得・コンボ倍率(2x→10x)        |
| 6 | エリート敵        | 30秒ごとに出現する強敵                 |
| 7 | 技術スタック      | Vanilla JS / Canvas / DeviceOrientation|
| 8 | 工夫したポイント  | 縦持ちで横画面を実現する回転トリック等 |
| 9 | 今後の展望        | ランキング / SE / ステージ追加         |
|10 | エンディング      | プレイURL / QR / Thanks                |

### 3. HTMLファイルの実装

ファイル名: `slides.html`(または用途に応じて `slides-<目的>.html`)を **リポジトリ直下** に作成。

実装は下記「テンプレート」をベースに、各 `<section class="slide">` の中身を埋めていく。

### 4. 動作確認

```bash
# Pythonでローカルサーバ起動
python3 -m http.server 8000
```

ブラウザで `http://localhost:8000/slides.html` を開き、以下を確認:
- 矢印キーでスライド送りができる
- 全スライドが崩れず表示される
- フォント(Orbitron)が読み込まれている

UIが伴うため、確認できない場合は **その旨ユーザーに明示**(「ブラウザでの目視確認はできていません」)する。

### 5. コミット

```bash
git add slides.html
git commit -m "Add presentation slides for GRAVIX"
git push -u origin claude/create-slide-skills-R8u39
```

## テンプレート

新規作成時は `template.html` をコピー元として使う(同ディレクトリに同梱)。
最低限の構造は以下のとおり:

```html
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>GRAVIX - Presentation</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&display=swap" rel="stylesheet">
<style>
  *{margin:0;padding:0;box-sizing:border-box}
  html,body{width:100%;height:100%;background:#000;color:#fff;
    font-family:'Orbitron',sans-serif;overflow:hidden}
  .slide{position:absolute;inset:0;display:none;
    flex-direction:column;justify-content:center;align-items:center;
    padding:6vh 8vw;opacity:0;transition:opacity .4s}
  .slide.active{display:flex;opacity:1}
  h1{font-size:8vw;font-weight:900;letter-spacing:.05em;
    color:#00E5FF;text-shadow:0 0 30px #00E5FF}
  h2{font-size:4vw;color:#FF00FF;text-shadow:0 0 20px #FF00FF;margin-bottom:4vh}
  p,li{font-size:2.4vw;line-height:1.8;max-width:80vw}
  .footer{position:fixed;bottom:2vh;right:3vw;font-size:1.4vw;color:#555}
</style>
</head>
<body>

<section class="slide active">
  <h1>GRAVIX</h1>
  <p style="margin-top:3vh;color:#FFDD00">傾きで遊ぶ重力アクション</p>
</section>

<!-- ここにスライドを追加 -->

<div class="footer"><span id="pageNum">1</span> / <span id="pageTotal">1</span></div>

<script>
  const slides = document.querySelectorAll('.slide');
  let cur = 0;
  document.getElementById('pageTotal').textContent = slides.length;
  function show(i){
    slides[cur].classList.remove('active');
    cur = (i + slides.length) % slides.length;
    slides[cur].classList.add('active');
    document.getElementById('pageNum').textContent = cur + 1;
  }
  document.addEventListener('keydown', e => {
    if (['ArrowRight','ArrowDown','PageDown',' '].includes(e.key)) show(cur + 1);
    else if (['ArrowLeft','ArrowUp','PageUp'].includes(e.key)) show(cur - 1);
    else if (e.key === 'f' || e.key === 'F') document.documentElement.requestFullscreen();
  });
</script>
</body>
</html>
```

## 注意点

- **ゲーム本体(`tilt-to-live.html`)は変更しない**。スライドは別ファイル。
- **画像アセットを追加するな**。テキスト + CSS の発光表現で十分。どうしても必要なら SVG をインライン化。
- **絵文字はユーザーが明示的に求めない限り使用しない**(プロジェクト方針)。
- **READMEなどのドキュメントを勝手に増やさない**。スライドHTML一枚で完結させる。
- ゲーム本体のスクリーンショットを載せたい場合は、`tilt-to-live.html` の Canvas 描画ロジックを抜粋した小型デモを `<iframe srcdoc="...">` で埋め込む方法もある(任意)。
