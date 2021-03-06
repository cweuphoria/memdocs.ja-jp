---
title: BitLocker レポートの表示
titleSuffix: Configuration Manager
description: Configuration Manager の BitLocker 管理レポートの詳細
ms.date: 08/11/2020
ms.prod: configuration-manager
ms.technology: configmgr-protect
ms.topic: reference
ms.assetid: 0bae9477-0500-41cf-8aa3-5e6efadd0554
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 7c44ec9a9ed91d8543fedbdd5fba191b3989da19
ms.sourcegitcommit: d225ccaa67ebee444002571dc8f289624db80d10
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/12/2020
ms.locfileid: "88129216"
---
# <a name="view-bitlocker-reports"></a>BitLocker レポートの表示

*適用対象:Configuration Manager (Current Branch)*

<!--3601034-->

レポート サービス ポイントに[レポートをインストール](setup-websites.md)すると、レポートを表示できます。 これらのレポートには、会社と個々のデバイスに関する BitLocker の準拠状態が表示されます。 表形式の情報とグラフが掲載されており、さまざまな観点からデータを表示できるフィルターがあります。

Configuration Manager コンソールで、 **[監視]** ワークスペースに移動して、 **[レポート]** を展開し、 **[レポート]** ノードを選択します。 次のレポートは **[BitLocker 管理]** カテゴリに含まれます。

