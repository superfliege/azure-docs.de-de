---
title: Verwalten der Mehrinstanzenfähigkeit in Azure Site Recovery mit dem CSP-Programm (Cloud Solution Provider) | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Mandantenabonnements über CSP erstellen und verwalten und wie Sie Azure Site Recovery in einer Umgebung mit mehreren Mandanten bereitstellen.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2018
ms.author: manayar
ms.openlocfilehash: 6dc8b573e66eaae1b5cb923ae72333fb959d0969
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2018
ms.locfileid: "29767635"
---
# <a name="manage-multi-tenancy-with-the-cloud-solution-provider-csp-program"></a>Verwalten der Mehrinstanzenfähigkeit mit dem CSP-Programm (Cloud Solution Provider)

Das [CSP-Programm](https://partner.microsoft.com/en-US/cloud-solution-provider) fördert ein besseres Zusammenspiel für Microsoft Cloud Services (einschließlich Office 365, Enterprise Mobility Suite und Microsoft Azure). Mit CSP können sich Partner um sämtliche Aspekte der Kundenbeziehung kümmern und als Hauptansprechpartner fungieren. Partner können Azure-Abonnements für Kunden bereitstellen und diese Abonnements mit ihren eigenen angepassten Angeboten mit einem Mehrwert kombinieren.

Mit Azure Site Recovery können Partner die vollständige Lösung für Notfallwiederherstellung für Kunden direkt im CSP-Programm verwalten. Oder sie können im CSP-Programm Site Recovery-Umgebungen einrichten und Kunden ihre eigenen Anforderungen an die Notfallwiederherstellung in Self-Service-Manier erfüllen lassen. In beiden Szenarien fungieren Partner als Verbindungsglied zwischen Site Recovery und ihren Kunden. Partner sind für die Kundenbeziehung zuständig und stellen Kunden die Nutzung von Site Recovery in Rechnung.

In diesem Artikel erfahren Sie, wie ein Partner über CSP Mandantenabonnements für eine VMware-Umgebung mit mehreren Mandanten erstellen und verwalten kann.

## <a name="prerequisites"></a>Voraussetzungen

- [Vorbereiten](tutorial-prepare-azure.md) von Azure-Ressourcen, einschließlich eines Azure-Abonnements, eines virtuellen Azure-Netzwerks und eines Speicherkontos.
- [Vorbereiten](tutorial-prepare-on-premises-vmware.md) von lokalen VMware-Servern und -VMs.
- Erstellen Sie für jeden Mandanten einen separaten Verwaltungsserver, der mit den VMs des Mandanten und dem vCenter des Partners kommunizieren kann. Nur der Partner verfügt über Zugriffsrechte für diesen Server. Weitere Informationen zu verschiedenen Umgebungen mit mehreren Mandanten finden Sie unter [Unterstützung mehrerer Mandanten in Azure Site Recovery für die Replikation von VMware-VMs in Azure über das CSP-Programm](site-recovery-multi-tenant-support-vmware-using-csp.md).

## <a name="create-a-tenant-account"></a>Erstellen eines Mandantenkontos

1. Melden Sie im [Microsoft Partner Center](https://partnercenter.microsoft.com/) bei Ihrem CSP-Konto an.

2. Klicken Sie im Menü **Dashboard** auf **Kunden**.

    ![Der Microsoft Partner Center-Link „Kunden“](./media/site-recovery-manage-multi-tenancy-with-csp/csp-dashboard-display.png)

3. Klicken Sie auf der Seite, die geöffnet wird, auf **Kunden hinzufügen**.

    ![Die Schaltfläche „Kunden hinzufügen“](./media/site-recovery-manage-multi-tenancy-with-csp/add-new-customer.png)

4. Füllen Sie auf der Seite **Neuer Kunde** alle Kontoinformationen für den Mandanten aus, und klicken Sie dann auf **Weiter: Abonnements**.

    ![Die Seite „Kontoinformationen“](./media/site-recovery-manage-multi-tenancy-with-csp/customer-add-filled.png)

5. Aktivieren Sie auf der Seite zur Auswahl des Abonnements das Kontrollkästchen**Microsoft Azure**. Sie können weitere Abonnements sofort oder zu einem späteren Zeitpunkt hinzufügen.

    ![Das Kontrollkästchen „Microsoft Azure-Abonnement“](./media/site-recovery-manage-multi-tenancy-with-csp/azure-subscription-selection.png)

6. Bestätigen Sie auf der Seite **Überprüfung** die Mandantendetails, und klicken Sie dann auf **Senden**.

    ![Die Seite „Überprüfen“](./media/site-recovery-manage-multi-tenancy-with-csp/customer-summary-page.png)  

    Nachdem Sie das Mandantenkonto erstellt haben, wird eine Bestätigungsseite mit den Details des Standardkontos und dem Kennwort dieses Abonnements angezeigt.

7. Speichern Sie die Informationen, und ändern Sie das Kennwort später auf der Anmeldungsseite des Azure-Portals den Anforderungen entsprechend.  

    Sie können diese Informationen dem Mandanten unverändert mitteilen oder bei Bedarf ein getrenntes Konto erstellen und freigeben.

## <a name="access-the-tenant-account"></a>Zugreifen auf das Mandantenkonto

Sie können über das Microsoft Partner Center-Dashboard auf das Abonnement des Mandanten zugreifen, wie unter „Schritt 1: Erstellen eines Mandantenkontos“ beschrieben.

1. Wechseln Sie zur Seite **Kunden**, und klicken Sie dann auf den Namen des Mandantenkontos.

2. Auf der Seite **Abonnements** des Mandantenkontos können Sie die vorhandenen Abonnements des Kontos überwachen und bei Bedarf weitere Abonnements hinzufügen. Wählen Sie zum Verwalten der Vorgänge zur Notfallwiederherstellung des Mandanten **Alle Ressourcen (Azure-Portal)** aus.

    ![Der Link „Alle Ressourcen“](./media/site-recovery-manage-multi-tenancy-with-csp/all-resources-select.png)  

    Durch Klicken auf **Alle Ressourcen** wird Ihnen Zugriff auf die Azure-Abonnements des Mandanten gewährt. Sie können den Zugriff überprüfen, indem Sie rechts oben im Azure-Portal auf den Link „Azure Active Directory“ klicken.

    ![Der Link „Azure Active Directory“](./media/site-recovery-manage-multi-tenancy-with-csp/aad-admin-display.png)

Sie können nun alle Standortwiederherstellungsvorgänge für den Mandanten über das Azure-Portal ausführen und die Notfallwiederherstellungsvorgänge verwalten. Um für die verwaltete Notfallwiederherstellung über das CSP-Programm auf das Mandantenabonnement zugreifen zu können, befolgen Sie den zuvor beschriebenen Prozess.

## <a name="deploy-resources-to-the-tenant-subscription"></a>Bereitstellen von Ressourcen für das Mandantenabonnement
1. Erstellen Sie im Azure-Portal eine Ressourcengruppe, und stellen Sie dann einen Recovery Services-Tresor wie üblich bereit.

2. Laden Sie den Tresorregistrierungsschlüssel herunter.

3. Registrieren Sie den Konfigurationsserver des Mandanten mithilfe des Tresorregistrierungsschlüssels.

4. Geben Sie die Anmeldeinformationen für die beiden Zugriffskonten (das vCenter-Zugriffskonto und VM-Zugriffskonto) ein.

    ![Verwalten von Konfigurationsserverkonten](./media/site-recovery-manage-multi-tenancy-with-csp/config-server-account-display.png)

## <a name="register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>Registrieren der Site Recovery-Infrastruktur im Recovery Services-Tresor
1. Registrieren Sie im Azure-Portal für den Tresor, den Sie zuvor erstellt haben, den vCenter-Server beim Konfigurationsserver, den Sie unter „Schritt 3: Bereitstellen von Ressourcen für das Mandantenabonnement“ registriert haben. Verwenden Sie für diesen Zweck das vCenter-Zugriffskonto.
2. Beenden Sie den Prozess zur Vorbereitung der Infrastruktur für Site Recovery wie üblich.
3. Die VMs können jetzt repliziert werden. Vergewissern Sie sich, dass nur die VMs des Mandanten auf dem Blatt **Virtuelle Computer auswählen** unter der Option **Replizieren** angezeigt werden.

    ![Liste der VMs des Mandanten auf dem Blatt „Virtuelle Computer auswählen“](./media/site-recovery-manage-multi-tenancy-with-csp/tenant-vm-display.png)

## <a name="assign-tenant-access-to-the-subscription"></a>Zuweisen des Mandantenzugriffs auf das Abonnement

Geben Sie für die Self-Service-Notfallwiederherstellung dem Mandanten die Kontodetails entsprechend Schritt 6 unter „Schritt 1: Erstellen eines Mandantenkontos“ an. Führen Sie diese Aktion aus, nachdem der Partner die Infrastruktur für die Notfallwiederherstellung eingerichtet hat. Unabhängig davon, ob der Typ der Notfallwiederherstellung „Verwaltet“ oder „Self-Service“ ist, müssen Partner im CSP-Portal auf Abonnements des Mandanten zugreifen. Sie richten den Tresor in ihrem Besitz ein und registrieren Infrastruktur für die Abonnements des Mandanten.

Partner können auch über das CSP-Portal dem Mandantenabonnement einen neuen Benutzer wie folgt hinzufügen:

1. Wechseln Sie zur CSP-Abonnementseite des Mandanten, und wählen Sie die Option **Benutzer und Lizenzen** aus.

    ![Der CSP-Seite mit dem Abonnement des Mandanten](./media/site-recovery-manage-multi-tenancy-with-csp/users-and-licences.png)

    Sie können nun einen neuen Benutzer erstellen, indem Sie die relevanten Details eingeben und Berechtigungen auswählen. Sie können aber auch die Liste der Benutzer als CSV-Datei hochladen.

2. Nachdem ein neuer Benutzer erstellt wurde, navigieren Sie zurück zum Azure-Portal und wählen auf dem Blatt **Abonnement** das entsprechende Abonnement aus.

3. Wählen Sie auf dem geöffneten Blatt **Zugriffssteuerung (IAM)** aus, und klicken Sie auf **+Hinzufügen**, um einen Benutzer mit der entsprechenden Zugriffsebene hinzuzufügen.      
    Die über das CSP-Portal erstellten Benutzer werden automatisch auf dem Blatt angezeigt, das nach dem Klicken auf eine Zugriffsebene geöffnet wird.

    ![Hinzufügen eines Benutzers](./media/site-recovery-manage-multi-tenancy-with-csp/add-user-subscription.png)

    Für die meisten Verwaltungsvorgänge ist die Rolle *Mitwirkender* ausreichend. Benutzer mit dieser Zugriffsebene können fast alle Aufgaben für ein Abonnement ausführen, jedoch keine Zugriffsebenen ändern (wofür die Zugriffsebene *Besitzer* erforderlich ist).

  Azure Site Recovery verfügt auch über drei [vordefinierte Benutzerrollen](site-recovery-role-based-linked-access-control.md), die bei Bedarf zur weiteren Einschränkung von Zugriffsebenen verwendet werden können.

## <a name="next-steps"></a>Nächste Schritte
  Weitere Informationen zum Verwalten von Azure Site Recovery-Bereitstellungen mithilfe der rollenbasierten Zugriffssteuerung finden Sie [hier](site-recovery-role-based-linked-access-control.md).

  [Verwalten von VMware-Umgebungen mit mehreren Mandanten](site-recovery-multi-tenant-support-vmware-using-csp.md)
