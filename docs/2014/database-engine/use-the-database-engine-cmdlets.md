---
title: データベース エンジン コマンドレットの使用 | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Cmdlets [SQL Server], Encode-Sqlname
- Encode-Sqlname cmdlet
- PowerShell [SQL Server], Encode-Sqlname
- Convert-UrnToPath cmdlet
- PowerShell [SQL Server], cmdlets
- Cmdlets [SQL Server]
- PowerShell [SQL Server], Convert-UrnToPath
- Cmdlets [SQL Server], Convert-UrnToPath
- Decode-Sqlname cmdlet
- PowerShell [SQL Server], Decode-Sqlname
- Cmdlets [SQL Server], Decode-Sqlname
ms.assetid: 720aa982-09ae-41a3-b603-a91004cfbe3e
caps.latest.revision: 24
author: mashamsft
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 9e06c46020f0e3e04e81b3f46fada587ab55f7b6
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37267128"
---
# <a name="use-the-database-engine-cmdlets"></a>データベース エンジン コマンドレットの使用
  Windows PowerShell コマンドレットは、単一の機能を実現するコマンドで、通常は **Get-Help** や **Set-MachineName** のように動詞と名詞を組み合わせた名前付け規則に従います。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 固有のコマンドレットは、Windows PowerShell 用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]プロバイダーによって提供されます。  
  
## <a name="database-engine-cmdlets"></a>データベース エンジンのコマンドレット  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] では、 [!INCLUDE[ssDE](../includes/ssde-md.md)]用の少数のコマンドレットが実装されます。 これらのコマンドレットは、主に新しい PowerShell スクリプトからの既存の Transact-SQL スクリプトの実行、ポリシー ベースの管理ポリシーの評価、および SQL Server プロバイダー パスでの SQL Server 識別子の指定の支援に使用されます。  
  
 Windows PowerShell スクリプトの多くは、SQL Server PowerShell プロバイダーおよび SQL Server 管理オブジェクト モデルを使用して、 [!INCLUDE[ssDE](../includes/ssde-md.md)] と共に動作します。 詳細については、「 [SQL Server PowerShell](../powershell/sql-server-powershell.md)」を参照してください。  
  
### <a name="get-cmdlet-help"></a>コマンドレットのヘルプの利用  
 Windows PowerShell 環境では、 **Get-Help** コマンドレットを使用して各コマンドレットのヘルプ情報を参照できます。 **Get-Help** を実行すると、構文、パラメーター定義、入力と出力の種類、コマンドレットで実行される処理の説明などの情報が返されます。 詳細については、「 [Get Help SQL Server PowerShell](../../2014/database-engine/get-help-sql-server-powershell.md)」を参照してください。  
  
### <a name="partial-parameter-names"></a>部分的なパラメーター名  
 コマンドレット パラメーターの完全な名前を指定する必要はありません。 そのコマンドレットでサポートされている他のパラメーターと区別するのに足りる名前の一部のみを指定するだけでかまいません。 たとえば、次の例は、 **Invoke-Sqlcmd -QueryTimeout** パラメーターを指定する 3 つの方法を示します。  
  
```  
Invoke-Sqlcmd -Query "SELECT @@VERSION;" -QueryTimeout 3  
Invoke-Sqlcmd -Query "SELECT @@VERSION;" -QueryTime 3  
Invoke-Sqlcmd -Query "SELECT @@VERSION;" -QueryT 3  
```  
  
## <a name="database-engine-cmdlet-tasks"></a>データベース エンジン コマンドレットのタスク  
  
|タスクの説明|トピック|  
|----------------------|-----------|  
|**Invoke-Sqlcmd** の使用による、 **または XQuery ステートメントを含む** sqlcmd [!INCLUDE[tsql](../includes/tsql-md.md)] スクリプトやコマンドの実行について説明します。 **sqlcmd** の入力を、文字列の入力パラメーターとして、または開くスクリプト ファイルの名前として受け取ることができます。|[Invoke-Sqlcmd コマンドレット](../../2014/database-engine/invoke-sqlcmd-cmdlet.md)|  
|**Invoke-PolicyEvaluation** の使用による、 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] オブジェクトの対象セットがポリシー ベースの管理ポリシーに定義されている条件に準拠しているかどうかの報告について説明します。 必要に応じて、このコマンドレットを使用して、ポリシー条件に準拠していない対象オブジェクト内の設定可能なオプションを再構成することができます。|[Invoke-PolicyEvaluation コマンドレット](../../2014/database-engine/invoke-policyevaluation-cmdlet.md)|  
|使用方法について説明`Encode-Sqlname`と`Decode-Sqlname`Windows PowerShell パスでサポートされていない文字が含まれている SQL Server 識別子の処理。|[SQL Server 識別子のエンコードとデコード](../powershell/encode-and-decode-sql-server-identifiers.md)|  
|使用方法について説明`Convert-UrnToPath`SQL Server 管理オブジェクト URN Uniform Resource Name () を同等の SQL Server プロバイダー パスに変換します。|[URN から SQL Server プロバイダー パスへの変換](../../2014/database-engine/convert-urns-to-sql-server-provider-paths.md)|  
  
## <a name="see-also"></a>参照  
 [SQL Server PowerShell Provider](../powershell/sql-server-powershell-provider.md)   
 [SQL Server PowerShell](../powershell/sql-server-powershell.md)   
 [AlwaysOn 可用性グループの PowerShell コマンドレットの概要&#40;SQL Server&#41;](availability-groups/windows/overview-of-powershell-cmdlets-for-always-on-availability-groups-sql-server.md)  
  
  