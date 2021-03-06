---
title: Web サーバー側でコンテンツを更新しても IE 上に反映されない (キャッシュのお話)
date: 2020-04-20
tags: 
  - Internet Explorer
  - キャッシュ
---

※ これまで Japan IE Support Team Blog にて公開していた記事を移しました。

「IE8 では Web サーバー側でファイルを更新したらすぐクライアントに反映されていたのに、IE9 以降にしたらすぐに反映されなくなった。。」というお問い合わせをいただくことがあります。
この現象は、IE9 以降の IE で [インターネット オプション] の [全般タブ] [インターネット一時ファイルと履歴の設定] で [自動的に確認する] (既定値) が設定されている場合に 発生することがあり、以下の技術文書でもご案内しています。

Internet Explorer 9 において Webサーバーとの通信なしでキャッシュからコンテンツを表示する場合がある
https://support.microsoft.com/ja-jp/help/2530998

今回は、この IE の "キャッシュ" の動作について詳しくご紹介いたします。

---

## まず最初に、キャッシュとは


Internet Explorer における "キャッシュ" とは、**閲覧しているページを次回もっと早く表示できるように、PC に保存しておく Web ページ (html、aspx など)、画像 (jpg、gif など)、その他リソースファイル (スタイルシートファイル (.css)、JavaScript ファイル (.js)、動画や音声ファイル などなど) 等などのコピー** のことです。
次回同じページにアクセスした時、Web サーバー側で特に更新されていなければキャッシュからファイルを取得することで、通信量を抑えたりページ表示の速度を速めたりするための仕組みです。

保存されているキャッシュは、[インターネット オプション] – [全般] タブ - 閲覧の履歴 – [設定] ボタンをクリックすると表示される [Web サイト データの設定] ダイアログ上で、[ファイルの表示] ボタンを押すと確認できます。

![TemporaryInternetFiles](/articles/internet-explorer-microsoft-edge/IE-Cache/TemporaryInternetFiles.jpg)

エクスプローラが起動して、以下のようにキャッシュが表示されます。

![Explorer](/articles/internet-explorer-microsoft-edge/IE-Cache/Explorer.jpg)

いくつかの項目が表示されていますが、このうち **"有効期限日時"** は Web サーバーが指定してくる (具体的には、HTTP レスポンス ヘッダーの Set-Cookie ヘッダーで指定してくる) ものになります。指定がなければ、"なし" と表示されます。

一度閲覧した Web ページを次回表示した時、IE は基本的にファイル取得を以下の流れで行います。

ユーザーが Web ページのアドレスを IE のアドレスバーに入力して Enter キーを押す
IE は、対象のコンテンツのキャッシュがキャッシュフォルダに存在するかを確認する
→　キャッシュが存在していて、
→→　 有効期限内であればキャッシュから取得。
→ →　有効期限が切れていれば、Web サーバーへ更新確認 (If-Modified-Since ヘッダーを含む Get リクエスト) を行う。
→→→　更新確認の結果
→ →→→　Web サーバー側のファイルが更新されていたら、Web サーバーから取得。（ファイルをダウンロード。HTTP ステータスコード 200 OK が返る）
→ →→→　更新されていなければ (HTTP ステータスコード 304 Not Modified が返れば) 引き続きキャッシュから取得。

 

上記の流れを考えると、Web サーバー上のファイルが更新されれば Web サーバーから取得するのだから、「Web サーバー側でファイルを更新したのに、クライアントの IE で Web サイトにアクセスしても更新が反映されない。。」ということはなさそうに思われます。

が、

IE9 以降の IE では、以下両方に該当する場合、更新確認も行わずにコンテンツをキャッシュから取得するようになっています。

クライアント側の IE で [インターネット オプション] [インターネット一時ファイルと履歴の設定] で [自動的に確認する] (既定値) が設定されている
Web サーバー側で "有効期限日時" を指定していないコンテンツである
 

