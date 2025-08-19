# キャンペーン作成にあたって準備するもの

* キャンペーン ID  
  * キャンペーンを一意に特定する ID  
  * 大文字小文字英数字とハイフン、アンダーバーのみ使用することができる  
  * 64 文字以下  
* キャンペーン一覧に表示される 1920 x 1080 のサムネイル  
* キャンペーンページ  
  * アプリで読み込む用のウェブページ  
* キャンペーンシーン  
  * キャンペーン用の STYLY シーン

# キャンペーンページの作成

キャンペーンページはサーバー上にある web ページ です。ローカルの PC で開発する方法を説明します。見た目に関しては基本的な web 技術をつかって Android / iOS で問題なく見れるページを作成すれば問題ありません。アプリ内部機構との連携をする時には PVL の用意するインターフェースを用いて行う必要があります。アプリ内部機構とは具体的に以下です。

* STYLY シーンの起動  
* カメラを起動する  
* 各種ユーザーデータの保存、読み出し、削除  
* GPS で現在地を取得する  
  * 開発中

これらの機能を利用する場合は PVL の提供する SATCH Campaign JavaScript Library を利用する必要があります。開発初期段階では、 PVL の提供する [Chrome Extension](#storage.savefile-についての注意事項) を Chrome にインストールして、動作の確認をしてください。また、ある程度制作が進んだあとは実機での確認が必要となりますので、[実機での確認方法](#アプリで開発中のキャンペーンの動作を確認する方法)の章を参照ください。

## デモページ

[Chrome Extension](#storage.savefile-についての注意事項) を Google Chrome にインストールして、[デモページ](https://satch-campaign.s3-ap-northeast-1.amazonaws.com/campaigns/example_v0.0.2/index.html)を開くと各種 API の挙動が確認できます。コードの内容はデモページをダウンロードしてソースコードを確認ください。また、デモページの URL を「[実機での確認方法](#アプリで開発中のキャンペーンの動作を確認する方法)」で使用する事によって実機でも確認いただけます。

## SATCH Campaign Javascript Library

### スクリプトの読み込み

以下の[リリースバージョン](#リリースバージョン)に書かれている JavaScript Library をダウンロードして HTML 内で読み込む。

| \<script type="application/javascript" src="./v0.0.1.js"\>\</script\> |
| :---- |

スクリプトを読み込むと window オブジェクトに以下のプロパティが生成される

| window.StylyMobilePartnerLibrary |
| :---- |

StylyMobilePartnerLibrary には各種 API が用意されているので 仕様書の HTML を参考にして利用すること。

例

| window.StylyMobilePartnerLibrary.scene.play("STYLYシーンのID") |
| :---- |

### リリースバージョン {#リリースバージョン}

* v0.0.6  
  * 2022/3/10  
  * ライブラリをまとめているのグローバル変数が SatchCampaignLibrary から StylyMobilePartnerLibrary に変更になりました  
  * unit.getNetworkInfo() でネットワークの種類が取得できるようになりました。  
  * [API 仕様書](https://drive.google.com/file/d/14u0rb5OmvsFNuQ7NmZks0KYzFUDJKha9/view?usp=sharing)  
  * [JavaScript ファイル](https://drive.google.com/file/d/14u0rb5OmvsFNuQ7NmZks0KYzFUDJKha9/view?usp=sharing)  
* v0.0.5  
  * 2021/03/09  
  * ファイル保存 API 追加 storage.saveFile  
  * [API 仕様書](https://drive.google.com/file/d/1pTQNMsyUQ9Hy7qkkPFPtFBecsWBZBBgo/view?usp=sharing)  
  * [JavaScript ファイル](https://drive.google.com/file/d/1KEY2a3X_KwElw1IDeAl-iqovwccQAIBM/view?usp=sharing)  
* v0.0.4  
  * 2021/02/18  
  * SATCH トップに遷移する機能追加  
  * STYLY に遷移する機能追加  
  * [API 仕様書](https://drive.google.com/file/d/1es43lpc5BPiNOTIZAxgtpQbSvi2eRZYT/view?usp=sharing)  
  * [JavaScript ファイル](https://drive.google.com/file/d/1Z2qfJbAvwpRYQKpjrXq5ZWRB7PIY5VEw/view?usp=sharing)  
* v0.0.3  
  * 2021/01/28  
  * [API 仕様書](https://drive.google.com/file/d/13906bKAKXEND5kApDppXg4ffo-NJsG_9/view?usp=sharing)  
  * [JavaScript ファイル](https://drive.google.com/file/d/1jhPpAsSbkfbraMttePwFnCLT6zUPQktq/view?usp=sharing)  
* v0.0.2  
  * 2021/01/14 リリース  
  * [API 仕様書](https://drive.google.com/file/d/16Zclgr7ZkHKqL9IXLIv2BnZSFBQAAEY-/view?usp=sharing)  
  * [JavaScript ファイル](https://drive.google.com/file/d/16xZuYjFHu79fd_-nqAMY4ZIbI2WkSrWQ/view?usp=sharing)

### Storage API について

storage API の space 引数はシーン内の値の保存と連携する場合、シーンを作った STYLY アカウントの User ID を文字列にしたものを指定するようにして下さい。また、シーンとの連携がない場合には key が campaign 同士でかぶらないように接頭辞をつけるようにしてください。

#### User ID の取得方法

[STYLY Gallery](https://gallery.styly.cc/) にアクセスして、ログインしてください。ブラウザの Developer ツールを開いてください（Chrome Developer Tool など)。「Network」のタブから以下の サーバー API の呼び出し履歴を探し、レスポンス内の id という項目を探してください。User ID はアカウントの一意な数字となっています。 

| https://api.styly.cc/api/v2/user/me |
| :---- |

### storage.saveFile についての注意事項 {#storage.savefile-についての注意事項}

この関数の実行時にファイルの保存ができても timeout エラーが発生することがありますが、技術的な問題により解決することが難しいです。無視しても問題にならないので無視してください。

### ローカル開発用 Chrome Extension

ローカルでの開発時、アプリとの連携前に使用する Chrome Extension を用意しています。Chrome ブラウザにインストールをして、有効にすると PC ブラウザ上でアプリの挙動を再現できます。以下のバージョンから最新版をダウンロードして、chrome://extensions のページからインストールしてください。詳しくは、[インストール参考ページ](https://naokixtechnology.net/javascript/2851) の「ローカル環境に保存した拡張機能をインストールする」の項目を御覧ください。有効にするためには extension のアイコンをクリックして、ポップアップ内の有効化のチェックボックスを ON にしてください。

#### リリースバージョン

* [v0.0.4](https://drive.google.com/file/d/1RWnRralWDUugOr5K9N3AyV2PhAc0LB6X/view?usp=sharing)  
  * 2021/02/18  
  * ロゴ追加  
  * util.goToStyly / util.goToSatch 追加  
* [v0.0.3](https://drive.google.com/file/d/1eEF-MELe_i-DWL6wxV9ozWIggcMdRKBe/view?usp=sharing)  
  * 2021/1/28  
  * GPS 関連追加  
* [v0.0.2](https://drive.google.com/file/d/1Tr1-ZBYa3jb0-hIANstFqoKYC_7VKPAi/view?usp=sharing)  
  * 2021/01/15 リリース  
  * 細かなエラーなどの修正

