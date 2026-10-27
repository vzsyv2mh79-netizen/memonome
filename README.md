# 🌱 芽メモ / Memonome

日本語のiPhoneメモアプリと、本人のChatGPT利用枠で試作品を作るMac連携。MITライセンスで公開しています。

**開発版です。App Storeへ提出済みの1.0とは別のコードです。自動連携の実アカウントでの検証と、チップ商品のApple側登録はまだ完了していません。**

## できること

- AIなし・オフラインでもメモ、検索、お気に入り、バックアップ、24時間以内の削除取り消し。
- ダークモード、ロック画面から新規メモ。
- 自分専用の同期フォルダでiPhoneとMacを接続。送信対象はメモごとに選択。
- 公式Sign in with ChatGPT。Macに保存する認証情報を同期フォルダへ入れません。
- メモからHTML/CSS/JSの試作品を生成し、Macへ保存。進捗と停止・再開をiPhoneに表示。
- 任意のチップ用StoreKit画面。価格はAppleから取得。未登録時は準備中と表示。

現在の自動開発は**静的Webアプリの試作品作成まで**です。実行・動作テスト・iPhoneアプリ化・App Store提出・公開は行いません。40%は試作品作成の段階を示す目安で、工数や完成度の保証ではありません。

## 追加料金への対策と限界

有料APIキー、別プロバイダーへの切り替え、クレジット購入、クラウド容量の購入は実装していません。運営側の常設クラウドサーバーも使用しません。

**ChatGPT設定で、このアプリの「契約枠を超えた後のクレジット使用」をオフにする必要があります。** この設定を照会・強制する公開APIは確認できていません。接続後の確認が終わるまで推論を止めます。あとからChatGPT側で設定を変えた場合まで、アプリ単体で料金ゼロを保証することはできません。既存のChatGPT・iCloud等の契約料金は別です。

- 1日最大3回。実行前に回数を記録し、クラッシュ後に同じ依頼を再送しません。
- AI利用枠エラー、利用可否不明、通信中断、保存エラーで停止。自動再試行なし。
- 同期フォルダ5MB、メモ3万文字、生成ファイル30個まで。
- iCloudなどの容量不足はOSが同期を止める場合があります。芽メモがそれを確実に検出できるとは限りません。同期先への書き込み成功はMacへの到着成功を意味しません。
- 同期フォルダを他人と共有しないでください。自分のメモだけを送ってください。

## ソースコードの取得

公開リポジトリの `memonome-source.zip` に、iPhoneアプリ・Mac連携・テストの全ソースを同梱しています。解凍したフォルダで以下を実行してください。

## Mac連携の起動

Python 3.12以降を用意して実行します。

```sh
python3 -m venv .venv
.venv/bin/pip install -r companion/requirements.txt
.venv/bin/python companion/main.py --sync '/自分専用の同期フォルダ'
```

表示された127.0.0.1のアドレスをブラウザで開き、Continue with ChatGPTで接続。利用設定で追加クレジットを無効にし、モデルを選んで開始します。対応状況は本人のアカウント・ワークスペースによります。

iPhoneの設定 → ChatGPT・Mac連携で、同じ同期フォルダを選びます。「このiPhone内」のフォルダではMacへ同期されません。メモのAIにおまかせを押すと送信します。Macの起動が必要です。iPhoneが閉じている間の即時同期は保証しません。停止も同期到着後に反映されます。

Macの認証情報・実行履歴・生成プロジェクトは通常 `~/Library/Application Support/Memonome` に保存します。`--local` で変更できますが同期フォルダの内側/外側に包含させることは禁止しています。実行中は同じローカル保存先を使う複数プロセスをロックします。ログイン情報をGitや同期フォルダに入れないでください。接続解除はChatGPT設定でも行えます。

## iPhoneビルド

`ios/Seed.xcodeproj` をXcodeで開き、自分の開発チームと一意のBundle IDを設定してビルドします。公開コードには開発者個人の署名情報を含めません。

チップ商品IDは `jp.shizuie.memonome.tip.small` / `.medium` / `.large`。すべて消耗型。別アプリとして配信する場合は商品IDも変更し、App Store Connectで設定してください。料金は必ずStoreKitの表示価格を使い、購入は本人のApple確認画面で完了します。機能の購入条件ではありません。

## テスト

```sh
.venv/bin/python -m unittest discover -s tests -v
```

## 公式仕様

- [ChatGPT利用枠での接続](https://developers.openai.com/siwc/token-sharing-open-source)
- [利用上限・停止処理](https://developers.openai.com/siwc/token-sharing-open-source/errors-and-recovery)
- [ChatGPT利用枠の設定](https://chatgpt.com/settings/usage)

本アプリはOpenAI・Appleの公式アプリではありません。手動共有用のClaude/Geminiリンクはありますが、これらの自動連携は実装していません。