この動作変更は、IE9 以降 RFC2616 に準拠して、"表示の高速化、効率化のため、より積極的にキャッシュを利用する" ために行われました。

具体的には、**[インターネット一時ファイルと履歴の設定] で [自動的に確認する] が設定されている場合、Web サーバーから "有効期限の指定のない" コンテンツを取得すると、IE は独自に有効期限を計算して設定し、キャッシュを作成する** ようになりました。

この機能は "ヒューリスティックキャッシュ" と呼ばれます。

---

(※) ヒューリスティック キャッシュについて

有効期限の指定のないコンテンツについて、従来の IE では限定されたリソース (画像ファイル) にのみ独自に期限を設定していましたが、IE9 以降ではキャッシュ可能な全てのリソースについて独自に期限を設定するようにしました。
ヒューリスティック キャッシュの詳細については、以下のブログ記事で紹介されています。

Caching Improvements in Internet Explorer 9
https://docs.microsoft.com/en-us/archive/blogs/ie/caching-improvements-in-internet-explorer-9

ヒューリスティック キャッシュでは、以下の計算式で有効期限を算出します。最初にコンテンツを取得してキャッシュを作成するとき、算出した有効期限を設定します。

有効期間 = (最終チェック日時 - 最終更新日時) * 0.1

つまり最終チェック日時と最終更新時間の差が大きい、つまり更新されていない期間が長いコンテンツは有効期間が長く設定され、クライアントからの更新確認の頻度が下がる仕組みです。

なおエクスプローラー上では、"有効期限日時" は "なし" と表示され、IE が設定した有効期限は見ることができません。

---

つまり、
例えば先ほどのエクスプローラー画面の iis-85.png の場合

![Cache-iis85](/articles/internet-explorer-microsoft-edge/IE-Cache/Cache-iis85.jpg)

- 最終チェック日時 :  2018/03/07 10:52
- 最終変更日時 : 2016/04/22 9:49

であるため、キャッシュ作成時、有効期間は約 68 日間と設定されることとなります。
言い換えますと、IE が最初に iis-85.png を Web サーバーから取得してキャッシュを作成してから約 68 日間はキャッシュが有効期限内になり、この間に  Web サーバー側で更新しても、IE は Web サーバーへの更新確認は行わずキャッシュから取得します。

以上から、頻繁に更新したり、更新後すぐに確実にクライアントの IE に反映させたい！といったコンテンツについては、Web サーバー側で明示的に有効期限を指定されることをお勧めしています。

---

## 有効期限を設定していなかったコンテンツを Web サーバー上で更新した時、クライアントの IE に直ぐ反映させるにはどうすればいい？

まず、クライアントには「IE が有効期限を独自に設定したキャッシュ」が既に存在している状態です。

[インターネット一時ファイルと履歴の設定] で [自動的に確認する] が選択されている場合、これが有効期限内であるうちは Web サーバーに更新確認を行わないので更新は反映されません。
Web サーバー側からは、クライアントのキャッシュを削除する方法がありません。
そのため、どうにかしてクライアントのキャッシュを削除する必要があります。

その他の方法として、[インターネット一時ファイルと履歴の設定] で [Web サイトを表示する度に確認する] または [Internet Explorer を表示する度に確認する] を指定すると、IE は有効期限内のコンテンツであっても Web サーバーに更新確認を行います。

 

キャッシュ削除の最もシンプルな方法は、ユーザー自身が [インターネット オプション] – [全般] タブから、閲覧の履歴を削除することです。

クライアント側でキャッシュを削除するには、インターネット オプション – 全般 タブ - 閲覧の履歴 の [削除] ボタンをクリックして [閲覧の履歴の削除] ダイアログから行います。

![DeleteBrowsingHistory](/articles/internet-explorer-microsoft-edge/IE-Cache/DeleteBrowsingHistory.jpg)

