---
title: インターネットに接続せずに Microsoft 365 Apps 更新プログラムを同期する
titleSuffix: Configuration Manager
description: インターネットから切断されている最上位のソフトウェアの更新ポイントで Microsoft 365 Apps 更新プログラムを同期します。
ms.date: 08/11/2020
ms.topic: conceptual
ms.prod: configuration-manager
ms.technology: configmgr-sum
ms.assetid: a8fa7e7a-bf55-42de-b0c2-c56777dc1508
manager: dougeby
author: mestew
ms.author: mstewart
ms.openlocfilehash: 49e0f5e1dff466e62cdba0def917dd34510e48ee
ms.sourcegitcommit: 99084d70c032c4db109328a4ca100cd3f5759433
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/20/2020
ms.locfileid: "88696771"
---
# <a name="synchronize-microsoft-365-apps-updates-from-a-disconnected-software-update-point"></a><a name="bkmk_O365"></a> 切断されているソフトウェアの更新ポイントからの Microsoft 365 Apps 更新プログラムの同期

*適用対象:Configuration Manager (Current Branch)*
<!--4065163-->
Configuration Manager バージョン 2002 以降、ツールを使用して、インターネットに接続されている WSUS サーバーから、切断された Configuration Manager 環境に Microsoft 365 Apps 更新プログラムをインポートすることができます。 以前は、更新されたソフトウェアのメタデータをエクスポートし、切断された環境にインポートした場合、Microsoft 365 Apps 更新プログラムを展開できませんでした。 Microsoft 365 Apps 更新プログラムには Office API と Office CDN からダウンロードした追加のメタデータが必要ですが、これは、切断された環境では不可能です。

> [!Note]
> 2020 年 4 月 21 日以降、Office 365 ProPlus は、**Microsoft 365 Apps for enterprise** に名前が変更されます。 詳細については、「[Office 365 ProPlus の名前の変更](/deployoffice/name-change)」を参照してください。 コンソールの更新中は、Configuration Manager コンソールやサポート ドキュメントに古い名前へのリファレンスが表示される場合があります。

## <a name="prerequisites"></a>[前提条件]

- Windows Server 2012 以上を稼働している、インターネットに接続された WSUS サーバー。
- WSUS サーバーは、次の 2 つのインターネット エンドポイントに接続できる必要があります。
   - `officecdn.microsoft.com`
   - `config.office.com`
- OfflineUpdateExporter ツールとその依存関係を、インターネットに接続されている WSUS サーバーにコピーします。
  - ツールとその依存関係は、 **&lt;ConfigMgr インストール ディレクトリ>/tools/OfflineUpdateExporter** ディレクトリにあります。
- このツールを実行するユーザーは、**WSUS 管理者**グループに属している必要があります。
- Microsoft 365 Apps 更新プログラムのメタデータとコンテンツを格納するために作成したディレクトリには、ファイルをセキュリティで保護するための適切なアクセス制御リスト (ACL) が必要です。
    - また、このディレクトリは空である必要もあります。
- オンラインの WSUS サーバーから切断された環境に移動するデータは、安全に移動する必要があります。

> [!IMPORTANT]
> すべての Microsoft 365 Apps 言語でコンテンツがダウンロードされます。 各更新プログラムには約 10 GB のコンテンツを含めることができます。

## <a name="synchronize-then-decline-unneeded-microsoft-365-apps-updates"></a>同期して、不要な Microsoft 365 Apps 更新プログラムを拒否する

