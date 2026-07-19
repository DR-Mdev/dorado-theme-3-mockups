# Dorado サイト再設計メモ(index / about / services)

作成日: 2026-07-18
状態: **設計ドラフト段階。HTML本体はまだ未着手。** 次のチャットではこのファイルを読んでから作業を再開する。

---

## 背景・課題認識(ユーザーからの指摘)

- **about.html** が事例(The record / 4 sectors / 引用)で埋まっているが、事例は use-cases.html の領分。About にはフィロソフィーと社長のプロフィール、他事業(ペットシッターサービス経営など)を入れたい。
- **services.html**(現状 Validate / Sell / Operate の3ステージ紹介)の存在意義が薄い。**pricing.html ですべて吸収できている。**
- **index.html の HERO** が、最も多くの人が着地する場所にもかかわらず「何をやる会社か」が伝わらない。Advisory / Performance / Retainer を売っていることと、"See Our Services" 的なボタンが要る。
- HERO のコピーの軸:顧客は既に素晴らしい知識・プロダクト・サービスを持っているのが大前提。一方で日本側バイヤーが欲しいものと、自分たちの提案の間にギャップがあり、それをうまく埋められていないケースが多い。そこを解消する、という訴求にしたい。
- **pricing.html を services.html に改名するのが適切**(現Servicesページは廃止・吸収)。

---

## 確定した構造方針

### サイト全体のIA変更

| 現状 | 変更後 |
|---|---|
| services.html(Validate/Sell/Operate) | **廃止。** 3ステージの枠組みは「仕事の進み方」の話として **how-we-operate.html(Process)に吸収**。`#validate` `#sell` `#operate` のアンカーは同名のままhow-we-operate.html側に用意し、他ページ(use-cases.html, about.html等)からのリンク切れを防ぐ。 |
| pricing.html(Advisory/Performance/Retainer) | **services.html に改名。** 「売っているもの=この3プラン」として再定義するページになる。 |
| about.html(事例中心) | **全面刷新。** フィロソフィー+代表プロフィール+会社概要。事例系(The record / Across every stage / Four sectors / 引用群)は削除し use-cases.html に譲る。 |
| ナビ | Services / Process / Use cases / Pricing / Insights / Partners / About / Contact → **Pricingがナビから消え、Servicesが指す先が変わる** |

### 各ページのアウトライン(決定事項)

#### index.html
```
1. HERO(全面書き換え)
   H1:  Make your product/service make sense to Japanese buyers.
   サブ: 顧客は既に優れたプロダクト・知識・サービスを持っている。
        足りないのは、日本のバイヤーが欲しいものと自分たちの提案の間の
        ギャップを埋めること。それを解消するのが仕事。
   CTA: [See Our Services →](services.html) / [Contact]

2. Facts strip(維持)
3. Services teaser(HERO直後に新設。Advisory/Performance/Retainerを2行+価格で紹介 → services.htmlへ)
   ※ Services teaser を先に置く方針で決定(The gapより前)
4. The gap(statement — 維持。HEROの深掘り)
5. The other side(維持)
6. What we actually do(維持・Services teaserとの重複は軽量化)
7. Process(維持 → how-we-operateへ。Validate/Sell/Operate吸収先への導線)
8. Use cases teaser(維持)
9. Insights / Products(維持)
10. Contact
```

#### about.html(全面刷新)
```
1. HERO — フィロソフィー宣言(短い一文)+ 写真スロット
2. Philosophy — 会社の信条のステートメント
3. Beliefs — 4つの信条(引用スタイル)
4. Working principles — 8つの行動原則(グリッド)
5. Founder profile — 名前・肩書・写真・経歴タイムライン
6. Company profile — 登記情報テーブル。事業内容の欄にペットシッター事業を一行で記載(会社概要の一行のみ、という方針で決定)
7. Contact
```
削除:The record / Across every stage / Four sectors / 引用群 → use-cases.html の領分。

#### services.html(旧pricing.htmlを改名・拡張)
```
1. HERO — "Three ways to work with us"(Advisory / Performance / Retainer)
2. The structure(現unit維持 — 各プランが生むもの)
3. 各サービス詳細 ×3(現plans拡張。旧Servicesページの物語資産
   "Interest was never your problem" 等はPerformance/Retainerの説明に転用)
4. In practice(維持)
5. The exact numbers(維持)
6. FAQ(維持)
7. Contact
```

#### how-we-operate.html(追加変更)
Validate / Sell / Operate の3ステージを「エンゲージメントの進み方」として受け入れるセクションを追加。`#validate` `#sell` `#operate` のアンカーを維持してリンク切れを防ぐ。既存の運用システムの話(One question → One test → One decision)の前段に置く。

---

## about.html 用フィロソフィー・コピー ドラフト(英文)

ユーザー提供の価値観メモ(日英混在の生の言葉)を、4つの信条(Beliefs)+8つの行動原則(Working principles)に構造化したもの。**このコピーはまだユーザーの最終確認前。**