"キャッシュ" を削除するには、"インターネット一時ファイルおよび Web サイトのファイル" にチェックを入れて [削除] ボタンをクリックします。
Cookie や、フォームへの入力内容などを削除したくない場合には該当のチェックは外しても大丈夫です。

他の項目についての詳細は、以下の技術文書にてご案内しています。

Internet Explorer の閲覧履歴の表示および削除
https://support.microsoft.com/ja-jp/help/17438/windows-internet-explorer-view-delete-browsing-history

 

社内の Active Directory 環境のクライアント端末を管理されているような方で、 ドメインに参加しているクライアント全部、キャッシュを削除したい、、ということであれば、 一つの手として「グループ ポリシーで [終了時に閲覧の履歴を削除する] を有効とするポリシーをクライアントに配布する」という方法もあります。
これが適用されたクライアントでは、IE を終了するときにキャッシュが削除されます。

---

## 今後、Web サーバー側で更新したコンテンツを、すぐクライアントに反映させるためにはどうすればいい？

頻繁に更新されるようなコンテンツでは、"Web サーバー側でコンテンツの有効期限を指定する" 方法をお勧めしています。
例えば以下のような HTTP 応答ヘッダー指定を行い、コンテンツのキャッシュの有効期限を制御します。
```
Cache-Control : no-cache
```

IE11 では、上記の HTTP 応答ヘッダーを含むファイルを受信すると有効期限切れのキャッシュを作成します。そのため次回アクセス時には Web サーバーへ更新確認を行い、更新されていれば Web サーバーから取得、更新がなければキャッシュから取得する、という動作になります。

なお HTTP 応答ヘッダーによる指定をクライアントのキャッシュに反映させるためには、Web サーバー側で指定を行った後、やっぱりクライアントのキャッシュを削除する必要がありますのでご注意ください。

キャッシュの有効期限を指定する HTTP 応答ヘッダーには以下があります。

Expires
https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Expires
キャッシュを有効期限切れとする日時を指定します。

Cache-Control
https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control
キャッシュを有効とする期間 (秒) を指定します。キャッシュの作成を抑止する指定も可能です。

---

### ご参考 : META タグによるキャッシュ制御の指定について

Web ページ (HTML、ASPX 等) 内では以下のような META 要素でキャッシュを制御できます。
```
<META http-equiv=cache-control content=no-cache>
<META http-equiv=expires content=0>
```

ただしこの指定は、"記載されている Web ページ" のみが対象であり、ページに読み込まれる .css や .js 等の Web リソースファイルに対しては有効ではありません。Web リソースファイルのキャッシュを制御するには、後述の通り Web サーバーにて設定を行う必要があります。

また Web ページにおいても、キャッシュを確実に制御されたい場合、以下の理由から META タグではなく HTTP 応答ヘッダーによる指定を強くお勧めしております。

META タグでのキャッシュ制御は、あくまでも HTML ファイル内の "タグ" で行なうものであり、" HTML ファイルを読み込んでから解釈しキャッシュの処理をする" ものです。
キャッシュの制御を HTML パーサーで行なうこととなるためキャッシュは一旦作成されます。SSL の場合は直接キャッシュ自体を削除し、非 SSL の場合は、期限切れのキャッシュと同等に扱われます。

---

### ご参考 :キャッシュの削除について

IE のキャッシュ削除の方法として、 正式にサポートされているのは以下となります。

- [インターネット オプション] – [閲覧の履歴の削除] ダイアログから削除
- エクスプローラでキャッシュの場所を開いて削除
- キャッシュ削除のためのアプリケーションを開発、実行して削除 (※)

(※) キャッシュ削除のために用意された "DeleteURLCacheEntry" API を利用したアプリケーションを作成し、実行することによって削除できます。
"インターネット一時ファイルおよびWebサイトのファイル" のみを削除するような処理も実装可能です。

DeleteUrlCacheEntry function
https://docs.microsoft.com/ja-jp/windows/win32/api/wininet/nf-wininet-deleteurlcacheentry

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。