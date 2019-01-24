---
title: Zugreifen auf ein Classroom-Lab in Azure Lab Services | Microsoft-Dokumentation
description: In diesem Tutorial greifen Sie in einem Classroom-Lab, das von einer Lehrkraft eingerichtet wurde, auf virtuelle Computer zu.
services: devtest-lab, lab-services, virtual-machines
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
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 8ea1afbfb90a55ec1c34f5c59cf38ea5417118ff
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54390909"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Zugreifen auf ein Classroom-Lab in Azure Lab Services
In diesem Artikel wird beschrieben, wie Sie auf ein Classroom-Lab zugreifen, eine Verbindung mit der VM im Lab herstellen und die VM beenden können. 

## <a name="register-to-a-lab"></a>Registrieren bei einem Lab
1. Navigieren Sie zur **Registrierungs-URL**, die Sie von der Lehrkraft erhalten haben. 
2. Melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto beim Dienst an, um die Registrierung abzuschließen. 
3. Vergewissern Sie sich nach der Registrierung, dass Sie den virtuellen Computer für das Lab sehen, auf das Sie Zugriff haben. 
2. Warten Sie, bis der virtuelle Computer bereit ist, und **starten** Sie dann den virtuellen Computer. Dieser Prozess nimmt einige Zeit in Anspruch.  

    ![Starten der VM](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)


## <a name="view-all-the-classroom-labs"></a>Anzeigen aller Classroom-Labs
Nachdem Sie sich bei den Labs registriert haben, können Sie alle Classroom-Labs anzeigen, indem Sie die folgenden Schritte ausführen: 

1. Navigieren Sie zu [https://labs.azure.com](https://labs.azure.com). 
2. Melden Sie sich mit dem Benutzerkonto, das Sie zum Registrieren beim Lab verwendet haben, beim Dienst an. 
3. Überprüfen Sie, ob alle Labs angezeigt werden, auf die Sie Zugriff haben. 

    ![Anzeigen aller Labs](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Herstellen einer Verbindung mit dem virtuellen Computer in einem Classroom-Lab

1. Wählen Sie **Starten** aus, um die VM zu starten (sofern noch nicht geschehen).
2. Klicken Sie auf der Kachel des virtuellen Computers des Labs, auf den Sie zugreifen möchten, auf **Verbinden**. 

    ![Anzeigen aller Labs](../media/how-to-use-classroom-lab/connect-button.png)
3. Speichern Sie die RDP-Datei (für eine Windows-VM) auf der Festplatte, und öffnen Sie sie. 
4. Verwenden Sie den **Benutzernamen** und das **Kennwort**, den bzw. das Sie von Ihrer Lehrkraft erhalten haben, um sich beim Computer anzumelden. 

## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>Beenden des virtuellen Computers in einem Classroom-Lab

Klicken Sie auf der Kachel auf **Beenden**, um die VM zu beenden. Nach Beenden der VM wird die Schaltfläche **Starten** auf der Kachel aktiviert. 

## <a name="next-steps"></a>Nächste Schritte
Entsprechende Informationen finden Sie in den folgenden Artikeln:

- [Erstellen und Verwalten von Labkonten als Administrator](how-to-manage-lab-accounts.md)
- [Erstellen und Verwalten von Labs als Labbesitzer](how-to-manage-classroom-labs.md)
- [Einrichten und Veröffentlichen von Vorlagen als Labbesitzer](how-to-create-manage-template.md)
- [Konfigurieren und Steuern der Nutzung eines Labs als Labbesitzer](how-to-configure-student-usage.md)
 