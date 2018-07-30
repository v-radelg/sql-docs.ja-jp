---
title: テーブル値パラメーターのデータ変換およびその他のエラーと警告 |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology: native-client
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), data conversion
- table-valued parameters (ODBC), error messages
ms.assetid: edd45234-59dc-4338-94fc-330e820cc248
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cf8506311d0d95ac9d0f8eedc7632379b6bd9ce0
ms.sourcegitcommit: f8ce92a2f935616339965d140e00298b1f8355d7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37431871"
---
# <a name="table-valued-parameter-data-conversion-and-other-errors-and-warnings"></a>テーブル値パラメーターのデータ変換およびその他のエラーと警告
  テーブル値パラメーターの列の値は、他の列やパラメーターの値と同じ方法で、クライアントのデータ型とサーバーのデータ型間で変換できます。 ただし、テーブル値パラメーターには複数の列と複数の行を含めることができるため、エラーが発生した箇所の実際の値を識別できることが重要です。  
  
 テーブル値パラメーターの列でエラーまたは警告が検出された場合、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client によって診断レコードが生成されます。 エラー メッセージには、テーブル値パラメーターのパラメーター番号に加え、列序数と行番号が含まれます。 アプリケーションでは、診断レコード内の SQL_DIAG_SS_TABLE_COLUMN_NUMBER および SQL_DIAG_SS_TABLE_ROW_NUMBER という診断フィールドを使用して、エラーと警告に関連付けられている値を特定することもできます。 これらの診断フィールドは、[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以降のバージョンで使用できます。  
  
 診断レコードの SQLSTATE コンポーネントとメッセージ コンポーネントは、他のすべての点で既存の ODBC の動作に準拠しています。 つまり、パラメーター、行、列の識別情報以外は、テーブル値パラメーター以外のパラメーターの場合と同様に、テーブル値パラメーターの値がエラー メッセージに含まれます。  
  
## <a name="see-also"></a>参照  
 [テーブル値パラメーター &#40;ODBC&#41;](table-valued-parameters-odbc.md)  
  
  