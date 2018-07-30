---
title: dta ユーティリティ | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- physical design structures [SQL Server]
- command prompt utilities [SQL Server], dta
- dta utility
- tuning databases [SQL Server], Database Engine Tuning Advisor
- workloads [SQL Server], analyzing
- dta utility, about dta utility
- performance [SQL Server], Database Engine Tuning Advisor
- Database Engine Tuning Advisor [SQL Server], command prompt
- optimizing databases [SQL Server]
ms.assetid: a0b210ce-9b58-4709-80cb-9363b68a1f5a
caps.latest.revision: 52
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 5dcf0994c14496f32de3734d5456d462ad74fe74
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37200822"
---
# <a name="dta-utility"></a>dta ユーティリティ
  **dta** ユーティリティは、データベース エンジン チューニング アドバイザーのコマンド プロンプト バージョンです。 **dta** ユーティリティは、データベース エンジン チューニング アドバイザーの機能をアプリケーションとスクリプトで使用するために作成されました。  
  
 データベース エンジン チューニング アドバイザーと同様、 **dta** ユーティリティは、ワークロードを分析し、そのワークロードに対するサーバー パフォーマンスを向上するために推奨される物理デザイン構造を提示します。 ワークロードには、プラン キャッシュ、 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] のトレース ファイルやトレース テーブル、または [!INCLUDE[tsql](../../includes/tsql-md.md)] スクリプトを指定できます。 物理デザイン構造には、インデックス、インデックス付きビュー、およびパーティション分割が含まれます。 ワークロードの分析後、 **dta** ユーティリティは、データベースの物理デザインに対する推奨設定を作成します。また、その推奨設定を実装するために必要なスクリプトを生成することができます。 ワークロードは、 **-if** 引数または **-it** 引数を使用してコマンド プロンプトから指定できます。 **-ix** 引数を使用して、コマンド プロンプトから XML 入力ファイルを指定することもできます。 その場合、ワークロードは XML 入力ファイルで指定します。  
  
## <a name="syntax"></a>構文  
  
```  
  
      dta  
[ -? ] |  
[  
      [ -S server_name[ \instance ] ]  
      { { -U login_id [-P password ] } | –E  }  
      { -D database_name [ ,...n ] }  
      [ -ddatabase_name ]   
      [ -Tltable_list | -Tf table_list_file ]  
      { -if workload_file | -it workload_trace_table_name  |   
        -ip | -ipf }  
      { -ssession_name | -IDsession_ID }  
      [ -F ]  
      [ -of output_script_file_name ]  
      [ -oroutput_xml_report_file_name ]  
      [ -ox output_XML_file_name ]  
      [ -rl analysis_report_list [ ,...n ] ]  
      [ -ix input_XML_file_name ]  
      [ -A time_for_tuning_in_minutes ]  
      [ -nnumber_of_events ]  
      [ -m minimum_improvement ]  
      [ -fa physical_design_structures_to_add ]  
      [ -fi ]  
      [ -fppartitioning_strategy ]  
      [ -fk keep_existing_option ]  
      [ -fxdrop_only_mode ]  
      [ -B storage_size ]  
      [ -cmax_key_columns_in_index ]  
      [ -C max_columns_in_index ]  
      [ -e | -e tuning_log_name ]  
      [ -N online_option]  
      [ -q ]  
      [ -u ]  
      [ -x ]  
      [ -a ]  
]  
```  
  
## <a name="arguments"></a>引数  
 **-?**  
 使用方法についての情報を表示します。  
  
 **-A** *time_for_tuning_in_minutes*  
 チューニングの制限時間を分単位で指定します。 **dta** は、指定された時間を使用してワークロードをチューニングし、推奨される物理デザインに変更するスクリプトを生成します。 既定では、 **dta** は、チューニング時間を 8 時間と想定します。 0 を指定すると、チューニング時間を無制限になります。 **dta** では、制限時間に達する前にワークロード全体のチューニングを終了する場合があります。 ワークロード全体を確実にチューニングするためには、チューニング時間を無制限に指定 (-A 0) することをお勧めします。  
  
 **-a**  
 ワークロードをチューニングし、確認のプロンプトを表示せずに推奨設定を適用します。  
  
 **-B** *storage_size*  
 推奨されるインデックスとパーティション分割で使用できる最大容量を MB 単位で指定します。 複数のデータベースをチューニングする場合は、すべてのデータベースの推奨設定が容量計算の対象になります。 既定では、 **dta** は、次のストレージ サイズより小さいサイズを想定します。  
  
