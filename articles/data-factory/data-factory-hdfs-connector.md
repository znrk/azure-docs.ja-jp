---
title: "オンプレミスの HDFS からデータを移動する | Microsoft Docs"
description: "Azure Data Factory を使用してオンプレミスの HDFS からデータを移動する方法を説明します。"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3215b82d-291a-46db-8478-eac1a3219614
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: 6ec8ac288a4daf6fddd6d135655e62fad7ae17c2
ms.openlocfilehash: 0f0eaaa927ea73cec845dbb369dc2c4a7a8466ba


---
# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Azure Data Factory を使用してオンプレミスの HDFS からデータを移動する
この記事では、Azure Data Factory のコピー アクティビティを使用して、オンプレミスの HDFS から他のデータ ストアにデータを移動する方法について説明します。 この記事は、「 [データ移動アクティビティ](data-factory-data-movement-activities.md) 」という記事に基づき、コピー アクティビティによるデータ移動の一般概要とサポートされるデータ ストアの組み合わせについて紹介しています。

Data Factory が現在サポートしているのは、オンプレミスの HDFS から他のデータ ストアへのデータの移動だけで、他のデータ ストアからオンプレミスの HDFS への移動はサポートしていません。

## <a name="enabling-connectivity"></a>接続を有効にする
Data Factory サービスでは、Data Management Gateway を使用したオンプレミスの HDFS への接続をサポートします。 Data Management Gateway の詳細およびゲートウェイの設定手順については、「 [オンプレミスの場所とクラウド間のデータ移動](data-factory-move-data-between-onprem-and-cloud.md) 」を参照してください。 Azure IaaS VM でホストされている場合でも、HDFS への接続にゲートウェイを使用します。

ゲートウェイは同じオンプレミスのマシンまたは Azure VM に HDFS としてインストールできますが、別個のマシンおよびAzure IaaS VM にゲートウェイをインストールすることをお勧めします。 ゲートウェイを別のコンピューターにインストールすることで、リソースの競合が減少し、パフォーマンスが向上します。 別のマシンにゲートウェイをインストールすると、そのマシンが HDFS を持つマシンにアクセスできるようになります。

## <a name="copy-data-wizard"></a>データのコピー ウィザード
オンプレミスの HDFS との間でデータをコピーするパイプラインを作成する最も簡単な方法は、データのコピー ウィザードを使用することです。 データのコピー ウィザードを使用してパイプラインを作成する簡単な手順については、「 [チュートリアル: コピー ウィザードを使用してパイプラインを作成する](data-factory-copy-data-wizard-tutorial.md) 」をご覧ください。

