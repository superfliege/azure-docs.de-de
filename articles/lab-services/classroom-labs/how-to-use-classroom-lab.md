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
ms.date: 10/17/2018
ms.author: spelluru
ms.openlocfilehash: 4b137396dd6a8ff924c9380aeb87a81b95f91414
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/19/2018
ms.locfileid: "49466221"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Zugreifen auf ein Classroom-Lab in Azure Lab Services
In diesem Artikel wird beschrieben, wie Sie auf ein Classroom-Lab zugreifen, eine Verbindung mit der VM im Lab herstellen und die VM beenden können. 

## <a name="view-all-the-classroom-labs"></a>Anzeigen aller Classroom-Labs

1. Navigieren Sie zur **Registrierungs-URL**, die Sie von der Lehrkraft erhalten haben. 
2. Melden Sie sich mit Ihrem Schul- oder Unikonto beim Dienst an, um die Registrierung abzuschließen. 
3. Vergewissern Sie sich nach der Registrierung, dass Sie die virtuellen Computer für die Labs sehen, auf die Sie Zugriff haben. 

    ![Anzeigen aller Labs](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Herstellen einer Verbindung mit dem virtuellen Computer in einem Classroom-Lab

1. Klicken Sie auf der Kachel des virtuellen Computers des Labs, auf den Sie zugreifen möchten, auf **Verbinden**.

    ![Anzeigen aller Labs](../media/how-to-use-classroom-lab/connect-button.png)
2. Es dauert einige Minuten, bis der virtuelle Computer betriebsbereit ist.

    ![Virtuellen Computer betriebsbereit machen](../media/how-to-use-classroom-lab/getting-virtual-machine-ready.png)
3. Speichern Sie die RDP-Datei auf der Festplatte, und öffnen Sie sie. 
    
    ![Speichern der RDP-Datei](../media/how-to-use-classroom-lab/save-rdp-file.png)
4. Verwenden Sie den **Benutzernamen** und das **Kennwort**, den bzw. das Sie von Ihrer Lehrkraft erhalten haben, um sich beim Computer anzumelden. 

## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>Beenden des virtuellen Computers in einem Classroom-Lab

Klicken Sie auf der Kachel, die das Classroom-Lab darstellt, auf **Beenden**. Nach Beenden der VM wird die Schaltfläche **Starten** auf der Kachel aktiviert. 

## <a name="next-steps"></a>Nächste Schritte
Machen Sie sich mit der Einrichtung eines Labs mit Azure Lab Services vertraut:

- [Einrichten eines Classroom-Labs](how-to-manage-classroom-labs.md)
- [Einrichten eines Labs](../tutorial-create-custom-lab.md)

 