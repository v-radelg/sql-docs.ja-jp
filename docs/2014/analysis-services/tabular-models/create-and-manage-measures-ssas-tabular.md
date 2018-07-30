---
title: 作成およびメジャー (SSAS テーブル) の管理 |Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- analysis-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: edc1a4b2-96d3-4f34-bb70-6cacec79e819
caps.latest.revision: 17
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: 8e88968a2be7baf03f0c7702db87053e6e570d3a
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37200182"
---
# <a name="create-and-manage-measures-ssas-tabular"></a>メジャーを作成および管理する (SSAS テーブル)
  メジャーとは、レポートまたは Excel ピボットテーブル (またはピボットグラフ) で使用するために作成される数式のことです。 COUNT や SUM などの標準の集計関数に基づいてメジャーを作成することも、DAX を使用して独自の数式を定義することもできます。 このトピックのタスクでは、テーブルのメジャー グリッドを使用してメジャーを作成し管理する方法について説明します。  
  
 このトピックでは、次のタスクについて説明します。  
  
-   [標準の集計式を使用してメジャーを作成するには](#bkmk_create_stand)  
  
-   [カスタム式を使用してメジャーを作成するには](#bkmk_create_custom)  
  
-   [メジャー プロパティを編集するには](#bkmk_edit)  
  
-   [メジャー名を変更するには](#bkmk_rename)  
  
-   [メジャーを削除するには](#bkmk_delete)  
  
## <a name="tasks"></a>処理手順  
 メジャーを作成、管理するには、テーブルのメジャー グリッドを使用します。 テーブルのメジャー グリッドは、モデル デザイナーのデータ ビューのみで表示することができます。 ダイアグラム ビューでは、メジャーの作成もメジャー グリッドの表示もできません。ただし、既存のメジャーであれば、ダイアグラム ビューで表示できます。 あるテーブルのメジャー グリッドを表示するには、 **[テーブル]** メニューをクリックしてから **[メジャー グリッドの表示]** をクリックします。  
  
###  <a name="bkmk_create_stand"></a> 標準の集計式を使用してメジャーを作成するには  
  
-   メジャーを作成する列をクリックし、 **[列]** メニューをクリックしてから **[オート SUM]** をポイントし、集計の種類をクリックします。  
  
     既定名を持つメジャー、そしてその後に数式が列の直下にあるメジャー グリッドの最初のセルに自動的に作成されます。  
  
###  <a name="bkmk_create_custom"></a> カスタム式を使用してメジャーを作成するには  
  
-   メジャー グリッド内のメジャーを作成する列の下で、任意のセルをクリックしてから、数式バーに名前、コロン (:)、等号 (=)、および数式を続けて入力します。 Enter キーを押して数式を確定します。  
  
###  <a name="bkmk_edit"></a> メジャー プロパティを編集するには  
  
-   メジャー グリッド内で、メジャーをクリックしてから **[プロパティ]** ウィンドウで別のプロパティ値を入力するか選択します。  
  
###  <a name="bkmk_rename"></a> メジャー名を変更するには  
  
-   メジャー グリッド内で、メジャーをクリックしてから **[プロパティ]** ウィンドウの **[メジャー名]** ボックスに新しい名前を入力し、Enter キーを押します。  
  
     数式バー内でメジャーの名前を変更することもできます。 メジャー名の後に数式を付け、その後にコロンを付けます。  
  
###  <a name="bkmk_delete"></a> メジャーを削除するには  
  
-   メジャー グリッドで、メジャーを右クリックしてから **[削除]** をクリックします。  
  
## <a name="see-also"></a>参照  
 [メジャー &#40;SSAS 表形式&#41;](measures-ssas-tabular.md)   
 [Kpi &#40;SSAS 表形式&#41;](kpis-ssas-tabular.md)   
 [計算列&#40;SSAS 表形式&#41;](ssas-calculated-columns.md)  
  
  