以下の例は、[Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)、[Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)、または [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) を使用してパイプラインを作成する際に使用できるサンプルの JSON 定義です。 これらの例は、オンプレミスの HDFS から Azure BLOB ストレージにデータをコピーする方法を示しています。 ただし、Azure Data Factory のコピー アクティビティを使用して、 [こちら](data-factory-data-movement-activities.md#supported-data-stores-and-formats) に記載されているシンクのいずれかにデータをコピーすることができます。

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>サンプル: オンプレミスの HDFS から Azure BLOB へのデータのコピー
このサンプルは、オンプレミスの HDFS から Azure BLOB ストレージにデータをコピーする方法を示します。 Azure Data Factory のコピー アクティビティを使用して、 **こちら** に記載されているシンクのいずれかにデータを [直接](data-factory-data-movement-activities.md#supported-data-stores-and-formats) コピーすることもできます。  

このサンプルでは、次の Data Factory のエンティティがあります。

1. [OnPremisesHdfs](#hdfs-linked-service-properties)型のリンクされたサービス。
2. [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service)型のリンクされたサービス。
3. [FileShare](#hdfs-dataset-type-properties) 型の入力[データセット](data-factory-create-datasets.md)。
4. [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) 型の出力[データセット](data-factory-create-datasets.md)。
5. [FileSystemSource](#hdfs-copy-activity-type-properties) および [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) を使用するコピー アクティビティを含む[パイプライン](data-factory-create-pipelines.md)。

このサンプルはオンプレミスの HDFS のデータを Azure BLOB に 1 時間ごとにコピーします。 これらのサンプルで使用される JSON プロパティの説明はサンプルに続くセクションにあります。

最初の手順として、データ管理ゲートウェイを設定します。 設定手順は、 [オンプレミスの場所とクラウドの間でのデータ移動](data-factory-move-data-between-onprem-and-cloud.md) に関する記事に記載されています。

**HDFS のリンクされたサービス** : この例では Windows 認証を使用しています。 使用できるさまざまな種類の認証については、 [HDFS のリンクされたサービス](#hdfs-linked-service-properties) に関するセクションをご覧ください。

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }

**Azure Storage のリンクされたサービス**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**HDFS 入力データセット** : このデータセットは HDFS フォルダー DataTransfer/UnitTest/ を参照します。 パイプラインは、このフォルダー内のすべてのファイルをコピー先にコピーします。

”external” を ”true” に設定すると、データセットが Data Factory の外部にあり、Data Factory のアクティビティによって生成されたものではないことが Data Factory サービスに通知されます。

    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure BLOB の出力データセット**

データは新しい BLOB に 1 時間おきに書き込まれます (頻度: 時間、間隔: 1)。 BLOB のフォルダー パスは、処理中のスライスの開始時間に基づき、動的に評価されます。 フォルダー パスは開始時間の年、月、日、時刻の部分を使用します。

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**コピー アクティビティのあるパイプライン**

パイプラインには、この入力データセットと出力データセットを使用するように構成され、1 時間おきに実行するようにスケジュールされているコピー アクティビティが含まれています。 パイプライン JSON 定義で、**source** 型が **FileSystemSource** に設定され、**sink** 型が **BlobSink** に設定されています。 **query** プロパティに指定されている SQL クエリは過去のデータを選択してコピーします。

    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>HDFS のリンクされたサービスのプロパティ
次の表は、HDFS のリンクされたサービスに固有の JSON 要素の説明をまとめたものです。

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| type |type プロパティを **Hdfs** |はい |
| Url |HDFS への URL |はい |
| encryptedCredential |[New-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) 出力。 |なし |
| userName |Windows 認証のユーザー名。 |あり (Windows 認証用) |
| パスワード |Windows 認証のパスワード。 |あり (Windows 認証用) |
| authenticationType |Windows、または匿名。 |はい |
| gatewayName |Data Factory サービスが、HDFS への接続に使用するゲートウェイの名前。 |はい |

オンプレミスの HDFS の資格情報の設定について詳しくは、「[Data Management Gateway を使用してオンプレミスのソースとクラウドの間でデータを移動する](data-factory-move-data-between-onprem-and-cloud.md)」を参照してください。

### <a name="using-anonymous-authentication"></a>匿名認証を使用する
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Windows 認証を使用する
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }



## <a name="hdfs-dataset-type-properties"></a>HDFS データセットの type プロパティ
データセットの定義に利用できるセクションとプロパティの完全な一覧については、「[データセットの作成](data-factory-create-datasets.md)」という記事を参照してください。 データセット JSON の構造、可用性、ポリシーなどのセクションは、データセットのすべての型 (Azure SQL、Azure BLOB、Azure テーブルなど) でほぼ同じです。

**typeProperties** セクションはデータセット型ごとに異なり、データ ストアのデータの場所などに関する情報を提供します。 **FileShare** 型のデータセットの typeProperties セクション (HDFS データセットを含む) には次のプロパティがあります。

| プロパティ | 説明 | 必須 |
| --- | --- | --- |
| folderPath |フォルダーへのパス。 例: `myfolder`<br/><br/>文字列内の特殊文字にはエスケープ文字 "\" を使用します。 例: folder\subfolder には、folder\\\\subfolder を指定し、d:\samplefolder には、d:\\\\samplefolder を指定します。<br/><br/>このプロパティを **partitionBy** と組み合わせて、スライスの開始/終了日時に基づくフォルダー パスを使用できます。 |はい |
| fileName |テーブルでフォルダー内の特定のファイルを参照するには、**folderPath** にファイルの名前を指定します。 このプロパティの値を設定しない場合、テーブルはフォルダー内のすべてのファイルを参照します。<br/><br/>出力データセットに fileName が指定されていない場合、生成されるファイルの名前は次の形式になります。 <br/><br/>Data.<Guid>.txt (例: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt |いいえ |
| partitionedBy |partitionedBy を使用して時系列データに動的な folderPath と fileName を指定できます。 例: 1 時間ごとのデータに対して folderPath がパラメーター化されます。 |なし |
| fileFilter |すべてのファイルではなく、folderPath 内のファイルのサブセットを選択するために使用するフィルターを指定します。 <br/><br/>使用可能な値: `*` (複数の文字) および `?` (単一の文字)。<br/><br/>例 1: `"fileFilter": "*.log"`<br/>例 2: `"fileFilter": 2014-1-?.txt"`<br/><br/>**注**: fileFilter は FileShare 入力データセットに適用されます。 |いいえ |
| BlobSink の format |次の種類の形式がサポートされます: **TextFormat**、**AvroFormat**、**JsonFormat**、**OrcFormat**、および **ParquetFormat**。 形式の **type** プロパティをいずれかの値に設定します。 詳細については、「[TextFormat の指定](#specifying-textformat)」、「[AvroFormat の指定](#specifying-avroformat)」、「[JsonFormat の指定](#specifying-jsonformat)」、「[OrcFormat の指定](#specifying-orcformat)」、および「[ParquetFormat の指定](#specifying-parquetformat)」を参照してください。 ファイル ベースのストア間でファイルをそのままコピーする場合は (バイナリ コピー)、入力と出力の両方のデータセット定義で format セクションをスキップできます。 |なし |
| compression |データの圧縮の種類とレベルを指定します。 サポートされる種類は、**GZip**、**Deflate**、**BZip2** です。サポートされるレベルは、**Optimal** と **Fastest** です。 現時点では、**AvroFormat** と **OrcFormat** のデータの圧縮設定はサポートされていません。 詳細については、「[圧縮のサポート](#compression-support)」セクションを参照してください。 |いいえ |

> [!NOTE]
> fileName と fileFilter は、同時に使用することができません。
>
>

### <a name="using-partionedby-property"></a>partitionedBy プロパティの使用
前のセクションで説明したように、partitionedBy を使用して時系列データに動的な folderPath、fileName を指定できます。 これは、Data Factory のマクロ、および指定したデータ スライスの論理的な期間を示す システム変数 SliceStart と SliceEnd で行うことができます。

時系列データセット、スケジュール作成、スライスの詳細については、「[データセットの作成](data-factory-create-datasets.md)」、「[スケジュールと実行](data-factory-scheduling-and-execution.md)」、および「[パイプラインの作成](data-factory-create-pipelines.md)」に関する記事を参照してください。

#### <a name="sample-1"></a>サンプル 1:
    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy":
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

この例では、{Slice} は、指定された形式 (YYYYMMDDHH) で、Data Factory システム変数の SliceStart の値に置き換えられます。 SliceStart はスライスの開始時刻です。 folderPath はスライスごとに異なります。 例: wikidatagateway/wikisampledataout/2014100103 または wikidatagateway/wikisampledataout/2014100104

#### <a name="sample-2"></a>サンプル 2:
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],

この例では、SliceStart の年、月、日、時刻が folderPath プロパティと fileName プロパティで使用される個別の変数に抽出されます。

[!INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]

[!INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>HDFS のコピー アクティビティの type プロパティ
アクティビティの定義に利用できるセクションとプロパティの完全な一覧については、[パイプラインの作成](data-factory-create-pipelines.md)に関する記事を参照してください。 名前、説明、入力テーブル、出力テーブル、ポリシーなどのプロパティは、あらゆる種類のアクティビティで使用できます。

一方、アクティビティの typeProperties セクションで使用できるプロパティは、各アクティビティの種類によって異なります。 コピー アクティビティの場合、ソースとシンクの種類によって異なります。

コピー アクティビティで、source が **FileSystemSource** 型の場合は、typeProperties セクションで次のプロパティを使用できます:

**FileSystemSource** では次のプロパティがサポートされます。

| プロパティ | 説明 | 使用できる値 | 必須 |
| --- | --- | --- | --- |
| recursive |データをサブ フォルダーから再帰的に読み取るか、指定したフォルダーからのみ読み取るかを指定します。 |True、False (既定値) |なし |

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>パフォーマンスとチューニング
Azure Data Factory でのデータ移動 (コピー アクティビティ) のパフォーマンスに影響する主な要因と、パフォーマンスを最適化するための各種方法については、「[コピー アクティビティのパフォーマンスとチューニングに関するガイド](data-factory-copy-activity-performance.md)」を参照してください。



<!--HONumber=Nov16_HO3-->


