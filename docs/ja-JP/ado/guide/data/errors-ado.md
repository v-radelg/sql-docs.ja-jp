---
title: エラー (ADO) |Microsoft ドキュメント
ms.prod: sql
ms.prod_service: drivers
ms.service: ''
ms.component: ado
ms.technology:
- drivers
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 8ae6611b-3069-4155-b014-c0c9da37be39
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d23582c5700f5d528f0b16f45e43aba01ec7f405
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="errors-ado"></a>エラー (ADO)
ADO オブジェクトに関連するすべての操作は、1 つまたは複数のプロバイダー エラーを生成できます。 各エラーが発生すると、1 つまたは複数**エラー**でオブジェクトを配置している、**エラー**のコレクション、**接続**オブジェクト。 ADO アプリケーションで警告およびエラーの処理の詳細については、「 [Error Handling](../../../ado/guide/data/error-handling.md)です。  
  
 別のメカニズムによって、アプリケーション エラーが発生することができます。 たとえば、Visual Basic の場合で、 **Err**アプリケーション レベルのエラーをオブジェクトに含まれます。