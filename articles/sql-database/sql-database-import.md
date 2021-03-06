---
title: "BACPAC ファイルをインポートして Azure SQL Database を作成する | Microsoft Docs"
description: "既存の BACPAC ファイルをインポートして、Azure SQL データベースを作成します。"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: cf9a9631-56aa-4985-a565-1cacc297871d
ms.service: sql-database
ms.devlang: NA
ms.date: 08/31/2016
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 26364edba47c4ac77b2125e067b99f87eefb434c


---
# <a name="import-a-bacpac-file-to-create-an-azure-sql-database"></a>BACPAC ファイルをインポートして Azure SQL データベースを作成する
**1 つのデータベース**

> [!div class="op_single_selector"]
> * [Azure ポータル](sql-database-import.md)
> * [PowerShell](sql-database-import-powershell.md)
> * [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
> * [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
> 
> 

この記事は、 [Azure Portal](https://portal.azure.com)を使用して BACPAC ファイルから Azure SQL データベースを作成する手順について説明します。

BACPAC は、データベース スキーマとデータを含む .bacpac ファイルです。 Azure Storage BLOB コンテナーからインポートされた BACPAC からデータベースが作成されます。 Azure Storage に .bacpac ファイルがない場合は、「 [SQL Database の BACPAC の作成およびエクスポート](sql-database-export.md)」の手順に従って作成できます。

> [!NOTE]
> Azure SQL Database では、復元できるすべてのユーザー データベースのバックアップが自動的に作成され、保守されます。 詳細については、「 [ビジネス継続性の概要](sql-database-business-continuity.md)」を参照してください。
> 
> 

.bacpac から SQL Database をインポートするには、以下が必要です。

* Azure サブスクリプション。 
* Azure SQL Database V12 サーバー。 V12 サーバーがない場合は、「 [最初の Azure SQL Database を作成する](sql-database-get-started.md)」という記事の手順に従って 1 つ作成してください。
* [Azure Storage アカウント (標準)](../storage/storage-create-storage-account.md) の BLOB コンテナーにインポートするデータベースの .bacpac ファイル。

> [!IMPORTANT]
> BACPAC を Azure Blob Storage からインポートする場合は、Standard Storage を使用します。 Premium Storage からの BACPAC のインポートはサポートされていません。
> 
> 

## <a name="select-the-server-to-host-the-database"></a>データベースをホストするサーバーの選択
次のように、[SQL Server] ブレードを開きます。

1. [Azure ポータル](https://portal.azure.com)にアクセスします。
2. **[SQL Server]**をクリックします。
3. データベースを復元するサーバーをクリックします。
4. [SQL Server] ブレードで、**[データベースのインポート]** をクリックして、**[データベースのインポート]** ブレードを開きます。
   
   ![[データベースのインポート]][1]
5. **[Storage]** をクリックし、ストレージ アカウント、BLOB コンテナー、および .bacpac ファイルを選択して、**[OK]** をクリックします。
   
   ![ストレージ オプションの構成][2]
6. 新しいデータベースの価格レベルを選択し、 **[選択]**をクリックします。 エラスティック プールへのデータベースの直接インポートはサポートされていませんが、最初に 1 つのデータベースにインポートしてからプールにデータベースを移動できます。
   
   ![価格レベルの選択][3]
7. **[データベース名]** に BACPAC ファイルから作成するデータベースの名前を入力します。
8. 認証の種類を選択し、サーバーの認証情報を指定します。 
9. **[作成]** をクリックして、BACPAC からデータベースを作成します。
   
   ![データベースの作成][4]

**[作成]** をクリックすると、サービスにデータベースのインポート要求が送信されます。 データベースのサイズに応じて、インポート操作の完了に時間がかかる場合があります。

## <a name="monitor-the-progress-of-the-import-operation"></a>インポート操作の進行状況の監視
1. **[SQL Server]**をクリックします。
2. 復元先のサーバーをクリックします。
3. [SQL Server] ブレードの [操作] 領域で、 **[インポート/エクスポート履歴]**をクリックします。
   
   ![インポート/エクスポート履歴][5]
   ![インポート/エクスポート履歴][6]

## <a name="verify-the-database-is-live-on-the-server"></a>サーバーにデータベースが存在することの確認
1. **[SQL Database]** をクリックし、新しいデータベースが**オンライン**であることを確認します。

## <a name="next-steps"></a>次のステップ
* インポートされた SQL データベースへの接続とクエリの実行については、「 [SQL Server Management Studio を使用して SQL Database に接続し、T-SQL サンプル クエリを実行する](sql-database-connect-query-ssms.md)

<!--Image references-->
[1]: ./media/sql-database-import/import-database.png
[2]: ./media/sql-database-import/storage-options.png
[3]: ./media/sql-database-import/pricing-tier.png
[4]: ./media/sql-database-import/create.png
[5]: ./media/sql-database-import/import-history.png
[6]: ./media/sql-database-import/import-status.png



<!--HONumber=Nov16_HO3-->


