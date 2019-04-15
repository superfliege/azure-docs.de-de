---
title: Verwenden eines Katalogs mit freigegebenen Images in Azure Lab Services | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Labkonto für die Verwendung eines Katalogs mit freigegebenen Images konfigurieren, sodass ein Benutzer ein Image für andere freigeben und ein anderer Benutzer mithilfe dieses Images eine Vorlage für virtuelle Computer im Lab erstellen kann.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: spelluru
ms.openlocfilehash: 93136c7d685bd9fc8ec4bcdea3a900b28029059b
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652846"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Verwenden eines Katalogs mit freigegebenen Images in Azure Lab Services
In diesem Artikel wird gezeigt, wie Lehrkräfte/Labadministratoren ein VM-Vorlagenimage speichern können, damit es von anderen wiederverwendet werden kann. Diese Images werden in einem [Katalog mit freigegebenen Images](../../virtual-machines/windows/shared-image-galleries.md) von Azure gespeichert. Als Erstes fügt der Labadministrator einen vorhandenen Katalog mit freigegebenen Images an das Labkonto an. Nachdem der Katalog mit freigegebenen Images angefügt wurde, können in Labs, die unter dem Labkonto erstellt wurden, Images in dem Katalog mit freigegebenen Images gespeichert werden. Andere Lehrkräfte können dieses Image im Katalog mit freigegebenen Images auswählen, um eine Vorlage für ihre Klassen zu erstellen. 

## <a name="prerequisites"></a>Voraussetzungen
Erstellen Sie mithilfe von [Azure PowerShell](../../virtual-machines/windows/shared-images.md) oder über die [Azure-Befehlszeilenschnittstelle](../../virtual-machines/linux/shared-images.md) einen Katalog mit freigegebenen Images.

## <a name="attach-a-shared-image-gallery-to-a-lab-account"></a>Anfügen eines Katalogs mit freigegebenen Images an ein Labkonto
Gehen Sie wie folgt vor, um einen Katalog mit freigegebenen Images an ein Labkonto anzufügen: 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü links **Alle Dienste** aus. Wählen Sie im Abschnitt **DEVOPS** die Option **Lab-Dienste** aus. Wenn Sie das Sternchen (`*`) neben **Lab-Dienste** auswählen, wird die Option im linken Menü dem Abschnitt **FAVORITEN** hinzugefügt. Beim nächsten Mal wählen Sie **Lab-Dienste** unter **FAVORITEN** aus.

    ![Alle Dienste -> Lab-Dienste](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Wählen Sie Ihr Labkonto aus, um die Seite **Labkonto** anzuzeigen. 
4. Wählen Sie im linken Menü die Option **Katalog mit freigegebenen Images** und anschließend auf der Symbolleiste die Option **Anfügen** aus. 

    ![Katalog mit freigegebenen Images: Schaltfläche zum Hinzufügen](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. Wählen Sie auf der Seite **Vorhandenen Katalog mit freigegebenen Images anfügen** Ihren Katalog mit freigegebenen Images und anschließend **OK** aus.

    ![Auswählen eines vorhandenen Katalogs](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. Daraufhin wird der folgende Bildschirm angezeigt: 

    ![Eigener Katalog in der Liste](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    In diesem Beispiel enthält der Katalog mit freigegebenen Images noch keine Images.

Die Azure Lab Services-Identität wird als Mitwirkender dem an das Lab angefügten Katalog mit freigegebenen Images hinzugefügt. Dadurch können Lehrkräfte/IT-Administratoren VM-Images im Katalog mit freigegebenen Images speichern. Alle Labs, die unter diesem Labkonto erstellt wurden, haben Zugriff auf den angefügten Katalog mit freigegebenen Images. 

Alle Images im angefügten Katalog mit freigegebenen Images werden standardmäßig aktiviert. Sie können Images aktivieren oder deaktivieren, indem Sie sie in der Liste auswählen und die Schaltfläche **Ausgewählte Images aktivieren** oder **Ausgewählte Images deaktivieren** verwenden. 

## <a name="detach-a-shared-image-gallery"></a>Trennen eines Katalogs mit freigegebenen Images
An ein Lab kann immer nur ein einzelner Katalog mit freigegebenen Images angefügt sein. Wenn Sie einen anderen Katalog mit freigegebenen Images anfügen möchten, müssen Sie zuerst den aktuellen Katalog trennen. Wenn Sie einen Katalog mit freigegebenen Images von Ihrem Lab trennen möchten, wählen Sie auf der Symbolleiste die Option **Trennen** aus, und bestätigen Sie den Vorgang. 

## <a name="save-an-image-to-the-shared-image-gallery"></a>Speichern eines Images im Katalog mit freigegebenen Images
Nachdem ein Katalog mit freigegebenen Images angefügt wurde, kann eine Lehrkraft ein Vorlagenimage in dem Katalog mit freigegebenen Images speichern, das dann von anderen Lehrkräften wiederverwendet werden kann.

![Speichern eines VM-Images im Katalog](../media/how-to-use-shared-image-gallery/save-virtual-machine.png)

## <a name="use-an-image-from-the-shared-image-gallery"></a>Verwenden eines Images aus dem Katalog mit freigegebenen Images
Lehrer/Dozenten können beim Erstellen eines neuen Labs ein benutzerdefiniertes Image aus dem Katalog mit freigegebenen Images als Vorlage auswählen.

![Verwenden eines VM-Images aus dem Katalog](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Katalogen mit freigegebenen Images finden Sie in der [Übersicht über den Katalog mit freigegebenen Images](../../virtual-machines/windows/shared-image-galleries.md).
