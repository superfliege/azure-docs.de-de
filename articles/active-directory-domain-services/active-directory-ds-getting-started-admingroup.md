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
ms.openlocfilehash: 32d51f883ad92b5be6b1f455dfb1a865748f3102
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals


## <a name="task-3-configure-administrative-group"></a>Aufgabe 3: Konfigurieren der administrativen Gruppe
In dieser Konfigurationsaufgabe erstellen Sie eine administrative Gruppe in Ihrem Azure AD-Verzeichnis. Diese spezielle administrative Gruppe heißt *AAD DC Administrators*. Mitgliedern dieser Gruppe werden Administratorrechte auf Computern erteilt, die in die verwaltete Domäne eingebunden sind. Auf in die Domäne eingebundenen Computern wird diese Gruppe der Gruppe „Administratoren“ hinzugefügt. Mitglieder dieser Gruppe können außerdem mithilfe von Remotedesktop Remoteverbindungen mit den in die Domäne eingebundenen Computern herstellen.

> [!NOTE]
> Sie verfügen nicht über Domänenadministrator- oder Unternehmensadministratorberechtigungen in der verwalteten Domäne, die Sie mithilfe von Azure Active Directory Domain Services erstellt haben. In verwalteten Domänen sind diese Berechtigungen für den Dienst reserviert und stehen Benutzern innerhalb des Mandanten nicht zur Verfügung. Sie können jedoch die in dieser Konfigurationsaufgabe verwendete spezielle administrative Gruppe verwenden, um einige privilegierte Vorgänge auszuführen. Zu diesen Vorgängen gehört das Einbinden von Computern in die Domäne, die Mitgliedschaft in der Administratorengruppe auf der Domäne beigetretenen Computern und das Konfigurieren von Gruppenrichtlinien.
>

Der Assistent erstellt automatisch die administrative Gruppe in Ihrem Azure AD-Verzeichnis. Diese Gruppe heißt „AAD DC Administrators“. Wenn eine Gruppe mit diesem Namen in Ihrem Azure AD-Verzeichnis besteht, wählt der Assistent diese Gruppe aus. Sie können die Gruppenmitgliedschaften konfigurieren, indem Sie das Blatt **Administratorgruppen** des Assistenten verwenden.

1. Um die Gruppenmitgliedschaften zu konfigurieren, klicken Sie auf **AAD DC Administrators**.

    ![Konfigurieren der Gruppenmitgliedschaft](./media/getting-started/domain-services-blade-admingroup.png)

2. Klicken Sie auf die Schaltfläche **Add members** (Mitglieder hinzufügen), um Benutzer aus Ihrem Azure AD-Verzeichnis zur Administratorgruppe hinzuzufügen.

3. Wenn Sie fertig sind, klicken Sie auf **OK**, um zur Seite **Summary** (Zusammenfassung) des Assistenten zu gelangen.

4. Überprüfen Sie die Konfigurationseinstellungen für die verwaltete Domäne auf der Seite **Summary** (Zusammenfassung) des Assistenten. Sie können zu jedem Schritt des Assistenten zurückgehen, um eventuell notwendige Änderungen vorzunehmen. Wenn Sie fertig sind, klicken Sie auf **OK**, um die neue verwaltete Domäne zu erstellen.

    ![Zusammenfassung](./media/getting-started/domain-services-blade-summary.png)

5. Sie sehen eine Benachrichtigung, die den Status Ihrer Bereitstellung über Azure AD Domain Services anzeigt. Klicken Sie auf die Benachrichtigung, um den detaillierten Status Ihrer Bereitstellung anzuzeigen.

    ![Benachrichtigung: Die Bereitstellung wird ausgeführt](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a>Bereitstellen Ihrer verwalteten Domäne
Der Prozess der Bereitstellung Ihrer verwalteten Domänen kann bis zu einer Stunde dauern.

1. Während Ihre Bereitstellung ausgeführt wird, können Sie im Suchfeld **Search resources** (Ressourcen suchen) nach „Domänendienste“ suchen. Wählen Sie aus dem Suchergebnis **Azure AD Domain Services** aus. Das Blatt **Azure AD Domain Services** listet die verwaltete Domäne auf, die gerade bereitgestellt wird.

    ![Suchen der bereitgestellten verwalteten Domäne](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Klicken Sie auf den Namen der verwalteten Domäne (z.B. „contoso100.com“), um weitere Details zu der verwalteten Domäne zu erfahren.

    ![Domänendienste – Bereitstellungsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. Die Registerkarte **Übersicht** zeigt an, dass die verwaltete Domäne gerade bereitgestellt wird. Die verwaltete Domäne kann nicht konfiguriert werden, bis sie vollständig bereitgestellt ist. Es kann bis zu einer Stunde dauern, bis Ihre verwaltete Domäne vollständig bereitgestellt ist.

    ![Domänendienste: Registerkarte „Übersicht“ während des Bereitstellungszustands ](./media/getting-started/domain-services-provisioning-state-details.png)

4. Wenn die verwaltete Domäne vollständig bereitgestellt ist, zeigt die Registerkarte **Overview** (Übersicht) den Domänenstatus als **Running** (Wird ausgeführt) an.

    ![Domänendienste – Registerkarte „Übersicht“ nach der vollständigen Bereitstellung](./media/getting-started/domain-services-provisioned.png)

5. Auf der Registerkarte **Properties** (Eigenschaften) sehen Sie zwei IP-Adressen, auf denen Domänencontroller für Ihr virtuelles Netzwerk verfügbar sind.

    ![Domänendienste: Registerkarte „Übersicht“ nach der vollständigen Bereitstellung](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="need-help"></a>Sie brauchen Hilfe?
Es dauert möglicherweise ein oder zwei Stunden, bis beide Domänencontroller für die verwaltete Domäne bereitgestellt werden. Wenn bei Ihrer Bereitstellung Fehler auftreten, oder sie im Status „Ausstehend“ steckenbleibt, [bitten Sie das Produktteam um Hilfe](active-directory-ds-contact-us.md).


## <a name="next-step"></a>Nächster Schritt
[Aufgabe 4: Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk](active-directory-ds-getting-started-dns.md)
