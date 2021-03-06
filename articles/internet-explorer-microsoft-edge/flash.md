---
title: Internet Explorer および Microsoft Edge での Flash の今後の対応について
date: 2020-02-03
tags: 
  - Internet Explorer
  - Flash
---

<font color="red">**2020/9/9 追記: 今後に関する最新情報を追記しました。**</font>

> 本記事は Technet Blog の更新停止に伴い、もとの [記事](https://docs.microsoft.com/ja-jp/archive/blogs/jpieblog/flash-roadmap) に加筆／修正を加えた内容です。
> 元の記事の最新の更新情報については、本内容をご参照ください。

こんにちは。
たまにお問い合わせとしていただく Internet Explorer および Microsoft Edge での Flash の今後の対応について改めてご紹介します。

## 今後のロードマップ
Adobe Flash のサポート終了のロードマップは、以前下記の資料にて今後のロードマップについてお知らせしていました。

> The End of an Era – Next Steps for Adobe Flash
> https://blogs.windows.com/msedgedev/2017/07/25/flash-on-windows-timeline/

その後、2019 年 8 月 30 日に下記の情報をお知らせしています。

> Update on removing Flash from Microsoft Edge and Internet Explorer
> https://blogs.windows.com/msedgedev/2019/08/30/update-removing-flash-microsoft-edge-internet-explorer/

<span style="color: #ff0000;">(2020/9/9 追記)</span>

> Update on Adobe Flash Player End of Support
> https://blogs.windows.com/msedgedev/2020/09/04/update-adobe-flash-end-support/

当初の予定から大きくは変わらず、今年 2020 年末には完全に廃止される予定です。
この方針は、マイクロソフトが独自に進めているのではなく、開発元の Adobe 社が中心となりブラウザー各社連携をとり進められています。
来年 2021 年には、どのブラウザーをご利用の場合でも Flash が実行できなくなることが予想されますため、html5 などの代替テクノロジーへの移行のご検討を何卒よろしくお願いします。

なお、新しい Chromium 版の Microsoft Edge は、Chromium をベースとするほかのブラウザーと同様のタイムラインで、既定での無効化および廃止の対応が行われる見込みです。このため、EdgeHTML 版の Microsoft Edge や Internet Explorer での対応予定とは異なる可能性が十分に考えられます。
代替テクノロジーへの移行を予定されている場合は、[Google 社の情報](https://www.blog.google/products/chrome/) も併せてご確認ください。

---
以降は、Internet Explorer および EdgeHTML 版の Microsoft Edge での Flash 無効化に向けたロードマップです。

### <span style="color: #cccccc;">■ 2017 年末から 2018 年にかけて</span>
<span style="color: #cccccc;">
Windows 10 Creators Update (v1703) 以降の Microsoft Edge では、初めて訪れる Web サイトでの Flash コンテンツの実行の許可を求め、許可した Web サイトの再訪問時には求められません。
また、Internet Explorer では Flash の実行に関しては特別な制御は行われておりません。
</span>

### <span style="color: #cccccc;">■ 2018 年後半にかけて</span>
<span style="color: #cccccc;">
Microsoft Edge 上で Flash が含まれる Web サイトを閲覧するたびに実行の許可を求める動作となります。
Internet Explorer においては引き続き Flash の実行は許可され、特別な制御は行われません。
</span>

### <span style="color: #cccccc;">■ 2019 年後半にかけて
<span style="color: #cccccc;">
Microsoft Edge および Internet Explorer 上での Flash が既定で無効となります。
ただし、Flash を実行できるよう構成を変更することも可能です。
Flash を実行できるよう構成したい場合、Microsoft Edge については、[2018 年後半にかけて] と同様に、Flash が含まれる Web サイトを閲覧するたびに実行の許可が求められる動作となります。
</span>

#### 補足
上記の資料のとおり、2019 年中については 2018 年までの動作から変更はおこなれませんでした。
つまり、Internet Explorer では Flash の既定での無効化の対応は実施されていません。

### ■ 2020 年末
サポートされるすべての Windows 上の Microsoft Edge (EdgeHTML 版、Chromium 版ともに) および Internet Explorer で Flash を実行することができなくなります。
Flash を再び実行できるように構成することもできなくなる予定です。

なお、どのようにこの対応が展開されるのか？いつそれが行われるのか？ 2021 年以降も Flash を使い続ける方法があるのか？というお問い合わせをいただくことがありますが、恐れ入りますが、現段階でご紹介できる情報は上記の内容のみです。
今後の予定についてお伝えできる情報が明確になった時点で改めて本情報を更新いたします。

<span style="color: #ff0000;">(2020/9/9 追記)</span>
今後の最新情報が公開されました。以下に概要をまとめます。

> Update on Adobe Flash Player End of Support
> https://blogs.windows.com/msedgedev/2020/09/04/update-adobe-flash-end-support/

- これまでの予定と変わりなく、<span style="color: #ff0000;font-weight:bold;">Adobe Flash Player のサポート終了は 2020 年 12 月 31 日です。</span>
- それ以降の法人向け対応サポートについては、[Adobe 社の情報](https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html)をご覧ください。
- [Adobe 社の情報](https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html)で案内される法人向け対応サポート契約を結び、今まで弊社から提供をしてきた Adobe Flash Player に代わるプラグインの提供を受けた場合、それを『Internet Explorer 11』 および 『Microsoft Edge (Chromium 版) の IE モード』で動作させることができますが、『サードパーティー製のプラグイン』の扱いとなり弊社でのサポートはありません。
- [2021 年 1 月リリース予定](https://docs.microsoft.com/en-us/DeployEdge/microsoft-edge-release-schedule) の Microsoft Edge (Chromium 版) の [バージョン 88 にて Flash が削除される予定](https://docs.microsoft.com/en-us/microsoft-edge/web-platform/site-impacting-changes) です。
- 2020 年末以降は、 Microsoft Edge (EdgeHTML 版) と Internet Explorer 向けの『Adobe Flash Player のセキュリティ更新プログラム』の提供はありません。
- 2021 年 1 月初旬に、Microsoft Edge (EdgeHTML 版) と Internet Explorer で Flash は既定で無効化され、また、2020 年 6 月リリースの [KB4561600](https://support.microsoft.com/ja-jp/help/4561600/security-update-for-adobe-flash-player) より古いバージョンの実行はブロックされます。
- Windows OS のコンポーネントとしての Flash を永久的に削除するための『Update for Removal of Adobe Flash Player』というタイトルの更新プログラムがリリース予定です。<span style="color: #ff0000;font-weight:bold;">この更新を適用したあとは元に戻すことはできません。</span>
- 2020 年秋に『Update for Removal of Adobe Flash Player』を Microsoft Update カタログから入手できるようになる予定です。2020 年 12 月 31 日のサポート終了より前に Flash を永久削除したい場合は、カタログから更新プログラムを入手して実行できます。
- 『Update for Removal of Adobe Flash Player』は、2021 年の初めに Windows Update と WSUS で『オプション』として配信予定で、その 2 ～ 3 か月後には『推奨』として配信予定です。
- 2021 年夏には、Windows 10 での OS のロールアップ更新プログラム、および Windows 8.1, Windows Server 2012 and Windows Embedded 8 Standard での Internet Explorer 用の累積的なセキュリティ更新プログラムやマンスリー ロールアップの一部として『Update for Removal of Adobe Flash Player』が組み込まれ、<span style="color: #ff0000;font-weight:bold;">更新プログラムの適用により Flash が永久的に削除されます。</span>

#### Adobe 社公開情報

Adobe Flash Player法人向けサポート終了情報ページ
https://www.adobe.com/jp/products/flashplayer/enterprise-end-of-life.html

Update for Enterprise Customers Using Adobe Flash Player
https://blog.adobe.com/en/fpost/2020/update-for-enterprise-adobe-flash-player.html

Adobe Flash Playerサポート終了情報ページ
https://www.adobe.com/jp/products/flashplayer/end-of-life.html


本日の記事は以上となります。
本情報はあくまでも現時点での予定となりますため、今後何らかの影響により本予定も変更となる可能性が十分にありえます。
そのため、Web サイト側での対応を計画されている場合には、十分に余裕をもった計画とされることをお勧めいたします。

---
なお、本ブログは弊社の公式見解ではなく、予告なく変更される場合があります。
もし公式な見解が必要な場合は、弊社ドキュメント (https://docs.microsoft.com/ や https://support.microsoft.com) をご参照いただく、もしくは私共サポートまでお問い合わせください。