-   現在の生データ サイズの 3 倍。このサイズには、データベース内のテーブルのヒープとクラスター化インデックスの合計サイズが含まれます。  
  
-   アタッチ先のすべてのディスク ドライブの空き容量に生データのサイズを加算した値  
  
 既定の記憶領域サイズには、非クラスター化インデックスとインデックス付きビューは含まれません。  
  
 **-C** *max_columns_in_index*  
 **dta** が提示する、インデックス内に含まれる列の最大数を指定します。 最大値は 1024 です。 既定では、この引数は 16 に設定されています。  
  
 **-c** *max_key_columns_in_index*  
 **dta** が提示する、インデックス内に含まれるキー列の最大数を指定します。 既定値は 16 (許可される最大値) です。 **dta** では、付加列を使用するインデックスの作成も考慮されます。 付加列を使用するインデックス推奨設定は、この引数で指定される列数を超える場合があります。  
  
 **-D** *database_name*  
 チューニングする各データベースの名前を指定します。 最初のデータベースは既定のデータベースです。 データベース名をコンマで区切り、複数のデータベースを指定することができます。次に例を示します。  
  
```  
dta –D database_name1, database_name2...  
```  
  
 または、各データベースに **–D** 引数を使用することで、複数のデータベースを指定することもできます。次に例を示します。  
  
```  
dta –D database_name1 -D database_name2... n  
```  
  
 **-D** 引数は必須です。 **-d** 引数が指定されていない場合、 **dta** は、ワークロードの最初の `USE database_name` 句で指定されるデータベースに最初に接続します。 ワークロードに明示的な `USE database_name` 句がない場合、 **-d** 引数を使用する必要があります。  
  
 たとえば、明示的な `USE database_name` 句を含まないワークロードで、次のような **dta** コマンドを使用する場合、推奨設定は生成されません。  
  
```  
dta -D db_name1, db_name2...  
```  
  
 しかし、同じワークロードを使用して、 **-d** 引数を使用した次のような **dta** コマンドを使用すると、推奨設定が生成されます:  
  
```  
dta -D db_name1, db_name2 -d db_name1  
```  
  
 **-d** *database_name*  
 ワークロードをチューニングするときに、 **dta** が最初に接続するデータベースを指定します。 この引数で指定できるデータベースは 1 つだけです。 以下に例を示します。  
  
```  
dta -d AdventureWorks2012 ...  
```  
  
 複数のデータベース名を指定すると、 **dta** はエラーを返します。 **-d** 引数は省略可能です。  
  
 XML 入力ファイルを使用している場合を最初のデータベースを指定できます**dta**を使用して接続、`DatabaseToConnect`要素の下にある、`TuningOptions`要素。 詳細については、「 [Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)」を参照してください。  
  
 データベースを 1 つだけチューニングする場合、 **-d** 引数は **sqlcmd** ユーティリティの **-d** 引数と同様の機能を提供しますが、USE *database_name* ステートメントは実行しません。 詳細については、「 [sqlcmd Utility](../sqlcmd-utility.md)」を参照してください。  
  
 **-E**  
 パスワードを要求せずに、セキュリティ接続を使用します。 ログイン ID を指定する **-E** 引数または **-U** 引数のいずれかを使用する必要があります。  
  
 **-e** *tuning_log_name*  
 **dta** がチューニングできなかったイベントを記録するテーブル名またはファイル名を指定します。 このテーブルは、チューニングが行われるサーバー上に作成されます。  
  
 テーブルを使用する場合、 *[database_name].[owner_name].table_name*の形式で名前を指定します。 次の表は、各パラメーターの既定値を示しています。  
  
|パラメーター|既定値|  
|---------------|-------------------|  
|*database_name*|*–D* オプションで指定された **database_name**|  
|*owner_name*|**dbo**<br /><br /> 注: *owner_name*あります**dbo**します。 それ以外の値を指定すると、 **dta** の実行は失敗し、エラーが返されます。|  
|*table_name*|なし|  
  
 ファイルを使用する場合、拡張子として .xml を指定します。 たとえば、TuningLog.xml です。  
  
