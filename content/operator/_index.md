+++
title="運用ガイド"
weight=2
+++

## Concrntのドメインを自分で建てよう！

MastodonやMisskeyなどでは、自分でインスタンスを建てても、自分のローカルタイムラインは独りぼっちだし、ほかのインスタンスのユーザーはそれぞれのローカルタイムラインのコンテキストで話をしがちだし、正直疎外感があることもありますよね。
Concrntはこうした問題を解決するために生まれたところもあります！Concrntは自分専用でサーバーを建てても、ほかのConcrntサーバーにあるコミュニティータイムラインを読み書きしたり、また自分のサーバーにコミュニティタイムラインを作ればほかのサーバーのユーザーに利用してもらうこともできます！

また、Activitypubはサーバーがオフラインである間、ほかのサーバーが自分のサーバーに対しての投稿をジョブキューに保存し、定期的に再試行を試みなければならないため、迷惑が掛かってしまうことがあります。
Concrntはサーバーがオフラインであっても、発生するペナルティが最小限に抑えられるように設計されているため気軽にサーバーを運用することができます！ぜひ挑戦してみましょう。

## Overview

Concurrentはモジュラリティの思想の元、マイクロサービスのような構成を取っています。

例えば、画像ストレージサービスはストレージやそのコンテンツの配信のため、自宅サーバーで運用するにはやや重いサービスです。
Concurrentはより気軽にドメインをホストできるようにするため、これらの「重い」オプショナルな要素をモジュールとして切り出しています。

Concurrentドメインを動かすために最低限必要なサービスは`gateway`サービスと`api`サービスです。

続いてほぼmustなのが`webui`, `url-summary`サービスで、完全にオプションなのが`activitypub`サービスとなっています。

これらのサービスはgatewayサービスを経由してユーザーに提供されます。

<TODO: なんかわかりやすい図>

## サービス概略
- `gateway` 文字通り外部とのゲートウェイとなるサービスです。認証等と各サービスへのアクセスの分配をおこなってくれます。gatewayサービスだけ外部公開することを想定しています。
- `api` Concurrentの基本機能を提供するサービスです。
- `webui` ユーザー登録ページを表示したり、admin panelを提供するサービスです
- `hyperproxy` ユーザーの代わりにurlで指定されたページのogタグ等をフェッチし、プレビューを表示するための情報を集めるサービスです。(これがないとconcurrent.worldではURLのプレビューが表示されません)
- `activitypub` concurrent-worldでの投稿をactivitypubとして外に公開し、また外部のユーザーのフォロー・相互通信を行うサービスです。(外部のノートをconcurrentオブジェクトにコピーして保存するのでDBを消費します)


