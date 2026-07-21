# index.html 再設計仕様書(実装者向け)

作成日: 2026-07-21
対象ファイル: `dorado/theme-3.0/html-mockups/index.html`
参照ファイル: `about.html` / `insights.html` / `use-cases.html` / `how-we-operate.html`(いずれも同ディレクトリ)

この仕様書だけを読んで実装できるように書いてある。**コピー(英文)はすべてこの仕様書に確定稿として記載済み。勝手に書き換えないこと。** デザイントークン(`--ink` `--lime` 等)、ヘッダー、SPナビ、フッター、reveal用JSは現状のまま変更しない。

---

## 0. 新しいセクション構成(完成形)

| # | section id | 背景 | 内容 | 変更種別 |
|---|---|---|---|---|
| 1 | `#hero` | white | ヒーロー | 変更なし |
| 2 | `#services-teaser` | ink | 3プラン紹介 | 変更なし |
| 3 | `#gap` | white | The gap we close | 変更なし |
| 4 | `#otherside` | gray | The buyer decided… + **旧#whatwedoの4項目を吸収** | 拡張 |
| 5 | `#method` | white | Process への薄い導線 | 大幅縮小 |
| 6 | `#depth` | gray | 成果物4種(2×2テキストカード化) | 圧縮 |
| 7 | `#usecases` | ink | ユースケース3枚のカードティザー | 作り替え |
| 8 | `#philosophy` | white | Aboutの哲学ステートメント(新設) | 新設 |
| 9 | `#insights` | gray | インサイト6枚のカードグリッド | 作り替え |
| 10 | `#products` → `#databases` | white | 2つのデータベース紹介 | 表現変更 |
| 11 | `#contact` | lime | コンタクト | 変更なし |

