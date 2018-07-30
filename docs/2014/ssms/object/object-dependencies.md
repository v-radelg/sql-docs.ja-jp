---
title: '[オブジェクトの依存関係] | Microsoft Docs'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- dbe-cross-instance
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql12.swb.common.objectdependencies.f1
ms.assetid: c63d1160-3f3d-45df-99be-6fe081125fb5
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: f360b9553c754c5e6fa9ee2bdd5e39d73f9194b9
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37329012"
---
# <a name="object-dependencies"></a>[オブジェクトの依存関係]
  一部のデータベース オブジェクトには、他のデータベース オブジェクトと依存関係があります。 たとえば、ビューとストアド プロシージャは、そのビューまたはプロシージャが返すデータを格納するテーブルの存在に依存します。 現在のオブジェクトの **[オブジェクトの依存関係]\([全般] ページ)** には、オブジェクトが正常に動作するために必要なデータベース オブジェクト、および選択されているオブジェクトに依存するオブジェクトの両方が一覧表示されます。 オブジェクトは、その定義で別のオブジェクトを参照する場合 (この定義はシステム カタログに格納されます)、 *参照元エンティティ*と呼ばれます。 別のオブジェクトによって参照されるオブジェクトは、 *参照先エンティティ*と呼ばれます。  
  
 現在のオブジェクトの **[オブジェクトの依存関係]\([詳細設定] ページ)** には、オブジェクトに依存する [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] データベース オブジェクトおよび [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] オブジェクトが一覧表示されます。 それらのオブジェクトは、別のサーバーに格納されている場合もあります。  
  
 このダイアログ ボックスを使用すると、選択されているオブジェクトを変更または削除する前に依存関係について理解できます。  
  
## <a name="uielement-list"></a>UI 要素の一覧  
 **オブジェクトに依存する***\<選択したオブジェクト >  *  
 このボタンをクリックすると、依存関係が監視され、選択されているオブジェクトに依存するオブジェクトの一覧が表示されます。  
  
 **対象のオブジェクト***\<選択したオブジェクト >***依存    **  
 このボタンをクリックすると、依存関係が監視され、選択されているオブジェクトが依存するオブジェクトの一覧が表示されます。  
  
 **依存関係**  
 場合**に依存するオブジェクト** *\<選択したオブジェクト >* は選択したオブジェクトに依存するオブジェクトの階層ビューが表示されますをクリックします。 場合**オブジェクト** *\<選択したオブジェクト >* **依存**がクリックすると、これを表示、選択したオブジェクトが依存するオブジェクトの階層ビュー.  
  
 **名前**  
 上記の **[依存関係]** ツリー ビューで選択されたオブジェクトの名前が表示されます。  
  
 **型**  
 上記の **[依存関係]** ツリー ビューで選択されたオブジェクトの種類が表示されます。  
  
 **[最終同期時刻]**  
 > [!NOTE]  
>  このオプションは、 **[詳細設定]** ページのみで使用できます。  
  
 依存関係情報が最後に更新された日時を指定します。  
  
 **[依存関係の種類]**  
 > [!NOTE]  
>  このオプションは、 **[全般]** ページのみで使用できます。  
  
 2 つのオブジェクト間の依存関係の種類が表示されます。 次のいずれかになります。  
  
-   スキーマ バインド依存関係  
  
     参照元オブジェクトが存在する限り、参照先オブジェクトを削除または変更することができない 2 つのオブジェクト間のリレーションシップです。 スキーマ バインド依存関係は、WITH SCHEMABINDING 句を使用してビューまたはユーザー定義の関数を作成した場合か、テーブルが CHECK 制約または DEFAULT 制約を通じてあるいは計算列の定義で別のオブジェクトを参照している場合に作成されます。  
  
-   非スキーマ バインド依存関係  
  
     参照先オブジェクトを削除または変更できる 2 つのオブジェクト間のリレーションシップです。  
  
-   使用不可または未解決のエンティティ  
  
     依存関係の種類を判断できないことを示します。 これは、選択したオブジェクトが [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] よりも前 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]のインスタンスである場合にのみ発生します。  
  
  