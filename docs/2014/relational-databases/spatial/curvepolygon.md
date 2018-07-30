---
title: CurvePolygon | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- dbe-spatial
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: e000a1d8-a049-4542-bfeb-943fd6ab3969
caps.latest.revision: 18
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1989f166f519ddf732cca8cd47e32a14c414cae1
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37268648"
---
# <a name="curvepolygon"></a>CurvePolygon
  A`CurvePolygon`は外部境界リングと 0 によって定義された、位相的に閉じた表面または以上の内部リング  
  
> [!IMPORTANT]  
>  詳細な説明とで導入された空間機能の例の[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]など、`CurvePolygon`サブタイプは、ダウンロード、ホワイト ペーパー「 [SQL Server 2012 の新しい空間機能](http://go.microsoft.com/fwlink/?LinkId=226407)します。  
  
 次の条件の属性を定義する、`CurvePolygon`インスタンス。  
  
-   `CurvePolygon` インスタンスの境界は、外部リングとすべての内部リングによって定義されます。  
  
-   `CurvePolygon` インスタンスの内部は、外部リングとすべての内部リングとの間にある空間です。  
  
 A`CurvePolygon`インスタンスとは異なります、`Polygon`インスタンス、`CurvePolygon`インスタンスは、次の円弧セグメントを含めることができます:`CircularString`と`CompoundCurve`します。  
  
## <a name="compoundcurve-instances"></a>CompoundCurve インスタンス  
 次の図に示した有効`CurvePolygon`の数値。  
  
### <a name="accepted-instances"></a>許容されるインスタンス  
 `CurvePolygon`インスタンスが許容いずれかを指定する必要があるに空であるか、許容される円弧リングのみを含めることができます。 許容される円弧リングは、次の要件を満たしています。  
  
1.  許容される `LineString` インスタンス、`CircularString` インスタンス、または `CompoundCurve` インスタンスであること。 許容されるインスタンスの詳細については、「 [LineString](linestring.md)」、「 [CircularString](circularstring.md)」、および「 [CompoundCurve](compoundcurve.md)」を参照してください。  
  
2.  4 つ以上の点があること。  
  
3.  始点および終点の X 座標と Y 座標が同じであること。  
  
    > [!NOTE]  
    >  Z 値および M 値は無視されます。  
  
 次の例は許容される`CurvePolygon`インスタンス。  
  
```  
DECLARE @g1 geometry = 'CURVEPOLYGON EMPTY';  
DECLARE @g2 geometry = 'CURVEPOLYGON((0 0, 0 0, 0 0, 0 0))';  
DECLARE @g3 geometry = 'CURVEPOLYGON((0 0 1, 0 0 2, 0 0 3, 0 0 3))'  
DECLARE @g4 geometry = 'CURVEPOLYGON(CIRCULARSTRING(1 3, 3 5, 4 7, 7 3, 1 3))';  
DECLARE @g5 geography = 'CURVEPOLYGON((-122.3 47, 122.3 -47, 125.7 -49, 121 -38, -122.3 47))';  
```  
  
 `@g3` 始点と終点の Z 値が異なりますが Z 値は無視されるため、許容されます。 `@g5` は、`geography` 型インスタンスは無効ですが、許容されます。  
  
 次の例では、 `System.FormatException`がスローされます。  
  
```  
DECLARE @g1 geometry = 'CURVEPOLYGON((0 5, 0 0, 0 0, 0 0))';  
DECLARE @g2 geometry = 'CURVEPOLYGON((0 0, 0 0, 0 0))';  
```  
  
 `@g1` 始点と終点の Y 値が同じでないため、許容されません。 `@g2` リングに十分な数の点が含まれていないため、許容されません。  
  
### <a name="valid-instances"></a>有効なインスタンス  
 `CurvePolygon`を有効にするインスタンス両方外部リングと内部リングは、次の条件を満たす必要があります。  
  
1.  1 つの接点でのみ接していること。  
  
2.  互いに交差していないこと。  
  
3.  それぞれのリングに 4 つ以上の点が含まれていること。  
  
4.  それぞれのリングは許容される curve 型であること。  
  
 `CurvePolygon` インスタンスがいるかどうかに応じて、特定の条件を満たすためにも必要がある`geometry`または`geography`データ型。  
  
#### <a name="geometry-data-type"></a>geometry データ型  
 有効な **geometryCurvePolygon** インスタンスは、次の属性を持つ必要があります。  
  
1.  すべての内部リングが外部リング内に含まれていること。  
  
2.  複数の内部リングを含むことはできますが、内部リング内に別の内部リングを含めることはできません。  
  
3.  リングがそれ自体または別のリングと交差していないこと。  
  
4.  リングどうしは、1 つの接点でのみ接することができます (リングが接する点の数は有限であることが必要です)。  
  
5.  多角形の内部が接続されていること。  
  
 次の例は、有効な **geometryCurvePolygon** インスタンスを示しています。  
  
```  
DECLARE @g1 geometry = 'CURVEPOLYGON EMPTY';  
DECLARE @g2 geometry = 'CURVEPOLYGON(CIRCULARSTRING(1 3, 3 5, 4 7, 7 3, 1 3))';  
SELECT @g1.STIsValid(), @g2.STIsValid();  
```  
  
 CurvePolygon インスタンスには Polygon インスタンスと同じ妥当性規則が適用されますが、例外として、CurvePolygon インスタンスでは新しい円弧型が許容されます。 他の有効または無効なインスタンスの例については、「 [Polygon](polygon.md)」を参照してください。  
  
#### <a name="geography-data-type"></a>geography データ型  
 有効な **geographyCurvePolygon** インスタンスは、次の属性を持つ必要があります。  
  
1.  多角形の内部が左辺ルールを使用して接続されている必要があります。  
  
2.  リングがそれ自体または別のリングと交差していないこと。  
  
3.  リングどうしは、1 つの接点でのみ接することができます (リングが接する点の数は有限であることが必要です)。  
  
4.  多角形の内部が接続されていること。  
  
 次の例は、有効な geography CurvePolygon インスタンスを示しています。  
  
```  
DECLARE @g geography = 'CURVEPOLYGON((-122.3 47, 122.3 47, 125.7 49, 121 38, -122.3 47))';  
SELECT @g.STIsValid();  
```  
  
## <a name="examples"></a>使用例  
  
### <a name="a-instantiating-a-geometry-instance-with-an-empty-curvepolygon"></a>A. 空の CurvePolygon を使用して geometry インスタンスをインスタンス化する  
 次の例は、空の `CurvePolygon` インスタンスを作成する方法を示しています。  
  
```tsql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON EMPTY');  
```  
  
### <a name="b-declaring-and-instantiating-a-geometry-instance-with-a-curvepolygon-in-the-same-statement"></a>B. CurvePolygon を同じステートメント内で使用して geometry インスタンスを宣言およびインスタンス化する  
 このコード スニペットを宣言および使用して geometry インスタンスを初期化する方法を示しています、`CurvePolygon`同じステートメント内で。  
  
```tsql  
DECLARE @g geometry = 'CURVEPOLYGON(CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))'  
```  
  
### <a name="c-instantiating-a-geography-instance-with-a-curvepolygon"></a>C. CurvePolygon を使用して geography インスタンスをインスタンス化する  
 このコード スニペットを宣言して初期化する方法を示しています、`geography`インスタンス、 `CurvePolygon`:  
  
```tsql  
DECLARE @g geography = 'CURVEPOLYGON(CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
```  
  
### <a name="d-storing-a-curvepolygon-with-only-an-exterior-bounding-ring"></a>D. 外部境界リングのみを含む CurvePolygon を格納する  
 この例は、単純な円を `CurvePolygon` インスタンスに格納する方法を示しています (外部境界リングのみを使用して円が定義されています)。  
  
```tsql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))');  
SELECT @g.STArea() AS Area;  
```  
  
### <a name="e-storing-a-curvepolygon-containing-interior-rings"></a>E. 内部リングを含む CurvePolygon を格納する  
 この例は、内にドーナツを作成、`CurvePolygon`インスタンス (ドーナツを定義する、両方の外部境界リングと内部リングを使用します)。  
  
```tsql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 4, 4 0, 8 4, 4 8, 0 4), CIRCULARSTRING(2 4, 4 2, 6 4, 4 6, 2 4))');  
SELECT @g.STArea() AS Area;  
```  
  
 この例ではどちらも、有効な`CurvePolygon`インスタンスと内部リングを使用する場合は、無効なインスタンス。  
  
```tsql  
DECLARE @g1 geometry, @g2 geometry;  
SET @g1 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 5, 5 0, 0 -5, -5 0, 0 5), (-2 2, 2 2, 2 -2, -2 -2, -2 2))');  
IF @g1.STIsValid() = 1  
  BEGIN  
     SELECT @g1.STArea();  
  END  
SET @g2 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 5, 5 0, 0 -5, -5 0, 0 5), (0 5, 5 0, 0 -5, -5 0, 0 5))');  
IF @g2.STIsValid() = 1  
  BEGIN  
     SELECT @g2.STArea();  
  END  
SELECT @g1.STIsValid() AS G1, @g2.STIsValid() AS G2;  
```  
  
 @g1 と @g2 はどちらも同じ外部境界リング (半径 5 の円) を使用し、内部リングに正方形を使用しています。  しかし、インスタンス @g1 は有効ですが、インスタンス @g2 は無効です。  @g2 が無効な理由は、外部リングによって範囲指定された内部空間が内部リングによって 4 つの別個の領域に分割されているためです。  この状況を次の図に示します。  
  
## <a name="see-also"></a>参照  
 [Polygon](polygon.md)   
 [CircularString](circularstring.md)   
 [CompoundCurve](compoundcurve.md)   
 [geometry データ型メソッド リファレンス](/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql)   
 [geography データ型メソッド リファレンス](/sql/t-sql/spatial-geography/spatial-types-geography)   
 [空間データ型の概要](spatial-data-types-overview.md)  
  
  