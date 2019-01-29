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
ms.openlocfilehash: 1328835714086dcec71b0e9dd4d1916794f557a6
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2019
ms.locfileid: "54390179"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Tutorial: Zugreifen auf ein Classroom-Lab in Azure Lab Services
In diesem Tutorial stellen Sie als Kursteilnehmer eine Verbindung mit einem virtuellen Computer in einem Classroom-Lab her. 

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Verwenden des Registrierungslinks 
> * Herstellen einer Verbindung mit dem virtuellen Computer

## <a name="use-the-registration-link"></a>Verwenden des Registrierungslinks

1. Navigieren Sie zur **Registrierungs-URL**, die Sie von der Lehrkraft erhalten haben. 
2. Melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto beim Dienst an, um die Registrierung abzuschließen. 
3. Vergewissern Sie sich nach der Registrierung, dass Sie den virtuellen Computer für das Lab sehen, auf das Sie Zugriff haben. 
2. Warten Sie, bis der virtuelle Computer bereit ist, und **starten** Sie dann den virtuellen Computer. Dieser Prozess nimmt einige Zeit in Anspruch.  

    ![Starten der VM](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer

1. Klicken Sie auf der Kachel für die VM des Labs, auf das Sie zugreifen möchten, auf **Verbinden**. 

    ![Herstellen einer Verbindung mit dem virtuellen Computer](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Speichern Sie die RDP-Datei auf der Festplatte, und öffnen Sie sie (vorausgesetzt, dass es sich um eine Windows-VM handelt).
3. Verwenden Sie den **Benutzernamen** und das **Kennwort**, den bzw. das Sie von Ihrer Lehrkraft erhalten haben, um sich beim Computer anzumelden. 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie über einen Registrierungslink, den Sie von Ihrem Lehrer bzw. Dozenten erhalten haben, auf ein Classroom-Lab zugegriffen.

Als Besitzer des Labs möchten Sie die beim Lab registrierten Personen anzeigen und die Nutzung der VMs nachverfolgen. Im nächsten Tutorial erfahren Sie, wie Sie die Nutzung des Labs nachverfolgen:

> [!div class="nextstepaction"]
> [Tutorial: Nachverfolgen der Nutzung eines Labs in Azure Lab Services](tutorial-track-usage.md) 