> [!NOTE]  
>  **dta** ユーティリティは、セッションを削除する際に、ユーザー指定のチューニング ログ テーブルの内容を削除しません。 大量のワークロードをチューニングするときは、テーブルをチューニング ログ専用にすることを推奨します。 大量のワークロードをチューニングすると、チューニング ログが大きくなり、テーブルが急速に消費され、セッションが削除される可能性があります。  
  
 **-F**  
 既存の出力ファイルの上書きを **dta** に許可します。 同じ名前の出力ファイルが既に存在し、 **-F** が指定されていない場合、 **dta**はエラーを返します。 **-F** と **-of**、 **-or**、または **-ox**を同時に使用することができます。  
  
 **-fa** *physical_design_structures_to_add*  
 **dta** が推奨設定に含む必要がある物理デザイン構造の種類を指定します。 次の表は、この引数で指定できる値の一覧と説明です。 値が指定されていないときに**dta**既定を使用して **-fa**`IDX`します。  
  
|値|説明|  
|-----------|-----------------|  
|IDX_IV|インデックスおよびインデックス付きビュー。|  
|IDX|インデックスのみ。|  
|IV|インデックス付きビューのみ。|  
|NCL_IDX|非クラスター化インデックスのみ。|  
  
 **-fi**  
 フィルター選択されたインデックスが新しい推奨設定用と見なされるように指定します。 詳細については、「 [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md)」を参照してください。  
  
 **-fk** *keep_existing_option*  
 推奨設定を生成する際に **dta** が保持する必要のある既存の物理デザイン構造を指定します。 次の表は、この引数で指定できる値の一覧と説明です。  
  
|値|説明|  
|-----------|-----------------|  
|なし|既存の構造なし|  
|ALL|既存のすべての構造|  
|ALIGNED|パーティションで固定された構造をすべて保持します。|  
|CL_IDX|テーブル上にあるクラスター化されたすべてのインデックス|  
|IDX|テーブル上にある、クラスター化されたすべてのインデックスおよびすべての非クラスター化インデックス|  
  
 **-fp** *partitioning_strategy*  
 **dta** によって提示される新しい物理デザイン構造 (インデックスおよびインデックス付きビュー) をパーティション分割する必要があるかどうか、およびパーティション分割の方法を指定します。 次の表は、この引数で指定できる値の一覧と説明です。  
  
|値|説明|  
|-----------|-----------------|  
|なし|パーティション分割しない。|  
|FULL|完全パーティション分割 (選択するとパフォーマンスが向上します)。|  
|ALIGNED|固定パーティション分割 (選択すると管理機能が強化されます)。|  
  
 ALIGNED は、 **dta** によって生成された推奨設定では、提示されるすべてのインデックスが、インデックス定義の基になるテーブルとまったく同じ方法でパーティション分割されることを意味します。 インデックス付きビューの非クラスター化インデックスは、インデックス付きビューに準じます。 この引数で指定できる値は 1 つだけです。 既定値は **-fp**`NONE`します。  
  
 **-fx** *drop_only_mode*  
 **dta** が、既存の物理デザイン構造の削除のみを考慮することを指定します。 新しい物理デザイン構造は考慮されません。 このオプションを指定すると、 **dta** は既存の物理デザイン構造の有用性を評価し、その後、使用頻度の低い構造を削除する推奨設定を提示します。 この引数は値を取りません。 この引数と **-fa**、 **-fp**、または **-fk ALL** 引数を同時に使用することはできません  
  
 **-ID** *session_ID*  
 チューニング セッションの識別子を数値で指定します。 指定しない場合、 **dta** は ID 番号を生成します。 この識別子を使用して、既存のチューニング セッションの情報を表示することができます。 **-ID**に値を指定しない場合は、セッション名を **-s**で指定する必要があります。  
  
 **-ip**  
 プラン キャッシュをワークロードとして使用することを指定します。 明示的に選択したデータベースの上位 1,000 個のプラン キャッシュ イベントが分析されます。 この値は **–n** オプションを使用して変更できます。  
  
 **ipf、**  
 プラン キャッシュをワークロードとして使用することを指定します。 すべてのデータベースの上位 1,000 個のプラン キャッシュ イベントが分析されます。 この値は **–n** オプションを使用して変更できます。  
  
 **-if** *workload_file*  
 チューニングの入力として使用するワークロード ファイルのパスとファイルの名前を指定します。 ファイルは、.trc (SQL Server Profiler トレース ファイル)、.sql (SQL ファイル)、.log ([!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] トレース ファイル) のいずれかの形式になっている必要があります。 ワークロード ファイル、またはワークロード テーブルを 1 つ指定する必要があります。  
  
 **-it** *workload_trace_table_name*  
 チューニングのワークロード トレースを含むテーブルの名前を指定します。 名前は [*database_name*]**.**[*owner_name*]**.***table_name* の形式で指定します。  
  
 次の表は、各パラメーターの既定値を示しています。  
  
