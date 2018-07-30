---
title: Windows アプリケーション ログへのジョブ状態の書き込み | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- dbe-cross-instance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- status information [SQL Server], jobs
- SQL Server Agent jobs, status
- writing job status to log
- jobs [SQL Server Agent], status
- logs [SQL Server], jobs
ms.assetid: 3b813702-8f61-40ec-bf3b-ce9deb7e68be
caps.latest.revision: 27
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 944c75875c138d8dae6c6b17af827462e2d8060a
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37236102"
---
# <a name="write-the-job-status-to-the-windows-application-log"></a>Windows アプリケーション ログへのジョブ状態の書き込み
  このトピックでは、[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、[!INCLUDE[tsql](../../includes/tsql-md.md)]、または SQL Server 管理オブジェクトを使用して、[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] でジョブの状態を Windows アプリケーションのイベント ログに書き込むように [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] エージェントを構成する方法について説明します。  
  
 ジョブ応答により、データベース管理者はジョブの完了日時や実行頻度を確認できます。 以下に、一般的なジョブ応答を示します。  
  
-   電子メール、ポケットベル、または **net send** メッセージによってオペレーターに通知します。 オペレーターが事後の作業を実行する必要がある場合に、これらのジョブ応答のいずれかを使用します。 たとえば、バックアップ ジョブが正常に終了した場合、オペレーターは、バックアップ テープを取り出して、安全な場所に保管するように通知を受ける必要があります。  
  
-   Windows アプリケーション ログにイベント メッセージを書き込みます。 この応答は、失敗したジョブに対してのみ使用できます。  
  
-   ジョブを自動的に削除します。 このジョブを再実行する必要がないことが明らかな場合に、このジョブ応答を使用します。  
  
 **このトピックの内容**  
  
-   **作業を開始する準備:**  
  
     [Security](#Security)  
  
-   **ジョブの状態を Windows アプリケーション ログに書き込む方法:**  
  
     [SQL Server Management Studio](#SSMS)  
  
     [SQL Server 管理オブジェクト](#SMO)  
  
##  <a name="BeforeYouBegin"></a> 作業を開始する準備  
  
###  <a name="Security"></a> セキュリティ  
 詳細については、「 [Implement SQL Server Agent Security](implement-sql-server-agent-security.md)」をご覧ください。  
  
##  <a name="SSMS"></a> SQL Server Management Studio の使用  
  
#### <a name="to-write-job-status-to-the-windows-application-log"></a>ジョブの状態を Windows アプリケーション ログに書き込むには  
  
1.  **オブジェクト エクスプローラー** で、 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]のインスタンスに接続し、そのインスタンスを展開します。  
  
2.  **[SQL Server エージェント]** を展開し、 **[ジョブ]** を展開します。次に、編集するジョブを右クリックし、 **[プロパティ]** をクリックします。  
  
3.  **[通知]** ページをクリックします。  
  
4.  **[Windows アプリケーション イベント ログに書き込む]** チェック ボックスをオンにし、次のいずれかを選択します。  
  
    -   ジョブが正常終了したときにジョブの状態をログに記録する場合は、**[ジョブ成功時]** をクリックします。  
  
    -   ジョブが正常終了したときにジョブの状態をログに記録する場合は、**[ジョブ失敗時]** をクリックします。  
  
    -   ジョブが正常終了したときにジョブの状態をログに記録する場合は、**[ジョブ完了時]** をクリックします。  
  
##  <a name="SMO"></a> SQL Server 管理オブジェクトの使用  
 **ジョブの状態を Windows アプリケーション ログに書き込むには**  
  
 Visual Basic、Visual C#、PowerShell などのプログラミング言語で `EventLogLevel` クラスの `Job` プロパティを呼び出します。  
  
 次のコード例では、ジョブの実行の終了時に、オペレーティング システムのイベント ログ エントリを生成するようにジョブを設定します。  
  
 **PowerShell**  
  
```  
$srv = new-object Microsoft.SqlServer.Management.Smo.Server("(local)")  
$jb = new-object Microsoft.SqlServer.Management.Smo.Agent.Job($srv.JobServer, "Test Job")  
$jb.EventLogLevel = [Microsoft.SqlServer.Management.Smo.Agent.CompletionAction]::Always  
```  
  
  