- [BitLocker コンピューターの準拠](#bkmk-compliancereport)

- [BitLocker エンタープライズ準拠ダッシュボード](#bkmk-dashboard)

- [BitLocker エンタープライズ準拠の詳細](#bkmk-compliancedetails)

- [BitLocker エンタープライズ準拠の概要](#bkmk-compliancesummary)

- [回復の監査レポート](#bkmk-audit)

これらのレポートはすべて、レポート サービス ポイントの Web サイトから直接アクセスできます。

> [!NOTE]
> これらのレポートでデータ全体を表示するには:
>
> - BitLocker 管理ポリシーを作成して、デバイス コレクションに展開します。
> - ターゲット コレクション内のクライアントがハードウェア インベントリを送信する必要があります。

## <a name="bitlocker-computer-compliance"></a><a name="bkmk-compliancereport"></a> BitLocker コンピューターの準拠

このレポートを使用し、コンピューターに固有の情報を収集します。 OS ドライブと任意の固定データ ドライブに関する詳細な暗号化情報が提供されます。 各ドライブの詳細情報を表示するには、コンピューター名エントリを展開します。 また、コンピューターの各ドライブの種類に適用されるポリシーも表示されます。

:::image type="content" source="media/bitlocker-computer-compliance.png" alt-text="BitLocker コンピューターのコンプライアンス レポートのスクリーンショット例" lightbox="media/bitlocker-computer-compliance.png":::

このレポートを使用して、紛失したり盗難されたりしたコンピューターの最後に認識された BitLocker 暗号化状態を確認することもできます。 Configuration Manager は、展開する BitLocker ポリシーに基づいてデバイスの準拠状態を判断します。 デバイスの BitLocker 暗号化状態を調べる前に、そのデバイスに対して展開したポリシーを確認します。

> [!NOTE]
> このレポートには、リムーバブル データ ボリュームの暗号化状態は表示されません。

### <a name="computer-details"></a>コンピューターの詳細

|Column&nbsp;name|[説明]|
|----------------|----|
|コンピューター名|ユーザー指定の DNS コンピューター名。|
|ドメイン名|コンピューターの完全修飾ドメイン名。|
|コンピューターの種類|コンピューターの種類。有効な種類は、 **[ポータブル以外]** と **[ポータブル]** です。|
|オペレーティング システム|コンピューターの OS の種類|
|総合的な準拠|コンピューターの全体的な BitLocker 準拠状態。 有効な状態は **[準拠]** と **[非準拠]** です。 [ドライブごとの準拠状態](#bkmk_volume)は、異なる準拠状態を示す場合があります。 ただし、このフィールドには、指定したポリシーからの準拠状態が表示されます。|
|オペレーティング システムの準拠|コンピューター上の OS の準拠状態。 有効な状態は **[準拠]** と **[非準拠]** です。|
|固定データ ドライブの準拠|コンピューター上の固定データ ドライブの準拠状態。 有効な状態は **[準拠]** と **[非準拠]** です。|
|最終更新日|コンピューターが準拠状態をレポートするためにサーバーに接続した最終日時。|
|除外|ユーザーが BitLocker ポリシーから除外されているかどうかを示します。|
|除外ユーザー|BitLocker ポリシーから除外されるユーザー。|
|除外日|除外が認められた日付。|
|対応状態の詳細|指定したポリシーに基づくコンピューターの準拠状態に関するエラー メッセージと状態メッセージ。|
|ポリシーの暗号強度|BitLocker 管理ポリシーで選択した暗号強度。|
|ポリシー: オペレーティング システム ドライブ|OS ドライブに暗号化が必要かどうかを示し、適切な保護の種類を表示します。|
|ポリシー: 固定データ ドライブ|固定データ ドライブで暗号化が必要かどうかを指定します。|
|製造元|コンピューターの BIOS に表示されるコンピューターの製造元の名前。|
|モデル|コンピューターの BIOS に表示されるコンピューターの製造元のモデル名。|
|デバイス ユーザー|コンピューターの既知のユーザー。|

### <a name="computer-volume"></a><a name="bkmk_volume"></a> コンピューター ボリューム

|Column&nbsp;name|[説明]|
|----------------|----|
|ドライブ文字|コンピューターのドライブ文字。|
|ドライブの種類|ドライブの種類。 有効な値は、 **[オペレーティング システム ドライブ]** と **[固定データ ドライブ]** です。 これらのエントリは論理ボリュームではなく物理ドライブです。|
|暗号強度|BitLocker 管理ポリシーで選択した暗号強度。|
|保護の種類|ドライブを暗号化するためにポリシーで選択した保護の種類。 OS ドライブの有効な保護の種類は、 **[TPM]** または **[TPM+PIN]** です。 固定データ ボリュームの有効な保護の種類は **[パスワード]** です。|
|保護の状態|ポリシーで指定した保護の種類がコンピューターで有効になっていることを示します。 有効な状態は **[オン]** または **[オフ]** です。|
|暗号化の状態|ドライブの暗号化の状態。 有効な状態は、 **[暗号化]** 、 **[暗号化なし]** 、 **[暗号化中]** です。|

## <a name="bitlocker-enterprise-compliance-dashboard"></a><a name="bkmk-dashboard"></a> BitLocker エンタープライズ準拠ダッシュボード

このレポートには、組織全体の BitLocker 準拠状態を示す次のグラフが表示されます。

- 準拠状態の配布

- 非準拠 - エラー配布

- ドライブの種類による準拠状態の配布

:::image type="content" source="media/bitlocker-enterprise-compliance-dashboard.png" alt-text="BitLocker エンタープライズのコンプライアンス ダッシュボードのスクリーンショット例" lightbox="media/bitlocker-enterprise-compliance-dashboard.png":::

### <a name="compliance-status-distribution"></a>準拠状態の配布

この円グラフには、組織のコンピューターの準拠状態が示されます。 また、選択したコレクションに含まれる合計コンピューター数に占める、準拠している状態のコンピューターの割合が表示されます。 各状態のコンピューターの実数も表示されます。

この円グラフには次の準拠状態が表示されます。

- [準拠]

- 非対応

- ユーザーを除外する

- ユーザー - 一時除外

- ポリシー - 非実行

- 不明。 エラー状態がレポートされたコンピューターや、コレクションに含まれていてまだ準拠状態がレポートされていないコンピューターが該当します。 準拠状態の情報の欠落は、コンピューターが組織から切断されている場合などに生じます。

### <a name="non-compliant---errors-distribution"></a>非準拠 - エラー配布

この円グラフには、BitLocker ドライブ暗号化ポリシーに準拠していない組織内のコンピューターのカテゴリが示されます。 また、各カテゴリのコンピューターの数も示されます。 コレクションに含まれる非準拠コンピューターの合計数から、それぞれの割合が計算されます。

- ユーザーが暗号化を延期しました

- 互換性のある TPM が見つかりません

- システムのパーティションが使用できないか、小さすぎます

- TPM は表示されていますが、初期化されていません

- ポリシーの競合

- TPM の自動プロビジョニングを待機中

- 不明なエラーが発生しました

- 情報がありません。 これらのコンピューターには、BitLocker 管理エージェントがインストールされていないか、インストールされているがアクティブ化されていません。 たとえば、サービスが動作していません。

### <a name="compliance-status-distribution-by-drive-type"></a>ドライブの種類による準拠状態の配布

この棒グラフには、ドライブの種類別に、現在の BitLocker の準拠状態が表示されます。 状態には **[準拠]** と **[非準拠]** があります。 固定データ ドライブと OS ドライブを示す棒が表示されます。 このレポートには、固定データ ドライブのないコンピューターが含まれ、 **[オペレーティング システム ドライブ]** の棒にのみ値が表示されます。 BitLocker ドライブ暗号化ポリシーから除外が認められているユーザーまたは "ポリシーなし" カテゴリのユーザーはグラフに含まれません。

## <a name="bitlocker-enterprise-compliance-details"></a><a name="bkmk-compliancedetails"></a> BitLocker エンタープライズ準拠の詳細

このレポートでは、BitLocker 管理ポリシーを展開した先のコンピューターのコレクションについて、組織全体の BitLocker 準拠に関する情報が示されます。

:::image type="content" source="media/bitlocker-enterprise-compliance-details.png" alt-text="BitLocker エンタープライズのコンプライアンスの詳細のスクリーンショット例" lightbox="media/bitlocker-enterprise-compliance-details.png":::

|列名|[説明]|
|--- |--- |
|マネージド コンピューター|BitLocker 管理ポリシーを展開した先のコンピューターの数。|
|% 準拠|組織内の準拠コンピューターの割合。|
|% 非準拠|組織内の非準拠コンピューターの割合。|
|% 不明な準拠|準拠状態が不明なコンピューターの割合。|
|除外 (%)|BitLocker 暗号化の要件から除外されているコンピューターの割合。|
|非除外 (%)|BitLocker 暗号化の要件から除外されていないコンピューターの割合。|
|[準拠]|組織内の準拠コンピューターの数。|
|非準拠|組織内の非準拠コンピューターの数。|
|不明な準拠|準拠状態が不明なコンピューターの数。|
|除外|BitLocker 暗号化の要件から除外されているコンピューターの数。|
|非除外|BitLocker 暗号化の要件から除外されていないコンピューターの数。|

### <a name="computer-details"></a>コンピューターの詳細

|列名|[説明]|
|--- |--- |
|コンピューター名|マネージド デバイスの DNS コンピューター名。|
|ドメイン名|コンピューターの完全修飾ドメイン名。|
|準拠状態|コンピューターの全体的な準拠状態。 有効な状態は **[準拠]** と **[非準拠]** です。|
|除外|ユーザーが BitLocker ポリシーから除外されているかどうかを示します。|
|デバイス ユーザー|デバイスのユーザー。|
|対応状態の詳細|指定したポリシーに基づくコンピューターの準拠状態に関するエラー メッセージと状態メッセージ。|
|最後接続日時|コンピューターが準拠状態をレポートするためにサーバーに接続した最終日時。|

## <a name="bitlocker-enterprise-compliance-summary"></a><a name="bkmk-compliancesummary"></a> BitLocker エンタープライズ準拠の概要

このレポートを使用すると、組織の全体的な BitLocker 準拠状態が示されます。 また、BitLocker 管理ポリシーを展開した先の個々のコンピューターの準拠状態も示されます。

:::image type="content" source="media/bitlocker-enterprise-compliance-summary.png" alt-text="BitLocker エンタープライズのコンプライアンスの概要のスクリーンショット例" lightbox="media/bitlocker-enterprise-compliance-summary.png":::

|列名|[説明]|
|--- |--- |
|マネージド コンピューター|BitLocker ポリシーを使用して管理するコンピューターの数。|
|% 準拠|組織内の準拠コンピューターの割合。|
|% 非準拠|組織内の非準拠コンピューターの割合。|
|% 不明な準拠|準拠状態が不明なコンピューターの割合。|
|除外 (%)|BitLocker 暗号化の要件から除外されているコンピューターの割合。|
|非除外 (%)|BitLocker 暗号化の要件から除外されていないコンピューターの割合。|
|[準拠]|組織内の準拠コンピューターの数。|
|非対応|組織内の非準拠コンピューターの数。|
|不明な準拠|準拠状態が不明なコンピューターの数。|
|除外|BitLocker 暗号化の要件から除外されているコンピューターの数。|
|非除外|BitLocker 暗号化の要件から除外されていないコンピューターの数。|

## <a name="recovery-audit-report"></a><a name="bkmk-audit"></a> 回復の監査レポート

> [!NOTE]
> バージョン 2002 以降では、このレポートは [BitLocker の管理と監視の Web サイト](helpdesk-portal.md#reports)からのみ入手できます。<!-- 7629549 -->

このレポートを使用し、BitLocker 回復キーへのアクセス権を要求したユーザーを監査します。 次の条件に基づいてフィルター処理できます。

- 特定の種類のユーザー (ヘルプ デスク ユーザーやエンド ユーザーなど)
- 要求が失敗したか成功したか
- 要求された特定の種類のキー:回復キーのパスワード、回復キー ID、または TPM パスワード ハッシュ
- 取得が実行された日付の範囲

:::image type="content" source="media/bitlocker-recovery-audit-report.png" alt-text="BitLocker 回復監査レポートのスクリーンショット例" lightbox="media/bitlocker-recovery-audit-report.png":::

|Column&nbsp;name|説明|
|----------------|----|
|要求日時|エンド ユーザーまたはヘルプ デスク ユーザーがキーを要求した日付と時刻。|
|監査要求のソース|要求元のサイト。 有効な値は、 **[セルフサービス ポータル]** または **[ヘルプデスク]** です。|
|要求結果|要求の状態。 有効な値は **[成功]** または **[失敗]** です。|
|ヘルプデスク ユーザー|キーを要求した管理ユーザー。 ヘルプデスク管理者がユーザー名を指定せずにキーを回復した場合、 **[エンド ユーザー]** フィールドは空白になります。 標準のヘルプデスク ユーザーは、このフィールドに表示されるユーザー名を指定する必要があります。 セルフサービス ポータルを使用した回復の場合、このフィールドと **[エンド ユーザー]** フィールドには、要求を行っているユーザーの名前が表示されます。|
|エンド ユーザー|キーの取得を要求したユーザーの名前。|
|Computer|回復されたコンピューターの名前。|
|キーの種類|ユーザーが要求したキーの種類。 キーには、次の 3 種類があります。<br/><br/>-  **回復キーのパスワード**: 回復モードでコンピューターを回復するために使用<br/>- **回復キー ID**: 他のユーザーのために回復モードでコンピューターを回復するために使用<br/>- **TPM パスワード ハッシュ**: ロックされている TPM でコンピューターを回復するために使用|
|理由の説明|指定した種類のキーを、フォームで選択したオプションに基づいてユーザーが要求した理由。|
