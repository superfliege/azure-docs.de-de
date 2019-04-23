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
ms.openlocfilehash: 3be1da54b16a24ce3c4431dfe26eb778cea5c83d
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2019
ms.locfileid: "59545262"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Tutorial: Zugreifen auf ein Classroom-Lab in Azure Lab Services
In diesem Tutorial stellen Sie als Kursteilnehmer eine Verbindung mit einem virtuellen Computer in einem Classroom-Lab her. 

In diesem Tutorial führen Sie die folgenden Aktionen aus:

> [!div class="checklist"]
> * Verwenden des Registrierungslinks 
> * Herstellen einer Verbindung mit dem virtuellen Computer

## <a name="use-the-registration-link"></a>Verwenden des Registrierungslinks

1. Navigieren Sie zur **Registrierungs-URL**, die Sie von der Lehrkraft erhalten haben. Die Registrierungs-URL muss nach Abschluss der Registrierung nicht mehr verwendet werden. Verwenden Sie stattdessen die folgende URL: [https://labs.azure.com](https://labs.azure.com). Beachten Sie den Hinweis, dass Internet Explorer 11 noch nicht unterstützt wird. 
1. Melden Sie sich mit Ihrem Geschäfts-, Schul- oder Unikonto beim Dienst an, um die Registrierung abzuschließen. 
2. Vergewissern Sie sich nach der Registrierung, dass Sie den virtuellen Computer für das Lab sehen, auf das Sie Zugriff haben. 
3. Warten Sie, bis der virtuelle Computer bereit ist, und **starten** Sie dann den virtuellen Computer. Dieser Prozess nimmt einige Zeit in Anspruch.  

    ![Starten der VM](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Herstellen einer Verbindung mit dem virtuellen Computer

1. Klicken Sie auf der Kachel für die VM des Labs, auf das Sie zugreifen möchten, auf **Verbinden**. 

    ![Herstellen einer Verbindung mit dem virtuellen Computer](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Führen Sie einen der folgenden Schritte aus: 
    1. Speichern Sie für virtuelle **Windows**-Computer die **RDP**-Datei auf der Festplatte. Öffnen Sie die RDP-Datei, um eine Verbindung mit dem virtuellen Computer herzustellen. Verwenden Sie die Kombination aus **Benutzername** und **Kennwort**, die Sie von Ihrer Lehrkraft erhalten haben, um sich bei dem Computer anzumelden. 
    3. Bei virtuellen **Linux**-Computern können Sie **SSH** oder **RDP** (sofern RDP aktiviert ist) für die Verbindungsherstellung verwenden. Weitere Informationen finden Sie unter [Enable and use remote desktop for Linux virtual machines in a lab in Azure Lab Services](how-to-enable-remote-desktop-linux.md) (Aktivieren und Verwenden von Remotedesktop für virtuelle Linux-Computer in einem Lab in Azure Lab Services). 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie über einen Registrierungslink, den Sie von Ihrem Lehrer bzw. Dozenten erhalten haben, auf ein Classroom-Lab zugegriffen.

Als Besitzer des Labs möchten Sie die beim Lab registrierten Personen anzeigen und die Nutzung der VMs nachverfolgen. Im nächsten Tutorial erfahren Sie, wie Sie die Nutzung des Labs nachverfolgen:

> [!div class="nextstepaction"]
> [Tutorial: Nachverfolgen der Nutzung eines Labs in Azure Lab Services](tutorial-track-usage.md) 
