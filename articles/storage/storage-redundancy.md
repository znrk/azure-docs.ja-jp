---
title: "Azure Storage のレプリケーション | Microsoft Docs"
description: "Microsoft Azure ストレージ アカウント内のデータは、持続性と高可用性を保証するため、レプリケートされています。 レプリケーション オプションには、ローカル冗長ストレージ (LRS)、ゾーン冗長ストレージ (ZRS)、geo 冗長ストレージ (GRS)、読み取りアクセス geo 冗長ストレージ (RA-GRS) などがあります。"
services: storage
documentationcenter: 
author: tamram
manager: carmonm
editor: tysonn
ms.assetid: 86bdb6d4-da59-4337-8375-2527b6bdf73f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 53c96db06a16be454e8205bc7cd81e063df6facd


---
# <a name="azure-storage-replication"></a>Azure Storage のレプリケーション
Microsoft Azure ストレージ アカウント内のデータは、持続性と高可用性を保証するため、常にレプリケートされています。 レプリケーションによりデータは同じデータ センター内か、2 番目のデータ センターにコピーされます。このコピー先は、選んだレプリケーション オプションによって変わります。 レプリケーションにより、データが保護され、一時的なハードウェアの障害が発生した際にアプリケーションのアップタイムが維持されます。 2 番目のデータ センターにデータがレプリケートされると、1 次拠点での重大なエラーに対してデータを保護することもできます。

