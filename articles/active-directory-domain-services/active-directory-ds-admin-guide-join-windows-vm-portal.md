---
title: "Azure Active Directory Domain Services: Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne | Microsoft Docs"
description: "Einbinden eines virtuellen Windows Server-Computers in die Azure AD-Domänendienste"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: 5f661dba2e647ac905e7d84927fdbf6dbc76094f
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne
Dieser Artikel zeigt Ihnen, wie Sie mithilfe des Azure-Portals einen neuen virtuellen Windows Server-Computer bereitstellen. Danach erfahren Sie, wie Sie den virtuellen Computer in eine verwaltete Domäne von Azure AD Domain Services einbinden.

## <a name="step-1-create-the-windows-server-virtual-machine"></a>Schritt 1: Erstellen des virtuellen Windows Server-Computers
Führen Sie die folgenden Schritte durch, um einen virtuellen Windows-Computer zu erstellen, der in das virtuelle Netzwerk eingebunden wird, in dem Sie die Azure Active Directory-Domänendienste aktiviert haben:

1. Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) am Azure-Portal an.
2. Klicken Sie in der linken oberen Ecke des Azure-Portals auf die Schaltfläche **Neu**.
3. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**.

    ![Image auswählen](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. Konfigurieren Sie die grundlegenden Einstellungen für den virtuellen Computer auf der Seite **Grundlagen** des Assistenten.

    ![Grundeinstellungen für virtuellen Computer konfigurieren](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > Der Benutzername und das Kennwort, das Sie hier eingeben, dienen der Anmeldung beim virtuellen Computer über ein lokales Administratorkonto. Wählen Sie ein sicheres Kennwort zum Schutz des virtuellen Computers gegen Brute-Force-Kennwortangriffe. Geben Sie hier nicht die Anmeldeinformationen eines Domänenbenutzerkontos ein.
    >

5. Wählen Sie eine **Größe** für den virtuellen Computer. Wählen Sie die Option **Alle anzeigen**, oder ändern Sie den Filter **Supported disk type** (Unterstützter Datenträgertyp), um weitere Größen anzuzeigen.

    ![Auswählen der entsprechenden Größe für den virtuellen Computer](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. Wählen Sie im Assistenten auf der Seite **Einstellungen** das virtuelle Netzwerk aus, in dem Ihre verwaltete Domäne von Azure AD Domain Services bereitgestellt wird. Wählen Sie ein anderes Subnetz aus als das, in dem Ihre verwaltete Domäne bereitgestellt wird. Behalten Sie bei den anderen Einstellungen die Standardwerte bei, und klicken Sie auf **OK**.

    ![Virtuelles Netzwerk für den virtuellen Computer auswählen](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Wählen Sie das richtige virtuelle Netzwerk und Subnetz aus.**
    > Wählen Sie entweder das virtuelle Netzwerk aus, in dem Ihre verwaltete Domäne bereitgestellt wird, oder ein virtuelles Netzwerk, das mithilfe von Peering damit verbunden ist. Wenn Sie ein unverbundenes virtuelles Netzwerk auswählen, können Sie die VM nicht mit der verwalteten Domäne verknüpfen.
    > Es wird empfohlen, Ihre verwaltete Domäne in einem dedizierten Subnetz bereitzustellen. Wählen Sie daher nicht das Subnetz aus, in dem Sie die verwaltete Domäne aktiviert haben.

7. Überprüfen Sie auf der Seite **Purchase** (Kauf) die Einstellungen, und klicken Sie zum Bereitstellen des virtuellen Computers auf **OK**.
8. Die Bereitstellung des virtuellen Computers (VM) wird im Dashboard des Azure-Portals angeheftet.

    ![Vorgehensweise](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
9. Sobald die Bereitstellung abgeschlossen ist, finden Sie auf der Seite **Übersicht** Informationen zur VM.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Schritt 2: Herstellen einer Verbindung mit dem virtuellen Windows Server-Computer über das lokale Administratorkonto.
Stellen Sie jetzt eine Verbindung mit dem neu erstellten virtuellen Windows Server-Computer her, damit dieser der Domäne beitreten kann. Verwenden Sie die Anmeldeinformationen des lokalen Administrators, die Sie beim Erstellen des virtuellen Computers angegeben haben.

Führen Sie die folgenden Schritte aus, um eine Verbindung mit dem virtuellen Computer herzustellen:

1. Klicken Sie auf der Seite **Übersicht** auf die Schaltfläche **Verbinden**. Eine Remotedesktopprotokoll-Datei (RDP-Datei) wird erstellt und heruntergeladen.

    ![Verbindung mit virtuellem Windows-Computer herstellen](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. Öffnen Sie die heruntergeladene RDP-Datei, um eine Verbindung mit Ihrem virtuellen Computer herzustellen. Klicken Sie in der angezeigten Aufforderung auf **Verbinden**.
3. Geben Sie an der Anmeldeaufforderung die **Anmeldeinformationen des lokalen Administrators**ein, die Sie beim Erstellen des virtuellen Computers angegeben haben. Wir haben in diesem Beispiel „localhost\mahesh“ verwendet.
4. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Klicken Sie auf „Ja“ bzw. „Weiter“, um mit dem Herstellen der Verbindung fortzufahren.

Dadurch sollten Sie mithilfe der lokalen Administratoranmeldeinformationen beim neu erstellen virtuellen Windows-Computer angemeldet werden. Der nächste Schritt besteht darin, den virtuellen Computer in die Domäne einzubinden.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Schritt 3: Einbinden des virtuellen Windows Server-Computers in die durch die Azure AD-Domänendienste verwaltete Domäne
Führen Sie die folgenden Schritte aus, um den virtuellen Windows Server-Computer der durch die AAD-Domänendienste verwalteten Domäne hinzuzufügen.

1. Stellen Sie eine Verbindung mit dem Windows Server-Computer her, wie in Schritt 2 gezeigt. Öffnen Sie auf dem Startbildschirm den **Server-Manager**.
2. Klicken Sie im linken Bereich des Server-Manager-Fensters auf **Lokaler Server** .

    ![Server-Manager auf dem virtuellen Computer starten](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Klicken Sie im Abschnitt **EIGENSCHAFTEN** auf **ARBEITSGRUPPE**. Klicken Sie auf der Seite **Systemeigenschaften** auf **Ändern**, um der Domäne beizutreten.

    ![Seite mit Systemeigenschaften](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Geben Sie im Textfeld **Domäne** den Domänennamen Ihrer durch die Azure AD Domain Services verwalteten Domäne ein, und klicken Sie auf **OK**.

    ![Domäne für den Beitritt angeben](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Sie werden aufgefordert, Ihre Anmeldeinformationen für den Domänenbeitritt einzugeben. Stellen Sie sicher, dass Sie die **Anmeldeinformationen eines Benutzers angeben, der zur Administratorengruppe für Azure AD-Domänencontroller gehört** . Nur Mitglieder dieser Gruppen verfügen über die Berechtigungen, Computer in die verwaltete Domäne einzubinden.

    ![Anmeldeinformationen für den Domänenbeitritt angeben](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Sie können Anmeldeinformationen auf eine der folgenden Arten eingeben:

   * **UPN-Format (empfohlen)**: Hierbei handelt es sich um das UPN-Suffix für das Benutzerkonto, wie in Azure AD konfiguriert. In diesem Beispiel ist „bob@domainservicespreview.onmicrosoft.com“ das UPN-Suffix des Benutzers „Bob“.
   * **SAMAccountName-Format**: Sie können den Kontonamen im Format „SAMAccountName“ angeben. Der Benutzer „bob“ würde in diesem Beispiel „CONTOSO100\bob“ eingeben.

     > [!TIP]
     > **Wir empfehlen die Verwendung des UPN-Formats, um Anmeldeinformationen anzugeben.**
     > Wenn das UPN-Präfix eines Benutzers übermäßig lang ist (z.B. „Janwirklichlangerbenutzername“), wird der Name „SAMAccountName“ möglicherweise automatisch generiert. Falls mehrere Benutzer in Ihrem Azure AD-Mandanten das gleiche UPN-Präfix besitzen (beispielsweise „bob“), wird ihr SAMAccountName-Format automatisch generiert. In diesen Fällen kann das UPN-Format zur Anmeldung bei der Domäne verwendet werden.
     >

7. Nach dem erfolgreichen Domänenbeitritt wird folgende Willkommensmeldung angezeigt. Starten Sie den virtuellen Computer neu, um den Domänenbeitritt abzuschließen.

    ![Willkommen in der Domäne](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Problembehandlung beim Domänenbeitritt
### <a name="connectivity-issues"></a>Konnektivitätsprobleme
Wenn der virtuelle Computer die Domäne nicht findet, führen Sie folgende Schritte zur Problembehandlung aus:

* Stellen Sie sicher, dass der virtuelle Computer mit dem gleichen virtuellen Netzwerk verbunden ist, in dem Sie die Domänendienste aktiviert haben. Andernfalls kann der virtuelle Computer keine Verbindung mit der Domäne herstellen und ihr daher auch nicht beitreten.
* Stellen Sie sicher, dass der virtuelle Computer mit einem virtuellen Netzwerk verbunden ist, das wiederum mit dem virtuellen Netzwerk verbunden ist, in dem Sie Domain Services aktiviert haben.
* Pingen Sie die Domäne unter Verwendung des Domänennamens der verwalteten Domäne (Beispiel: „ping contoso100.com“). Sollte das nicht möglich sein, pingen Sie die IP-Adressen für die Domäne, die auf der Seite angezeigt werden, auf der Sie die Azure AD-Domänendienste aktiviert haben (Beispiel: „ping 10.0.0.4“). Wenn Sie die IP-Adressen pingen können, aber nicht die Domäne, ist DNS möglicherweise falsch konfiguriert. Überprüfen Sie, ob die IP-Adressen der Domäne für das virtuelle Netzwerk als DNS-Server konfiguriert sind.
* Leeren Sie den DNS-Resolvercache im virtuellen Computer („ipconfig /flushdns“).

Wenn ein Dialogfeld angezeigt wird, in dem Anmeldeinformationen für den Domänenbeitritt angefordert werden, bestehen keine Verbindungsprobleme.

### <a name="credentials-related-issues"></a>Probleme mit Anmeldeinformationen
Führen Sie folgende Schritte aus, wenn Probleme mit Anmeldeinformationen auftreten und ein Domänenbeitritt nicht möglich ist.

* Verwenden Sie das UPN-Format für die Angabe von Anmeldeinformationen. Der „SAMAccountName“ für Ihr Konto wird möglicherweise automatisch generiert, wenn mehrere Benutzer in Ihrem Mandanten das gleiche UPN-Präfix verwenden oder wenn Ihr UPN-Präfix übermäßig lang ist. Das Format „SAMAccountName“ für Ihr Konto ist möglicherweise anders als Sie erwarten bzw. unterscheidet sich von dem, was Sie in Ihrer lokalen Domäne verwenden.
* Versuchen Sie, die Anmeldeinformationen eines Benutzerkontos zu verwenden, das zur Administratorengruppe für Azure AD-Domänencontroller gehört.
* Stellen Sie sicher, dass Sie die [Aktivierung der Kennwortsynchronisierung](active-directory-ds-getting-started-password-sync.md) in Übereinstimmung mit den Schritten durchgeführt haben, die im Leitfaden zu den ersten Schritten angegeben sind.
* Verwenden Sie den UPN des Benutzers bei der Anmeldung exakt so, wie er in Azure AD konfiguriert ist (z.B. „bob@domainservicespreview.onmicrosoft.com“).
* Stellen Sie sicher, dass Sie lange genug warten, damit die Kennwortsynchronisierung abgeschlossen ist, wie im Leitfaden „Erste Schritte“ angegeben.

## <a name="related-content"></a>Verwandte Inhalte
* [Erste Schritte mit Azure AD Domain Services](active-directory-ds-getting-started.md)
* [Verwalten einer durch Azure AD-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)
