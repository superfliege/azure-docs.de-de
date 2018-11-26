---
title: Nachverfolgen der Nutzung eines Labs in Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial verfolgen Sie als Lab-Ersteller/-Besitzer die Nutzung Ihres Labs nach.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/14/2018
ms.author: spelluru
ms.openlocfilehash: 49d5761e3e37e1265938d1f1b27324de667a13ca
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51707094"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Tutorial: Nachverfolgen der Nutzung eines Labs in Azure Lab Services
In diesem Tutorial wird gezeigt, wie ein Lab-Ersteller/-Besitzer die Nutzung eines Labs nachverfolgen kann.

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Anzeigen der beim Lab registrierten Benutzer
> * Anzeigen der Nutzung der virtuellen Computer im Lab
> * Verwalten von Studenten-VMs 


## <a name="view-users-registered-with-the-lab"></a>Anzeigen der beim Lab registrierten Benutzer

1. Navigieren Sie zur Website [Azure Lab Services](https://labs.azure.com). 
2. Wählen Sie **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein. Azure Lab Services unterstützt Geschäfts-, Schul- oder Unikonten und Microsoft-Konten.
3. Wählen Sie auf der Seite **My labs** (Meine Labs) das Lab aus, dessen Nutzung Sie nachverfolgen möchten. 
4. Klicken Sie auf die Registerkarte **Benutzer**. Daraufhin werden bei Ihrem Lab registrierte Studenten angezeigt. Klicken Sie auf **Registration link** (Registrierungslink), kopieren Sie den Link, und senden Sie ihn an einen neuen Studenten, der noch nicht bei Ihrem Lab registriert ist. 

    ![Registrierte Benutzer](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Anzeigen der Nutzung der virtuellen Computer im Lab 

1. Wählen Sie im Menü auf der linken Seite die Option **Virtuelle Computer** aus. 
2. Überprüfen Sie, ob Sie den Status der virtuellen Computer und die Ausführungszeit der virtuellen Computer anzeigen können. Die für die VM eines Teilnehmers aufgewendete Zeit zählt nicht zur Nutzungszeit in der letzten Spalte. 

    ![VM-Nutzung](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Verwalten von Studenten-VMs 
Wenn Sie mit der Maus auf eine Zeile in der Liste der VMs zeigen, werden Steuerelemente zum Ausführen der folgenden Aufgaben angezeigt (siehe hierzu die Abbildung im vorherigen Abschnitt): 

- Herstellen einer Verbindung mit einem virtuellen Computer
- Starten eines virtuellen Computers
- Anhalten eines virtuellen Computers
- Löschen eines virtuellen Computers



## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Classroom-Labs finden Sie in den Artikeln unter [Anleitungen](how-to-manage-lab-accounts.md).
