---
title: Posten einer Ankündigung in einem Lab in Azure DevTest Labs | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie eine Ankündigung in einem Lab in Azure DevTest Labs posten.
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 3282a90069ecd119154df492ac6dc366d26b5300
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611204"
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Posten einer Ankündigung in einem Lab in Azure DevTest Labs

Als Lab-Administrator können Sie eine benutzerdefinierte Ankündigung in einem vorhandenen Lab posten, um Benutzer über kürzlich vorgenommene Änderungen oder Hinzufügungen zum Lab zu benachrichtigen. Es kann beispielsweise sein, dass Sie Benutzer über Folgendes informieren möchten:

- Neue verfügbare VM-Größen
- Derzeit nicht verwendbare Images
- Aktualisierungen von Lab-Richtlinien

Nach dem Posten wird die Ankündigung auf der Seite „Übersicht“ des Labs angezeigt und kann von Benutzern ausgewählt werden, um weitere Details einzublenden.

Das Ankündigungsfeature ist für vorübergehende Ankündigungen gedacht.  Sie können eine Ankündigung leicht deaktivieren, wenn sie nicht mehr benötigt wird.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Schritte zum Bereitstellen einer Ankündigung in einem vorhandenen Lab

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie ggf. **Alle Dienste** und dann in der Liste die Option **DevTest Labs**. (Ihr Lab wird im Dashboard unter Umständen bereits unter **Alle Ressourcen** angezeigt.)
1. Wählen Sie in der Liste mit den Labs das Lab aus, in dem Sie eine Ankündigung posten möchten.  
1. Wählen Sie im Bereich **Übersicht** des Labs die Option **Konfiguration und Richtlinien** aus.  

    ![Schaltfläche „Konfiguration und Richtlinien“](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. Wählen Sie auf der linken Seite unter **EINSTELLUNGEN** die Option **Labankündigung**.

    ![Schaltfläche „Labankündigung“](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Legen Sie **Aktiviert** auf **Ja** fest, um eine Nachricht für die Benutzer dieses Labs zu erstellen.

1. Sie können unter **Ablaufdatum** ein Datum und eine Uhrzeit eingeben, nach deren Ablauf die Ankündigung Benutzern nicht mehr angezeigt wird. Wenn Sie kein Ablaufdatum eingeben, wird die Ankündigung erst ausgeblendet, wenn Sie sie deaktivieren.

   > [!NOTE]
   > Nach Ablauf der Ankündigung wird sie Benutzern nicht mehr angezeigt, ist aber noch im Bereich **Labankündigung** vorhanden. Sie können Änderungen daran vornehmen und diese wieder aktivieren.
   >
   >

1. Geben Sie einen **Ankündigungstitel** und den **Ankündigungstext** ein.

   Der Titel kann bis zu 100 Zeichen enthalten und wird auf der Seite „Übersicht“ des Labs für Benutzer angezeigt. Wenn Benutzer den Titel auswählen, wird der Ankündigungstext angezeigt.

   Für den Ankündigungstext kann Markdown verwendet werden. Beim Eingeben des Ankündigungstexts können Sie die Nachricht unten auf dem Bildschirm im Vorschaubereich anzeigen.

    ![Bildschirm mit Labankündigung zur Erstellung der Nachricht](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Wählen Sie **Speichern**, nachdem Ihre Ankündigung bereit zum Posten ist.

Wenn diese Ankündigung den Lab-Benutzern nicht mehr angezeigt werden soll, können Sie auf der Seite **Labankündigung** die Option **Aktiviert** auf **Nein** festlegen. Wenn Sie ein Ablaufdatum angegeben haben, wird die Ankündigung automatisch an diesem Datum bzw. zu dieser Uhrzeit deaktiviert.

## <a name="steps-for-users-to-view-an-announcement"></a>Schritte für Benutzer zum Anzeigen einer Ankündigung

1. Wählen Sie im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) ein Lab aus.

1. Wenn für das Lab eine Ankündigung gepostet wird, wird oben auf der Seite „Übersicht“ des Labs ein Hinweis mit entsprechenden Informationen angezeigt. Dieser Hinweis enthält den Ankündigungstitel, der bei der Erstellung der Ankündigung angegeben wurde.

    ![Labankündigung auf der Seite „Übersicht“](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. Der Benutzer kann die Nachricht auswählen, um die gesamte Ankündigung anzuzeigen.

    ![Weitere Informationen zur Labankündigung](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nächste Schritte
* Wenn Sie eine Lab-Richtlinie ändern oder festlegen, kann es ratsam sein, eine Ankündigung zu posten, um Benutzer zu informieren. [Festlegen von Richtlinien und Zeitplänen](devtest-lab-set-lab-policy.md) enthält Informationen zum Anwenden von Einschränkungen und Konventionen für Ihr gesamtes Abonnement mit benutzerdefinierten Richtlinien.
* Erkunden Sie den [DevTest Labs-Azure Resource Manager-Katalog mit Schnellstartvorlagen](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