|パラメーター|既定値|  
|---------------|-------------------|  
|*database_name*|*–D* オプションで指定された **database_name** 。|  
|*owner_name*|**dbo**。|  
|*table_name*|[なし] :|  
  
> [!NOTE]  
>  *owner_name* は必ず **dbo** としてください。 それ以外の値を指定すると、 **dta** の実行は失敗し、エラーが返されます。 ワークロード ファイルまたはワークロード テーブルを 1 つ指定する必要があることにも注意してください。  
  
 **-ix** *input_XML_file_name*  
 **dta** の入力情報を含む XML ファイルの名前を指定します。 これは、DTASchema.xsd に従った有効な XML ドキュメントであることが必要です。 チューニング オプションのコマンド プロンプトから指定した引数と競合する場合、この XML ファイルの対応する値がオーバーライドされます。 唯一の例外は、ユーザー定義の構成が XML 入力ファイルに評価モードで入力されている場合だけです。 たとえば、XML 入力ファイルの **Configuration** 要素に構成が入力されており、**EvaluateConfiguration** 要素も同様にチューニング オプションの 1 つとして指定されている場合、XML 入力ファイルで指定されたチューニング オプションは、コマンド プロンプトから入力されるいずれのチューニング オプションをオーバーライドします。  
  
 **-m** *minimum_improvement*  
 推奨構成が満たす必要がある、最小向上率を指定します。  
  
 **-N** *online_option*  
 物理デザイン構造をオンラインで作成するかどうかを指定します。 次の表は、この引数で指定できる値の一覧と説明です。  
  
|値|説明|  
|-----------|-----------------|  
|OFF|推奨される物理デザイン構造をオンラインで作成しません。|  
|ON|推奨される物理デザイン構造をすべてオンラインで作成します。|  
|MIXED|データベース エンジン チューニング アドバイザーは、可能な場合にオンラインで作成できる物理デザイン構造を推奨します。|  
  
 インデックスがオンラインで作成される場合、このオブジェクト定義には ONLINE = ON が追加されます。  
  
 **-n** *number_of_events*  
 **dta** がチューニングする必要があるワークロード内のイベント数を指定します。 この引数が指定され、ワークロードが実行時間の情報を含むトレース ファイルの場合、 **dta** は、実行時間の長いものから順にイベントをチューニングします。 この引数は、物理デザイン構造の 2 つの構成を比較する場合に利用できます。 2 つの構成を比較するには、両方の構成でチューニングする同じイベント数を指定し、次に示すように両方のチューニング時間を無制限に指定します。  
  
```  
dta -n number_of_events -A 0  
```  
  
 この場合、チューニング時間を無制限 (`-A 0`) に指定することが重要です。 無制限に指定しない場合、データベース エンジン チューニング アドバイザーでは、既定の 8 時間のチューニング時間が前提となります。  
  
 **-of** *output_script_file_name*  
 **dta** が、指定したファイル名と出力先に推奨設定を [!INCLUDE[tsql](../../includes/tsql-md.md)] スクリプトとして書き込むことを指定します。  
  
 このオプションと **-F** を同時に使用することができます。 特に **-or** および **-ox**も使用している場合は、ファイル名が一意であることを確認してください。  
  
 **-or** *output_xml_report_file_name*  
 **dta** が、XML 形式の出力レポートに推奨設定を書き込むことを指定します。 ファイル名を指定した場合、推奨設定はその出力先へ書き込まれます。 ファイル名を指定しない場合、 **dta** はセッション名を使用してファイル名を生成し、それを現在のディレクトリに書き込みます。  
  
 このオプションと **-F** を同時に使用することができます。 特に **-of** および **-ox**も使用している場合は、ファイル名が一意であることを確認してください。  
  
 **-ox** *output_XML_file_name*  
 **dta** が、指定したファイル名と出力先に推奨設定を XML ファイルとして書き込むことを指定します。 データベース エンジン チューニング アドバイザーには、宛先ディレクトリへの書き込み権限があることを確認してください。  
  
 このオプションと **-F** を同時に使用することができます。 特に **-of** および **-or**も使用している場合は、ファイル名が一意であることを確認してください。  
  
 **-P** *password*  
 ログイン ID のパスワードを指定します。 このオプションを使用しない場合、 **dta** はパスワードの入力を要求します。  
  
 **-q**  
 非表示モードを設定します。 進行状況やヘッダー情報を含め、コンソールには情報が一切表示されません。  
  
 **-rl** *analysis_report_list*  
 生成する分析レポートのリストを指定します。 次の表は、この引数で指定できる値の一覧を示しています。  
  
