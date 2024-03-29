---
title: はじめての個人ブログを公開しました
subTitle: My first personal blog site is now published
type: "release"
date: 2024-01-13
published: true
---

[フライさん](https://github.com/loxygenK) です。こんにちは。

お正月中のお休みを使ってブログにコミットを続けて、とりあえず公開できるかなというところまで作ることができたので、
現在のバージョンで一度公開することにしました。**はじめての個人ブログです**!

# 能登半島地震にて被災された方々へ

能登半島地震にて亡くなられた方々に謹んで哀悼の意を表すとともに、被災された方々に深くお見舞い申し上げます。
また、被害の拡大なくいち早く復興が進み、皆様が一刻も早く元の生活を送ることができることができますよう心からお祈り申し上げます。

-----

# 作った理由

## 自分が思うように情報を発信したい

個人ブログを作りたかった一番の理由は、自分が発する情報をできるだけ自分が思うようにコントロールしたかったからというのが大きいです。

個人的な思想として……

- 情報を発信する人間は、**基本的には客観的に信頼できる情報を発信するべき**。
- 客観的に信頼できない情報を発信したい際は、**その旨を明示して信頼できる情報と信頼できない情報を区別できるようにするべき**。

… というものがあります。こうすると、仮に読み手の方が情報の検証を自分からしなかったとしても、
**こちらから能動的に信頼性を伝えることができるので、推測など事実でない情報が事実であるというような形で**
**拡散されてしまうのを防ぐことができるかな**と思っています。
~~あとそういうことがあった場合に「私は推測って言ったもん」って言い張れる~~

### そういう機能がついています

……上の段落は私の思想が結構詰まっていました。このブログには、このような「客観的に信頼できない」情報をひっくるめるための機能をつけています:

<Section type="opinionated">
  個人的な思想として、情報を発信する人間は、基本的には客観的に信頼できる情報を発信する一方で、客観的に信頼できない情報を発信したい際は、
  その旨を明示して信頼できる情報と信頼できない情報を区別できるようにするべき、というものがあります。
</Section>

こんな感じです! スクリーンショットを取ろうにも、後ろの透かしがついているので「この情報は推測です」みたいな感じのメタデータがついて回ります。
テキストをコピー & ペーストされたらしょうがないですが、選択箇所へのリンクとかであれば信頼性の情報を同時に伝えられます。

ちなみに記事は MDX 形式を使って書かれているので、このような形で私独自のタグを使って好きにセクションを生やすことができます。

```tsx
<Section type="opinionated">
  ここに思想が濃い段落が入ります。`type` は今は 2 つ準備していますが、めっちゃ簡単に type を増やせるようにしています。
</Section>
```

## 手軽に情報をアウトプットできる場所がほしい

今まで Zenn に記事を投稿する際、1 記事数日というそれなりの時間をかけていましたが、それ以外にアウトプットできる場所がなく、
**手軽に学んだ場所をアウトプットする場所がない**という悩みがありました。かといってぱっと書いたような記事を Zenn にあげてしまうと……
ひどい場合だと**検索汚染**のような情報になってしまうリスクもあります。**そういうことは私はしたくないです!**
そんなわけで、Zenn とは他に手軽にアウトプットできる場所としてずっと個人ブログを望んでいました。

手軽にアウトプットできるサイトが欲しかったのはそうですが、**もちろん個人ブログを肥溜めにするつもりもありません!**
ちゃんと精査した情報とかもこっちに流すようにしたいなあとは思います。

一方で、精査されてる情報とそうじゃない情報が混ざってしまうのはあまりいただけないのはそうです。これのソリューションとして、
記事ごとにさきほどのような「この情報は強い自論を含んでいます」みたいな属性を付ける案も考えています。
データとしては用意しているので、それが精査されてる情報なのかまだ学び途中な情報なのかをすぐに見分けられるようにしたいと考えています。

## 久しぶりに個人開発がしたかった
;
長い間マジで業務しかやってなくて、少しプログラミングに飽きてきたような感情もありました。これなので何か責任範囲が狭いものをやりたいぞという気持ちになり、そこでずっとやりたかった個人ブログを作ることにしました。

責任範囲は狭いですし (ブログが壊れても私しか困らない)、いろいろなことを試してみています。

### OGP サポート

このブログのページは OGP をちゃんとサポートしているので、埋め込みとかがちゃんと働きます!

実装は Next.js の [Metadata files](https://nextjs.org/docs/app/api-reference/file-conventions/metadata/opengraph-image) を使いたかった……
**のですが、** Next.js が今抱えているバグ ( [vercel/next.js#51147](https://github.com/vercel/next.js/issues/51147) )
を踏んでしまっていい感じに使うことができませんでした。

Open Graph のための画像の生成時、記事データ (MDX) はビルド時にしか持っておらず、画像は Static に生成しなくてはならなかったのですが、
上の Issue にぶつかって、`page.tsx` に `generateStaticParams` があるなど SSG できるような状態でもそこだけ
Dynamic になってしまうというようなことがありました。

なので代わりに、[上の Issue でも紹介されていた](https://github.com/vercel/next.js/issues/51147#issuecomment-1842197049)、
Route Handler から `ImageResponse` を返して、そこを `og:image` に指定するというメソッドで乗り越えました。

### A11y

まだまだ改善の余地がありますが、少しだけ A11y に気を配っています。WAI-ARIA とかに全く詳しくないので、
変な用法をしてしまっているようなところも多くあるかと思うので、そういうところは直していきたいです。

現時点でそもそも終わってるところもあります。例えばさっきのコードブロックはたぶんスクリーンリーダーを使われている方だと、
結構しんどいものがあるかと思います……。なので、`alt` 属性を指定できるようにしてどんなコードなのかを説明できるようにするなどの
工夫を今後盛り込みたいです。

# 技術的な詳細

私はフロントエンドエンジニア (フロントエンドエンジニアレベル: へなちょこ) なので、フロントエンドエンジニアとしてこのブログがどうなっているのかを説明したいと思います。

![ブログの構成図。詳しい説明は後述しています。](https://raw.githubusercontent.com/loxygenK/blog-articles/main/pngs/13-introduction_construct.png)

### フロントエンド側の技術的な詳細

Next.js の **App Router** をちゃんと使ってみました。

スタイリングには CSS Modules を使いました。今までは SCSS を書いていたのですが、思い切って特にそういうのは使わず CSS
をそのまま書くことにしました。結果普通にワークしたので、依存しているツールチェインの数が減ってよかったなあという顔になっています。

また、Linter には **Biome** を使ってみました。今までは ESLint と Prettier でブン回していたのですが、管理するものが減るって……いいな!
という気持ちになったので使ってみたような感じです。結果こちらもいい感じにワークしています。あと早いし。

早いといえば、**Bun** を使おうとしたのですがモジュール解決周りがなんかうまく行かず断念しています。
一時期依存関係が謎に破滅していたときがあったのでそれが原因かもしれないので、今やったらうまくいくかな?

### 記事データの管理メソッド

**記事データは別リポジトリで管理するようにしました。**

ビルド時には Submodule を使って利用可能にしています。API 組んでどうとかはしてないです。

ビルド上は記事データが「ソースコード」みたいな立ち位置で扱われていて、ランタイムでは記事データが unavailable になります。
このため、記事データが必要なものはすべて Static に生成しています。

ビルドパイプラインはこんな感じにしています:

1. 私が `blog-articles` リポジトリ上の記事を更新する。
1. `push` イベントで GitHub Action が発火して、`loxygenK/blog` 上の GitHub Action を発火する。
3. GitHub Action が submodule のリビジョンを更新して、`production` ブランチに Push する。
4. Vercel がその変更を検知して、デプロイする。

合計時間はだいたい 1 分半から 2 分くらいな気がします。

### 総評

技術的な面の総評として、管理するものを結構減らせているので、結構 shallow な構成になっていいかなあと思っています。
Linter と Formatter はまとめたし、スタイリングについても Next.js のビルドインサポートで収まっているので。

それでいて自分の作りたいものが作れて、開発体験や執筆体験 (???) もそれなりによいところに落とすことができたので満足しています!

もちろん改善できるところはあると思うので、今後もいろいろやってみようと思います。
フロントエンドの技術、1 年たったら obsolete になってるやつ結構あるだろうし……

# 改善したいところ

改善点は結構あります。例えば……

- 先程述べたような A11y 上の問題点や
- `404.tsx` が Next.js そのまんま
- 雑なコードを書いてしまっているところがあるのでそこの整理

など、その他にも新機能や OG に盛り込む内容をもっと充実させたりなどもあります。

また、今のところ画像をアップロードする方法が一回 GitHub に Push して URL 作ってもらって、という感じで結構つらいです。
これを自動化できるユーティリティを作るか、自分でそこらへんのインフラ用意するかとかができるといいかなあみたいなのを思っています。

それと、単純に**なんか読みにくい**かもしれません。段落の間のスペースとかの調整がたぶん甘いと思うので、この調整をちゃんとしたいです。

# 今後の情報発信 ― このブログの使い道

先程述べたように、Zenn に記事を公開する際に時間をためすぎてアウトプットがなかなかできないというのが悩みだったので、
今後はこのサイトにいろいろぽんぽんアウトプットしていきたいと思っています。

一方で、その情報の信頼性をちゃんと明示していって、読み手の方を困らせないようなブログサイトになるよう更新を続け、
**いわゆる「クソサイト」とは真逆のサイトとして更新しつつ**情報を発信できればと思っています。

------

ここまで読んでいただきありがとうございました! 手軽にアウトプットする場として、いっぱい更新していこうと思うので今後もよろしくお願いします。
また、冒頭の繰り返しになりますが、能登半島地震の犠牲となった方に哀悼の意を表すとともに、
被災された方々に一刻も早く普段の生活が戻ることを切に願っています。