レプリケーションにより、障害が発生しても、ストレージ アカウントは[ストレージのサービス レベル アグリーメント (SLA)](https://azure.microsoft.com/support/legal/sla/storage/) を満たすことができます。 Azure Storage の持続性と可用性の保証については、SLA をご覧ください。 

ストレージ アカウントを作成するときは、次のレプリケーション オプションのいずれかを選択できます。  

* [ローカル冗長ストレージ (LRS)](#locally-redundant-storage)
* [ゾーン冗長ストレージ (ZRS)](#zone-redundant-storage)
* [geo 冗長ストレージ (GRS)](#geo-redundant-storage)
* [読み取りアクセス geo 冗長ストレージ (RA-GRS)](#read-access-geo-redundant-storage)

読み取りアクセス geo 冗長ストレージ (RA-GRS) は、新しいストレージ アカウントを作成するときの既定のオプションです。

次の表は、LRS、ZRS、GRS、RA-GRS の違いについて、簡単に概要を示したものです。以降のセクションでは、それぞれのレプリケーションの種類について詳細に説明します。

| レプリケーションの方法 | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| 複数のデータセンター間でのデータのレプリケート |なし |はい |あり |はい |
| 1 次拠点に加えて 2 次拠点からもデータの読み取り可能 |いいえ |いいえ |いいえ |はい |
| 個別のノードで保持されるデータ コピーの数 |3 |3 |6 |6 |

さまざまな冗長オプションの料金情報については、「 [Azure Storage 料金](https://azure.microsoft.com/pricing/details/storage/) 」をご覧ください。

> [!NOTE]
> Premium Storage でサポートされるのは、ローカル冗長ストレージ (LRS) だけです。 Premium Storage については、「 [Premium Storage: Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ](storage-premium-storage.md)」をご覧ください。
> 
> 

## <a name="locally-redundant-storage"></a>ローカル冗長ストレージ
ローカル冗長ストレージ (LRS) は、ストレージ アカウントが作成されたリージョンのデータセンターでホストされているストレージ スケール ユニット内でデータを 3 回レプリケートします。 3 つのレプリカのすべてに書き込まれた場合にのみ、書き込み要求は正常に終了します。 これらの 3 つのレプリカはそれぞれ、1 つのストレージ スケール ユニット内の異なる障害ドメインとアップグレード ドメインに存在します。 

ストレージ スケール ユニットは、ストレージ ノードのラックのコレクションです。 障害ドメイン (FD) は、障害の物理ユニットを表すノードのグループで、同じ物理ラックに属するノードと見なすことができます。 アップグレード ドメイン (UD) は、サービス アップグレード (ロールアウト) のプロセス実行時に同時にアップグレードされるノードのグループです。 3 つのレプリカは 1 つのストレージ スケール ユニット内の複数の UD と FD にわたって分散されるため、ハードウェア障害によって単一のラックが影響を受ける場合や、ロールアウト中にノードがアップグレードされる場合でも、データを使うことができます。 

LRS は、コストが最も安いオプションであり、他のオプションと比較して最低の持続性を提供します。 データセンター レベルの障害 (火災、洪水など) が発生した場合は、3 つのレプリカすべてが失われたり、回復不能になる可能性があります。 このリスクを軽減するため、ほとんどのアプリケーションには geo 冗長ストレージ (GRS) が推奨されます。

それでも、ローカル冗長ストレージが望ましい場合があるシナリオもあります。 

* Azure Storage のレプリケーション オプションで最高の最大帯域幅を提供します。
* 再構築が簡単なデータをアプリケーションで格納する場合は、LRS を選択することもできます。
* 一部のアプリケーションは、データ ガバナンスの要件によって、国内でのみデータをレプリケートするように制限されています。 ペアになったリージョンが別の国にある場合もあります。リージョンのペアについては、「[Azure リージョン](https://azure.microsoft.com/regions/)」をご覧ください。

## <a name="zone-redundant-storage"></a>ゾーン冗長ストレージ
ゾーン冗長ストレージ (ZRS) は、LRS と同様に 3 つのレプリカを格納するだけでなく、1 つまたは 2 つのリージョン内のデータセンター間で非同期的にデータをレプリケートするので、LRS よりも高い持続性を提供します。 ZRS に格納されたデータは、プライマリ データセンターが使用不能または回復不能な場合でも、持続可能です。
ZRS を使用する予定のお客様は、次の点に注意してくださいです。 

* ZRS は、汎用ストレージ アカウントのブロック BLOB に対してのみ使うことができ、バージョン 2014-02-14 以降のストレージ サービスでのみサポートされます。 
* 非同期レプリケーションには遅延が伴うため、地域的な災害が発生した場合にデータをプライマリから復旧できないと、セカンダリにまだレプリケートされていない変更は失われる可能性があります。
* Microsoft がセカンダリへのフェールオーバーを開始するまで、レプリカは利用できない場合があります。
* ZRS アカウントを後で LRS または GRS に変換することはできません。 同様に、既存の LRS または GRS アカウントを ZRS アカウントに変換することはできません。
* ZRS アカウントには、メトリックまたはログ機能はありません。 

## <a name="geo-redundant-storage"></a>geo 冗長ストレージ
geo 冗長ストレージ (GRS) では、プライマリ リージョンから数百マイル離れたセカンダリ リージョンにデータがレプリケートされます。 ご使用のストレージ アカウントで GRS が有効になっている場合は、地域的な停電やプライマリ リージョンが復旧できない災害が発生しても、データは保持されます。

GRS が有効なストレージ アカウントでは、更新は最初にプライマリ リージョンにコミットされます。プライマリ リージョンでは、更新は 3 回レプリケートされます。 次に、更新はセカンダリ リージョンに非同期にレプリケートされます。セカンダリ リージョンでも、更新は 3 回レプリケートされます。 

GRS では、プライマリ リージョンとセカンダリ リージョンの両方が、LRS で説明したように、ストレージ スケール ユニット内の異なる障害ドメインとアップグレード ドメイン間でレプリカを管理します。

考慮事項:

* 非同期レプリケーションには遅延が伴うため、地域的な災害が発生した場合にデータをプライマリ リージョンから復旧できないと、セカンダリ リージョンにまだレプリケートされていない変更は失われます。
* Microsoft がセカンダリ リージョンへのフェールオーバーを開始しない限り、レプリカは利用できません。
* アプリケーションでセカンダリ リージョンから読み取る場合は、RA-GRS を有効にする必要があります。 

ストレージ アカウントの作成時に、アカウントのプライマリ リージョンを選択します。 セカンダリ リージョンはプライマリ リージョンに基づいて決定され、変更することはできません。 次の表に、プライマリ リージョンとセカンダリ リージョンのペアを示します。

| プライマリ | セカンダリ |
| --- | --- |
| 米国中北部 |米国中南部 |
| 米国中南部 |米国中北部 |
| 米国東部 |米国西部 |
| 米国西部 |米国東部 |
| 米国東部 2 |米国中央部 |
| 米国中央部 |米国東部 2 |
| 北ヨーロッパ |西ヨーロッパ |
| 西ヨーロッパ |北ヨーロッパ |
| 東南アジア |東アジア |
| 東アジア |東南アジア |
| 中国東部 |中国北部 |
| 中国北部 |中国東部 |
| 東日本 |西日本 |
| 西日本 |東日本 |
| ブラジル南部 |米国中南部 |
| オーストラリア東部 |オーストラリア南東部 |
| オーストラリア南東部 |オーストラリア東部 |
| インド南部 |インド中部 |
| インド中部 |インド南部 |
| 米国政府アイオワ州 |米国政府バージニア州 |
| 米国政府バージニア州 |米国政府アイオワ州 |
| カナダ中部 |カナダ東部 |
| カナダ東部 |カナダ中部 |
| 英国西部 |英国南部 |
| 英国南部 |英国西部 |
| ドイツ中部 |ドイツ北東部 |
| ドイツ北東部 |ドイツ中部 |
| 米国西部 2 |米国中西部 |
| 米国中西部 |米国西部 2 |

Azure でサポートされているリージョンに関する最新の情報については、「 [Azure のリージョン](https://azure.microsoft.com/regions/)」を参照してください。

## <a name="read-access-geo-redundant-storage"></a>読み取りアクセス geo 冗長ストレージ
読み取りアクセス geo 冗長ストレージ (RA-GRS) では、GRS が提供する 2 つのリージョンにまたがるレプリケーションに加えて、2 次拠点のデータにも読み取り専用アクセスを提供することで、ストレージ アカウントの可用性が最大限に発揮されます。 

セカンダリ リージョンのデータに対して読み取り専用アクセスを有効にすると、ストレージ アカウントのプライマリ エンドポイントだけでなく、セカンダリ エンドポイントのデータも使用できます。 セカンダリ エンドポイントはプライマリ エンドポイントと似ていますが、アカウント名に `–secondary` というサフィックスが追加されます。 たとえば、BLOB サービスのプライマリ エンドポイントが `myaccount.blob.core.windows.net` の場合、セカンダリ エンドポイントは `myaccount-secondary.blob.core.windows.net` になります。 ストレージ アカウントのアクセス キーは、プライマリ エンドポイントとセカンダリ エンドポイントの両方で同じです。

考慮事項:

* RA-GRS を使うときは、通信対象のエンドポイントをアプリケーションで管理する必要があります。 
* RA-GRS は高可用性を目的として作られています。 スケーラビリティのガイダンスについては、[パフォーマンス チェックリスト](storage-performance-checklist.md)をご覧ください。

## <a name="next-steps"></a>次のステップ
* [Azure Storage の料金](https://azure.microsoft.com/pricing/details/storage/)
* [Azure ストレージ アカウントについて](storage-create-storage-account.md)
* [Azure Storage のスケーラビリティおよびパフォーマンスのターゲット](storage-scalability-targets.md)
* [Microsoft Azure Storage 冗長オプションと読み取りアクセス geo 冗長ストレージ ](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)  
* [SOSP ペーパー - Azure Storage: 強力な整合性を備えた高可用クラウド ストレージ サービス](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)  




<!--HONumber=Nov16_HO3-->


