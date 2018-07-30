---
title: データベースの処理、テーブル、またはパーティション |Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- analysis-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- SQL12.ASVS.SSMS.PARTITIONS.PROCESSINGOPTIONS.IMBI.F1
ms.assetid: 307d69c3-cabb-4dfa-b90c-9852492c1213
caps.latest.revision: 6
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: e8e6bb8c09154c7136d53dbf948c0baa199077f4
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37229712"
---
# <a name="process-database-table-or-partition"></a>データベース、テーブル、またはパーティションの処理
  このトピックのタスクを使用してテーブル モデル データベース、テーブル、またはパーティションを手動で処理する方法を説明する、**プロセス\<オブジェクト >**  ダイアログ ボックスで[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]します。  
  
 テーブル モデルの処理の詳細については、[「データの処理 (SSAS テーブル)」](../process-data-ssas-tabular.md) を参照してください。  
  
##  <a name="bkmk_process_tasks"></a> 処理手順  
  
###  <a name="bkmk_process_db"></a> データベースを処理するには  
  
1.  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]で処理対象のデータベースを右クリックし、 **[データベースの処理]** をクリックします。  
  
2.  **[データベースの処理]** ダイアログ ボックスの **[モード]** ボックスの一覧で、次のプロセス モードのいずれかを選択します。  
  
    |モード|説明|  
    |----------|-----------------|  
    |**既定の処理**|データベース オブジェクトの処理状態を検出し、処理されていないオブジェクトや部分的に処理されたオブジェクトを完全処理状態にするために必要な処理を行います。 空のテーブルとパーティションのデータが読み込まれ、階層、計算列、およびリレーションシップが構築または再構築 (再計算) されます。|  
    |**完全処理**|データベースとそのデータベースに含まれるすべてのオブジェクトを処理します。 既に処理されたオブジェクトに対して完全処理を実行すると、そのオブジェクト内のすべてのデータが [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] によって削除されてから、オブジェクトが処理されます。 この種の処理は、構造上の変更をオブジェクトに加えた場合に必要となります。 このオプションは最も多くのリソースを必要とします。|  
    |**消去の処理**|データベース オブジェクトからすべてのデータを削除します。|  
    |**再計算の処理**|階層、リレーションシップ、および計算列を更新して再計算します。|  
  
3.  **[処理]** チェックボックス列で、選択したモードで処理するパーティションを選択し、 **[OK]** をクリックします。  
  
###  <a name="bkmk_process_table"></a> テーブルを処理するには  
  
1.  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]で、処理対象のテーブルを含むテーブル モデル データベースの **[テーブル]** ノードを展開し、処理対象のテーブルを右クリックしてから **[テーブルの処理]** をクリックします。  
  
2.  **[テーブルの処理]** ダイアログ ボックスの **[モード]** ボックスの一覧で、次のプロセス モードのいずれかを選択します。  
  
    |モード|説明|  
    |----------|-----------------|  
    |**既定の処理**|テーブル オブジェクトの処理状態を検出し、処理されていないオブジェクトや部分的に処理されたオブジェクトを完全処理状態にするために必要な処理を行います。 空のテーブルとパーティションのデータが読み込まれ、階層、計算列、およびリレーションシップが構築または再構築 (再計算) されます。|  
    |**完全処理**|テーブル オブジェクトとそこに含まれているすべてのオブジェクトを処理します。 既に処理されたオブジェクトに対して完全処理を実行すると、そのオブジェクト内のすべてのデータが [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] によって削除されてから、オブジェクトが処理されます。 この種の処理は、構造上の変更をオブジェクトに加えた場合に必要となります。 このオプションは最も多くのリソースを必要とします。|  
    |**データの処理**|階層またはリレーションシップを再構築したり、計算列とメジャーを再計算したりせずに、テーブルにデータを読み込みます。|  
    |**消去の処理**|テーブルおよびテーブル パーティションからすべてのデータを削除します。|  
    |**デフラグの処理**|補助テーブルのインデックスをデフラグします。|  
  
3.  テーブルのチェックボックス列でテーブルを確認し、必要に応じて処理対象の追加のテーブルを選択し、 **[OK]** をクリックします。  
  
###  <a name="bkmk_process_partition"></a> 1 つ以上のパーティションを処理するには  
  
1.  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]で、処理対象のパーティションが存在するテーブルを右クリックして **[パーティション]** をクリックします。  
  
2.  **[パーティション]** ダイアログ ボックスの **[パーティション]** で、[処理] ボタンをクリックします。  
  
3.  **[パーティションの処理]** ダイアログ ボックスの **[モード]** ボックスの一覧で、次のプロセス モードのいずれかを選択します。  
  
    |モード|説明|  
    |----------|-----------------|  
    |**既定の処理**|パーティション オブジェクトの処理状態を検出して、未処理または部分的に処理されたパーティション オブジェクトを完全に処理された状態にするために必要な処理を実行します。 空のテーブルとパーティションのデータが読み込まれ、階層、計算列、およびリレーションシップが構築または再構築 (再計算) されます。|  
    |**完全処理**|パーティション オブジェクトとそこに含まれているすべてのオブジェクトを処理します。 既に処理されたオブジェクトに対して完全処理を実行すると、そのオブジェクト内のすべてのデータが [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] によって削除されてから、オブジェクトが処理されます。 この種の処理は、構造上の変更をオブジェクトに加えた場合に必要となります。|  
    |**データの処理**|階層またはリレーションシップを再構築したり、計算列とメジャーを再計算したりせずに、パーティションまたはテーブルにデータを読み込みます。|  
    |**消去の処理**|パーティションからすべてのデータを削除します。|  
    |**追加の処理**|パーティションを新しいデータで増分更新します。|  
  
4.  **[処理]** チェックボックス列で、選択したモードで処理するパーティションを選択し、 **[OK]** をクリックします。  
  
## <a name="see-also"></a>参照  
 [テーブル モデル パーティション&#40;SSAS 表形式&#41;](tabular-model-partitions-ssas-tabular.md)   
 [テーブル モデル パーティション作成し、管理&#40;SSAS 表形式&#41;](create-and-manage-tabular-model-partitions-ssas-tabular.md)  
  
  