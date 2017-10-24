---
title: "Installationsanweisungen für die Azure Stream Analytics-Tools für Visual Studio | Microsoft-Dokumentation"
description: "Installationsanweisungen für die Azure Stream Analytics-Tools für Visual Studio"
keywords: Visual Studio
documentationcenter: 
services: stream-analytics
author: su-jie
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 9/19/2017
ms.author: sujie
ms.openlocfilehash: 88abf40cefaca150c67e3a1068006fb0fa254305
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="installation-instructions-for-stream-analytics-tools-for-visual-studio"></a>Installationsanweisungen für die Stream Analytics-Tools für Visual Studio
Die Stream Analytics-Tools unterstützen jetzt Visual Studio 2017, 2015 und 2013. Dieses Dokument enthält eine Einführung in die Installation und Deinstallation der Tools.

[Erfahren Sie, wie Sie die Azure Stream Analytics-Tools für Visual Studio verwenden](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-tools-for-visual-studio).

## <a name="install"></a>Installieren
### <a name="visual-studio-2017"></a>Visual Studio 2017
* Laden Sie [Visual Studio 2017 (15.3 oder höher)](https://www.visualstudio.com/) herunter. Die Editionen Enterprise (Ultimate/Premium), Professional und Community werden unterstützt; Express Edition wird nicht unterstützt. 
* Die Stream Analytics-Tools sind Teil der Workloads „Azure-Entwicklung“ und „Datenspeicherung und -verarbeitung“ in Visual Studio 2017. Aktivieren Sie eine dieser beiden Workloads im Rahmen Ihrer Visual Studio-Installation.

![Installieren der Stream Analytics-Tools für Visual Studio 1](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-2017-install-01.png)
![Installieren der Stream Analytics-Tools für Visual Studio 2](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-2017-install-02.png)


### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
* Installieren Sie Visual Studio 2015 oder Visual Studio 2013 Update 4. Die Editionen Enterprise (Ultimate/Premium), Professional und Community werden unterstützt; Express Edition wird nicht unterstützt. 
* Installieren Sie Microsoft Azure SDK für .NET Version 2.7.1 oder höher mit dem [Webplattform-Installer](http://www.microsoft.com/web/downloads/platform.aspx).
* Installieren Sie die [Azure Stream Analytics-Tools für Visual Studio](http://aka.ms/asatoolsvs).



## <a name="update"></a>Aktualisieren

### <a name="visual-studio-2017"></a>Visual Studio 2017
Die neue Versionserinnerung wird in der Visual Studio-Benachrichtigung angezeigt. 

### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
Die installierten Stream Analytics-Tools für Visual Studio suchen automatisch nach neuen Versionen, und Sie können die Anweisungen im Popupfenster befolgen, um die neueste Version zu installieren. 


## <a name="uninstall"></a>Deinstallieren

### <a name="visual-studio-2017"></a>Visual Studio 2017
Doppelklicken Sie auf den Visual Studio-Installer, und wählen Sie **Ändern** aus. Deaktivieren Sie **Azure Data Lake- und Stream Analytics-Tools** in einer der Workloads **Datenspeicherung und -verarbeitung** oder **Azure-Entwicklung**.

### <a name="visual-studio-2013-2015"></a>Visual Studio 2013, 2015
Wechseln Sie zur Systemsteuerung, und deinstallieren Sie „Microsoft Azure Data Lake- und Stream Analytics-Tools für Visual Studio“.





