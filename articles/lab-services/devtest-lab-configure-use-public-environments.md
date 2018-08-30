---
title: Konfigurieren und Verwenden von öffentlichen Umgebungen in Azure DevTest Labs | Microsoft-Dokumentation
description: Erfahren Sie mehr zum Konfigurieren und Verwenden von öffentlichen Umgebungen in Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/13/2018
ms.author: spelluru
ms.openlocfilehash: d93818cd875c4050b1b35f21ce580933776c5bc5
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234687"
---
# <a name="configure-and-use-public-environments-in-azure-devtest-labs"></a>Konfigurieren und Verwenden von Umgebungen in Azure DevTest Labs
Azure DevTest Labs verfügt über ein [öffentliches Repository von Azure Resource Manager-Vorlagen](https://github.com/Azure/azure-devtestlab/tree/master/Environments), die Sie zum Erstellen von Umgebungen verwenden können, ohne selbst eine Verbindung zu einer externen GitHub-Quelle herstellen zu müssen. Dieses Repository enthält häufig verwendete Vorlagen wie Azure-Web-Apps, Service Fabric-Cluster und SharePoint Farm-Entwicklungsumgebungen. Dieses Feature ähnelt dem öffentlichen Repository von Artefakten, das für jedes von Ihnen erstellte Lab enthalten ist. Das Umgebungsrepository ermöglicht Ihnen einen schnellen und reibungslosen Einstieg mit zuvor erstellten Umgebungsvorlagen mit minimalen Eingabeparametern für PaaS-Ressourcen in Labs. 

## <a name="configuring-public-environments"></a>Konfigurieren von öffentlichen Umgebungen
Als Lab-Besitzer können Sie das öffentliche Repository für Ihr Lab während der Erstellung des Labs aktivieren. Um öffentliche Umgebungen für Ihr Lab zu aktivieren, klicken Sie beim Erstellen eines Labs im Feld **Öffentliche Umgebungen** auf das Feld **Ein**. 

![Aktivieren Sie die öffentliche Umgebung für ein neues Lab.](media/devtest-lab-configure-use-public-environments/enable-public-environment-new-lab.png)


Für bereits vorhandene Labs ist das Repository für öffentliche Umgebungen nicht aktiviert. Aktivieren Sie es manuell, um Vorlagen im Repository zu verwenden. Für Labs, die mit Resource Manager-Vorlagen erstellt werden, ist das Repository ebenfalls standardmäßig deaktiviert.

Sie können öffentliche Umgebungen für Ihr Lab aktivieren/deaktivieren und mit den folgenden Schritten auch nur bestimmte Umgebungen für Lab-Benutzer zur Verfügung stellen: 

1. Wählen Sie **Konfiguration und Richtlinien** für Ihr Lab aus. 
2. Wählen Sie im Abschnitt **BASEN VIRTUELLER COMPUTER** **Öffentliche Umgebungen** aus.
3. Klicken Sie auf **Ja**, um öffentliche Umgebungen für das Lab zu aktivieren. Klicken Sie andernfalls auf **Nein**. 
4. Wenn Sie öffentliche Umgebungen aktiviert haben, werden alle Umgebungen im Repository standardmäßig aktiviert. Sie können die Auswahl für eine Umgebung aufheben, um diese für Lab-Benutzer nicht verfügbar zu machen. 

![Seite „Öffentliche Umgebungen“](media/devtest-lab-configure-use-public-environments/public-environments-page.png)

## <a name="use-environment-templates-as-a-lab-user"></a>Verwenden von Umgebungsvorlagen als Lab-Benutzer
Als Lab-Benutzer können Sie eine neue Umgebung aus der aktivierten Liste von Umgebungsvorlagen erstellen, indem Sie einfach auf **+Hinzufügen** auf der Symbolleiste der Lab-Seite klicken. Die Liste von Basen enthält die Vorlagen von öffentlichen Umgebungen, die von Ihrem Lab-Administrator am oberen Rand der Liste aktiviert wurden.

![Vorlagen für öffentliche Umgebungen](media/devtest-lab-configure-use-public-environments/public-environment-templates.png)

## <a name="next-steps"></a>Nächste Schritte
Dies ist ein Open Source-Repository, an dem Sie mitwirken können, indem Sie eigene häufig verwendete und nützliche Resource Manager-Vorlagen hinzufügen. Übermitteln Sie einfach einen Pull Request für das Repository, um mitzuwirken.  
