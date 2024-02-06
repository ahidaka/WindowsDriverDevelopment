# WindowsDriverDevelopment

Windows ドライバー開発環境のインストール

Guide to getting started with Windows driver development

[https://ahidaka.github.io/WindowsDriverDevelopment/](https://ahidaka.github.io/WindowsDriverDevelopment/)

## はじめに

CQ出版社、[CQエレクトロニクスセミナー「Windows11時代のデバイス・ドライバ開発」](https://seminar.cqpub.co.jp/ccm/ES23-0142) 実習用環境のインストールと動作確認手順です。

https://seminar.cqpub.co.jp/ccm/ES23-0142(https://seminar.cqpub.co.jp/ccm/ES23-0142)

Windows 11とWindows 10用のドライバー開発にそのまま利用できます。
従ってここで紹介する環境を構築すれば、現在サポートされている全 Windows 用のドライバーを開発することが可能です。

### 参考資料

基本的に次の[「Windows Driver Kit (SDK) のダウンロード」ページ](https://learn.microsoft.com/ja-jp/windows-hardware/drivers/download-the-wdk)が約に立ちますが、一部不正確な部分があるため、本文中で補足説明します。

[https://learn.microsoft.com/ja-jp/windows-hardware/drivers/download-the-wdk](https://learn.microsoft.com/ja-jp/windows-hardware/drivers/download-the-wdk)

[![Windows Driver Kit (SDK) のダウンロード](wdk-1.png)](https://learn.microsoft.com/ja-jp/windows-hardware/drivers/download-the-wdk)
<br/>**「Windows Driver Kit (SDK) のダウンロード」ページ**<br/>

<br/>
この資料ページはブラウザの検索窓に WDK と入力すると表示されます。

[![WDK](wdk-2.png)](https://learn.microsoft.com/ja-jp/windows-hardware/drivers/download-the-wdk)

<br/>

### 必要なもの

- Windows PC (Windows 11 または Windows 10 x64版、空きDISKスペース50GB以上を推奨)
- Visual Studio 2022（最新版にアップデート済、Preview版は不可、全Edition）
- ブラウザでインターネットにアクセスできる環境

続いて開発環境のインストール手順を紹介します。

### 対象の検証済環境

- Visual Studio Version: 17.8.1 以降
- WDK Version: 10.1.22621.2428（Windows 11 WDK）

それでは Windows ドライバー開発環境を構築する方法を紹介します。インストールは次の順で行います。

- Visual Studio 2022 のダウンロード、設定とインストール
- WDKのダウンロードとインストール

Visual Studio 2022 をすでにインストール済の方は、再インストールの必要はありません。
**Visual Studio Installer** を起動して、インストール設定を確認、必要に応じて個別のコンポーネントを追加をインストールしてください。

<br/>

### Visual Studio の設定

ドライバー開発用 Visual Studio 2019 のインストール時の設定は、次の3点に気をつけてください。**現時点では Visual Studio 2022 を使用してドライバー開発はできません。**

- .NET デスクトップ開発、C++によるデスクトップ開発とユニバーサルプラットフォーム開発の **ワークロード** インストール
- Windows 11 SDK (10.0.22001.1) のインストール
- MSVC v142 - VS2019 C++ x64/x86 Spectre 軽減ライブラリ（最新）の **個別コンポーネント** のインストール

#### インストール ワークロード の選択

Visual Studio のインストール時に次のワークロードを選択します。
追加で他のワークロードを追加で選択してインストールしても問題ありません。
すでにインストール済の方は、Visual Studio Installer の変更機能を使用して、確認・変更して下さい。

![ワークロード選択](VS-Insta111.png)
<br/>**ワークロード選択 画面**<br/>

#### Windows SDK 最新版 (10.0.19041.1) インストール

Windows SDK 最新版 (10.0.22000.1) のインストールを確認して下さい。
インストール済の方は、これも Visual Studio Installer で個別のコンポーネント機能で確認・変更して下さい。

![Windows SDK 最新版](VS10pxq.png)
<br/>**Windows SDK 10.0.22000.1 選択画面**<br/>

#### Spectre 軽減ライブラリ（最新）インストール

![Spectre 軽減ライブラリ（最新）](VS11pxq.png)
<br/>**Spectre 軽減ライブラリ（最新）選択画面**<br/>

個別のコンポーネントのインストールタブを選択して、
**MSVC v142 - VS2019 C++ x64/x86 Spectre 軽減ライブラリ（最新）**
のインストールを確認して下さい。

このライブラリをインストールしてない場合、Visual Studio のビルドで次のエラーが発生します。

![エラー：Spectre 軽減ライブラリ が必要です](VS-Insta211.png)
<br/>**Spectre 軽減ライブラリが無いエラー事例 画面**<br/>
<br/>

Windows Driver Kit (WDK) の **今すぐダウンロード** をクリックしてダウンロード用ページを表示します。このページの内容と URL は時々更新されます。

![今すぐダウンロード](WDK11.png)
<br/>**今すぐダウンロード 表示画面**<br/>

表示されたページの中程にある、

	手順 3:Windows 11 WDK をインストールする

	WDK for Windows 11 のダウンロード

のリンクをクリックして **wdksetup.exe** を入手して実行、インストールを開始します。<br/>
手順2 のWindows 11 SDKは前項の通り、Visual Studio 2019 に含まれるため、飛ばします。<br/>

![WDK のダウンロード](WDK12p.png)
<br/>**WDK のダウンロード 選択画面**<br/>

すぐ下にある **EWDK with Visual Studio Build Tools** は、Visual Studio 無しで Windows ドライバーをビルドするツールです。
使用しないためダウンロード不要です。
<br/>

### WDK のインストール

wdksetup.exe を実行してインストールする手順を示します。
起動画面では **Install the Windows Driver Kit - Windows 10.0.22001.1 to this computer**
を選択して **Next** で進みます。

下の選択肢の **Download the Windows Driver Kit - Windows 10.0.22001.1 for installation on a separate computer**
は、オフライン インストール用の全 WDK バイナリーのダウンロードを行います。
