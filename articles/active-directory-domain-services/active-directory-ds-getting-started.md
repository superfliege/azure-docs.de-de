---
title: Erste Schritte mit Azure Active Directory Domain Services | Microsoft-Dokumentation
description: Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maheshu
ms.openlocfilehash: d86b9e99619dea408e784f32d76ba6296948bdc0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals
Dieser Artikel zeigt, wie Azure Active Directory Domain Services (Azure AD DS) mithilfe des Azure-Portals aktiviert werden kann.

Um den Assistenten zum **Aktivieren von Azure AD Domain Services** zu starten, führen Sie die folgenden Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im linken Bereich auf **Neu**.
3. Geben Sie auf dem Blatt **Neu** in der Suchleiste **Domain Services** ein.

    ![Suche nach Domänendiensten](./media/getting-started/search-domain-services.png)

4. Klicken Sie auf **Azure AD Domain Services** in der Liste der Suchvorschläge. Klicken Sie auf der Seite **Azure AD Domain Services** auf die Schaltfläche **Erstellen**.

    ![Domain Services-Ansicht](./media/getting-started/domain-services-blade.png)

5. Der Assistent zum **Aktivieren von Azure AD Domain Services** wird gestartet.


## <a name="task-1-configure-basic-settings"></a>Aufgabe 1: Grundeinstellungen konfigurieren
Auf der Seite **Grundlagen** des Assistenten können Sie den DNS-Domänennamen für die verwaltete Domäne angeben. Sie können ebenfalls die Ressourcengruppe und den Azure-Standort angeben, für die die verwaltete Domäne bereitgestellt werden soll.

![Konfigurieren von Grundlagen](./media/getting-started/domain-services-blade-basics.png)

1. Wählen Sie den **DNS-Domänennamen** für Ihre verwaltete Domäne.

   * Standardmäßig ist der Standarddomänenname des Verzeichnisses (mit dem Suffix **.onmicrosoft.com**) ausgewählt.

   * Sie können auch einen benutzerdefinierten Domänennamen eingeben. In diesem Beispiel lautet der benutzerdefinierte Domänenname *contoso100.com*.

     > [!WARNING]
     > Das Präfix des angegebenen Domänennamens (beispielsweise *contoso100* im Domänennamen *contoso100.com*) darf maximal 15 Zeichen lang sein. Sie können keine verwaltete Domäne mit einem Präfix, das länger als 15 Zeichen ist, erstellen.
     >
     >

2. Stellen Sie sicher, dass der DNS-Domänenname, den Sie für die verwaltete Domäne ausgewählt haben, nicht bereits im virtuellen Netzwerk vorhanden ist. Überprüfen Sie insbesondere, ob Folgendes zutrifft:

   * Im virtuellen Netzwerk ist bereits eine Domäne mit dem gleichen DNS-Domänennamen vorhanden.

   * Das virtuelle Netzwerk, in dem Sie die verwaltete Domäne aktivieren möchten, hat eine VPN-Verbindung mit Ihrem lokalen Netzwerk. In diesem Szenario stellen Sie sicher, dass Sie keine Domäne mit demselben DNS-Domänennamen in Ihrem lokalen Netzwerk haben.

   * Im virtuellen Netzwerk ist bereits ein Clouddienst mit diesem Namen vorhanden.

3. Wählen Sie den **Typ des virtuellen Netzwerks**. Standardmäßig wird der virtuelle Netzwerktyp **Resource Manager** ausgewählt. Sie sollten diesen virtuellen Netzwerktyp für alle neu erstellten verwalteten Domänen verwenden.

    > [!TIP]
    > **Die Einstellung der Unterstützung klassischer virtueller Netzwerke ist bereits geplant.** Wählen Sie für alle neuen Bereitstellungen den Typ für virtuelle Resource Manager-Netzwerke aus. Klassische virtuelle Netzwerke werden für neue Bereitstellungen in Kürze nicht mehr unterstützt. Vorhandene verwaltete Domänen, die in klassischen virtuellen Netzwerken bereitgestellt wurden, werden weiterhin unterstützt.
    >

4. Wählen Sie das **Azure-Abonnement**, in dem Sie die verwaltete Domäne erstellen möchten.

5. Wählen Sie die **Ressourcengruppe**, zu der die verwaltete Domäne gehören soll. Wählen Sie entweder die Option **Create new** (Neu erstellen) oder **Use existing** (Vorhandene verwenden), um die Ressourcengruppe auszuwählen.

6. Wählen Sie den **Azure-Speicherort**, in dem die verwaltete Domäne erstellt werden soll. Auf der Seite **Netzwerk** des Assistenten sehen Sie nur virtuelle Netzwerke, die zu dem von Ihnen gewählten Standort gehören.

7. Wenn Sie fertig sind, klicken Sie auf **OK**, um zur Seite **Netzwerk** des Assistenten zu gelangen.


## <a name="next-step"></a>Nächster Schritt
[Aufgabe 2: Konfigurieren der Netzwerkeinstellungen](active-directory-ds-getting-started-network.md)
