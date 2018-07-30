---
title: メモリ最適化テーブルが含まれるデータベースのバックアップ | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- database-engine-imoltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 83d47694-e56d-4dae-b54e-14945bf8ba31
caps.latest.revision: 14
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 38a2dbeed7220f0300015ba8741795603856f898
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37279398"
---
# <a name="backing-up-a-database-with-memory-optimized-tables"></a>メモリ最適化テーブルが含まれるデータベースのバックアップ
  メモリ最適化されたテーブルは、通常のデータベースのバックアップの一部としてバックアップされます。 ディスク ベース テーブルについては、ストレージの破損を検出するために、データベース バックアップの一部としてデータのチェックサムおよびデルタ ファイルのペアが検証されます。  
  
> [!NOTE]  
>  バックアップ中に、メモリ最適化ファイル グループ内の 1 つ以上のファイルでチェックサム エラーが検出された場合、データベースを復元および再起動できなくなります。 このような場合は、最新の既知の良好なバックアップからデータベースを復元します。 バックアップがない場合、データベースを削除して再作成した後に、メモリ最適化テーブルおよびディスク ベース テーブルからデータをエクスポートして再読み込みできます。  
  
 1 つ以上のメモリ最適化されたテーブルを持つデータベースの完全バックアップには、メモリ最適化されたテーブルごとに、ディスク ベース テーブルに対して割り当てられているストレージ (存在する場合)、アクティブなトランザクション ログ、およびデータ ファイルとデルタ ファイルのペア (チェックポイント ファイル ペアとも呼びます) が含まれています。 ただし、「 [メモリ最適化テーブルの持続性](memory-optimized-tables.md)」で説明されているように、メモリ最適化されたテーブルで使用されるストレージはメモリ内にあるときのサイズを大きく上回ることがあり、データベースのバックアップのサイズに影響します。  
  
## <a name="full-database-backup"></a>データベースの完全バックアップ  
 この説明では、持続性のあるメモリ最適化されたテーブルを持つデータベースに関するデータベース バックアップに注目します。ディスク ベース テーブルに関するバックアップも同じであるためです。 メモリ最適化されたファイル グループ内にあるチェックポイント ファイル ペアは、さまざまな状態をとることがあります。 ファイルのどの部分がバックアップされるかを、次の表で説明します。  
  
|チェックポイント ファイル ペアの状態|バックアップ|  
|--------------------------------|------------|  
|PRECREATED|ファイルのメタデータのみ|  
|UNDER CONSTRUCTION|ファイルのメタデータのみ|  
|Active|ファイルのメタデータと使用されているバイト|  
|Merge source|ファイルのメタデータと使用されているバイト|  
|Merge target|ファイルのメタデータのみ|  
|REQUIRED FOR BACKUP/HA|ファイルのメタデータと使用されているバイト|  
|IN TRANSITION TO TOMBSTONE|ファイルのメタデータのみ|  
|TOMBSTONE|ファイルのメタデータのみ|  
  
 データベースのバックアップと 1 つ以上のメモリ最適化テーブルのサイズはメモリ内のサイズより大きく、ディスク上ストレージよりも小さい。 追加のサイズは、削除された行の数と、ワークロードに間接的に依存する Merge source および REQUIRED FOR BACKUP/HA という状態にあるチェックポイント ファイル ペアの数によって異なります。 チェックポイント ファイル ペアの状態の説明については、次を参照してください。 [sys.dm_db_xtp_checkpoint_files &#40;TRANSACT-SQL&#41;](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql)します。  
  
### <a name="estimating-size-of-full-database-backup"></a>データベースの完全バックアップの推計サイズ  
  
> [!IMPORTANT]  
>  BackupSizeInBytes 値を使用してインメモリ OLTP のバックアップのサイズを見積もることはお勧めしません。  
  
 最初のワークロードのシナリオは、(ほとんどが) 挿入の場合です。 このシナリオでは、ほとんどのデータ ファイルは Active 状態で、完全読み込みであり、削除済みの行はごくわずかです。 データベースのバックアップのサイズは、メモリ内にあるデータのサイズに近づきます。  
  
 2 番目のワークロードのシナリオは、INSERT、DELETE、UPDATE の各操作が頻繁に実行される場合です。最悪のケースでは、削除された行を考慮に入れた後、各チェックポイント ファイル ペアは 50% が読み込み済みになります。 したがって、データベースのバックアップのサイズは、メモリ内にあるデータのサイズの少なくとも 2 倍になります。 また、データベースのバックアップのサイズを大きくする Merge source および Required for backup/high availability という状態にあるチェックポイント ファイル ペアはわずかです。  
  
## <a name="differential-backups-of-databases-with-memory-optimized-tables"></a>メモリ最適化テーブルを含むデータベースの差分バックアップ  
 メモリ最適化テーブルのストレージは、「 [メモリ最適化テーブルの持続性](memory-optimized-tables.md)」で説明しているようにデータ ファイルとデルタ ファイルで構成されます。 メモリ最適化テーブルが含まれるデータベースの差分バックアップには、次のデータが含まれています。  
  
-   ディスク ベース テーブルを格納するファイル グループの差分バックアップは、メモリ最適化テーブルの存在による影響を受けません。  
  
-   アクティブなトランザクション ログは、データベースの完全バックアップの場合と同じです。  
  
-   メモリ最適化データ ファイル グループの差分バックアップでは、データベースの完全バックアップと同じアルゴリズムを使用して、バックアップのデータ ファイルとデルタ ファイルを識別しますが、その後、次のようにファイルのサブセットを除外します。  
  
    -   データ ファイルには新たに挿入された行が含まれ、ファイルがいっぱいになるとファイルが閉じられ、読み取り専用としてマークされます。 データ ファイルは、最後のデータベースの完全バックアップの後で閉じられた場合にのみバックアップされます。 差分バックアップでは、挿入された行が含まれるデータ ファイルだけがバックアップされますが、挿入された行の一部が既にガベージ コレクション向けにマークされているか、ガベージ コレクションに収集された可能性がある更新および削除のシナリオは除きます。  
  
    -   デルタ ファイルには、削除されたデータ行への参照が格納されます。 将来のトランザクションでは行を削除できるため、デルタ ファイルは存続期間中のいつでも変更することができ、閉じられません。 デルタ ファイルは常にバックアップされます。 通常、デルタ ファイルはストレージの 10% 未満を使用するため、差分バックアップのサイズにほとんど影響を与えません。  
  
 データベース サイズの大部分がメモリ最適化テーブルである場合は、差分バックアップによってデータベース バックアップのサイズを大幅に削減することができます。  
  
 一般的な OLTP ワークロードの場合、差分バックアップは完全バックアップより非常に小さくなります。  
  
## <a name="see-also"></a>参照  
 [メモリ最適化テーブルのバックアップ、復元、復旧](restore-and-recovery-of-memory-optimized-tables.md)  
  
  