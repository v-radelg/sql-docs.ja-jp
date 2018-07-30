---
title: データベース メール アカウントの作成 | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Database Mail [SQL Server], accounts
- accounts [Database Mail]
ms.assetid: c07abbc6-fc6a-470b-8fa3-532f2e06b16a
caps.latest.revision: 25
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 91934cd120a48f05ba73dbddf2433af4d1a6cd18
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37172913"
---
# <a name="create-a-database-mail-account"></a>データベース メール アカウントの作成
  データベース メール アカウントの作成には、 **データベース メール構成ウィザード** または [!INCLUDE[tsql](../../includes/tsql-md.md)] を使用します。  
  
-   **作業を開始する準備:**  [前提条件](#Prerequisites)  
  
-   **データベース メール アカウントの作成:**  [データベース メール構成ウィザード](#SSMSProcedure)、 [Transact-SQL の使用](#TsqlProcedure)  
  
-   **補足情報:**  [データベース メールを構成する次の手順](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> 作業を開始する準備  
  
###  <a name="Prerequisites"></a> 前提条件  
  
-   電子メールの送信に使用する SMTP (簡易メール転送プロトコル) サーバーの名前とポート番号を特定します。SMTP サーバーが認証を必要とする場合は、その SMTP サーバー用のユーザー名とパスワードを特定します。  
  
-   サーバーの種類とそのサーバー用のポート番号を指定できます。この指定は省略できます。 送信メール用のサーバーの種類は、常に 'SMTP' になります。 ほとんどの SMTP サーバーは、既定のポート 25 を使用しています。  
  
##  <a name="SSMSProcedure"></a> データベース メール構成ウィザードの使用  
 **ウィザードを使用して、データベース メール アカウントを作成するには**  
  
-   オブジェクト エクスプローラーで、データベース メールを構成する [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] インスタンスに接続し、サーバー ツリーを展開します。  
  
-   **[管理]** ノードを展開します。  
  
-   [データベース メール] をダブルクリックして、データベース メール構成ウィザードを開きます。  
  
-   **[構成タスクの選択]** ページで、 **[データベース メール アカウントおよびプロファイルを管理する]** を選択して、 **[次へ]** をクリックします。  
  
-   **[プロファイルとアカウントの管理]** ページで、 **[新しいアカウントを作成する]** を選択し、 **[次へ]** をクリックします。  
  
-   **[新しいアカウント]** ページで、アカウント名、説明、メール サーバー情報、および認証の種類を指定します。 **[次へ]** をクリックします。  
  
-   **[ウィザードの完了]** ページで、実行される動作を確認し、 **[完了]** をクリックして、新しいアカウントの作成を完了します。  
  
##  <a name="TsqlProcedure"></a> Transact-SQL の使用  
 **Transact-SQL を使用して、データベース メール アカウントを作成するには**  
  
 **msdb.dbo.sysmail_add_account_sp** ストアド プロシージャを実行してアカウントを作成し、次の情報を指定します。  
  
-   作成するアカウントの名前。  
  
-   省略可能なアカウントの説明。  
  
-   送信する電子メール メッセージに表示する電子メール アドレス。  
  
-   送信する電子メール メッセージに表示する表示名。  
  
-   SMTP サーバーのサーバー名。  
  
-   SMTP サーバーが認証を必要とする場合、SMTP サーバーへのログオンに使用するユーザー名。  
  
-   SMTP サーバーが認証を必要とする場合、SMTP サーバーへのログオンに使用するパスワード。  
  
 次の例では、新しいデータベース メール アカウントを作成します。  
  
```  
EXECUTE msdb.dbo.sysmail_add_account_sp  
    @account_name = 'AdventureWorks Administrator',  
    @description = 'Mail account for administrative e-mail.',  
    @email_address = 'dba@Adventure-Works.com',  
    @display_name = 'AdventureWorks Automated Mailer',  
    @mailserver_name = 'smtp.Adventure-Works.com' ;  
```  
  
##  <a name="FollowUp"></a> 補足情報: データベース メールを構成する次の手順  
  
-   [データベース メール プロファイルの作成](create-a-database-mail-profile.md)  
  
  