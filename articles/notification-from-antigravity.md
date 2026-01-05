---
title: "Antigravityの作業終了時にデスクトップ通知を受け取る"
emoji: "🚀"
type: "idea"
topics: ["antigravity","gemini"]
published: true
---


## はじめに

Antigravityを使って複雑なタスクや重い実装をAIに任せているとき、処理が終わるまで画面をじっと見つめて待っていませんか？

私は、別のエディターを開いてコードを書いていて、気がついたらAIの作業が終わっていた、ということがよくあります。

AIには無駄なく働いてもらいたいので、この処理待ちの時間を有効活用したいと思い、**作業が完了したらMacのデスクトップ通知とサウンドで知らせてくれる**ようにしました。

:::message alert
Antigravityの設定画面に通知の欄があり、システムでも有効にしているのですが、作業完了時には通知が来ないため、この方法を試しました。
:::

### 対象環境

:::message

- この記事は**macOSユーザー向け**の内容です（`osascript`を使用）
- Linux/Windowsでも通知コマンド（`notify-send`、`msg`など）を使用すれば同様に実装可能です

:::

## 設定方法

[こちらの記事](https://zenn.dev/ken1141/articles/4a8810343e5c07)を参考に、Antigravityに確実に指示を守らせる方法を学びました。

以下の記述を追記することで、「処理が終了したら、作業内容を要約してmacOSの通知センターに送り、完了音を鳴らす」ことができます。

### `GEMINI.md`に以下を追加

````markdown:GEMINI.md
# CRITICAL PROTOCOLS (ABSOLUTE PRIORITY)

1. **処理の終了時通知**: 作業完了時は、`{msg}`に作業内容を簡潔に記載し、次のコマンドを実行してください
```bash
osascript -e 'display notification "{msg}" with title "GEMINI"' && afplay /System/Library/Sounds/Glass.aiff
```
````

## コマンドの解説

macOS標準のコマンドラインツールを使用するため、外部ツールのインストールは不要です。

### 1. 通知の表示（`osascript`）

基本はこのコマンドのみでOKです。

```bash
osascript -e 'display notification "{msg}" with title "GEMINI"'
```

- `osascript`: AppleScriptをコマンドラインから実行するツール
- `display notification`: macOSの通知センターに通知を表示
- `{msg}`: AIが作業内容の要約を埋め込む部分
- `with title "GEMINI"`: 通知のタイトルを設定

### 2. サウンドの再生（`afplay`）

```bash
afplay /System/Library/Sounds/Glass.aiff
```

- `afplay`: macOS標準のオーディオファイル再生コマンド
- `/System/Library/Sounds/`配下に他のサウンドファイルもあるので、お好みで変更可能
- 任意のサウンドファイルのパスを指定することもできます
- **Macをおやすみモード/集中モードにしていても音は鳴る**ため、通知に気づきやすいです

## 導入後の変化

この設定のおかげで、以下のような快適なワークフローが実現できました：

1. 重いリファクタリングやコード生成をAIに指示
2. 別のウィンドウでドキュメントを読んだり、Slackの返信をする
3. 「ピン！」という音とともに、画面右上に「○○の修正が完了しました」と通知
4. すぐに作業に戻る

**「AI作業待ち」のストレスが大幅に減り**、待ち時間を有効活用できるようになったのでおすすめです。

### 応用例

通知方法は他にもカスタマイズ可能です：

- SlackやDiscordへのWebhook送信
- カスタムスクリプトの実行

同じ`CRITICAL PROTOCOLS`の仕組みを使えば、様々な自動化が実現できそうです。

## おわりに

今回はMac環境での設定例でしたが、`GEMINI.md`への一行追加という非常にシンプルな方法でUXが大きく向上しました。

`CRITICAL PROTOCOLS (ABSOLUTE PRIORITY)`という強力な仕組みを教えてくれた、[ken1141さんの記事](https://zenn.dev/ken1141/articles/4a8810343e5c07)に感謝です！