### HERO
> **H1:** Make your product/service make sense to Japanese buyers.
>
> **サブコピー:**
> Every seller drifts into selling mode. KPIs push you to hear a "yes" in every reply, and the buyer's real need — the thing they cannot live without — disappears behind what you hoped they said. Dorado was built around one discipline: see the customer and the market as they are, not as the forecast needs them to be.
>
> **CTA:** [Our beliefs →](#beliefs) / [Who's behind this →](#founder)

意図: 「売る行為そのものが、避けたいギャップを生む」というアイロニーを会社の看板に昇格。index の「We bridge the gap...」(顧客へのベネフィット訴求)と対になる — Aboutはその裏にある思想を語る。

### Philosophy statement
> **本文:**
> The irony of our trade is that the gap between great products and the buyers who need them is usually created by selling itself. In selling mode you hear what you need to hear, and the KPI becomes an obstacle to observing things as they are. That distortion — not the product, not the market — is where most stalled deals begin. Everything we do is a method for staying in seeing mode while still moving deals forward.
>
> **マージンノート(Why this is hard):**
> Experience forms belief, and a belief that once produced success hurts to abandon. Destroying yesterday's success is a mental game before it is a strategic one. We treat that game as part of the job.

### Beliefs(4つ)

**Belief 01 — "What they want is not what they need."**
A need is something the customer cannot live without. A want is better to have, and never becomes a necessity. Even a real pain isn't always a need — companies live with problems that never justify a purchase. It's a cliché until you try to apply it to a live pipeline; then it becomes the hardest call in the work.

**Belief 02 — "The gap starts on the selling side."**
Buyers rarely create the misunderstanding. Sellers do — because selling mode distorts what the customer is actually saying. The conversations that lead nowhere almost always started there. Our first job on any engagement is to strip that distortion out.

**Belief 03 — "Business only moves when both sides win."**
商売は相手ありき — there is no deal without the other side. We don't do one-way pitches, and although we are an external firm, we move as part of your team. The point of all of it: companies that need a better product or service learn about it correctly, then buy it or resell it — and both sides end up economically better off. That's the entire purpose.

**Belief 04 — "Destroy yesterday's success."**
Markets, customers, targets, and partners change every time. No two situations repeat. The playbook that worked last quarter is a hypothesis again this quarter — nothing more.

### Working principles(8つ)

01. **Understand fastest** — No two situations are ever the same. The job is to be the fastest to understand this customer, this market, this moment — not to reuse last year's read.
02. **Reasons to win** — Walls are the default state of market entry. We look for the reasons something can work before cataloguing the reasons it can't.
03. **Misses move you forward** — A hypothesis that gets no response looks like a wasted shot. It isn't. Without it, you never advance. We hate waste — and we log every miss, because the misses are the map.
04. **Concentrate or endure** — When the evidence says go, commit resources at once. When it doesn't, be patient and keep doing what can be done. Both are decisions; drifting between them is not.
05. **Bad news first** — We don't hide things. If something is going wrong, you hear it from us before you feel it in the numbers.
06. **Two clocks** — Short-term KPIs keep the work honest week to week. Long-term KPIs are why the work exists. We run both clocks and never let one silence the other.
07. **New tools, proven use** — We keep up with technology — we built our own operating system for this work. And we use AI where measured results justify it, not everywhere it's fashionable.
08. **Hard days are the job** — Most days in market entry are hard days. The work is holding on through them and finding the move that breaks the wall.

### Founder profile(スケルトン — 要インプット)
- 見出し案: "The person you'll actually work with."
- 写真スロット(4:3、実写)
- 名前・肩書 【要インプット】
- 経歴タイムライン:15+年の日本市場経験の中身(業界・役割)→ Dorado設立 【要インプット】
- 結びの一文案: "The beliefs above weren't written for this page. They're what fifteen years of deals — won and lost — left behind."

### Company profile(テーブル)
- Company name: Dorado LLC (Dorado合同会社)
- Location: Tokyo, Japan 【住所の公開粒度を確認】
- Representative: 【要確認】
- Founded: 【要確認】
- Business: Japan market research & entry validation · go-to-market operations · customer success representation · pet care services(ペットシッター事業 — 正式名称・URL要確認)
- Languages: English / Japanese
- Products: grants.dorado-group.jp · buyers.dorado-group.jp

---

## コピー構造化にあたっての判断メモ

- 「困っていることとニーズは異なる」は Belief 01 に統合(「real pain isn't always a need」)。want/needの話と核が同じで独立させるほどの分量がないため。
- 「辛い時のほうが多い」は信条ではなく行動原則(08)に配置。信条4つ=「市場の見方」、原則8つ=「働き方」という切り分け。
- ペットシッター事業は会社概要テーブルの一行のみ(方針として決定済み)。
- HEROの「Selling creates the gap. Seeing closes it.」は index の「We bridge the gap between great products and the buyers who need them.」と意図的に対にしている。index=何をしてくれるか、About=なぜそれができるか。

---

## 未解決・次のチャットで必要なインプット

1. **代表プロフィール確定情報**
   - 公開する名前の表記
   - 肩書
   - 経歴の要点(業界・年数・役割)
   - 写真を実際に載せるか
2. **会社概要の確定情報**
   - 所在地(公開する粒度:番地まで出すか、区・市までか)
   - 設立年
   - 代表者名
   - 事業内容の正式な列挙(ペットシッター事業の正式名称・URLがあれば)
3. **フィロソフィー・コピーのトーン確認**
   - 上記ドラフトの英文が硬すぎ/柔らかすぎないか
   - 日本語原文のニュアンスをもっと反映すべき箇所があるか
4. **旧services.htmlの資産の転記作業**
   - "Interest was never your problem" 等の物語をどこまでservices.html(新)に転用するか、実際にHTMLを触りながら精査が必要

---

## 次のアクション

上記1〜3のインプットが揃い次第、以下の順で着手:
1. about.html 全面書き換え(このメモのアウトライン+コピーをベースに)
2. pricing.html → services.html へのリネーム・改修(旧services.htmlの資産吸収)
3. how-we-operate.html にValidate/Sell/Operateセクションを追加(アンカー維持)
4. index.html のHERO・Services teaserセクションの改修
5. サイト全体でservices.html関連のリンク(旧servicesへの参照)を新構造に張り替え
   - 影響範囲の洗い出しが必要:current services.html を参照している箇所(about.html, use-cases.html, どこか他にもあるか要grep)