削除されるセクション: `#whatwedo`(→ #otherside に吸収)、`#posture`(Honest calls — 完全削除)。

背景色の追従: `#method` は現在 `background:var(--gray)` だが **whiteに変更**(CSSの `#method{background:var(--gray)}` を削除)。`#depth` は **grayに変更**。`#insights` は **grayに変更**。`#products`(→`#databases`)は現在インラインで `style="background:var(--gray)"` → **削除してwhite**。上表の white/gray/ink/lime の並びになれば正しい。

---

## 1. #hero・#services-teaser・#gap — 変更なし

触らない。

---

## 2. #otherside — 旧 #whatwedo を吸収

`#otherside` セクションの既存内容(lead、`.narrative` 一式=n-steps 3つ+右側のスクショ+note-bar)はそのまま維持。その **`.narrative` の閉じタグ直後**(セクションの `.wrap` 内末尾)に、以下のブロックを追加する:

```html
<div class="do-bridge rv">
  <h3>What we actually do about it</h3>
  <p>Interest is rarely the problem in Japan &mdash; meetings and demos come
  easily. Converting them is the work, and it looks like this:</p>
</div>

<div class="do-grid">
  <div class="do-list rv">
    <div class="do-item">
      <span class="tick">1</span>
      <div>
        <h3>Diagnose the pipeline honestly</h3>
        <p>Every open opportunity audited and graded, then a deal-by-deal
        priority list: push, nurture, or stop investing entirely.</p>
      </div>
    </div>
    <div class="do-item">
      <span class="tick">2</span>
      <div>
        <h3>Find where the demand actually is</h3>
        <p>Candidate segments researched in depth before anyone sells anything:
        does the problem exist, who owns it, what volume, can it pay?</p>
      </div>
    </div>
  </div>
  <div class="do-list rv">
    <div class="do-item">
      <span class="tick">3</span>
      <div>
        <h3>Test ideas against evidence, including yours</h3>
        <p>When you propose a positioning, we score it, not applaud it. Clients
        pay us to be right, not agreeable.</p>
      </div>
    </div>
    <div class="do-item">
      <span class="tick">4</span>
      <div>
        <h3>Put numbers on it first</h3>
        <p>Market sizing, volume estimates per prospect, revenue models per
        scenario, built <b>before</b> outreach.</p>
      </div>
    </div>
  </div>
</div>
```

(do-item 4つの中身は旧 #whatwedo からの移設で、文言は同一。)

追加CSS(`.do-item` 群の既存CSSの近くに置く):

```css
.do-bridge{margin-top:64px;max-width:640px}
.do-bridge h3{font-size:clamp(1.3rem,2.2vw,1.7rem);font-weight:700;letter-spacing:-.015em;margin-bottom:12px}
.do-bridge p{font-size:14.5px;line-height:1.7;opacity:.78}
#otherside .do-grid{margin-top:32px}
```

注意: `.do-item` は gray 背景上に載ることになるが、既存CSSは背景色を持たないのでそのまま動く。`.do-item:first-child` の `border-top:2px solid var(--ink)` も維持でよい。

そのうえで **`<section id="whatwedo">` を丸ごと削除**する(見出し「Interest was never your problem.」とそのlead文はサイトから消える。復活させないこと)。

---

## 3. #method — 薄い導線セクションに縮小

セクションを以下の内容に **丸ごと差し替える**。h2タイトルとlead文は現行のまま残すのがポイント。LANE/BATCH/CALL の `.m-flow` 3セル、`.m-annot` 2ブロック(スクショ付き解説)、`.spine` はすべて削除。

```html
<!-- ================= PROCESS (teaser) ================= -->
<section id="method" class="sec">
  <div class="wrap">
    <p class="eyebrow rv">Process</p>
    <h2 class="h2 rv">Market development,<br>run like an engineering system.</h2>
    <p class="lead rv">
      Go-to-market work fails quietly: messages go out, meetings happen, and a
      year later nobody can say what worked or why. We built our own operating
      system to make that impossible.
    </p>

    <ul class="m-points rv">
      <li>Every engagement is organized around questions the market can actually answer.</li>
      <li>Every test runs through real, named companies &mdash; nothing is allowed to just fade out.</li>
      <li>Every result ends in a recorded decision: continue, adjust, or stop spending.</li>
    </ul>

    <a class="btn btn-ghost rv" href="how-we-operate.html" style="margin-top:36px">See the full system &rarr;</a>
  </div>
</section>
```

追加CSS(削除する `.m-flow` 群のあった場所に置けばよい):

```css
.m-points{margin-top:36px;display:flex;flex-direction:column;max-width:640px}
.m-points li{
  display:flex;align-items:flex-start;gap:14px;
  padding:16px 0;border-top:1px solid rgba(26,0,58,.15);
  font-size:15px;line-height:1.6;
}
.m-points li:first-child{border-top:2px solid var(--ink)}
.m-points li:last-child{border-bottom:1px solid rgba(26,0,58,.15)}
.m-points li::before{content:"";width:9px;height:9px;background:var(--lime);flex:none;margin-top:7px}
```

CSS削除: `#method{background:var(--gray)}` / `.m-flow` / `.m-cell`(と `.m-cell .nt` 等の子孫)/ `.m-annot` 一式 / `.spine` 一式。`.m-annot` は他セクションで使っていないことを確認済み。

---

## 4. #depth — 2×2 テキストカードに圧縮

イントロ(eyebrow / h2「Research that reads like the buyer wrote it.」/ lead)と末尾の `.cap`(These span precision manufacturing…)は現行のまま維持。**縦長の原因である `.depth-row` 4行(3:4スクショ枠つき)を、スクショなしの 2×2 グリッドに置き換える。** テキスト(kt / h3 / p)は現行の文言をそのまま流用する。

セクションタグを `<section id="depth" class="sec" style="background:var(--gray)">` に変更し、4つの `.depth-row` を削除して以下に差し替え:

```html
<div class="depth-grid rv">
  <div class="depth-card">
    <span class="kt">Deep dive</span>
    <h3>Buying-process deep dives</h3>
    <p>For a single target buyer, we reconstruct the actual route in: purchasing
    organization, regional sourcing units, supplier registration and screening,
    evaluation functions, qualification programs, verified against primary
    sources, with facts, hypotheses, and to-verify items clearly separated.</p>
  </div>
  <div class="depth-card">
    <span class="kt">Demand map</span>
    <h3>Segment demand maps</h3>
    <p>A dozen-plus candidate verticals for one product, researched to a decision:
    does the problem exist here, who owns it, what volume flows through it, can
    it pay, and which two segments deserve the next quarter.</p>
  </div>
  <div class="depth-card">
    <span class="kt">Horizon</span>
    <h3>Regulatory horizon reports</h3>
    <p>Five-year outlooks on how regulation reshapes a market: what new laws
    actually require, where budgets will be forced into existence, and what that
    means for how a product should be positioned <b>now</b>.</p>
  </div>
  <div class="depth-card">
    <span class="kt">Patterns</span>
    <h3>Win / loss pattern libraries</h3>
    <p>What actually moved orders for foreign suppliers into Japan, and the
    twelve ways deals die <b>after</b> the order is won. Patterns, not anecdotes.</p>
  </div>
</div>
```

CSS: `.depth-row` 一式を削除し、以下を追加(about.html の `.principles` と同じ「1pxギャップの罫線グリッド」方式):

```css
.depth-grid{
  display:grid;grid-template-columns:repeat(2,1fr);gap:1px;background:rgba(26,0,58,.2);
  border:1px solid rgba(26,0,58,.2);margin-top:48px;
}
@media (max-width:820px){.depth-grid{grid-template-columns:1fr}}
.depth-card{background:#fff;padding:26px 26px 28px}
.depth-card h3{font-size:1.2rem;font-weight:700;letter-spacing:-.01em;margin-bottom:10px}
.depth-card p{font-size:13.5px;line-height:1.62;opacity:.72}
```

`.depth-row .kt` のCSSは `.depth-card .kt` でも使うため、`.kt` のルール自体(ink地にlime文字のタグ)は **セレクタを `.kt` 単体に直して残す**。

---

## 5. #usecases — カードティザーに作り替え

セクションの器(ink背景、eyebrow「Use cases」)は維持。h2 は「One question in. One clear call out.」のまま。lead も維持。**`.uc-row` 3つを削除し、use-cases.html のカードデザイン(`.ins-card`)3枚のグリッドに差し替える。** 末尾に全件ページへのCTAを追加。

```html
<div class="uc-grid rv">

  <a class="ins-card" href="use-cases.html">
    <div class="shot-frame">
      <div class="bar"><i></i><i></i><i></i><span>pipeline audit &middot; software</span></div>
      <div class="ph">THUMBNAIL SLOT<br>16:10</div>
    </div>
    <div class="ins-body">
      <div class="ins-meta"><span class="cat-tag">Software / API vendor</span></div>
      <h3>&ldquo;Everyone is interested. Nobody is buying.&rdquo;</h3>
      <p>Every open opportunity audited and graded, a dozen segments researched
      &mdash; and an end to the free pilots that consumed the team's capacity
      without a path to a contract.</p>
      <span class="ins-link">Read the case <i>&rarr;</i></span>
    </div>
  </a>

  <a class="ins-card" href="use-cases.html">
    <div class="shot-frame">
      <div class="bar"><i></i><i></i><i></i><span>route map &middot; manufacturing</span></div>
      <div class="ph">THUMBNAIL SLOT<br>16:10</div>
    </div>
    <div class="ins-body">
      <div class="ins-meta"><span class="cat-tag">Precision manufacturer</span></div>
      <h3>&ldquo;We want this one buyer. How do we get in?&rdquo;</h3>
      <p>The buyer's actual route in, reconstructed from primary sources &mdash;
      and the missing credibility signals named, in the order they needed
      fixing.</p>
      <span class="ins-link">Read the case <i>&rarr;</i></span>
    </div>
  </a>

  <a class="ins-card" href="use-cases.html">
    <div class="shot-frame">
      <div class="bar"><i></i><i></i><i></i><span>buyer-first research &middot; new category</span></div>
      <div class="ph">THUMBNAIL SLOT<br>16:10</div>
    </div>
    <div class="ins-body">
      <div class="ins-meta"><span class="cat-tag">New category</span></div>
      <h3>&ldquo;No one has ever evaluated this before.&rdquo;</h3>
      <p>Buyer-first research where no playbook exists on either side, and a
      tested storyline the internal champion could carry into review.</p>
      <span class="ins-link">Read the case <i>&rarr;</i></span>
    </div>
  </a>

</div>

<p class="cap rv">Composite examples, anonymized. A full anonymized case study is available on request.</p>
<a class="btn btn-lime rv" href="use-cases.html" style="margin-top:28px">See all use cases &rarr;</a>
```

CSS:
- `.uc-row` 一式(`.uc-row .who` `.uc-row blockquote` `.uc-row .body …`)を削除。
- **insights.html から以下のCSSブロックをそのままコピーして追加**: `.ins-grid`(→ 後述の #insights で使う)、`.ins-card` 一式(hover含む)、`.cat-tag`、`.ins-meta` 一式。コピー元は insights.html の `/* category tag + meta line */` と `/* grid of insight cards */` のブロック。
- `.uc-grid` は `.ins-grid` と同じ定義でよいが、ink背景上なのでグリッド余白だけ定義する:

```css
.uc-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:26px;margin-top:52px}
@media (max-width:940px){.uc-grid{grid-template-columns:1fr 1fr}}
@media (max-width:680px){.uc-grid{grid-template-columns:1fr}}
```

- ink背景上の `.ins-card` は白カードなのでそのまま成立する。`.ins-card` の文字色は `--ink` を継承する必要があるため、`#usecases .ins-card{color:var(--ink)}` を追加すること(セクションが `color:#fff` のため)。
- `.ins-link` を `<span>` で使う(カード全体が `<a>` のため、内側に `<a>` を入れてはいけない)。

---

## 6. #philosophy — 新設(旧 #posture の位置)

**`<section id="posture">` を丸ごと削除**し(Honest calls の引用3つ+principles 4枚は完全に消す)、同じ位置に以下の新セクションを置く:

```html
<!-- ================= PHILOSOPHY ================= -->
<section id="philosophy" class="sec">
  <div class="wrap">
    <p class="eyebrow rv">Philosophy</p>
    <h2 class="h2 rv">Selling creates the gap.<br>Seeing closes it.</h2>
    <div class="cols">
      <p class="statement rv">
        The gap between great products and the buyers who need them is usually
        created by selling itself &mdash; not the product, not the market.
        Everything we do is a method for <em>staying in seeing mode</em> while
        still moving deals forward.
      </p>
      <aside class="margin-note rv">
        <b>Why this is hard</b>
        Destroying yesterday's success is a mental game before it's a
        strategic one. We treat that game as part of the job.
      </aside>
    </div>
    <a class="btn btn-ghost rv" href="about.html" style="margin-top:36px">Read our beliefs &rarr;</a>
  </div>
</section>
```

(index専用の短縮版。about.htmlの同セクションはフルバージョンのまま。indexは要約、aboutが詳説という関係にする。)

CSS対応: 現在 `#gap .cols` と `#statement`(id指定)になっているセレクタを、両セクションで共用できるように直す:
- `#gap .cols{…}` → `.cols{…}` に変更(メディアクエリ内の同セレクタも同様)。
- `#statement{…}` と `#statement em{…}` → `.statement{…}` `.statement em{…}` に変更し、**#gap 側のHTMLも `<p id="statement" class="rv">` → `<p class="statement rv">` に変更**する。
- `.pq-list` / `.pq-item` 一式、`.principles` / `.principle` 一式は #posture 削除により未使用になるので削除。

---

## 7. #insights — カードグリッドに作り替え

イントロ(eyebrow「Insights」/ h2「What the log keeps teaching us.」/ lead)は現行のまま維持。セクションに `style="background:var(--gray)"` を付与。**`.ins-list`(1列リスト)を削除し、insights.html の `#latest` にある `.ins-grid` + `.ins-card` を3枚(1行)だけコピーして差し替える。** 2行(6枚)だとスペースを取りすぎるため、indexでは1行に絞る方針(2026-07-21確定)。

- コピー元: insights.html の `<div class="ins-grid rv">…</div>` のうち先頭3枚(Buyer Side / Win & Loss / Regulation)。マークアップ・文言とも一字一句そのまま使う。リンク先は `#` のままでよい(記事は未執筆)。残り3枚(Operations / Win & Loss / Buyer Side)はindexには載せない。
- カード内の `href="#"` 構造(サムネaタグ、cat-tag、ins-link)も insights.html と同一にする。
- グリッドの後、既存の `.cap`(Titles are placeholders…)は維持し、その後にCTAを追加:

```html
<a class="btn btn-ghost rv" href="insights.html" style="margin-top:28px">See all insights &rarr;</a>
```

CSS: `.ins-list` / `.ins-item` 一式(`.ins-item .meta` `.ins-item .ar` 含む)を削除。`.ins-grid` / `.ins-card` / `.cat-tag` / `.ins-meta` は §5 で insights.html からコピー済みのものを共用。gray背景上の白カードで、insights.html と同じ見た目になる。

---

## 8. #products → #databases — 表現変更

「プロダクト」という表現を全廃し、「メンテナンスしている公開データベース」として語り直す。カードの器(`.prods` / `.p-card`)はそのまま使う。

変更点:
1. `<section id="products" class="sec" style="background:var(--gray)">` → `<section id="databases" class="sec">`(gray指定を外す)。
2. eyebrow: `Products` → `Databases`
3. h2: `We run our own tools in production.` → `Two public databases,<br>kept current.`
4. lead: 差し替え →
   ```
   Side outputs of the research work, published as-is: the same primary-source
   discipline we run for clients, in database form. Free to browse.
   ```
5. GRANTSカード: `<span>Database</span>` のまま。説明文もそのまま。
6. BUYERSカード: `<span>Intelligence</span>` → `<span>Database</span>`。説明文 `Buyer-side intelligence on the Japan market. Placeholder copy, to be confirmed.` → `A structured database of Japanese buyer organizations. Placeholder copy, to be confirmed.`
7. コメント `<!-- ===== PRODUCTS ===== -->` → `<!-- ===== DATABASES ===== -->`

---

## 9. #contact・フッター

- #contact: 変更なし。
- フッター: 変更なし(Resources欄はすでに「Grants database / Buyers database」表記で整合)。
- `.fbottom` の2つ目の span: `Mockup v5 · hero rewrite & services teaser · for direction review` → `Mockup v6 · section restructure (cards, philosophy, slim process) · for direction review`
- `<title>` の `(mockup v4, crafted)` → `(mockup v6)`

---

## 10. CSS削除チェックリスト(未使用化するもの)

差し替え完了後、以下のセレクタがHTML中で未使用になっていることを確認して削除する:

- `.m-flow` `.m-cell`(子孫含む)/ `.m-annot`(`.flip` 含む)/ `.spine`(子孫含む)
- `#method{background:var(--gray)}`
- `.depth-row`(子孫含む。ただし `.kt` 自体は §4 のとおり残す)
- `.uc-row`(子孫含む)
- `.pq-list` `.pq-item`(子孫含む)
- `.principles` `.principle`(子孫含む)
- `.ins-list` `.ins-item`(子孫含む)
- `.shot.doc`(depth の 3:4 スクショ廃止で未使用になる)

`.do-grid` `.do-list` `.do-item` は #otherside に移設して使い続けるので**削除しない**。`.shot` `.shot-frame` は hero / otherside / カード内サムネで使い続けるので削除しない。

---

## 11. 完了確認(QA)

1. ページ内のセクション順・背景色が §0 の表と一致している。
2. 「Interest was never your problem」「Honest calls, not comfortable ones」「We run our own tools in production」の3フレーズがページに存在しない(grepで確認)。
3. 「LANE」「BATCH」「CALL」の3セルが存在しない。
4. #usecases と #insights のカードが、それぞれ use-cases.html / insights.html のカードと同じ見た目(白カード、hoverで lime のオフセットシャドウ+左上に3pxずれる)になっている。
5. カード内にネストした `<a>` がない(HTMLバリデーションエラーになるため。カード全体が`<a>`の場合、内側はすべて`<span>`/`<div>`)。
6. SP幅(375px)で全セクションが1カラムに落ち、横スクロールが発生しない。
7. `.rv` reveal が新設ブロックにも付与されており、スクロールでフェードインする。
8. how-we-operate.html / about.html / use-cases.html / insights.html への各CTAリンクが正しく張られている。