|値|レポート|  
|-----------|------------|  
|ALL|すべての分析レポート|  
|STMT_COST|ステートメント コスト レポート|  
|EVT_FREQ|イベント頻度レポート|  
|STMT_DET|ステートメントの詳細レポート|  
|CUR_STMT_IDX|ステートメントとインデックスのリレーション レポート (現在の構成)|  
|REC_STMT_IDX|ステートメントとインデックスのリレーション レポート (推奨構成)|  
|STMT_COSTRANGE|ステートメント コスト範囲レポート|  
|CUR_IDX_USAGE|インデックス使用状況レポート (現在の構成)|  
|REC_IDX_USAGE|インデックス使用状況レポート (推奨構成)|  
|CUR_IDX_DET|インデックスの詳細レポート (現在の構成)|  
|REC_IDX_DET|インデックスの詳細レポート (推奨構成)|  
|VIW_TAB|ビューとテーブルのリレーション レポート|  
|WKLD_ANL|ワークロード分析レポート|  
|DB_ACCESS|データベース アクセス レポート|  
|TAB_ACCESS|テーブルのアクセス レポート|  
|COL_ACCESS|列のアクセス レポート|  
  
 値をコンマで区切って複数のレポートを指定します。次に例を示します。  
  
```  
... -rl EVT_FREQ, VIW_TAB, WKLD_ANL ...  
```  
  
 **-S** *server_name*[ *\instance*]  
 接続先となる [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] のコンピューターとインスタンスの名前を指定します。 *server_name* を指定しない場合、 **dta** はローカル コンピューター上にある [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] の既定のインスタンスに接続します。 名前付きインスタンスに接続する場合、またはネットワーク上のリモート コンピューターから **dta** を実行する場合、このオプションは必須です。  
  
 **-s** *session_name*  
 チューニング セッションの名前を指定します。 **-ID** を指定しない場合、これは必須です。  
  
 **-Tf** *table_list_file*  
 チューニングするテーブルの一覧が含まれているファイルの名前を指定します。 このファイル内では、各テーブルをそれぞれ行を変えて指定します。 テーブル名は、 **AdventureWorks2012.HumanResources.Department**のように、3 つの部分から構成されます。 必要に応じてテーブル スケーリング機能を呼び出すには、既存のテーブル名の後に、テーブル内の行の予測数を示す数字を指定します。 データベース エンジン チューニング アドバイザーでは、これらのテーブルを参照するワークロード内にあるステートメントのチューニングや評価を行うときに、行の予測数を考慮します。 *number_of_rows* と *table_name*の間には 1 つ以上の空白がある場合があることに注意してください。  
  
 次に、 *table_list_file*ファイルのフォーマットを示します。  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 *database_name*.[*schema_name*].*table_name* [*number_of_rows*]  
  
 この引数は、コマンド プロンプトでテーブルの一覧 (**-Tl**) を入力する代わりに使用します。 **-Tl**を使用する場合は、テーブルの一覧ファイル ( **-Tf**) を使用しないでください。 両方の引数を使用すると、 **dta** は失敗し、エラーを返します。  
  
 **-Tf** 引数および **-Tl** 引数を省略した場合、指定されたデータベース内のすべてのユーザー テーブルをチューニングするものと見なされます。  
  
 **-Tl** *table_list*  
 コマンド プロンプトでチューニングするテーブルの一覧を指定します。 テーブル名の間にコンマを挿入して区切ります。 **-D** 引数で 1 つのデータベースのみを指定する場合、テーブル名をデータベース名で修飾する必要はありません。 それ以外の場合、各テーブルには *database_name.schema_name.table_name* の形式で、完全修飾名が必要となります。  
  
 この引数は、テーブルの一覧ファイル (**-Tf**) の代わりに使用します。 **-Tl** と **-Tf** の両方を使用すると、 **dta** は失敗し、エラーを返します。  
  
 **-U** *login_id*  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]への接続に使用されるログイン ID を指定します。  
  
 **-u**  
 データベース エンジン チューニング アドバイザー GUI を起動します。 すべてのパラメーターは、ユーザー インターフェイスの初期設定として扱われます。  
  
 **-x**  
 チューニング セッションを開始し、終了します。  
  
