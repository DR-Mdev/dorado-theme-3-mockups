# SP ハンバーガーメニュー 実装仕様書

作成日: 2026-07-19
状態: **設計確定。実装はこの仕様書どおりに行うこと(デザイン判断を追加しない)。**
対象: `html-mockups/` 直下の全13ページ(下記リスト)

---

## 1. 背景と現状

- 現状、SP幅ではナビリンク(`nav a.nl` ×6本)が `display:none` で消え、Contactボタンだけが残る。メニューに到達する手段がない。
- ヘッダーのマークアップは全13ページ共通(ロゴ + `nav`内にリンク6本 + Contactボタン)。
- ただし**ブレークポイントだけページ間でばらつきがある**。実装時に全ページ **1020px に統一**する。

| 現在の値 | ファイル |
|---|---|
| `max-width:900px` | contact, cookie-policy, how-we-operate, index, insight-single, insights, partners, privacy-policy, services, terms-of-use |
| `max-width:1000px` | about |
| `max-width:1020px` | use-case-single, use-cases |

---

## 2. デザイン方針(確定事項)

- **パターン**: 全画面オーバーレイ型。背景は `var(--ink)`(#1A003A)、白文字、ライムのアクセント。サイト内のダークセクション(services.htmlの`#unit`等)と同じ配色文法。
- **角丸なし**: 全ページ `border-radius:0!important` がグローバルに効いているので何もしなくてよい。
- **開閉ボタン**: ハンバーガー(横棒3本、44×44px、インク色1px枠)。ヘッダー右端、Contactボタンの右隣。
- **閉じるボタン**: オーバーレイ内右上、同サイズの白枠✕ボタン。ハンバーガーと同じ位置に来るよう、オーバーレイ先頭に「ロゴ(白)+✕」の行を置く。
- **リンク行**: 大きめの太字(clamp 1.45〜1.9rem)、各行の左にライムの9px正方形(facts stripの `::before` と同じ文法)、行間は白の薄いボーダーで区切る。
- **現在ページ**: `aria-current="page"` を付け、文字色をライムにする。
- **Contact**: リンク一覧の下、オーバーレイ最下部に `btn btn-lime btn-full` で配置(`margin-top:auto` で下に押し付ける)。
- **極小幅(≤420px)**: ヘッダー内のContactボタンは非表示(メニュー内にあるため)。ロゴ+ハンバーガーのみ残る。
- **アニメーション**: フェード+わずかな下がり(translateY(-12px)→0、.35s、`var(--ease)`)。既存の `prefers-reduced-motion` グローバル無効化ルールがそのまま効くので追加対応不要。

---

## 3. 実装コード(全ページ共通・コピペ用)

各ページに対して以下の **A〜D の4操作**を行う。13ページすべて同一(Dのaria-currentのみページごとに異なる)。

### A. CSS追加

各ページの `<style>` 内、既存の `@media (max-width:900px){nav a.nl{display:none}}`(about は1000px、use-cases系は1020px)を**削除**し、ヘッダー関連CSSの末尾に以下を貼る:

```css
/* ============ SP nav (hamburger) ============ */
.menu-btn{
  display:none;width:44px;height:44px;flex:none;
  flex-direction:column;justify-content:center;align-items:center;gap:6px;
  background:transparent;border:1px solid rgba(26,0,58,.4);cursor:pointer;padding:0;
}
.menu-btn span{display:block;width:20px;height:2px;background:var(--ink)}
@media (max-width:1020px){
  nav a.nl{display:none}
  .menu-btn{display:flex}
}
@media (max-width:420px){nav .btn{display:none}}

.sp-nav{
  position:fixed;inset:0;z-index:200;background:var(--ink);color:#fff;
  display:flex;flex-direction:column;padding:0 var(--pad) 28px;
  opacity:0;visibility:hidden;transform:translateY(-12px);
  transition:opacity .35s var(--ease),transform .35s var(--ease),visibility 0s .35s;
  overflow-y:auto;
}
.sp-nav.open{
  opacity:1;visibility:visible;transform:none;
  transition:opacity .35s var(--ease),transform .35s var(--ease);
}
.sp-nav-top{display:flex;justify-content:space-between;align-items:center;padding:16px 0 24px}
.sp-close{
  width:44px;height:44px;flex:none;background:transparent;cursor:pointer;
  border:1px solid rgba(255,255,255,.35);color:#fff;font-size:18px;line-height:1;
}
.sp-nav ul{display:flex;flex-direction:column}
.sp-nav li a{
  display:flex;align-items:center;gap:14px;padding:18px 0;
  font-size:clamp(1.45rem,6vw,1.9rem);font-weight:700;letter-spacing:-.01em;color:#fff;
  border-bottom:1px solid rgba(255,255,255,.14);
}
.sp-nav li a::before{content:"";width:9px;height:9px;background:var(--lime);flex:none}
.sp-nav li a[aria-current="page"]{color:var(--lime)}
.sp-cta{margin-top:auto;padding-top:28px}
.btn-full{width:100%;justify-content:center}
```

`.btn-full` は現状 services.html / contact.html / partners.html にしか定義がない。他10ページにはこのブロックで新規追加することになるが、既存3ページでは重複定義になるだけで無害なので、全ページ共通のこのブロックにそのまま含める。

### B. ハンバーガーボタン追加

各ページの `<nav>` 内、Contactボタン(`<a class="btn btn-lime btn-sm" ...>`)の**直後**に:

```html
<button class="menu-btn" type="button" aria-label="Open menu" aria-expanded="false" aria-controls="sp-nav">
  <span></span><span></span><span></span>
</button>
```

### C. オーバーレイ追加

`</header>` の**直後**に:

```html
<div class="sp-nav" id="sp-nav" aria-hidden="true">
  <div class="sp-nav-top">
    <a class="logo" href="index.html"><img src="../assets/logo/logo-white.png" alt="DORADO" class="logo-img"></a>
    <button class="sp-close" type="button" aria-label="Close menu">&#10005;</button>
  </div>
  <nav aria-label="Mobile">
    <ul>
      <li><a href="services.html">Services</a></li>
      <li><a href="how-we-operate.html">Process</a></li>
      <li><a href="use-cases.html">Use cases</a></li>
      <li><a href="insights.html">Insights</a></li>
      <li><a href="partners.html">Partners</a></li>
      <li><a href="about.html">About</a></li>
    </ul>
  </nav>
  <div class="sp-cta"><a class="btn btn-lime btn-full" href="contact.html">Contact us &rarr;</a></div>
</div>
```

ロゴ画像 `../assets/logo/logo-white.png` は既存(各ページのフッターで使用中)。

### D. aria-current の付与(ページごと)

そのページ自身に対応するリンクにのみ `aria-current="page"` を付ける:

| ファイル | aria-currentを付けるリンク |
|---|---|
| services.html | Services |
| how-we-operate.html | Process |
| use-cases.html / use-case-single.html | Use cases |
| insights.html / insight-single.html | Insights |
| partners.html | Partners |
| about.html | About |
| index.html, contact.html, privacy-policy.html, terms-of-use.html, cookie-policy.html | 付けない |

### E. JS追加

各ページ末尾の既存 `<script>`(IntersectionObserverのIIFE)の**中に追記**、または直後に別`<script>`で:

```js
(function(){
  const btn = document.querySelector('.menu-btn');
  const panel = document.getElementById('sp-nav');
  if(!btn || !panel) return;
  const closeBtn = panel.querySelector('.sp-close');
  function setOpen(v){
    panel.classList.toggle('open', v);
    btn.setAttribute('aria-expanded', String(v));
    panel.setAttribute('aria-hidden', String(!v));
    document.body.style.overflow = v ? 'hidden' : '';
    (v ? closeBtn : btn).focus();
  }
  btn.addEventListener('click', () => setOpen(true));
  closeBtn.addEventListener('click', () => setOpen(false));
  panel.querySelectorAll('a').forEach(a => a.addEventListener('click', () => setOpen(false)));
  document.addEventListener('keydown', e => {
    if(e.key === 'Escape' && panel.classList.contains('open')) setOpen(false);
  });
})();
```

---

## 4. 対象ファイル一覧(13ページ)

about.html / contact.html / cookie-policy.html / how-we-operate.html / index.html / insight-single.html / insights.html / partners.html / privacy-policy.html / services.html / terms-of-use.html / use-case-single.html / use-cases.html

---

## 5. 実装後の検証チェックリスト

1. 全13ページで、幅1020px以下でハンバーガーが表示され、それ以上では非表示(デスクトップ表示は現状と同一)であること
2. 既存の `nav a.nl{display:none}` の旧メディアクエリ(900/1000/1020px)が**重複して残っていない**こと
3. 開く→リンクタップで閉じて遷移/✕で閉じる/Escで閉じる、の3経路すべて動くこと
4. オーバーレイ表示中に背景がスクロールしないこと(body overflow lock)
5. 各ページで自ページのリンクがライム色になっていること(§Dの表どおり)
6. 幅420px以下でヘッダーのContactボタンが消え、メニュー内のContactから遷移できること
7. 幅375pxで横スクロールが発生しないこと
8. `prefers-reduced-motion: reduce` でも開閉が機能すること(アニメなしで即時表示)
