---
title: getFetchDirection メソッド (SQLServerResultSet) |Microsoft ドキュメント
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: drivers
ms.service: ''
ms.component: jdbc
ms.reviewer: ''
ms.suite: sql
ms.technology:
- drivers
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLServerResultSet.getFetchDirection
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5ab385c2-e18c-4b75-ac2d-2402af5c52a5
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 953b198a51d0b9def12e8d9df81920f424641fca
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="getfetchdirection-method-sqlserverresultset"></a>getFetchDirection メソッド (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  このフェッチ方向を取得[SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md)オブジェクト。  
  
## <a name="syntax"></a>構文  
  
```  
  
public int getFetchDirection()  
```  
  
## <a name="return-value"></a>戻り値  
 **Int**現在のフェッチ方向を示すです。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>解説  
 この getFetchDirection メソッドは、java.sql.ResultSet インターフェイスの getFetchDirection メソッドによって指定されます。  
  
 このメソッドは、順方向専用カーソルへの呼び出しによって行われた最後の設定の FETCH_FORWARD を返します、 [setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverresultset.md)他のカーソルの種類では、メソッドは FETCH_UNKNOWN を返しますこれらの種類のカーソル場合および setFetchDirection メソッド呼び出されたことはありません。  
  
## <a name="see-also"></a>参照  
 [SQLServerResultSet のメンバー](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet クラス](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  