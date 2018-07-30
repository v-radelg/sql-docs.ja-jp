---
title: SQL Server インデックス デザイン ガイド | Microsoft Docs
ms.custom: ''
ms.date: 06/14/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b856ee9a-49e7-4fab-a88d-48a633fce269
caps.latest.revision: 17
author: craigg-msft
ms.author: craigg
manager: craigg
ms.openlocfilehash: bd1bc616c3a897f0c7b3b3ea4fda256b240f75ab
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37155423"
---
# SQL Server インデックス デザイン ガイド
  不完全なデザインのインデックスやインデックスの不備は、データベース アプリケーションのボトルネックの主な原因となります。 効率的なインデックスのデザインは、データベースとアプリケーションの高パフォーマンスを実現するための最優先事項です。 この SQL Server インデックス デザイン ガイドには、効果的なインデックスをデザインしてアプリケーションのニーズを満たすために役立つ情報および推奨事項が含まれています。  
  
**適用対象**:[!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]を通じて[!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]明記しない限りです。  
  
 このガイドでは、 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]で使用できるインデックスの種類に関して一般的な知識があることを前提としています。 インデックスの種類に関する全般的な説明については、「 [インデックス](http://msdn.microsoft.com/library/ms175049.aspx)」を参照してください。  
  
##  <a name="Top"></a> このガイドで  
 [インデックスのデザインの基礎](#Basics)  
  
 [一般的なインデックスのデザイン ガイドライン](#General_Design)  
  
 [クラスター化インデックスのデザイン ガイドライン](#Clustered)  
  
 [非クラスター化インデックスのデザイン ガイドライン](#Nonclustered)  
  
 [一意インデックスのデザイン ガイドライン](#Unique)  
  
 [フィルター選択されたインデックスのデザイン ガイドライン](#Filtered)  
  
 [その他の情報](#Additional_Reading)  
  
##  <a name="Basics"></a> インデックスのデザインの基礎  
 インデックスとは、テーブルまたはビューに関連付けられたディスク上の構造で、テーブルやビューからの行の取得を高速化します。 インデックスには、テーブル内またはビュー内の 1 つ以上の列から構築されたキーが含まれています。 これらのキーは 1 つの構造 (B-Tree) 内に格納されます。SQL Server はこの構造を使用して、キー値に関連した 1 つ以上の行を効率よく迅速に検出できます。  
  
 データベースとワークロードに適したインデックスの選択は、クエリの速度と更新コストのバランスを取る必要がある複雑な作業です。 インデックス キー内の列数が少ないインデックスを使用すると、ディスク領域とメンテナンスのオーバーヘッドが少なくて済みます。 これに対して、列数の多いインデックスを使用すると、より多くのクエリに対応できます。 効率の高いインデックスを決定するには、さまざまなデザインをテストする必要があります。 インデックスは、データベース スキーマやアプリケーションのデザインに影響を与えずに追加、変更、および削除できます。 さまざまなデザインのインデックスを積極的にテストするようにしてください。  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] のクエリ オプティマイザーでは、多くの場合、最も効率的なインデックスが選択されます。 インデックスのデザインの全体的な考え方としては、クエリ オプティマイザーでインデックスを選択するための選択肢として、さまざまなデザインのインデックスを用意し、オプティマイザーに決定を任せる必要があります。 このようにすると、分析時間を短縮でき、さまざまな状況でパフォーマンスを向上できます。 クエリ オプティマイザーで特定のクエリに使用されるインデックスを確認するには、 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]で、 **[クエリ]** メニューの **[実際の実行プランを含める]** を選択します。  
  
 インデックスを使用しても、常にパフォーマンスが向上するわけではありません。また、パフォーマンスが優れていても、常にインデックスが効率的に使用されているわけでもありません。 インデックスを使用すれば常にパフォーマンスが向上するならば、クエリ オプティマイザーのジョブは単純です。 しかし実際には、不適切なインデックスを選択すると、最適なパフォーマンスを実現することはできません。 したがって、クエリ オプティマイザーでは、パフォーマンスの向上につながる場合にのみインデックスまたはインデックスの組み合わせが選択され、パフォーマンスの低下につながる場合、インデックス付き検索は実行されません。  
  
### インデックスのデザインの作業  
 インデックスをデザインするには、次の作業を行うことをお勧めします。  
  
1.  データベース自体の特性を理解します。 たとえば、データが頻繁に変更されるオンライン トランザクション処理 (OLTP) データベースであるか、主に読み取り専用データが格納されており非常に大きなデータ セットの高速処理を要する意思決定支援システム (DSS) データベースまたはデータ ウェアハウジング (OLAP) データベースであるかを理解する必要があります。 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]で、 *xVelocity メモリ最適化列ストア* インデックスは、一般的なデータ ウェアハウスのデータ セットに特に適しています。 列ストア インデックスによって、フィルター処理クエリ、集計クエリ、グループ化クエリ、スター結合クエリなどの一般的なデータ ウェアハウス クエリのパフォーマンスを向上することで、ユーザーが快適にデータ ウェアハウスを利用できるようになります。 詳細については、次を参照してください。[列ストア インデックスの概念](../relational-databases/indexes/columnstore-indexes-described.md)します。  
  
2.  最もよく使用されるクエリの特性を理解します。 たとえば、よく使用されるクエリの中に、複数のテーブルを結合するクエリがあることを把握していると、使用する最適なインデックスの種類を決定するときに役立ちます。  
  
3.  クエリで使用される列の特性を理解します。 たとえば、整数データ型を格納する列で、一意の列または NULL 値を許容しない列であれば、インデックスに適しています。 適切に定義されたデータのサブセットが含まれている列に対し、 [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] 以上のバージョンでは、フィルター選択されたインデックスを使用できます。 詳細については、このガイドの「 [フィルター選択されたインデックスのデザイン ガイドライン](#Filtered) 」を参照してください。  
  
4.  インデックスの作成時またはメンテナンス時のパフォーマンスを向上させるインデックス オプションを決定します。 たとえば、既存の大きなテーブルにクラスター化インデックスを作成する際には ONLINE インデックス オプションが有益です。 ONLINE オプションを使用すると、インデックスの作成中または再構築中に、基になるデータで同時処理を続行できます。 詳細については、「 [インデックス オプションの設定](../relational-databases/indexes/set-index-options.md)」を参照してください。  
  
5.  インデックスの最適な格納場所を決定します。 非クラスター化インデックスは、基になるテーブルと同じファイル グループまたは別のファイル グループに格納できます。 インデックスの格納場所により、ディスク I/O のパフォーマンスが向上し、その結果クエリのパフォーマンスを向上させることができます。 たとえば、非クラスター化インデックスを、テーブル ファイル グループとは別のディスク上にあるファイル グループに格納すると、複数のディスクを同時に読み取ることができるため、パフォーマンスが向上します。  
  
     また、クラスター化インデックスと非クラスター化インデックスでは、複数のファイル グループにまたがってパーティション構成を使用できます。 大きなテーブルやインデックスをパーティション分割すると、コレクション全体の整合性を維持しながら、データのサブセットに対するアクセスや管理を迅速かつ効率的に行うことができるので、大きなテーブルやインデックスを管理しやすくなります。 詳しくは、「 [Partitioned Tables and Indexes](../relational-databases/partitions/partitioned-tables-and-indexes.md)」をご覧ください。 パーティション分割を検討するときは、インデックスを固定するかどうかを決定します。つまり、基本的にテーブルと同じ方法でパーティション分割するか、または別の方法でパーティション分割するかを決定するということです。  
  
##  <a name="General_Design"></a> インデックスのデザインの全般的なガイドライン  
 経験豊富なデータベース管理者であれば適切なインデックス セットをデザインできますが、それほど複雑でないデータベースとワークロードであっても、この作業はきわめて複雑で、時間がかかり、間違いを犯しやすいものです。 使用するデータベース、クエリ、データ列の特性を理解することが、最適なインデックスをデザインする際に役に立ちます。  
  
### データベースに関する注意点  
 インデックスをデザインするときは、次のデータベースのガイドラインを考慮してください。  
  
-   1 つのテーブルに多数のインデックスがあると、テーブル内でのデータ変更に応じてインデックスをすべて調整する必要があるので、INSERT、UPDATE、DELETE、および MERGE の各ステートメントのパフォーマンスに影響します。 たとえば、列が複数のインデックスで使用されており、この列のデータを変更する UPDATE ステートメントを実行する場合は、その列が含まれている各インデックスも、基になるベース テーブル (ヒープまたはクラスター化インデックス) と同様に更新する必要があります。  
  
    -   頻繁に更新するテーブルにはインデックスをデザインしすぎないようにし、インデックスの幅を狭く、つまり列数を可能な限り少なくします。  
  
    -   更新の必要が少なく、容量の大きいテーブルの場合、クエリのパフォーマンスを向上させるにはインデックスを多数使用します。 SELECT ステートメントなど、データを変更しないクエリの場合は、多数のインデックスを使用することで、クエリ オプティマイザーが最速のアクセス方法を決定する際に選択できるインデックスが多くなるため、クエリのパフォーマンスを向上できる可能性があります。  
  
-   小さなテーブルではインデックスを作成しない方がよい場合もあります。これは、クエリ オプティマイザーが単純なテーブル スキャンを実行するよりデータのインデックスを検索する方に時間がかかることがあるためです。 そのため、小さなテーブルのインデックスがまったく使用されない可能性があっても、テーブルのデータの変更に合わせてメンテナンスする必要があります。  
  
-   ビューが集計、テーブル結合、または集計と結合の組み合わせを使用している場合、ビューにインデックスを設定すると、パフォーマンスが大幅に向上します。 クエリで明示的に参照しなくても、クエリ オプティマイザーはそのビューを使用します。  
  
-   データベース エンジン チューニング アドバイザーを使用してデータベースを分析し、推奨インデックスを作成します。 詳細については、「 [Database Engine Tuning Advisor](../relational-databases/performance/database-engine-tuning-advisor.md)」を参照してください。  
  
### クエリに関する注意点  
 インデックスをデザインするときは、次のクエリのガイドラインを考慮してください。  
  
-   クエリの述語や結合条件で頻繁に使用される列に対して非クラスター化インデックスを作成します。 ただし、不要な列を追加しないようにする必要があります。 インデックス列を追加しすぎると、必要なディスク領域が増え、インデックスのメンテナンスのパフォーマンスも低下する可能性があります。  
  
-   クエリの対象にインデックスを含めると、クエリのパフォーマンスを向上できます。これは、クエリの要件を満たすために必要なデータがすべて、インデックス自体に保持されているためです。 つまり、要求されたデータの取得に必要なのはインデックス ページだけで、テーブルやクラスター化インデックスのデータ ページは必要ありません。このため、全体的にディスク I/O を削減できます。 たとえば、テーブルの列 **a** と **b** に対するクエリは、このテーブルに列 **a**、 **b**、 **c** に基づく複合インデックスが作成されていれば、インデックスのみから指定したデータを取得できます。  
  
-   複数のクエリを使用して同じ行を更新するよりも、1 つのステートメントでできるだけ多くの行を挿入または変更するクエリを作成します。 ステートメントを 1 つだけ使用することで、インデックスのメンテナンスを最適化できます。  
  
-   クエリの種類とクエリ内での列の使用方法を評価します。 たとえば、完全一致検索クエリで使用される列は、非クラスター化インデックスまたはクラスター化インデックスにする適切な候補になります。  
  
### 列に関する注意点  
 インデックスをデザインするときは、次の列のガイドラインを考慮してください。  
  
-   クラスター化インデックスのインデックス キー長は長くならないようにします。 また、クラスター化インデックスは一意列や非 NULL 列に作成すると効率的です。  
  
-   `ntext`、`text`、`image`, `varchar(max)`、`nvarchar(max)`、および `varbinary(max)` データ型の列をインデックス キー列に指定することはできません。 ただし、`varchar(max)`、`nvarchar(max)`、`varbinary(max)`、および `xml` データ型は、インデックスの非キー列として非クラスター化インデックスに含めることができます。 詳細については、このガイドの ['付加列インデックス'](#Included_Columns)に関するセクションを参照してください。  
  
-   `xml` データ型は、XML インデックスでのみキー列にできます。 詳細については、「[XML インデックス &#40;SQL Server&#41;](../relational-databases/xml/xml-indexes-sql-server.md)」をご覧ください。 SQL Server 2012 SP1 では、選択的 XML インデックスと呼ばれる新しい種類の XML インデックスが導入されています。 この新しいインデックスを使用すると、SQL Server に XML 形式で格納されたデータに対するクエリのパフォーマンスが向上するため、XML データの大量のワークロードに対するインデックスの設定がはるかに高速になります。また、インデックス自体のストレージ コストを削減できるため、スケーラビリティも向上します。 詳細については、「[選択的 XML インデックス &#40;SXI&#41;](../relational-databases/xml/selective-xml-indexes-sxi.md)」を参照してください。  
  
-   列の一意性を調べます。 同じ列の組み合わせに対して一意でないインデックスを作成するよりも一意インデックスを作成する方が、クエリ オプティマイザーに追加情報が提供され、インデックスの利用価値が高まります。 詳細については、このガイドの「 [一意インデックスのデザイン ガイドライン](#Unique) 」を参照してください。  
  
-   列内のデータの分布を調べます。 インデックスを設定した列にほとんど一意の値がない場合や、このような列を結合する場合、クエリに時間がかかることがよくあります。 これは、データとクエリにかかわる根本的な問題で、通常はこのような状況を特定しなければ解決できません。 たとえば、姓がアルファベット順に並んだ電話帳では、対象地域のすべての人が Smith や Jones という姓である場合、特定の人を探すときに役に立ちません。 データ分布の詳細については、「 [統計](../relational-databases/statistics/statistics.md)」を参照してください。  
  
-   スパース列、ほとんどが NULL 値の列、さまざまなカテゴリの値を含む列、および異なる範囲の値を含む列のようなサブセットが明確に定義されている列では、フィルター選択されたインデックスの使用を検討してください。 フィルター選択されたインデックスを適切に設計すると、クエリのパフォーマンスが向上し、インデックスのメンテナンス コストとストレージ コストを削減できます。  
  
-   インデックスに複数の列が含まれる場合は、列の順序を考慮します。 等しい (=)、より大きい (>)、より小さい (<)、BETWEEN などの検索条件の WHERE 句で使用されるか、結合に含まれる列は、先頭に配置します。 その他の列は、差異の程度、つまり最も差異の大きいものから最も差異の小さいものの順に配置します。  
  
     たとえば、インデックスが `LastName`, `FirstName` として定義されている場合、このインデックスは、検索条件が `WHERE LastName = 'Smith'` または `WHERE LastName = Smith AND FirstName LIKE 'J%'`である場合に効果があります。 ただし、クエリ オプティマイザーでは、 `FirstName (WHERE FirstName = 'Jane')`のみで検索するクエリには、このインデックスが使用されません。  
  
-   計算列のインデックス設定を検討します。 詳細については、「 [計算列のインデックス](../relational-databases/indexes/indexes-on-computed-columns.md)」を参照してください。  
  
### インデックスの特性  
 クエリにインデックスを設定することが適切であると判断した場合は、状況に応じて最適な種類のインデックスを選択します。 インデックスの特性は、次のとおりです。  
  
-   クラスター化と非クラスター化  
  
-   一意と非一意  
  
-   単一列と複数列  
  
-   昇順と降順 (インデックス内の列の並び)  
  
-   テーブル全体の非クラスター化インデックスとフィルター選択された非クラスター化インデックス  
  
 インデックスを最初に保存したときの特性をカスタマイズし、FILLFACTOR などのオプションを設定してパフォーマンスやメンテナンスを最適化できます。 また、パフォーマンスを最適化するために、ファイル グループやパーティション構成を使用してインデックスの保存場所を決定することもできます。  
  
###  <a name="Index_placement"></a> ファイル グループまたはパーティション構成に対するインデックス配置  
 インデックスの設計について考えるときは、データベースに関連付けられたファイル グループ上にインデックスを配置することを検討する必要があります。 ファイル グループまたはパーティション構成を慎重に選択することで、クエリのパフォーマンスを向上できる場合があります。  
  
 既定では、インデックスが作成されるベース テーブルと同じファイル グループにインデックスも格納されます。 パーティション分割されていないクラスター化インデックスおよびベース テーブルは、常に同じファイル グループに存在します。 しかし、次の操作を実行できます。  
  
-   ベース テーブルまたはクラスター化インデックスのファイル グループ以外のファイル グループに、非クラスター化インデックスを作成する。  
  
-   複数のファイル グループにまたがるクラスター化インデックスおよび非クラスター化インデックスをパーティション分割する。  
  
-   あるファイル グループから別のファイル グループにテーブルを移動する。この操作を行うには、クラスター化インデックスを削除して DROP INDEX ステートメントの MOVE TO 句に新しいファイル グループまたはパーティション構成を指定するか、DROP_EXISTING 句を指定した CREATE INDEX ステートメントを使用します。  
  
 異なるファイル グループに非クラスター化インデックスを作成した場合、そのファイル グループが独自のコントローラーを持つ異なる物理ドライブを使用していると、パフォーマンスの向上を実現できます。 データおよびインデックス情報は、複数のディスク ヘッドにより並列で読み込めるようになります。 たとえば、ファイル グループ `Table_A` の `f1` とファイル グループ `Index_A` の `f2` が同じクエリで使用される場合、両ファイル グループが競合することなく完全に使用されるため、パフォーマンスが向上します。 ただし、クエリによって `Table_A` がスキャンされる場合でも、 `Index_A` が参照されていないと、ファイル グループ `f1` のみが使用されます。 この場合、パフォーマンスは向上しません。  
  
 アクセスの種類や実行のタイミングは事前には予測できないため、テーブルとインデックスをすべてのファイル グループにわたって分散しておくことをお勧めします。 すべてのデータとインデックスが、すべてのディスクにわたって均等に分散されていれば、どのような方法でデータへのアクセスが行われても、確実にすべてのディスクがアクセスされます。 この方法は、システム管理者にとっても簡単な方法になります。  
  
#### 複数のファイル グループでのパーティション分割  
 複数のファイル グループにわたるクラスター化インデックスおよび非クラスター化インデックスをパーティション分割することもできます。 パーティション インデックスは、パーティション関数に基づいて、行方向または行ごとにパーティション分割されます。 パーティション関数では、パーティション分割列と呼ばれる特定の列の値に基づいて、一連のパーティションに各行をどのようにマップするのかを定義します。 パーティション構成では、一連のファイル グループにパーティションをマップするように指定します。  
  
 インデックスをパーティション分割すると、次のような利点があります。  
  
-   大きなインデックスがより管理しやすくなるスケーラブルなシステムを提供できる。 たとえば OLTP システムで、大きなインデックスを扱うパーティション対応のアプリケーションを実装できます。  
  
-   クエリをより高速かつ効率的に実行できる。 クエリからインデックスのいくつかのパーティションへアクセスしたときに、クエリ オプティマイザーでは個別のパーティションを同時に処理し、クエリによる影響を受けないパーティションを除外できます。  
  
 詳細については、「 [Partitioned Tables and Indexes](../relational-databases/partitions/partitioned-tables-and-indexes.md)」を参照してください。  
  
###  <a name="Sort_Order"></a> インデックス並べ替え順のデザイン ガイドライン  
 インデックスを定義する場合、インデックス キー列のデータを昇順と降順のどちらで格納する必要があるかを考慮する必要があります。 昇順は既定の並べ替え順で、以前のバージョンの [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]との互換性が維持されます。 CREATE INDEX、CREATE TABLE、および ALTER TABLE の各ステートメントの構文では、インデックスと制約の個別の列にキーワード ASC (昇順) と DESC (降順) を使用できます。  
  
 インデックスにキー値が格納される順序を指定することは、テーブルを参照しているクエリに ORDER BY 句があり、そのインデックスの 1 つ以上のキー列が ORDER BY 句によって異なる方向に指定されている場合に役立ちます。 このような場合、インデックスにより、クエリ プランで SORT 操作を実行する必要がなくなるので、クエリをより効率的に実行できるようになります。 たとえば、 [!INCLUDE[ssSampleDBCoFull](../includes/sssampledbcofull-md.md)] の購買部のバイヤーが、業者から購入する製品の品質を評価する必要がある場合について考えてみます。 バイヤーにとって最も関心があるのは、これらの業者から配送された製品の中から、返品率の高い製品を見つけ出すことです。 次のクエリに示すように、この基準を満たすデータを取得するには、 `RejectedQty` テーブルの `Purchasing.PurchaseOrderDetail` 列を降順 (大から小) に並べ替え、 `ProductID` 列を昇順 (小から大) に並べ替える必要があります。  
  
```sql
SELECT RejectedQty, ((RejectedQty/OrderQty)*100) AS RejectionRate,  
    ProductID, DueDate  
FROM Purchasing.PurchaseOrderDetail  
ORDER BY RejectedQty DESC, ProductID ASC;  
```  
  
 次に示すこのクエリの実行プランは、クエリ オプティマイザーにより SORT 操作が使用され、ORDER BY 句で指定された順序で結果セットが返されたことを示します。  
  
 ![実行プランは、演算子を使用して、並べ替えを示しています。](media/indexsort1.gif "実行プランは演算子を使用して、並べ替えを示します。")  
  
 作成したインデックスのキー列がクエリの ORDER BY 句で使用するキー列と一致する場合、クエリ プランの SORT 操作を削除できるので、クエリ プランがより効率的になります。  
  
```sql
CREATE NONCLUSTERED INDEX IX_PurchaseOrderDetail_RejectedQty  
ON Purchasing.PurchaseOrderDetail  
    (RejectedQty DESC, ProductID ASC, DueDate, OrderQty);  
```  
  
 もう一度クエリを実行した後、次の実行プランは、SORT 操作が削除され、新しく作成された非クラスター化インデックスが使用されたことを示します。  
  
 ![演算子は使用されず、SORT 示す実行プラン](media/insertsort2.gif "演算子は使用されず、SORT 示す実行プラン")  
  
 [!INCLUDE[ssDE](../includes/ssde-md.md)] は、どちらの方向でも同じように効率的に移動します。 `(RejectedQty DESC, ProductID ASC)` として定義されたインデックスは、ORDER BY 句の列の並べ替え方向が逆転されたクエリで引き続き使用できます。 たとえば、ORDER BY 句 `ORDER BY RejectedQty ASC, ProductID DESC` が含まれたクエリでは、このインデックスを使用できます。  
  
 並べ替え順は、キー列のみに指定できます。 [sys.index_columns](/sql/relational-databases/system-catalog-views/sys-indexes-transact-sql) カタログ ビューと INDEXKEY_PROPERTY 関数により、インデックス列が昇順と降順のどちらで格納されているかが報告されます。  
  
 ![上部のリンクに戻る で使用される矢印アイコン](media/uparrow16x16.gif "に戻る リンクの上位で使用される矢印アイコン")[このガイドで](#Top)  
  
##  <a name="Clustered"></a> クラスター化インデックスのデザイン ガイドライン  
 クラスター化インデックスは、データ行をそのキー値に基づいて並べ替え、テーブル内に格納します。 データ行自体は 1 つの順序でしか並べ替えられないため、1 つのテーブルに設定できるクラスター化インデックスは 1 つだけです。 ほとんどの場合、各テーブルには、次の条件を満たす単一または複数の列に基づいて定義されたクラスター化インデックスを作成することをお勧めします。  
  
-   頻繁に使用されるクエリに使用可能。  
  
-   一意性が高い。  
  
    > [!NOTE]  
    >  PRIMARY KEY 制約を作成すると、単一または複数の列に基づく一意のインデックスが自動的に作成されます。 既定では、クラスター化インデックスが作成されますが、制約を作成する際に非クラスター化インデックスを作成するように指定することもできます。  
  
-   範囲クエリで使用可能。  
  
 UNIQUE プロパティを指定せずにクラスター化インデックスが作成された場合、 [!INCLUDE[ssDE](../includes/ssde-md.md)] により、4 バイトの uniqueifier 列が自動的にテーブルに追加されます。 必要があれば、各キーを一意にするため、 [!INCLUDE[ssDE](../includes/ssde-md.md)] により自動的に uniqueifier 値が行に追加されます。 この列とその値は、内部的に使用されるもので、ユーザーが参照したりアクセスすることはできません。  
  
### クラスター化インデックスのアーキテクチャ  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]では、インデックスは B ツリーとして構成されます。 インデックス B ツリー内の各ページをインデックス ノードと呼びます。 B ツリーの最上位ノードはルート ノードといいます。 インデックス内の最下位ノードをリーフ ノードと呼びます。 ルート ノードとリーフ ノードの間にあるインデックス レベルは、総称して中間レベルといいます。 クラスター化インデックスでは、リーフ ノードに基になるテーブルのデータ ページが含まれています。 ルート ノードと中間レベル ノードには、インデックス行を保持するインデックス ページが含まれています。 各インデックス行には、キー値と、B ツリー内の中間レベル ページかインデックスのリーフ レベルのデータ行のいずれかへのポインターが含まれています。 インデックスの各レベルのページは、二重にリンクされた一覧でリンクされています。  
  
 クラスター化インデックスの場合、 [sys.partitions](/sql/relational-databases/system-catalog-views/sys-partitions-transact-sql)にはインデックスで使用されるパーティションごとに 1 つの行が含まれます。この場合、 **index_id** は 1 と等しくなります。 既定では、クラスター化インデックスのパーティションは 1 つです。 クラスター化インデックスにパーティションが複数ある場合、各パーティションは、そのパーティションのデータを保持する B ツリー構造になります。 たとえば、クラスター化インデックスに 4 つのパーティションがある場合、4 つの B ツリーを持つ構造になります。この場合、パーティションごとに 1 つの B ツリーがあります。  
  
 クラスター化インデックスのデータ型によっては、各クラスター化インデックスの構造に 1 つ以上のアロケーション ユニットが含まれ、そこに特定のパーティションのデータが格納され、管理されます。 各クラスター化インデックスには、パーティションごとに、少なくとも 1 つの IN_ROW_DATA アロケーション ユニットがあります。 また、クラスター化インデックスにラージ オブジェクト (LOB) 列が含まれている場合は、パーティションごとに 1 つの LOB_DATA アロケーション ユニットもあります。 さらに、行サイズの上限である 8,060 バイトを超える可変長列が含まれている場合は、パーティションごとに 1 つの ROW_OVERFLOW_DATA アロケーション ユニットがあります。  
  
 データ チェーン内のページとページ内の行は、クラスター化インデックス キーの値に基づいて並べ替えられます。 挿入はすべて、挿入される行のキー値が、順序付けられた既存の行の並びの中に正しく収まる位置で行われます。  
  
 次の図は、1 つのパーティション内のクラスター化インデックスの構造を示します。  
  
 ![クラスター化インデックスのレベル](media/bokind2.gif "クラスター化インデックスのレベル")  
  
### クエリに関する注意点  
 クラスター化インデックスを作成する前に、データがどのようにアクセスされるかを理解しておいてください。 次の処理を行うクエリには、クラスター化インデックスを使用することを検討してください。  
  
-   BETWEEN、>、>=、<、<= などの演算子を使用して、ある範囲の値を返す。  
  
     クラスター化インデックスを使用して最初の値を持つ行が検索されると、後続のインデックス値がある行は物理的に必ず隣接しています。 たとえば、クエリである範囲内の販売注文番号を持つ行を取得する場合、 `SalesOrderNumber` 列のクラスター化インデックスを使用すると、最初の販売注文番号を含む行をすばやく検索して、最後の販売注文番号に達するまでテーブル内の後続の行をすべて取得できます。  
  
-   大きな結果セットを返す。  
  
-   JOIN 句を使用する。通常、これらは外部キー列になります。  
  
-   ORDER BY 句または GROUP BY 句を使用する。  
  
     ORDER BY 句または GROUP BY 句の中で指定された列にインデックスが設定されている場合、行が既に並べ替えられているので、 [!INCLUDE[ssDE](../includes/ssde-md.md)] によるデータの並べ替えが必要ないことがあります。 このような場合は、クエリ パフォーマンスが向上します。  
  
### 列に関する注意点  
 通常は、クラスター化インデックス キーの定義に使用する列はできるだけ少なくする必要があります。 次の 1 つ以上の条件を満たす列を使用するようにしてください。  
  
-   一意な値または多数の異なる値を含む。  
  
     たとえば、従業員 ID は、従業員を一意に識別します。 `EmployeeID` 列にクラスター化インデックスまたは PRIMARY KEY 制約を設定すると、従業員 ID 番号に基づいて従業員情報を検索するクエリのパフォーマンスが向上します。 また、 `LastName`列、 `FirstName`列、 `MiddleName` 列を基にクラスター化インデックスを作成することもできます。従業員レコードは、これらの列でグループ化されたりクエリが実行されることが多く、これらの列を組み合わせると高い多様性が生まれます。  
  
-   順次アクセスされる。  
  
     たとえば、製品 ID は、 `Production.Product` データベースの [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] テーブルにある製品を一意に識別します。 `WHERE ProductID BETWEEN 980 and 999`など、順次検索が指定されているクエリでは、 `ProductID`列に基づくクラスター化インデックスによりパフォーマンスが向上する場合があります。 これは、行がこのキー列を基に並べ替えて格納されている場合があるためです。  
  
-   ID として定義されている。  
  
-   テーブルから取得したデータの並べ替えに頻繁に使用される。  
  
     このような列を基にテーブルをクラスター化する (つまり、物理的に並べ替える) と、この列に対してクエリを実行するたびに並べ替えにかかるコストを節約できるため便利です。  
  
 次のような場合は、クラスター化インデックスの使用は適していません。  
  
-   頻繁に変更される列  
  
     [!INCLUDE[ssDE](../includes/ssde-md.md)] では各行のデータ値を物理的な順序で維持する必要があるので、データが変更されると行全体が移動します。 データが頻繁に変更される大規模トランザクション処理システムでは、特にこの点に留意してください。  
  
-   広範なキー  
  
     広範なキーは、複数の列または複数のサイズの大きな列を組み合わせたものです。 クラスター化インデックスのキー値は、すべての非クラスター化インデックスにより、参照キーとして使用されます。 非クラスター化インデックスのエントリには、クラスター化キー以外に、非クラスター化インデックスのキー列も格納されるため、同じテーブルに非クラスター化インデックスが定義されている場合は、サイズがかなり大きくなります。  
  
 ![上部のリンクに戻る で使用される矢印アイコン](media/uparrow16x16.gif "に戻る リンクの上位で使用される矢印アイコン")[このガイドで](#Top)  
  
##  <a name="Nonclustered"></a> 非クラスター化インデックスのデザイン ガイドライン  
 非クラスター化インデックスには、インデックス キー値、およびテーブル データの格納場所を指す行ロケーターが含まれています。 1 つのテーブルまたはインデックス付きビューに複数の非クラスター化インデックスを作成できます。 一般に、非クラスター化インデックスは、頻繁に使用するクエリで、クラスター化インデックスで対応されないクエリのパフォーマンスを向上するようにデザインします。  
  
 クエリ オプティマイザーでデータ値を検索するときは、本の索引を使用する場合と同じように、非クラスター化インデックスを検索してテーブル内でのデータ値の位置を探し、その位置から直接データを取得します。 非クラスター化インデックスには、クエリの検索対象であるデータ値のテーブル内での位置を正確に記述するエントリが格納されているので、完全一致比較クエリの場合は非クラスター化インデックスが最適です。 たとえば、 `HumanResources. Employee` テーブルに対してクエリを実行し、ある 1 人の上司に直属するすべての従業員を取得する場合、クエリ オプティマイザーは `IX_Employee_ManagerID`をキー列として、非クラスター化インデックス `ManagerID` を使用することができます。 クエリ オプティマイザーはこのインデックスの中から、指定された `ManagerID`と一致するすべてのエントリを迅速に検索できます。 インデックスの各エントリのポインターは、テーブル (またはクラスター化インデックス) の、対応するデータが見つかる正確なページおよび行を指しています。 クエリ オプティマイザーは、インデックスの中からすべてのエントリを検出した後、正確なページおよび行に直接移動してデータを取得できます。  
  
### 非クラスター化インデックスのアーキテクチャ  
 非クラスター化インデックスもクラスター化インデックスと同じ B ツリー構造ですが、次に示す大きな相違点があります。  
  
-   基になるテーブルのデータ行は、非クラスター化キーに基づいた順序で並べ替えられたり格納されたりしません。  
  
-   非クラスター化インデックスのリーフ層は、データ ページではなくインデックス ページで構成されます。  
  
 非クラスター化インデックス行内の行ロケーターは、次に示すような、行を指すポインターまたは行のクラスター化インデックス キーのいずれかです。  
  
-   テーブルがヒープで、クラスター化インデックスが設定されていない場合、行ロケーターはその行へのポインターです。 このポインターは、ファイル識別子 (ID)、ページ番号、およびそのページ上での行の番号で構成されます。 ポインター全体は、RID (行 ID) と呼ばれます。  
  
-   テーブルにクラスター化インデックスがある場合、またはインデックスがインデックス付きビューにある場合は、行ロケーターが行のクラスター化インデックス キーになります。  
  
 非クラスター化インデックスの場合、[sys.partitions](/sql/relational-databases/system-catalog-views/sys-partitions-transact-sql) にはインデックスで使用されるパーティションごとに 1 つの行が含まれます。この場合、**index_id** は 1 より大きくなります。 既定では、非クラスター化インデックスのパーティションは 1 つです。 非クラスター化インデックスにパーティションが複数ある場合、各パーティションは、その特定のパーティションに対してインデックス行を保持する B ツリー構造になります。 たとえば、非クラスター化インデックスに 4 つのパーティションがある場合、4 つの B ツリーを持つ構造になります。この場合、パーティションごとに 1 つの B ツリーがあります。  
  
 非クラスター化インデックスのデータ型によっては、各非クラスター化インデックスの構造に 1 つ以上のアロケーション ユニットが含まれ、そこに特定のパーティションのデータが格納され、管理されます。 各非クラスター化インデックスには、インデックス B ツリーのページが格納されているパーティションごとに、少なくとも 1 つの IN_ROW_DATA アロケーション ユニットがあります。 また、非クラスター化インデックスにラージ オブジェクト (LOB) 列が含まれている場合は、パーティションごとに 1 つの LOB_DATA アロケーション ユニットもあります。 さらに、行サイズの上限である 8,060 バイトを超える可変長列が含まれている場合は、パーティションごとに 1 つの ROW_OVERFLOW_DATA があります。  
  
 次の図に、1 つのパーティション内の非クラスター化インデックスの構造を示します。  
  
 ![非クラスター化インデックスのレベル](media/bokind1.gif "非クラスター化インデックスのレベル")  
  
### データベースに関する注意点  
 非クラスター化インデックスをデザインするときは、データベースの特性を考慮してください。  
  
-   更新の必要が少なく、容量の大きいデータベースまたはテーブルの場合、クエリのパフォーマンスを向上させるには非クラスター化インデックスを多数作成するのが適しています。 テーブル全体の非クラスター化インデックスと比較してクエリのパフォーマンスが向上し、インデックスのストレージ コストとインデックスのメンテナンス コストが削減されるように、適切に定義されたデータのサブセットに対してフィルター選択されたインデックスを作成することを検討してください。  
  
     読み取り専用データが中心の意思決定支援システム アプリケーションおよびデータベースは、非クラスター化インデックスを多数作成するのが適しています。 非クラスター化インデックスを多数作成すると、クエリ オプティマイザーにより最速のアクセス手段が判断される際の選択肢になるインデックスが多く、データベースの更新頻度が低いのでインデックスのメンテナンスによってパフォーマンスが低下することはありません。  
  
-   テーブルの更新頻度が高いオンライン トランザクション処理アプリケーションおよびデータベースに、インデックスを過度に作成することはお勧めしません。 また、インデックスの列数はできる限り抑えてください。  
  
     1 つのテーブルに多数のインデックスがあると、テーブル内のデータが変更された場合にインデックスをすべて調整する必要があるので、INSERT、UPDATE、DELETE、および MERGE の各ステートメントのパフォーマンスに影響します。  
  
### クエリに関する注意点  
 非クラスター化インデックスを作成する前に、データがどのようにアクセスされるかを理解しておいてください。 次に示す特徴があるクエリには非クラスター化インデックスを使用することを検討してください。  
  
-   JOIN 句または GROUP BY 句を使用している。  
  
     結合操作やグループ化操作に使用する列の非クラスター化インデックスを複数作成し、外部キー列にクラスター化インデックスを作成してください。  
  
-   大きな結果セットを返さないクエリ。  
  
     大きなテーブルから適切に定義された行のサブセットを返すクエリに対応するために、フィルター選択されたインデックスを作成してください。  
  
-   完全一致を返すクエリの検索条件 (WHERE 句など) に頻繁に使用される列を含んでいる。  
  
### 列に関する注意点  
 次に示す特徴に 1 つ以上該当する列を考慮してください。  
  
-   クエリを包括している。  
  
     インデックスにクエリのすべての列が含まれていると、パフォーマンスが向上します。 クエリ オプティマイザーではインデックス内ですべての列値を参照できるので、テーブルやクラスター化インデックスのデータにアクセスすることがなく、ディスク I/O 操作が少なくてすみます。 列数の多いインデックス キーを作成する代わりに、包括する列を追加するには、付加列インデックスを使用します。  
  
     テーブルにクラスター化インデックスがある場合、クラスター化インデックスに定義された列がテーブルの各非クラスター化インデックスの末尾に自動的に付加されます。 その結果、非クラスター化インデックスの定義にクラスター化インデックスの列を指定することなく、インデックスにはクエリで使用するすべての列が含まれることになります。 たとえば、あるテーブルの列 `C`にクラスター化インデックスがある場合、列 `B` および `A` の非クラスター化インデックスのキー値は列 `B`、 `A`、および `C`となります。  
  
-   姓と名の組み合わせなど、多数の異なる値が格納されている (他の列にクラスター化インデックスが使用されている場合)。  
  
     1 と 0 のみなど異なる値が少数しかない場合、テーブル スキャンを行う方が通常は効率的なので、ほとんどのクエリではインデックスが使用されません。 このようなデータの場合は、少数の行のみに含まれる異なる値に対してフィルター選択されたインデックスを作成することを検討してください。 たとえば、ほとんどの値が 0 の場合は、クエリ オプティマイザーで 1 を含むデータ行に対してフィルター選択されたインデックスを使用できます。  
  
####  <a name="Included_Columns"></a> 付加列の使用による非クラスター化インデックスの拡張  
 非クラスター化インデックスのリーフ レベルに非キー列を追加することにより、非クラスター化インデックスの機能を拡張できます。 非キー列を含めることにより、より多くのクエリをカバーする非クラスター化インデックスを作成できます。 これは、非キー列には次の利点があるためです。  
  
-   非キー列には、インデックス キー列として許可されていないデータ型を設定できる。  
  
-   インデックス キー列の数やインデックス キーのサイズを計算するときに、 [!INCLUDE[ssDE](../includes/ssde-md.md)] では非キー列が考慮されない。  
  
 クエリ内のすべての列が、キー列または非キー列のいずれかとしてインデックスに含まれるているとき、非キー付加列を含むインデックスにより、クエリ パフォーマンスが大幅に向上します。 クエリ オプティマイザーではインデックス内のすべての列値を参照できるので、テーブルやクラスター化インデックスのデータにアクセスすることがなく、ディスク I/O 操作が少なくて済むため、パフォーマンスが向上します。  
  
> [!NOTE]  
>  クエリによって参照されるすべての列がインデックスに含まれているときは、一般的に、そのインデックスはクエリをカバーしていると呼ばれます。  
  
 キー列がインデックスのすべてのレベルに格納されている場合は、非キー列はリーフ レベルだけに格納されます。  
  
##### サイズ制限を回避するための付加列の使用  
 非クラスター化インデックスに非キー列を含めることで、現在のインデックス サイズの制限 (最大 16 個のキー列と最大 900 バイトのインデックス キーのサイズ) を超えないようにすることができます。 インデックス キー列の数やインデックス キーのサイズを計算するときに、 [!INCLUDE[ssDE](../includes/ssde-md.md)] では非キー列が考慮されません。  
  
 たとえば、 `Document` テーブルにある次の列にインデックスを設定するとします。  
  
 `Title nvarchar(50)`  
  
 `Revision nchar(5)`  
  
 `FileName nvarchar(400)`  
  
 `nchar` データ型および `nvarchar` データ型は各文字に 2 バイト要するため、これら 3 つの列が含まれるインデックスは 900 バイトのサイズ制限を 10 バイト超えます (455 * 2)。 `INCLUDE` ステートメントの `CREATE INDEX` 句を使用することにより、インデックス キーを (`Title, Revision`) として定義し、 `FileName` を非キー列として定義できます。 その結果、インデックス キーのサイズが 110 バイト (55 \* 2) になりましたが、インデックスには必要な列がすべて含まれています。 このようなインデックスは、次のステートメントで作成されます。  
  
```sql
CREATE INDEX IX_Document_Title   
ON Production.Document (Title, Revision)   
INCLUDE (FileName);   
```  
  
##### 付加列インデックスのガイドライン  
 付加列非クラスター化インデックスを設計するときは、次のガイドラインについて考慮してください。  
  
-   非キー列は、CREATE INDEX ステートメントの INCLUDE 句で定義されます。  
  
-   非キー列は、テーブルやインデックス付きビューの非クラスター化インデックスにのみ定義できます。  
  
-   `text`、`ntext`、および `image` を除く、すべてのデータ型を使用できます。  
  
-   決定的な計算列、および正確または不正確な計算列を、付加列にできます。 詳細については、「 [計算列のインデックス](../relational-databases/indexes/indexes-on-computed-columns.md)」を参照してください。  
  
-   キー列と同様に、計算列が `image`、`ntext`、および `text` の各データ型から派生している場合は、計算列のデータ型が非キー インデックス列として許可されている限り、非キー (付加) 列にできます。  
  
-   INCLUDE リストとキー列リストの両方に、列名を指定することはできません。  
  
-   INCLUDE リスト内で列名を繰り返すことはできません。  
  
##### 列サイズのガイドライン  
  
-   キー列は少なくとも 1 つ定義する必要があります。 非キー列の最大数は 1,023 列です。 これは、テーブルの最大列数から 1 を引いた数です。  
  
-   非キーを除くインデックス キー列は、既存のインデックス サイズの制限 (最大 16 個のキー列、インデックス キーの合計サイズ 900 バイト) に従う必要があります。  
  
-   すべての非キー列の合計サイズは、INCLUDE 句で指定された列のサイズによってのみ制限されます。たとえば、`varchar(max)` 列は 2 GB に制限されます。  
  
##### 列の変更のガイドライン  
 付加列として定義されたテーブル列を変更するときには、次の制限が適用されます。  
  
-   インデックスを先に削除しない限り、非キー列をテーブルから削除できません。  
  
-   次の操作以外に、非キー列は変更できません。  
  
    -   列の NULL 値の許容を NOT NULL から NULL に変更する。  
  
    -   長さを増やす`varchar`、 `nvarchar`、または`varbinary`列。  
  
        > [!NOTE]  
        >  これらの列の変更の制限は、インデックス キー列にも適用されます。  
  
##### 設計上の推奨事項  
 検索や参照に使用される列のみがキー列になるように、大きなサイズのインデックス キーを使用して、非クラスター化インデックスを設計し直します。 クエリをカバーする他のすべての列を、非キー付加列にします。 その結果、クエリをカバーするために必要なすべての列を含むことができますが、インデックス キー自体は小さく、効率的です。  
  
 たとえば、次のクエリをカバーするインデックスを設計するとします。  
  
```sql
SELECT AddressLine1, AddressLine2, City, StateProvinceID, PostalCode  
FROM Person.Address  
WHERE PostalCode BETWEEN N'98000' and N'99999';  
```  
  
 クエリをカバーするには、インデックスに各列を定義する必要があります。 すべての列をキー列として定義でき、その場合キーのサイズは 334 バイトになります。 実際に検索条件に使用されている唯一の列は、30 バイトの長さの `PostalCode` 列なので、より効果的な設計のインデックスにするには、キー列として `PostalCode` を定義し、他のすべての列を非キー列として含めます。  
  
 次のステートメントにより、クエリをカバーする付加列インデックスが作成されます。  
  
```sql
CREATE INDEX IX_Address_PostalCode  
ON Person.Address (PostalCode)  
INCLUDE (AddressLine1, AddressLine2, City, StateProvinceID);  
```  
  
##### パフォーマンスに関する考慮事項  
 不要な列は追加しないでください。 キーまたは非キーのインデックス列を追加しすぎると、次のようなパフォーマンス上の問題が発生することがあります。  
  
-   1 ページに収まるインデックス行が少なくなります。 これにより、ディスク I/O が増加しキャッシュ効率が低下します。  
  
-   インデックスを格納するために、さらに多くのディスク領域が必要になります。 特に、`varchar(max)`、`nvarchar(max)`、`varbinary(max)`、または `xml` のデータ型を非キー インデックス列として追加すると、必要なディスク領域が大幅に増加します。 これは、列の値がインデックスのリーフ レベルにコピーされるためです。 そのため、列の値がインデックスとベース テーブルの両方に存在します。  
  
-   インデックスのメンテナンスによって、基になるテーブルやインデックス付きビューに対する変更、挿入、更新、削除にかかる時間が長くなる場合があります。  
  
 データ変更によるパフォーマンスへの影響や追加ディスク領域の要件よりも、クエリのパフォーマンスから得られる利点の方が大きいかどうかを判断する必要があります。  
  
 ![上部のリンクに戻る で使用される矢印アイコン](media/uparrow16x16.gif "に戻る リンクの上位で使用される矢印アイコン")[このガイドで](#Top)  
  
##  <a name="Unique"></a> 一意インデックスのデザイン ガイドライン  
 一意インデックスを使用すると、インデックス キーの値が重複することがないので、テーブルのすべての行を一意にすることができます。 一意であることがデータ自体の特性である場合にだけ、一意インデックスを指定します。 たとえば、主キーが `NationalIDNumber` で、 `HumanResources.Employee` テーブルの `EmployeeID`列の値が必ず一意になるようにする場合は、 `NationalIDNumber` 列で UNIQUE 制約を作成します。 ユーザーが複数の従業員に対してその列に同じ値を入力しようとすると、エラー メッセージが表示され、重複する値は入力されません。  
  
 複数列に一意インデックスを指定すると、インデックス キーの値の組み合わせはそれぞれ一意になります。 たとえば、 `LastName`列、 `FirstName`列、および `MiddleName` 列の組み合わせに一意インデックスを作成した場合、テーブル内の 2 つの行がこれらの列に対して同じ値の組み合わせを持つことはできません。  
  
 クラスター化インデックスと非クラスター化インデックスは共に一意インデックスにできます。 列のデータが一意である場合、1 つのテーブルに 1 つの一意クラスター化インデックスと、複数の一意非クラスター化インデックスを作成できます。  
  
 一意インデックスの利点を次に示します。  
  
-   定義された列のデータの整合性が保証されます。  
  
-   クエリ オプティマイザーの役に立つ追加情報が提供されます。  
  
 PRIMARY KEY 制約または UNIQUE 制約を作成すると、指定した列に一意インデックスが自動的に作成されます。 UNIQUE 制約を作成することと、制約とは無関係の一意インデックスを作成することの間に大きな違いはありません。 データ検証も同じ方式で行われ、クエリ オプティマイザーでは、制約によって作成された一意インデックスと手動で作成された一意インデックスは区別されません。 ただし、データの整合性を維持することを目的とした列には、列に UNIQUE 制約または PRIMARY KEY 制約を作成する必要があります。 この作業を行うことで、インデックスの目的が明確になります。  
  
### 考慮事項  
  
-   重複するキー値がデータに存在する場合は、一意インデックス、UNIQUE 制約、または PRIMARY KEY 制約を作成できません。  
  
-   データが一意のときに一意性を強制する場合は、一意インデックスを作成する方が、同じ組み合わせの列に一意でないインデックスを作成するよりも、より効率的な実行プランを作成できる追加情報がクエリ オプティマイザーに提供されます。 この場合、一意インデックスを作成することをお勧めします (できるだけ UNIQUE 制約を作成することをお勧めします)。  
  
-   一意非クラスター化インデックスには、付加非キー列を含めることができます。 詳細については、 [付加列インデックス](#Included_Columns)に関する記述を参照してください。  
  
 ![上部のリンクに戻る で使用される矢印アイコン](media/uparrow16x16.gif "に戻る リンクの上位で使用される矢印アイコン")[このガイドで](#Top)  
  
##  <a name="Filtered"></a> フィルター選択されたインデックスのデザイン ガイドライン  
 フィルター選択されたインデックスは、最適化された非クラスター化インデックスであり、適切に定義されたデータのサブセットから選択するクエリに対応する際に特に適しています。 フィルター選択されたインデックスは、フィルター述語を使用して、テーブル内の一部の行にインデックスを作成します。 フィルター選択されたインデックスを適切にデザインすると、クエリのパフォーマンスが向上し、インデックスのメンテナンス コストを削減して、テーブル全体のインデックスと比較してインデックスのストレージ コストを削減することができます。  
  
||  
|-|  
|**適用対象**: [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] から [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)]|  
  
 フィルター選択されたインデックスは、テーブル全体のインデックスよりも次の点で優れています。  
  
-   **クエリのパフォーマンスとプランの品質の向上**  
  
     フィルター選択されたインデックスを適切にデザインすると、クエリのパフォーマンスと実行プランの品質が向上します。これは、このインデックスが、テーブル全体の非クラスター化インデックスよりも小さく、フィルター選択された統計情報を含むためです。 フィルター選択された統計情報は、フィルター選択されたインデックスの行のみを対象としているため、テーブル全体の統計情報よりも正確です。  
  
-   **インデックスのメンテナンス コストの削減**  
  
     インデックスのメンテナンスが行われるのは、データ操作言語 (DML) ステートメントがインデックス内のデータに影響を与える場合のみです。 フィルター選択されたインデックスにより、インデックスのメンテナンス コストは、テーブル全体の非クラスター化インデックスと比較して削減されます。これは、フィルター選択されたインデックスは小さく、インデックス内のデータが影響を受けた場合にのみメンテナンスされるためです。 特に、含まれるデータにほとんど影響がない場合は、多数のフィルター選択されたインデックスを作成できます。 同様に、フィルター選択されたインデックスに頻繁に影響を受けるデータのみが含まれている場合は、インデックスのサイズを小さくすると、統計情報の更新コストが削減されます。  
  
-   **インデックスのストレージ コストの削減**  
  
     テーブル全体のインデックスが不要な場合は、フィルター選択されたインデックスを作成すると、非クラスター化インデックスのディスク ストレージを削減できます。 ストレージ要件をあまり増やすことなく、テーブル全体の非クラスター化インデックスを複数のフィルター選択されたインデックスに置き換えることができます。  
  
 フィルター選択されたインデックスは、クエリが SELECT ステートメントで参照する、適切に定義されたデータのサブセットが列に含まれている場合に役立ちます。 以下に例を示します。  
  
-   NULL 以外の値を少数しか含まないスパース列。  
  
-   複数のカテゴリのデータを含む異種列。  
  
-   金額、時間、日付など、値の範囲を含む列。  
  
-   列の値の単純な比較ロジックで定義されるテーブル パーティション。  
  
 フィルター選択されたインデックスのメンテナンス コストの削減は、そのインデックスに含まれる行数がテーブル全体のインデックスと比較して少ない場合に、最も明確になります。 フィルター選択されたインデックスにテーブル内のほとんどの行が含まれる場合は、テーブル全体のインデックスよりもメンテナンス コストがかかることがあります。 この場合は、フィルター選択されたインデックスではなく、テーブル全体のインデックスを使用する必要があります。  
  
 フィルター選択されたインデックスは 1 つのテーブルで定義され、単純な比較演算子のみをサポートします。 複数のテーブルを参照するフィルター式や複雑なロジックを含むフィルター式が必要な場合は、ビューを作成する必要があります。  
  
### デザインに関する考慮事項  
 フィルター選択されたインデックスを効果的にデザインするには、アプリケーションで使用されるクエリを把握し、そのクエリがデータのサブセットとどのように関連するかを理解することが重要です。 適切に定義されたサブセットを持つデータの例として、ほとんどが NULL 値の列、異種カテゴリの値を含む列、および異なる範囲の値を含む列が挙げられます。 次のデザインに関する考慮事項では、フィルター選択されたインデックスがテーブル全体のインデックスよりも優れている場合のさまざまなシナリオを示します。  
  
#### データのサブセットのフィルター選択されたインデックス  
 クエリに関連する少数の値だけが列に含まれている場合、値のサブセットにフィルター選択されたインデックスを作成できます。 たとえば、列の値がほとんど NULL の場合に、クエリで常に NULL 以外の値を選択するときは、NULL 以外のデータ行にフィルター選択されたインデックスを作成できます。 作成したインデックスは、同じキー列に定義されているテーブル全体の非クラスター化インデックスよりも小さく、メンテナンス コストが少なくなります。  
  
 たとえば、 `AdventureWorks2012` データベースには、 `Production.BillOfMaterials` という 2,679 行のテーブルがあります。 `EndDate` 列では、NULL 以外の値を含む行は 199 行だけで、他の 2,480 行には NULL が含まれています。 次のフィルター選択されたインデックスは、インデックスで定義された列を返し、 `EndDate`で NULL 以外の値を含む行のみを選択するクエリに対応します。  
  
```sql
CREATE NONCLUSTERED INDEX FIBillOfMaterialsWithEndDate  
    ON Production.BillOfMaterials (ComponentID, StartDate)  
    WHERE EndDate IS NOT NULL ;  
GO  
```  
  
 フィルター選択されたインデックス `FIBillOfMaterialsWithEndDate` は、次のクエリに対して有効です。 クエリ実行プランを表示して、クエリ オプティマイザーでフィルター選択されたインデックスが使用されたかどうかを確認できます。  
  
```sql
SELECT ProductAssemblyID, ComponentID, StartDate   
FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL   
    AND ComponentID = 5   
    AND StartDate > '20080101' ;  
```  
  
 フィルター選択されたインデックスの作成方法およびフィルター選択されたインデックスの述語式の定義方法の詳細については、「 [フィルター選択されたインデックスの作成](../relational-databases/indexes/create-filtered-indexes.md)」を参照してください。  
  
#### 異種データのフィルター選択されたインデックス  
 テーブルに異種データの行が含まれている場合、1 つ以上のカテゴリのデータに対してフィルター選択されたインデックスを作成できます。  
  
 たとえば、 `Production.Product` テーブルに示される製品がそれぞれ `ProductSubcategoryID`に割り当てられ、Bikes、Components、Clothing、Accessories の製品カテゴリに関連付けられています。 `Production.Product` テーブル内にあるこうしたカテゴリの列の値はあまり密接に関連していないので、異種カテゴリとなります。 たとえば、 `Color`、 `ReorderPoint`、 `ListPrice`、 `Weight`、 `Class`、および `Style` の各列には、各製品カテゴリで固有の特性があります。 サブカテゴリ 27 ～ 36 を含む付属品に対して頻繁に使用されるクエリがあるとします。 次の例に示すように、付属品のサブカテゴリにフィルター選択されたインデックスを作成することで、付属品に対するクエリのパフォーマンスを向上させることができます。  
  
```sql
CREATE NONCLUSTERED INDEX FIProductAccessories  
    ON Production.Product (ProductSubcategoryID, ListPrice)   
        Include (Name)  
WHERE ProductSubcategoryID >= 27 AND ProductSubcategoryID <= 36;
```  
  
 フィルター選択されたインデックス `FIProductAccessories` は次のクエリに対応します。  
  
 これは、クエリ結果がインデックスに含まれ、クエリ プランにベース テーブルの参照が含まれないためです。 たとえば、クエリ述語式 `ProductSubcategoryID = 33` はフィルター選択されたインデックスの述語 `ProductSubcategoryID >= 27` および `ProductSubcategoryID <= 36`のサブセットで、クエリ述語の `ProductSubcategoryID` 列と `ListPrice` 列はどちらもインデックスのキー列であり、名前は付加列としてインデックスのリーフ レベルに格納されます。  
  
```sql
SELECT Name, ProductSubcategoryID, ListPrice  
FROM Production.Product  
WHERE ProductSubcategoryID = 33 AND ListPrice > 25.00 ;  
```  
  
#### [キー列]  
 フィルター選択されたインデックスの定義に少数のキーまたは付加列を含めること、およびフィルター選択されたインデックスをクエリ オプティマイザーによってクエリ実行プランで選択するために必要な列だけを組み込むことをお勧めします。 クエリ オプティマイザーでは、フィルター選択されたインデックスがクエリに対応するかどうかに関係なく、フィルター選択されたインデックスがクエリに対して選択されます。 ただし、フィルター選択されたインデックスがクエリに対応する場合は、そのインデックスが選択される可能性は高くなります。  
  
 場合によっては、フィルター選択されたインデックスは、その式の列をキー列または付加列としてフィルター選択されたインデックスの定義に含めなくても、クエリに対応します。 次のガイドラインでは、フィルター選択されたインデックスの式の列をフィルター選択されたインデックスの定義でキー列または付加列にする必要がある場合について説明します。 次の例では、以前に作成したフィルター選択されたインデックス `FIBillOfMaterialsWithEndDate` を使用します。  
  
 フィルター選択されたインデックスの式がクエリ述語と同じであり、フィルター選択されたインデックスの式の列がクエリ結果と共に返されない場合、その式の列を、フィルター選択されたインデックスの定義でキー列または付加列にする必要はありません。 たとえば、クエリ述語がフィルター式と同じであり、 `FIBillOfMaterialsWithEndDate` がクエリ結果と共に返されないため、 `EndDate` は次のクエリに対応します。 `FIBillOfMaterialsWithEndDate` は、フィルター選択されたインデックスの定義のキー列または付加列として `EndDate` を必要としません。  
  
```sql
SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL;   
```  
  
 フィルター選択されたインデックスの式と異なるクエリ述語で比較に列が使用される場合は、フィルター選択されたインデックスの式の列を、フィルター選択されたインデックスの定義でキー列または付加列にする必要があります。 たとえば、 `FIBillOfMaterialsWithEndDate` は、フィルター選択されたインデックスから行のサブセットを選択するので、次のクエリに対して有効です。 ただし、 `EndDate` が比較 `EndDate > '20040101'`で使用されるため、次のクエリには対応していません。この比較は、フィルター選択されたインデックスの式と異なります。 クエリ プロセッサでは、 `EndDate`の値を参照せずにこのクエリを実行することはできません。 したがって、 `EndDate` をフィルター選択されたインデックスの定義でキー列または付加列にする必要があります。  
  
```sql
SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
WHERE EndDate > '20040101';   
```  
  
 フィルター選択されたインデックスの式の列がクエリ結果セットに含まれる場合、その列をフィルター選択されたインデックスの定義でキー列または付加列にする必要があります。 たとえば、 `FIBillOfMaterialsWithEndDate` はクエリ結果に含まれる `EndDate` 列を返すので、次のクエリに対応しません。 したがって、 `EndDate` をフィルター選択されたインデックスの定義でキー列または付加列にする必要があります。  
  
```sql
SELECT ComponentID, StartDate, EndDate FROM Production.BillOfMaterials  
WHERE EndDate IS NOT NULL;  
```  
  
 テーブルのクラスター化インデックス キーは、フィルター選択されたインデックスの定義でキー列または付加列にする必要はありません。 クラスター化インデックス キーは、フィルター選択されたインデックスなど、すべての非クラスター化インデックスに自動的に含まれます。  
  
#### フィルター述語のデータ変換演算子  
 フィルター選択されたインデックスでは、その式に指定された比較演算子によって暗黙的または明示的なデータ変換が行われる場合、変換が比較演算子の左辺で行われると、エラーが発生します。 解決方法としては、比較演算子の右辺にデータ変換演算子 (CAST または CONVERT) を含む、フィルター選択されたインデックスの式を記述します。  
  
 次の例では、さまざまなデータ型が含まれるテーブルを作成します。  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.TestTable (a int, b varbinary(4));  
```  
  
 次のフィルター選択されたインデックスの定義では、列 `b` は、定数 1 と比較するために、整数データ型に暗黙的に変換されます。 これにより、フィルター選択された述語の演算子の左辺で変換が行われるため、エラー メッセージ 10611 が生成されます。  
  
```sql
CREATE NONCLUSTERED INDEX TestTabIndex ON dbo.TestTable(a,b)  
WHERE b = 1;  
```  
  
 解決策として、次の例に示すように、右辺の定数を、列 `b`と同じ型になるように変換します。  
  
```sql
CREATE INDEX TestTabIndex ON dbo.TestTable(a,b)  
WHERE b = CONVERT(Varbinary(4), 1);  
```  
  
 データ変換を比較演算子の左辺から右辺に移動すると、変換の意味が変わることがあります。 この例では、CONVERT 演算子を右辺に追加したときに、整数の比較から `varbinary` の比較に変わりました。  
  
 ![上部のリンクに戻る で使用される矢印アイコン](media/uparrow16x16.gif "に戻る リンクの上位で使用される矢印アイコン")[このガイドで](#Top)  
  
##  <a name="Additional_Reading"></a> その他の情報  
 [SQL Server 2008 のインデックス付きビューによるパフォーマンスの向上](http://msdn.microsoft.com/library/dd171921(v=sql.100).aspx)  
  
 [Partitioned Tables and Indexes](../relational-databases/partitions/partitioned-tables-and-indexes.md)  
  
  