## <a name="remarks"></a>コメント  
 Ctrl + C キーを 1 度押すと、チューニング セッションが停止し、 **dta** がこの時点までに完了した分析に基づいて、推奨設定が生成されます。 推奨設定を生成するかどうかの確認が要求されます。 Ctrl</localizedText> + <localizedText>C</localizedText> キーをもう一度押すと、推奨設定を生成せずにチューニング セッションを停止します。  
  
## <a name="examples"></a>使用例  
 **A.推奨設定で、インデックスとインデックス付きビューを含んだワークロードをチューニングする**  
  
 次の例では、セキュリティで保護された接続 (`-E`) を使用して MyServer の **tpcd1G** データベースに接続し、ワークロードの分析と推奨設定の作成を行います。 出力結果は script.sql という名前のスクリプト ファイルへ書き込まれます。 script.sql が既に存在する場合は、 **引数が指定されているため、** dta `-F` はファイルを上書きします。 チューニング セッションは、ワークロードの分析を完全に終了するように時間制限なしで実行されます (`-A 0`)。 推奨設定の最小向上率は 5% を指定する必要があります (`-m 5`)。 **dta** では、最終的な推奨設定にインデックスおよびインデックス付きビューが含まれます (`-fa IDX_IV`)。  
  
```  
dta –S MyServer –E -D tpcd1G -if tpcd_22.sql -F –of script.sql –A 0 -m 5 -fa IDX_IV  
```  
  
 **B.ディスク使用量を制限する**  
  
 次の例では、生データと追加のインデックスを含むデータベースの合計サイズを 3 GB までに制限し (`-B 3000`)、出力先には d:\result_dir\script1.sql を指定しています。 実行は、1 時間以内です (`-A 60`)。  
  
```  
dta –D tpcd1G –if tpcd_22.sql -B 3000 –of "d:\result_dir\script1.sql" –A 60  
```  
  
 **C.チューニングするクエリの数を制限する**  
  
 次の例では、orders_wkld.sql ファイルから読み取るクエリの数を最大 10 に制限し (`-n 10`)、15 分間実行します (`-A 15`)。どちらを先に指定しても同じです。 10 個のクエリすべてをチューニングするために、`-A 0` を使用してチューニング時間を無制限に指定します。 時間が重要な場合は、この例で示すように `-A` 引数でチューニングできる時間数を指定して、適切な制限時間を指定します。  
  
```  
dta –D orders –if orders_wkld.sql –of script.sql –A 15 -n 10  
```  
  
 **D.ファイル内に指定されている特定のテーブルをチューニングする**  
  
 次の例では、 *table_list_file* ( **-Tf** 引数) の使用方法を示しています。 table_list.txt ファイルの内容は、次のとおりです。  
  
```  
AdventureWorks2012.Sales.Customer  100000  
AdventureWorks2012.Sales.Store  
AdventureWorks2012.Production.Product  2000000  
```  
  
 table_list.txt 内には、次の内容が指定されています。  
  
-   データベース内にある **Customer**、 **Store**、および **Product** テーブルのみをチューニングの対象とする。  
  
-   **Customer** 、 **Product** テーブルの列数は、それぞれ 100,000 と 2,000,000 と想定する。  
  
-   **Store** の行数は、このテーブル内の現在の行数と同じと想定する。  
  
 *table_list_file*では、行数とテーブル名の間には 1 つ以上の空白がある場合があることに注意してください。  
  
 チューニング時間は 2 時間 (`-A 120`) で、出力は XML ファイルに書き込まれます (`-ox XMLTune.xml`)。  
  
```  
dta –D pubs –if pubs_wkld.sql –ox XMLTune.xml –A 120 –Tf table_list.txt  
```  
  
## <a name="see-also"></a>参照  
 [コマンド プロンプト ユーティリティ リファレンス&#40;データベース エンジン&#41;](../command-prompt-utility-reference-database-engine.md)   
 [データベース エンジン チューニング アドバイザー](../../relational-databases/performance/database-engine-tuning-advisor.md)  
  
  