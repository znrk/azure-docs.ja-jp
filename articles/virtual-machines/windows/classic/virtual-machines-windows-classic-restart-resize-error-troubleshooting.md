---
title: "VM の再起動またはサイズ変更に関する問題 | Microsoft Docs"
description: "Azure での既存の Windows 仮想マシンの再起動またはサイズ変更に関するクラシック デプロイメントの問題のトラブルシューティング"
services: virtual-machines-windows
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 09/20/2016
ms.devlang: na
ms.author: delhan
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: f63e21708192532b94fbfce4658d91dec712cc29


---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Azure での既存の Windows 仮想マシンの再起動またはサイズ変更に関するクラシック デプロイメントの問題のトラブルシューティング
> [!div class="op_single_selector"]
> * [クラシック](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../../virtual-machines-windows-restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

停止している Azure 仮想マシン (VM) を起動しようとしたとき、または既存の Azure VM のサイズを変更しようとしたときに発生する一般的なエラーは割り当てエラーです。 このエラーは、クラスターまたはリージョンに使用可能なリソースがないか、要求された VM サイズをサポートできない場合に発生します。

> [!IMPORTANT]
> Azure には、リソースの作成と操作に関して、[Resource Manager とクラシックの](../../../resource-manager-deployment-model.md) 2 種類のデプロイメント モデルがあります。  この記事では、クラシック デプロイ モデルの使用方法について説明します。 最新のデプロイでは、リソース マネージャー モデルを使用することをお勧めします。
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>監査ログの収集
トラブルシューティングを開始するには、監査ログを収集して問題に関連するエラーを特定します。

Azure ポータルで、**[参照]** > **[仮想マシン]** > *対象の Windows 仮想マシン* > **[設定]** > **[監査ログ]** をクリックします。

## <a name="issue-error-when-starting-a-stopped-vm"></a>問題: 停止している VM の起動時のエラー
停止している VM を起動しようとしたときに、割り当てエラーが発生します。

### <a name="cause"></a>原因
停止している VM の起動要求は、クラウド サービスをホストしている元のクラスターで行う必要がありますが、 クラスターに要求の処理に使用できる空き領域がありません。

### <a name="resolution"></a>解決策
* 新しいクラウド サービスを作成し、アフィニティ グループではなく、リージョンまたはリージョン ベースの仮想ネットワークに関連付けます。
* 停止している VM を削除します。
* ディスクを使用して、新しいクラウド サービスに VM を再作成します。
* 再作成された VM を起動します。

新しいクラウド サービスを作成しようとしたときにエラーが発生した場合は、後で再試行するか、クラウド サービスのリージョンを変更します。

> [!IMPORTANT]
> 新しいクラウド サービスには、新しい名前と VIP が割り当てられるので、既存のクラウド サービスでこの情報を使用しているすべての依存関係について、この情報を変更する必要があります。
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>問題: 既存の VM のサイズ変更時のエラー
既存の VM のサイズを変更しようとしたときに割り当てエラーが発生します。

### <a name="cause"></a>原因
VM のサイズ変更要求は、クラウド サービスをホストしている元のクラスターで行う必要がありますが、 クラスターが要求された VM サイズをサポートしていません。

### <a name="resolution"></a>解決策
要求する VM サイズを小さくし、サイズ変更要求を再試行します。

* **[すべて参照]** > **[仮想マシン (クラシック)]** > *お使いの仮想マシン* > **[設定]** > **[サイズ]** の順にクリックします。 詳細な手順については、「 [Resize the virtual machine (仮想マシンのサイズの変更)](https://msdn.microsoft.com/library/dn168976.aspx)」を参照してください。

VM サイズを小さくできない場合は、次の手順に従います。

* アフィニティ グループにリンクしたり、アフィニティ グループにリンクされた仮想ネットワークに関連付けたりせずに、新しいクラウド サービスを作成します。
* 作成したクラウド サービスにサイズの大きい新しい VM を作成します。

同じクラウド サービス内のすべての VM を統合できます。 既存のクラウド サービスがリージョン ベースの仮想ネットワークに関連付けられている場合は、新しいクラウド サービスを既存の仮想ネットワークに接続できます。

既存のクラウド サービスがリージョン ベースの仮想ネットワークに関連付けられていない場合は、既存のクラウド サービス内の VM を削除し、ディスクから新しいクラウド サービスに VM を再作成する必要があります。 ただし、新しいクラウド サービスには新しい名前と VIP が割り当てられるので、既存のクラウド サービスでこれらの情報を現在使用しているすべての依存関係について、情報を更新する必要があります。

## <a name="next-steps"></a>次のステップ
Azure で新しい Windows VM を作成するときに問題が発生する場合は、[Azure での新しい Windows 仮想マシンの作成に関するデプロイメントの問題のトラブルシューティング](../../virtual-machines-windows-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)に関する記事を参照してください。




<!--HONumber=Nov16_HO3-->