1. インターネットに接続されている WSUS で、WSUS コンソールを開きます。
1. **[オプション]** 、 **[製品と分類]** の順に選択します。
1. **[製品]** タブで **[Office 365 クライアント]** をオンにし、 **[分類]** タブで **[更新プログラム]** をオンにします。[![WSUS での Microsoft 365 Apps 更新プログラムの製品と分類](./media/4065163-o365-updates-product-classification.png)](./media/4065163-o365-updates-product-classification.png#lightbox)
1. **[同期]** に移動し、 **[今すぐ同期]** を選択して、Microsoft 365 Apps 更新プログラムを WSUS に取り込みます。
1. 同期が完了したら、Configuration Manager で展開しない Microsoft 365 Apps 更新プログラムをすべて拒否します。 Microsoft 365 Apps 更新プログラムをダウンロードするために、それらを承認する必要はありません。  
   - WSUS で不要な Microsoft 365 Apps 更新プログラムを拒否すると、それらが WsusUtil.exe のエクスポート中にエクスポートされることはありませんが、OfflineUpdateExporter ツールによるそれらのコンテンツのダウンロードが停止されます。
   - OfflineUpdateExporter ツールによって、Microsoft 365 Apps 更新プログラムが自動的に実行されます。 他の製品の更新プログラムをエクスポートする場合、ダウンロードのためには引き続きそれらを承認する必要があります。
    - [WSUS の新しい更新プログラム ビュー](/windows-server/administration/windows-server-update-services/manage/viewing-and-managing-updates#to-create-a-new-update-view-on-wsus)を作成して、不要な Microsoft 365 Apps 更新プログラムを WSUS で簡単に表示および拒否できるようにします。
1. 他の製品の更新プログラムをダウンロードおよびエクスポートのために承認する場合は、コンテンツのダウンロードが完了するまで待機してから、WsusUtil.exe エクスポートを実行し、WSUSContent フォルダーの内容をコピーします。 詳細については、「[切断されているソフトウェアの更新ポイントからのソフトウェア更新プログラムの同期](synchronize-software-updates-disconnected.md)」を参照してください

## <a name="exporting-the-microsoft-365-apps-updates"></a>Microsoft 365 Apps 更新プログラムのエクスポート

1. OfflineUpdateExporter フォルダーを Configuration Manager からインターネットに接続されている WSUS サーバーにコピーします。
    - ツールとその依存関係は、 **&lt;ConfigMgr インストール ディレクトリ>/tools/OfflineUpdateExporter** ディレクトリにあります。
1. インターネットに接続されている WSUS サーバーのコマンド プロンプトから、次の方法でツールを実行します。**OfflineUpdateExporter.exe -O -D &lt;ターゲット フォルダーのパス>**

   |OfflineUpdateExporter のパラメーター|[説明]|
   |---|---|
   |**-O**|  **-Office**。 更新プログラムをエクスポートする製品が Office 365 または Microsoft 365 Apps であることを指定します|
   |**-D**|**-Destination**。 Destination は必須パラメーターであり、ターゲット フォルダーへの完全なパスが必要です。|

   - **OfflineUpdateExporter** ツールによって、次が行われます。
      - WSUS に接続する
      - WSUS で Microsoft 365 Apps 更新プログラム メタデータを読み取る
      - Microsoft 365 Apps 更新プログラムに必要なコンテンツや追加のメタデータを、ターゲット フォルダーにダウンロードする

1. インターネットに接続されている WSUS サーバーのコマンド プロンプトで、WsusUtil.exe を含むフォルダーに移動します。 既定では、このツールは %*ProgramFiles*%\Update Services\Tools にあります。 たとえば、ツールが既定の場所にある場合は、 **cd %ProgramFiles%\Update Services\Tools**のように入力します。
   - Windows Server 2012 を使用している場合は、WSUS サーバーに [KB2819484](https://support.microsoft.com/help/2819484/cab-file-that-is-exported-by-using-the-wsusutil-exe-command-is-display) がインストールされていることを確認してください。
   - WsusUtil ツールを実行するユーザーは、サーバーのローカルの Administrators グループのメンバーである必要があります。

1. 次のように指定して、ソフトウェア更新プログラムのメタデータを GZIP ファイルにエクスポートします。  

    **WsusUtil.exe export**  <*パッケージ名*>  <*ログファイル*>  

    次に例を示します。  

    **WsusUtil.exe export export.xml.gz export.log**

1. **export.xml.gz** ファイルを、切断されたネットワーク上の最上位の WSUS サーバーにコピーします。
1. 他の製品の更新プログラムを承認した場合は、WSUSContent フォルダーの内容を、最上位レベルの切断された WSUS サーバーの WSUSContent フォルダーにコピーします。
1. **OfflineUpdateExporter** に使用されているターゲット フォルダーを、切断されたネットワーク上の最上位レベルの Configuration Manager サイト サーバーにコピーします。

## <a name="import-the-microsoft-365-apps-updates"></a>Microsoft 365 Apps 更新プログラムをインポートする

1. 切断された最上位レベルの WSUS サーバーで、インターネットに接続されている WSUS サーバー上で生成した **export.xml.gz** から、更新プログラムのメタデータをインポートします。
   
    次に例を示します。  

    **WsusUtil.exe import export.xml.gz import.log**
    
    既定では、WsusUtil.exe ツールは %*ProgramFiles*%\Update Services\Tools にあります。

1. インポートが完了したら、切断された最上位レベルの Configuration Manager サイト サーバーで、サイト コントロール プロパティを構成する必要があります。 この構成変更により、Configuration Manager に Microsoft 365 Apps のコンテンツが指定されます。 プロパティの構成を変更するには:
   1. [O365OflBaseUrlConfigured PowerShell スクリプト](#bkmk_o365_script)を、最上位レベルの切断された Configuration Manager サイト サーバーにコピーします。
   1. `"D:\Office365updates\content"` を、OfflineUpdateExporter によって生成された、Microsoft 365 Apps コンテンツとメタデータを含むコピーしたディレクトリの完全なパスに変更します。
      > [!IMPORTANT]
      > O365OflBaseUrlConfigured プロパティに対して機能するのは、ローカル パスのみです。
   1. スクリプトを `O365OflBaseUrlConfigured.ps1` として保存します
   1. 切断された最上位レベルの Configuration Manager サイト サーバー上で、管理者特権の PowerShell ウィンドウから、`.\O365OflBaseUrlConfigured.ps1` を実行します。
   1. サイト サーバーで、**SMS_Executive** サービスを再起動します。
1. **Configuration Manager** コンソールで、 **[管理]** 、 >  **[サイトの構成]** 、 >  **[サイト]** に移動します。
1. 最上位レベルのサイトを右クリックし、 **[サイト コンポーネントの構成]**  >  **[ソフトウェアの更新ポイント]** の順に選択します。
1. **[分類]** タブで、 *[更新プログラム]* をオンにします。 **[製品]** タブで、 *[Office 365 クライアント]* をオンにします。
1. Configuration Manager の[ソフトウェア更新プログラムを同期します](synchronize-software-updates.md#manually-start-software-updates-synchronization)
1. 同期が完了したら、通常のプロセスを使用して Microsoft 365 Apps 更新プログラムを展開します。

## <a name="proxy-configuration"></a><a name="bkmk_O365_ki"></a>プロキシの構成

- プロキシ構成が、このツールにネイティブで組み込まれていません。 ツールを実行しているサーバー上の [インターネット オプション] でプロキシが設定されている場合、理論的には、これが使用され適切に機能するはずです。
   - コマンド プロンプトで `netsh winhttp show proxy` を実行して、構成されているプロキシを表示します。



## <a name="modify-o365oflbaseurlconfigured-property"></a><a name="bkmk_o365_script"></a> O365OflBaseUrlConfigured プロパティを変更する

```powershell
# Name: O365OflBaseUrlConfigured.ps1
#
# Description: This sample sets the O365OflBaseUrlConfigured property for the SMS_WSUS_CONFIGURATION_MANAGER component on the top-level site.
# This script must be run on the disconnected top-level Configuration Manager site server
#
# Replace "D:\Office365updates\content" with the full path to the copied directory containing all the Office metadata and content generated by the OfflineUpdateExporter tool.
# Only local paths work for the O365OflBaseUrlConfigured property.

$PropertyValue = "D:\Office365updates\content"

# Don't change any of the lines below
$PropertyName = "O365OflBaseUrlConfigured"

# Get provider instance
$providerMachine = Get-WmiObject -namespace "root\sms" -class "SMS_ProviderLocation"

if($providerMachine -is [system.array])
{
    $providerMachine=$providerMachine[0]
}

$SiteCode = $providerMachine.SiteCode

$component = gwmi -ComputerName $providerMachine.Machine -namespace root\sms\site_$SiteCode -query 'select comp.* from sms_sci_component comp join SMS_SCI_SiteDefinition sdef on sdef.SiteCode=comp.SiteCode where sdef.ParentSiteCode="" and comp.componentname="SMS_WSUS_CONFIGURATION_MANAGER"'
$properties = $component.props

Write-host "Updating $PropertyName property for site " $SiteCode

foreach ($property in $properties)
{
  if ($property.propertyname -eq $PropertyName) 
  {
    Write-host "Current value for $PropertyName is $($property.value2)"
    $property.value2 = $PropertyValue
    Write-host "Updating value for $PropertyName to $($property.value2)"
    break
  }
}

$component.props = $properties
$component.put()
```

## <a name="next-steps"></a>次のステップ

[更新グループへのソフトウェア更新プログラムの追加](../deploy-use/add-software-updates-to-an-update-group.md)