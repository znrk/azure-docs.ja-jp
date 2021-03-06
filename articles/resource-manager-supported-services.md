---
title: リソース マネージャーでサポートされるサービス | Microsoft Docs
description: リソース マネージャーをサポートするリソース プロバイダー、そのスキーマと利用可能な API バージョン、およびリソースをホストできるリージョンについて説明します。
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: magoedte;tomfitz

---
# リソース マネージャーのプロバイダー、リージョン、API のバージョン、およびスキーマ
Azure リソース マネージャーは、アプリケーションを構成するサービスをデプロイして管理するための新しい方法を提供します。すべてではありませんがほとんどのサービスはリソース マネージャーをサポートし、一部のサービスはリソース マネージャーを部分的にのみサポートします。Microsoft は将来のソリューションに重要なすべてのサービスに対してリソース マネージャーを有効にする予定ですが、サポートが一貫したものになるまでは、各サービスの現在の状態を把握する必要があります。このトピックでは、Azure リソース マネージャーに対してサポートされるリソース プロバイダーの一覧を示します。

リソースをデプロイするときは、リソースをサポートしているリージョンとリソースで利用できる API バージョンも知っておく必要があります。「[サポートされているリージョン](#supported-regions)」セクションでは、サブスクリプションおよびリソースで有効なリージョンを確認する方法が示されています。「[サポートされる API バージョン](#supported-api-versions)」セクションでは、利用できる API バージョンを決定する方法について紹介します。

Azure ポータルとクラシック ポータルでサポートされているサービスについては、[Azure ポータルで利用できるサービスの表](https://azure.microsoft.com/features/azure-portal/availability/)を参照してください。リソースの移動をサポートするサーバーを確認する方法については、「[新しいリソース グループまたはサブスクリプションへのリソースの移動](resource-group-move-resources.md)」を参照してください。

次の表では、各サービスがリソース マネージャーによるデプロイと管理をサポートするかどうかを示します。"**クイック スタート テンプレート**" 列のリンクは、指定されたリソース プロバイダーに対するクエリを Azure クイック スタート テンプレート レポジトリに送信します。クイック スタート テンプレートは、頻繁に追加、更新されます。特定のサービスに対するリンクがあっても、必ずしもクエリによってリポジトリからテンプレートが返されるとは限りません。

## コンピューティング
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| Batch |あり |[Batch REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) |[2015-12-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json) | |
| コンテナー |はい |[コンテナー サービスの REST](https://msdn.microsoft.com/library/azure/mt711470.aspx) |[2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) |[Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics Lifecycle Services |はい | | | |
| スケール セット |はい |[スケール セットの REST](https://msdn.microsoft.com/library/azure/mt705635.aspx) |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) |[virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) |
| Service Fabric |はい |[Service Fabric Rest](https://msdn.microsoft.com/library/azure/dn707692.aspx) | |[Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Virtual Machines |あり |[VM REST](https://msdn.microsoft.com/library/azure/mt163647.aspx) |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) |[virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Virtual Machines (クラシック) |制限あり |- |- |- |
| リモート アプリ |No |- |- |- |
| Cloud Services (クラシック) |制限あり (下記参照) |- |- |- |

「Virtual Machines (クラシック)」とは、リソース マネージャー デプロイ モデルではなくクラシック デプロイ モデルを使用してデプロイされたリソースのことです。一般に、このようなリソースはリソース マネージャーの操作をサポートしていませんが、有効になっている操作がいくつかあります。これらのデプロイメント モデルの詳細については、「[リソース マネージャー デプロイと従来のデプロイを理解する](resource-manager-deployment-model.md)」を参照してください。

Cloud Services (クラシック) は、他のクラシック リソースと共に使用できますが、クラシック リソースは、リソース マネージャーのすべての機能を利用するわけではないため、今後のソリューションに適したオプションではありません。代わりに、アプリケーション インフラストラクチャを変更して、Microsoft.Compute、Microsoft.Storage、Microsoft.Network の各名前空間のリソースを使用することを検討してください。

## ネットワーク
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| Application Gateway |はい |[Application Gateway REST](https://msdn.microsoft.com/library/azure/mt684939.aspx) | |[applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS |はい |[DNS REST](https://msdn.microsoft.com/library/azure/mt163862.aspx) |[2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) |[dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute |あり |[ExpressRoute REST](https://msdn.microsoft.com/library/azure/mt586720.aspx) | |[expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Load Balancer |はい |[Load Balancer REST](https://msdn.microsoft.com/library/azure/mt163651.aspx) |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |[loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Traffic Manager |はい |[Traffic Manager REST](https://msdn.microsoft.com/library/azure/mt163667.aspx) |[2015-11-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json) |[trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtual Networks |あり |[Virtual Network REST](https://msdn.microsoft.com/ja-JP/library/azure/mt163650.aspx) |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |[virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN Gateway |あり |[ネットワーク ゲートウェイ REST](https://msdn.microsoft.com/library/azure/mt163859.aspx) | |[virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[connections](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |

## データ + ストレージ
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- | --- |
| DocumentDB |あり |[DocumentDB REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) |[2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json) |[Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis Cache |はい | |[2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) |[Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| 検索 |あり |[Search REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) | |[Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Storage |あり |[Storage REST](https://msdn.microsoft.com/library/azure/mt163683.aspx) |[ストレージ アカウント](resource-manager-template-storage.md) |[Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| SQL Database |あり |[SQL Database REST](https://msdn.microsoft.com/library/azure/mt163571.aspx) |[2014-04-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) |[Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL Data Warehouse |あり | | | |
| StorSimple |いいえ |- |- |- |

## Web とモバイル
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| API Apps |はい | |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[API Apps](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API Management |あり |[API Management REST](https://msdn.microsoft.com/library/azure/dn776326.aspx) | |[Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) |
| Logic Apps |はい |[ワークフロー サービスの REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) |[2015-02-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-02-01-preview/Microsoft.Logic.json) |[Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobile Apps |はい | | | |
| [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code) | | | |
| Mobile Engagement |あり | | |[Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Web Apps |あり | |[2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |[Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## 分析
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| Data Catalog |はい |[Data Catalog REST](https://msdn.microsoft.com/library/azure/mt267595.aspx) |[2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) | |
| Data Factory |あり |[Data Factory REST](https://msdn.microsoft.com/library/azure/dn906738.aspx) | |[Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Data Lake Analytics |はい | |[2015-10-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) |[Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Data Lake Store |はい |[Data Lake Store REST](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[2015-10-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) |[Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights |あり |[HDInsights REST](https://msdn.microsoft.com/library/azure/mt622197.aspx) | |[Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Machine Learning |はい |[Machine Learning REST](https://msdn.microsoft.com/library/azure/mt767538.aspx) |[2016-05-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) | |
| Stream Analytics |はい |[Steam Analytics REST](https://msdn.microsoft.com/library/azure/dn835031.aspx) | | |
| Power BI |はい |[Power BI Embedded REST](https://msdn.microsoft.com/library/azure/mt712303.aspx) |[2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) | |

## Intelligence
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| Cognitive Services |はい | |[2016-02-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) | |

## モノのインターネット
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| イベント ハブ |はい |[Event Hubs REST](https://msdn.microsoft.com/library/azure/dn790674.aspx) | | |
| IoTHubs |はい |[IoT Hub REST](https://msdn.microsoft.com/library/azure/mt589014.aspx) |[2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) |[Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Notification Hubs |あり |[Notification Hubs REST](https://msdn.microsoft.com/library/azure/dn495827.aspx) |[2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) |[Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## メディアと CDN
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| CDN |はい |[CDN REST](https://msdn.microsoft.com/library/azure/mt634456.aspx) |[2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) |[Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| メディア サービス |はい |[Media Services REST](https://msdn.microsoft.com/library/azure/hh973617.aspx) |[2015-10-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) | |

## ハイブリッド統合
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| BizTalk Services |はい | |[2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) | |
| Recovery Service |はい |[Site Recovery REST](https://msdn.microsoft.com/library/azure/mt750497.aspx) |[2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) |[Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Service Bus |あり | | |[Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## ID 管理とアクセス管理
Azure Active Directory はリソース マネージャーと連携して、サブスクリプションのロールベースのアクセス制御を可能にします。ロールベースのアクセス制御と Active Directory の使用方法については、「[Azure Active Directory リソースへのアクセスをロールの割り当てによって管理する](active-directory/role-based-access-control-configure.md)」を参照してください。

## 開発者サービス
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| Application Insights |はい |[Insights REST](https://msdn.microsoft.com/library/azure/dn931943.aspx) |[2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) |[Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bing Maps |はい | | | |
| DevTest Labs |はい | |[2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) |[Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code) |
| Visual Studio アカウント |はい | |[2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) | |

## 管理とセキュリティ
| サービス | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- |
| Automation |あり |[Automation REST](https://msdn.microsoft.com/library/azure/mt662285.aspx) |[2015-10-31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json) |[Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Key Vault |あり |[Key Vault](https://msdn.microsoft.com/library/azure/dn903609.aspx) |[Key Vault](resource-manager-template-keyvault.md)<br />[Key Vault シークレット](resource-manager-template-keyvault-secret.md) |[Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Operational Insights |はい | | | |
| Scheduler |はい |[Scheduler REST](https://msdn.microsoft.com/library/azure/mt629143.aspx) |[2014-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-08-01/Microsoft.Scheduler.json) |[Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| セキュリティ (プレビュー) |はい |[Security REST](https://msdn.microsoft.com/library/azure/mt704034.aspx) | |[Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code) |

## リソース マネージャー
| 機能 | リソース マネージャーが有効 | REST API | スキーマ | クイック スタート テンプレート |
| --- | --- | --- | --- | --- | --- |
| 承認 |あり |[管理ロック](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[ロール ベースのアクセス制御](https://msdn.microsoft.com/library/azure/dn906885.aspx) |[リソース ロック](resource-manager-template-lock.md)<br />[ロールの割り当て](resource-manager-template-role.md) |[Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| リソース |あり |[リンク済みリソース](https://msdn.microsoft.com/library/azure/mt238499.aspx) |[リソース リンク](resource-manager-template-links.md) |[Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |

## リソース プロバイダーと種類
リソースをデプロイするときに、リソース プロバイダーと種類に関する情報を取得しなければならないケースは少なくありません。この情報は、REST API、Azure PowerShell、Azure CLI のいずれかを使って取得できます。

リソース プロバイダーを使用するには、そのリソース プロバイダーをアカウントに登録する必要があります。既定では、多くのリソース プロバイダーが自動的に登録されます。ただし、一部のリソース プロバイダーは手動で登録することが必要な場合があります。次の例は、リソース プロバイダーの登録状態を取得し、必要に応じてリソース プロバイダーを登録する方法を示しています。

### REST API
使用可能なすべてのリソース プロバイダーとその種類、場所、API バージョン、登録状態を取得するには、[すべてのリソース プロバイダーの一覧表示](https://msdn.microsoft.com/library/azure/dn790524.aspx)操作を使用します。リソース プロバイダーを登録する必要がある場合は、「[リソース プロバイダーへのサブスクリプションの登録](https://msdn.microsoft.com/library/azure/dn790548.aspx)」をご覧ください。

### PowerShell
使用可能なすべてのリソース プロバイダーを取得する例を次に示します。

    Get-AzureRmResourceProvider -ListAvailable


次の例では、特定のリソース プロバイダーのリソースの種類を取得しています。

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes

次のように出力されます。

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...

リソース プロバイダーを登録するには、名前空間を指定します。

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### Azure CLI
使用可能なすべてのリソース プロバイダーを取得する例を次に示します。

    azure provider list

次のように出力されます。

    info:    Executing command provider list
    + Getting ARM registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

特定のリソース プロバイダーの情報は、次のコマンドでファイルに保存できます。

    azure provider show Microsoft.Web -vv --json > c:\temp.json

リソース プロバイダーを登録するには、名前空間を指定します。

    azure provider register -n Microsoft.ServiceBus

## サポートされているリージョン
通常、リソースをデプロイするときはリソースのリージョンを指定する必要があります。リソース マネージャーはすべてのリージョンでサポートされていますが、デプロイするリソースはすべてのリージョンではサポートされていない場合があります。さらに、サブスクリプションでの制限により、リソースをサポートする一部のリージョンを使用できない場合があります。これらの制限事項は、本国での税金に関する問題のため、またはサブスクリプション管理者によって設けられた、使用を特定のリージョンに制限するポリシーの結果である場合があります。

全 Azure サービスを対象とする全サポート リージョンの完全な一覧が必要な場合、「[リージョン別のサービス](https://azure.microsoft.com/regions/#services)」を参照してください。ただし、その一覧には、ご利用のサブスクリプションでサポートされていないリージョンが含まれている可能性があります。次のコマンドを実行すると、ご利用のサブスクリプションでサポートされているリージョンをリソース タイプ別に確認できます。

### REST API
ご利用のサブスクリプションの特定のリソースの種類で使用可能なリージョンを確認するには、[すべてのリソース プロバイダーの一覧表示](https://msdn.microsoft.com/library/azure/dn790524.aspx)操作を利用します。

### PowerShell
次の例では、Web サイトでサポートされるリージョンを取得する方法を示します。

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

次のように出力されます。

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### Azure CLI
次の例では、リソースの種類ごとにサポートされるすべての場所が返されます。

    azure location list

また、[jq](https://stedolan.github.io/jq/) などの JSON ユーティリティを使用して場所の結果をフィルターすることもできます。

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

次のような結果が返されます。

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## サポートされている API バージョン
テンプレートをデプロイするとき、各リソースの作成に使用する API バージョンを指定する必要があります。API バージョンは、リリース プロバイダーがリリースする REST API のバージョンに一致します。リソース プロバイダーは新しい機能を有効にするとき、REST API の新しいバージョンをリリースします。そのため、テンプレートで指定した API のバージョンにより、テンプレートで指定できるプロパティが変わります。一般的に、新しいテンプレートを作成するとき、最新の API バージョンを選択します。既存のテンプレートの場合、以前の API バージョンの使用を続けるか、テンプレートを最新版に更新して新しい機能を最大限に活用するか決定できます。

### REST API
リソースの種類で使用可能な API バージョンを確認するには、[すべてのリソース プロバイダーの一覧表示](https://msdn.microsoft.com/library/azure/dn790524.aspx)操作を利用します。

### PowerShell
次の例では、特定のリソースの種類で利用できる API バージョンを取得する方法を示します。

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

次のように出力されます。

    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### Azure CLI
次のコマンドで、リソース プロバイダーの情報 (利用可能な API バージョンを含む) をファイルに保存できます。

    azure provider show Microsoft.Web -vv --json > c:\temp.json

ファイルを開き、**apiVersions** 要素を検索できます。

## 次のステップ
* リソース マネージャーのテンプレートの作成の詳細については、[Azure リソース マネージャーのテンプレートの作成](resource-group-authoring-templates.md)に関するページを参照してください。
* リソースをデプロイする方法を確認するには、「[Azure リソース マネージャーのテンプレートを使用したアプリケーションのデプロイ](resource-group-template-deploy.md)」を参照してください。

<!---HONumber=AcomDC_0720_2016-->