---
title: 他のアプリケーションからのレポートの印刷 (レポート ビルダーおよび SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology:
- reporting-services-native
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: a5560581-fd57-4a45-b7ea-43b21a8a7419
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: craigg
ms.openlocfilehash: 380bebaa85da73108af8611ac537565dc5d91d00
ms.sourcegitcommit: c18fadce27f330e1d4f36549414e5c84ba2f46c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2018
ms.locfileid: "37278858"
---
# <a name="print-reports-from-other-applications-report-builder-and-ssrs"></a>他のアプリケーションからのレポートの印刷 (レポート ビルダーおよび SSRS)
  レポート ビルダーには、他のアプリケーションでレポートを簡単に表示できるエクスポート オプションが用意されています。 `Export`コマンドは、ブラウザーまたは Web ベースのアプリケーションで開くと、レポートの上部に表示されるレポート ツールバーで使用できます。 レポートをエクスポートすると、レポートは別のアプリケーションで表示されます。たとえば、レポートを Excel にエクスポートすると、 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)]でレポートが開きます。 印刷を実行する場合は、必要な印刷機能がアプリケーションに備わっている場合にのみレポートのエクスポートをお勧めします。  
  
 レポートを別のアプリケーションにエクスポートするには、該当するアプリケーションがインストールされている必要があります。 たとえば、Acrobat (PDF) 形式でエクスポートするには、コンピューターにあらかじめ Adobe Acrobat Reader がインストールされている必要があります。 TIFF 形式でレポートをエクスポートする場合は、レポート サーバーにより、TIFF ファイル形式に関連付けられている表示アプリケーションでレポートが開かれます。 インストールされている [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows のバージョンにより異なりますが、この表示アプリケーションには、通常 Windows 画像と FAX ビューアーが使用されます。 既定の解像度は、画面の解像度に合わせて、96 DPI になります。 Windows 画像と Fax ビューアーの解像度は、プリンターの機能に合わせて、300 DPI または 600 DPI に上げることができます。 解像度の調整方法の詳細については、Windows の製品マニュアルを参照してください。  
  
 Web アーカイブ (MHTML) を選択した場合、レポートは既定のブラウザーにエクスポートされます。 ブラウザーから印刷すると、レポートのパス情報がすべてのページの下部に追加される場合があります。 多くの場合、ブラウザーのオプションを設定して、印刷ページにパス情報が印刷されないように設定できます。 詳細については、使用しているブラウザーの製品マニュアルを参照してください。  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="see-also"></a>参照  
 [レポートの印刷 &#40;レポート ビルダーおよび SSRS&#41;](print-a-report-report-builder-and-ssrs.md)   
 [印刷コントロールを使用したブラウザーからのレポートの印刷 &#40;レポート ビルダーおよび SSRS&#41;](print-reports-from-a-browser-with-the-print-control-report-builder-and-ssrs.md)   
 [レポートのエクスポート&#40;レポート ビルダーおよび SSRS&#41;](export-reports-report-builder-and-ssrs.md)   
 [別の種類のファイルとしてレポートをエクスポートする &#40;レポート ビルダーおよび SSRS&#41;](../export-a-report-as-another-file-type-report-builder-and-ssrs.md)   
 [レポートの検索、表示、管理 &#40;レポート ビルダーおよび SSRS&#41;](finding-viewing-and-managing-reports-report-builder-and-ssrs.md)  
  
  