# TopazChat
VRChat向けの高音質・低遅延な音声・映像配信サービスです。
こんな用途に使われています：
- 演奏を高音質でワールド音声から再生
- VJやゲームプレイ画面をワールドに設置したモニターに表示

音質はAAC 320kbpsステレオ、遅延は東京で1秒未満です。映像は上限2Mbpsまで配信できます。  
ただし、映像配信機能は実験的なもので、想定以上に通信料が嵩んだ場合は予告なく停止する可能性があります。

Windows 10から音声を簡単に配信するためのアプリと、VRCSDK2向けの再生ギミックを配布しています。VRCSDK3でも視聴できますが、再生ギミックはまだ制作中です。

現状ではWindows 10のVRChatクライアントでのみ視聴できます。Quest向けでは動作しません。

個人利用であれば無償でご利用いただけます。法人の利用についてはお問い合わせください。

Twitter: よしたか ([@tyounanmoti](https://twitter.com/tyounanmoti))  
mailto: tyounan.moti@gmail.com

# このGitHubレポジトリについて
TopazChat関連のソフトウェアを配布、マニュアル等の配置、バグトラッキングや開発予定の公開に使用しています。

# Discordサーバー
TopazChatに関する質問や雑談のためのDiscordサーバーです。  
招待リンク：https://discord.com/invite/fCMcJ8A

# 使い方
## ざっくり仕組み
TopazChatサーバーに音声・映像を配信すると、[TopazChat Player](https://booth.pm/ja/items/1752066) が配置されたVRChatワールドで視聴できるようになります。

![TopazChatシンプル構成図](docs/images/TopazChat_configuration_ja.jpg)

## 視聴する人
Windows 10 バージョン 1809 以降であれば、ワールドに参加するだけで視聴できます。VRChatのワールド音量を大きくしてお楽しみください。

## 配信する人
### 必要なもの
- [TopazChat Streamer](https://booth.pm/ja/items/1756789)または[OBS](https://obsproject.com/ja)
- [TopazChat Player](https://booth.pm/ja/items/1752066)が設置されたVRChatワールド

楽器演奏など音声のみを配信する方は [TopazChat Streamer](https://booth.pm/ja/items/1756789) を利用すると、複雑なセットアップなしに配信できます。映像が必要であれば、[OBS](https://obsproject.com/ja) でTopazChatを通して配信できます。

### TopazChat Streamerを使用する場合
演奏の音声を音声入力デバイスで取得できるようにしておきます。VRChatのボイスで誰かに演奏を聴かせることができれば準備完了です。[SYNCROOM](https://syncroom.yamaha.com/)の仮想入力デバイスを指定することで、音楽セッションの出音も配信することができます。

1. [TopazChat Streamer](https://booth.pm/ja/items/1756789) をダウンロードしてzipファイルを展開
2. TopazChat.exeをダブルクリックして起動  
![TopazChat Streamer](docs/images/TopazStreamer.png)
3. 「サウンド入力デバイス」を変更して、ウィンドウ下部の音量インジケータが緑色に反応することを確認する
4. VRChatワールド内に設置されているTopazChat Playerのストリームキーを確認する（記載がなければワールド制作者にきいてください）  
下図の例では、「topazroom」になります。  
![TopazPlayerの設置例](docs/images/TopazPlayer.png)
5. ストリームキーをTopazChat Streamerウィンドウの「ストリームキー」テキストボックスに入力
6. ウィンドウ上部の「接続」ボタンを押す
7. VRChatに戻り、TopazChat Player付属の「Global Sync」ボタンを押す

以上の手順でワールドから演奏が再生されます。

### OBSを使用する場合
1. 映像ビットレートを 2000 kbps 以下に設定
2. 配信設定を以下のように設定
    - **サービス** カスタム
    - **サーバー** `rtmp://topaz.chat/live`
    - **ストリームキー** *ワールドに記載されたストリームキー*
3. OBSの配信開始ボタンを押す
4. VRChatワールドに設置されたTopazChat Playerの「Global Sync」ボタンを押す

OBSを使用する場合の注意点：
- x264エンコーダーのzerolatencyチューンやNVENCエンコーダーのLow Latencyプリセットを使用すると映像が灰色になってしまいます。
- 詳細設定の「輻輳を管理するためにビットレートを動的に変更する（ベータ版）」や「TCPペーシングを有効にする」にチェックを入れると、遅延が増大することがあります。
- 視聴者のVRChatクライアントが30fpsを下回ると、映像が大きく崩れることがあります。
- 映像2Mbps + 音声320kbpsのビットレートを大きく上回ると、配信が強制的に切断されます。

## ワールドを作る人
### VRCSDK2
あらかじめ[Windows版のAVProVideo](https://assetstore.unity.com/packages/tools/video/avpro-video-1-x-windows-57969)が必要です。試用版でも問題なく動作しますが、実際に使用する際には製品版を購入することを推奨します。

1. VRCSDK2をインポート
2. AVProVideoの製品版または試用版をインポート
3. [TopazChat Player](https://booth.pm/ja/items/1752066)をダウンロードしてunitypackageをインポート
4. Assets/TopazPlayer2/Prefabs/TopazPlayer2.prefab をシーンに配置
5. ヒエラルキーの TopazPlayer2/MediaPlayer ゲームオブジェクトをインスペクタで開く
6. MediaPlayer コンポーネントのSource Pathを変更する  
デフォルトは `rtspt://topaz.chat/live/StreamKey` となっているので、StreamKey を任意の文字列に置き換え
7. ヒエラルキーの TopazPlayer2/UI/Address ゲームオブジェクトをインスペクタで開く
8. InputField の Text を手順5で設定したストリームキーと同じにする

以上で設置完了です。

配信開始されていればワールド参加時に自動で再生されます。Global Syncを押すとインスタンス内にいる全員が配信へ再接続します。Resyncを押すと、押した人だけ（ローカルで）再接続します。

AVProVideoをインポートする前にTopazChat Playerをインポートすると、MediaPlayerコンポーネントなどの設定がリセットされてしまうことがあります。TopazChat Playerのunitypackageを再度インポートすると修正されます。

AudioOutputコンポーネントがついているゲームオブジェクトは、同時に選択するだけで再生チャンネルの設定が共通化されてしまい、左または右チャンネルのどちらかしか再生されなくなってしまうことがあります。アップロード後はステレオ音源を配信して動作確認してください。

距離減衰カーブや音源の位置、ボイスチャットの音量によってはVRChat内部で音量が過剰となり音割れが発生することがあります。AudioSourceのボリュームを少し下げるなどの調整をお願いいたします。

AudioSourceを一つにして立体音響処理を切ることで直接ステレオ再生が可能ですが、距離減衰しないためにインスタンス参加者にとって予期しない音量で再生されることがあります。スポーン位置に対してPan Levelを減衰させたり、ローパスフィルターの減衰カーブを設定したりなど、インスタンス参加者の聴覚保護に努めてください。

### VRCSDK3 Udon
問題なく視聴できますが、Udon用のTopazChat再生ギミックはまだ制作中です。
VRCSDK2と異なり、Windows版AVProVideoアセットは必要なく、VRChatワールド内でURLの変更が可能になります。

再生ギミックを作成する場合は、以下の点にご注意ください。
- 視聴者がVRChatの設定で「Allow Untrusted URLs」にチェックを入れる必要がある
- VRCAVProVideoPlayerコンポーネントのUse Low Latencyにチェックを入れる必要がある
- VRCSDK3付属のUdonSyncPlayer (AVPro)プレハブに付属のUdonGraphでは定期的に再生位置を同期する処理が誤作動を起こして音声が途切れるため、該当部分を削除する必要がある

# NeosVRでの使い方
VRChatでの使い方と同様に配信したあと、NeosVRのVideoPlayerのURLに `rtsp://topaz.chat/live/配信時に指定したストリームキー` を指定してください。

# 既知の不具合
- 視聴中にVRChatクライアントへの負荷が大きくなった際に音途切れが発生すると、徐々に遅延が蓄積することがあります。視聴者側でResyncボタンを押すか、配信者側で演奏間の無音区間などでGlobal Syncを押すと遅延の蓄積が解消されます。
- TopazChat Streamerで接続ボタンを押したあとサウンド入力デバイスを切り替えると配信した音声が途切れがちになることがあります。
- VoiceMeeter Bananaの仮想入力デバイスをサウンド入力デバイスに設定すると配信した音声が途切れがちになることがあります。

# サーバー費用について
サーバーアプリケーションのライセンス費用はVoxelKeiさん ([@VoxelKei](https://twitter.com/VoxelKei)) が肩代わりしてくださっています。

クラウドサーバーの使用料・通信料については、よしたか ([@tyounanmoti](https://twitter.com/tyounanmoti)) が支払っています。[PixivFANBOX](https://tyounanmoti.fanbox.cc/) にてカンパを募っておりますので、ご協力いただけると助かります。

# サードパーティソフトウェアのライセンス ACKNOWLEDGEMENTS
- GStreamer licensed under the LGPL2
- GstSharp by thiblahute licensed under the LGPLv2.1

# ライセンス
TopazChatサービスの利用は、個人利用であれば無償です。法人が運営主体となるイベントでの利用や、番組制作などの法人利用は有償となりますので、[Twitter](https://twitter.com/tyounanmoti)や[メール](tyounan.moti@gmail.com)にてお問い合わせください。

TopazChat Streamerは[MIT License](COPYING)で提供しています。ただし、下記以外のファイルはGStreamerおよびGstSharpのバイナリ再配布ですので、LGPLとなります。
- TopazChat.exe
- TopazChat.exe.config
- bin/WasapiNativePlugin.dll
- bin/ja/TopazChat.resources.dll

TopazChat Playerは[MIT License](COPYING)で提供しています。
