---
title: Just-In-Time-VM-Zugriff in Azure Security Center | Microsoft-Dokumentation
description: In diesem Dokument wird beschrieben, wie Sie mit Just-In-Time-VM-Zugriff in Azure Security Center den Zugriff auf die virtuellen Azure-Computer steuern können.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2018
ms.author: rkarlin
ms.openlocfilehash: 72acf0f06bbed0129ff322b10a7faf16fd94f712
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314744"
---
# <a name="manage-virtual-machine-access-using-just-in-time"></a>Verwalten des Zugriffs auf virtuelle Computer mithilfe des Just-In-Time-Features

Mit Just-In-Time-VM-Zugriff kann eingehender Datenverkehr auf den Azure-VMs gesperrt werden, um die Gefährdung durch Angriffe zu reduzieren und bei Bedarf einfachen Zugriff auf Verbindungen mit virtuellen Computern bereitzustellen.

> [!NOTE]
> Das Just-In-Time-Feature ist im Standard-Tarif von Security Center verfügbar.  Weitere Informationen zu den Tarifen von Security Center finden Sie unter [Preise](security-center-pricing.md).
>
>

## <a name="attack-scenario"></a>Angriffsszenario

Bei Brute-Force-Angriffen wird im Allgemeinen versucht, über Verwaltungsports Zugriff auf einen virtuellen Computer zu erlangen. Im Erfolgsfall kann ein Angreifer die Kontrolle über den virtuellen Computer erlangen und in Ihrer Umgebung Fuß fassen.

Eine Möglichkeit, die Gefährdung durch Brute-Force-Angriffe zu verringern, ist das Beschränken der Zeitspanne, in der ein Port geöffnet ist. Verwaltungsports müssen nicht jederzeit geöffnet sein. Sie müssen nur geöffnet sein, während eine Verbindung mit dem virtuellen Computer besteht, z.B. zum Ausführen von Verwaltungs- oder Wartungsaufgaben. Wenn Just-In-Time aktiviert ist, verwendet das Security Center [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md#security-rules)-Regeln (NSG), die den Zugriff auf Verwaltungsports beschränken, um sie vor Angriffen zu schützen.

![Just-In-Time-Szenario][1]

## <a name="how-does-just-in-time-access-work"></a>Funktionsweise des Just-In-Time-Zugriffs

Wenn Just-In-Time aktiviert ist, sperrt das Security Center durch das Erstellen einer NSG-Regel eingehenden Datenverkehr auf den Azure-VMs. Sie wählen die Ports auf dem virtuellen Computer aus, für die eingehender Datenverkehr gesperrt wird. Diese Ports werden durch die Just-In-Time-Lösung gesteuert.

Wenn ein Benutzer auf einen virtuellen Computer zugreift, überprüft das Security Center, ob der Benutzer über Berechtigungen der [rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/role-assignments-portal.md) verfügt, die Schreibzugriff für den virtuellen Computer bereitstellen. Wenn er über Schreibberechtigungen verfügt, wird die Anforderung genehmigt, und die Netzwerksicherheitsgruppen (NSGs) werden vom Security Center automatisch so konfiguriert, dass für die angegebene Zeitspanne eingehender Datenverkehr an die ausgewählten Ports zugelassen wird. Nach Ablauf dieser Zeitspanne stellt das Security Center die vorherigen Status der NSGs wieder her. Die bereits eingerichteten Verbindungen werden jedoch nicht unterbrochen.

> [!NOTE]
> Der durch das Security Center gesteuerte Just-In-Time-Zugriff auf virtuelle Computer unterstützt derzeit nur virtuelle Computer, die über Azure Resource Manager bereitgestellt wurden. Weitere Informationen über das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell finden Sie unter [Azure Resource Manager-Bereitstellung im Vergleich zur klassischen Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).
>
>

## <a name="using-just-in-time-access"></a>Verwenden von Just-In-Time-Zugriff

1. Öffnen Sie das Dashboard **Security Center**.

2. Klicken Sie im linken Bereich auf **JIT-VM-Zugriff**.

![Kachel „JIT-VM-Zugriff“][2]

Das Fenster **JIT-VM-Zugriff** wird geöffnet.

![Kachel „JIT-VM-Zugriff“][10]

**JIT-VM-Zugriff** enthält Informationen zum Status Ihrer virtuellen Computer:

- **Konfiguriert** – Virtuelle Computer, die für die Unterstützung von Just-In-Time-Zugriff konfiguriert wurden. Die dargestellten Daten betreffen die letzte Woche und enthalten für jeden virtuellen Computer die Anzahl der genehmigten Anforderungen, Datum und Uhrzeit des letzten Zugriffs sowie den letzten Benutzer.
- **Empfohlen** – Virtuelle Computer, die Just-In-Time-VM-Zugriff unterstützen, jedoch nicht entsprechend konfiguriert wurden. Es wird empfohlen, die Just-In-Time-VM-Zugriffssteuerung für diese virtuellen Computer zu aktivieren. Weitere Informationen finden Sie unter [Konfigurieren einer Just-In-Time-Zugriffsrichtlinie](#configuring-a-just-in-time-access-policy).
- **Keine Empfehlung** – Mögliche Gründe, dass ein virtueller Computer nicht empfohlen wird:
  - Fehlende NSG – Die Just-In-Time-Lösung erfordert, dass eine NSG festgelegt ist.
  - Klassischer virtueller Computer – Der durch das Security Center gesteuerte Just-In-Time-VM-Zugriff unterstützt derzeit nur virtuelle Computer, die über Azure Resource Manager bereitgestellt wurden. Klassische Bereitstellungen werden durch die Just-In Time-Lösung nicht unterstützt.
  - Andere – Ein virtueller Computer fällt in diese Kategorie, wenn die Just-In-Time-Lösung in der Sicherheitsrichtlinie des Abonnements oder der Ressourcengruppe deaktiviert ist oder wenn der virtuelle Computer über keine öffentliche IP-Adresse und über keine NSG verfügt.

## <a name="configuring-a-just-in-time-access-policy"></a>Konfigurieren einer Just-In-Time-Zugriffsrichtlinie

So wählen Sie den virtuellen Computer aus, den Sie aktivieren möchten:

1. Wählen Sie unter **JIT-VM-Zugriff** die Registerkarte**Empfohlen** aus.

  ![Just-In-Time-Zugriff aktivieren][3]

2. Wählen Sie unter **Virtueller Computer** die virtuellen Computer aus, die Sie aktivieren möchten. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert.
3. Wählen Sie **JIT auf VMs aktivieren** aus.
4. Wählen Sie **Speichern**aus.

### <a name="default-ports"></a>Standardports

Sie können die Standardports anzeigen, für die das Security Center die Aktivierung von Just-In-Time empfiehlt.

1. Wählen Sie unter **JIT-VM-Zugriff** die Registerkarte**Empfohlen** aus.

  ![Standardportnummern anzeigen][6]

2. Wählen Sie unter **VMs** einen virtuellen Computer aus. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert, und **Konfiguration des JIT-VM-Zugriffs** wird geöffnet. Auf diesem Blatt werden die Standardports angezeigt.

### <a name="add-ports"></a>Hinzufügen von Ports

Unter **Konfiguration des JIT-VM-Zugriffs** können Sie auch einen neuen Port, für den Sie die Just-In-Time-Lösung aktivieren möchten, hinzufügen und konfigurieren.

1. Wählen Sie unter **Konfiguration des JIT-VM-Zugriffs** die Option **Hinzufügen** aus. Daraufhin wird das Blatt **Portkonfiguration hinzufügen** geöffnet.

  ![Portkonfiguration][7]

2. Unter **Portkonfiguration hinzufügen** geben Sie den Port, den Protokolltyp, zulässige Quell-IP-Adressen und die maximale Anforderungszeit an.

  Zulässige Quell-IP-Adressen sind die IP-Adressbereiche, auf die der Zugriff zulässig ist, wenn die Anforderung genehmigt wurde.

  Die maximale Anforderungszeit ist das maximale Zeitfenster, in dem ein bestimmter Port geöffnet sein kann.

3. Klicken Sie auf **OK**.

> [!NOTE]
>Wenn der JIT-VM-Zugriff für eine VM aktiviert ist, erstellt das Azure Security Center Regeln zum Ablehnen von sämtlichem eingehenden Datenverkehr für die ausgewählten Ports in den Netzwerksicherheitsgruppen, die ihr zugeordnet sind. Die Regeln weisen entweder die höchste Priorität in Ihren Netzwerksicherheitsgruppen auf oder eine niedrigere Priorität als bereits vorhandene Regeln. Dies hängt von einer Analyse des Azure Security Center ab, mit der bestimmt wird, ob eine Regel sicher ist.
>


## <a name="set-just-in-time-within-a-vm"></a>Festlegen von Just-In-Time innerhalb eines virtuellen Computers

Um den Rollout des Just-In-Time-Zugriffs auf Ihre virtuellen Computer zu vereinfachen, können Sie einen virtuellen Computer so einrichten, dass er nur den direkten Just-In-Time-Zugriff innerhalb des virtuellen Computers gestattet.

1. Wählen Sie im Azure-Portal die Option für **virtuelle Computer** aus.
2. Klicken Sie auf den virtuellen Computer, den Sie auf den Just-In-Time-Zugriff beschränken möchten.
3. Klicken Sie im Menü auf **Konfiguration**.
4. Klicken Sie unter **Just-In-Time-Zugriff** auf **Just-In-Time-Richtlinie aktivieren**. 

Dies ermöglicht den Just-In-Time-Zugriff auf den virtuellen Computer mit den folgenden Einstellungen:

- Windows-Server:
    - RDP-Port 3389
    - 3 Stunden Zugriff
    - Zulässige Quell-IP-Adressen auf „Pro Anforderung“ festgelegt
- Linux-Server:
    - SSH-Port 22
    - 3 Stunden Zugriff
    - Zulässige Quell-IP-Adressen auf „Pro Anforderung“ festgelegt
     
Wenn für einen virtuellen Computer bereits Just-In-Time aktiviert ist, können Sie auf der Konfigurationsseite sehen, dass Just-In-Time aktiviert ist. Zudem können Sie über den Link die Richtlinie im Azure Security Center öffnen, um die Einstellungen anzuzeigen und zu ändern.

![JIT-Konfiguration im virtuellen Computer](./media/security-center-just-in-time/jit-vm-config.png)


## <a name="requesting-access-to-a-vm"></a>Anfordern des Zugriffs auf einen virtuellen Computer

So fordern Sie Zugriff auf einen virtuellen Computer an:

1. Wählen Sie unter **JIT-VM-Zugriff** die Registerkarte**Konfiguriert** aus.
2. Wählen Sie unter **VMs** die virtuellen Computer aus, für die Sie den Zugriff aktivieren möchten. Hierdurch wird der virtuelle Computer mit einem Häkchen markiert.
3. Wählen Sie **Zugriff anfordern** aus. Daraufhin wird **Zugriff anfordern** geöffnet.

  ![Zugriff auf einen virtuellen Computer anfordern][4]

4. Unter **Zugriff anfordern** können Sie für jeden virtuellen Computer die zu öffnenden Ports zusammen mit der Quell-IP-Adresse, für die der Port geöffnet wird, und dem Zeitfenster, in dem der Port geöffnet wird, konfigurieren. Sie können nur Zugriff auf die in der Just-In-Time-Richtlinie konfigurierten Ports anfordern. Für jeden Port gilt eine maximal zulässige Zeitspanne, die aus der Just-In-Time-Richtlinie abgeleitet ist.
5. Wählen Sie **Ports öffnen** aus.

> [!NOTE]
> Wenn ein Benutzer auf einen virtuellen Computer zugreift, überprüft das Security Center, ob der Benutzer über Berechtigungen der [rollenbasierten Zugriffssteuerung (Role-Based Access Control, RBAC)](../role-based-access-control/role-assignments-portal.md) verfügt, die Schreibzugriff für den virtuellen Computer bereitstellen. Wenn er über Schreibberechtigungen verfügt, wird die Anforderung genehmigt.
>
>

> [!NOTE]
> Wenn sich ein Benutzer, der Zugriff anfordert, hinter einem Proxy befindet, funktioniert die Option „My IP“ (Meine IP-Adresse) möglicherweise nicht. Unter Umständen muss der vollständige Bereich der Organisation definiert werden.
>
>

## <a name="editing-a-just-in-time-access-policy"></a>Bearbeiten einer Just-In-Time-Zugriffsrichtlinie

Sie können die vorhandene Just-In-Time-Richtlinie eines virtuellen Computers ändern, indem Sie einen neuen Port, der für diesen virtuellen Computer geöffnet werden soll, hinzufügen und konfigurieren. Oder Sie ändern einen beliebigen anderen Parameter eines bereits geschützten Ports.

Zum Bearbeiten einer vorhandenen Just-In-Time-Richtlinie eines virtuellen Computers wird die Registerkarte **Konfiguriert** verwendet:

1. Wählen Sie unter **VMs** einen virtuellen Computer aus, dem ein Port hinzugefügt werden soll, indem Sie auf die drei Punkte in der Zeile für den betreffenden virtuellen Computer klicken. Dadurch wird ein Menü geöffnet.
2. Wählen Sie im Menü **Bearbeiten** aus. Daraufhin wird **Konfiguration des JIT-VM-Zugriffs** geöffnet.

  ![Richtlinie bearbeiten][8]

3. Unter **Konfiguration des JIT-VM-Zugriffs** können Sie die vorhandenen Einstellungen eines bereits geschützten Ports bearbeiten, indem Sie auf den Port klicken, oder Sie können **Hinzufügen** auswählen. Daraufhin wird das Blatt **Portkonfiguration hinzufügen** geöffnet.

  ![Port hinzufügen][7]

4. Geben Sie unter **Portkonfiguration hinzufügen** den Port, den Protokolltyp, zulässige Quell-IP-Adressen und die maximale Anforderungszeit an.
5. Klicken Sie auf **OK**.
6. Wählen Sie **Speichern**aus.

## <a name="auditing-just-in-time-access-activity"></a>Überwachen der Just-In-Time-Zugriffsaktivität

Mit der Protokollsuche erhalten Sie Einblicke in VM-Aktivitäten. So zeigen Sie Protokolle an:

1. Wählen Sie unter **JIT-VM-Zugriff** die Registerkarte**Konfiguriert** aus.
2. Wählen Sie unter **VMs** einen virtuellen Computer aus, zu dem Informationen angezeigt werden sollen, indem Sie auf die drei Punkte in der Zeile für den betreffenden virtuellen Computer klicken. Dadurch wird ein Menü geöffnet.
3. Wählen Sie im Menü **Aktivitätsprotokoll** aus. Daraufhin wird **Aktivitätsprotokoll** geöffnet.

  ![Aktivitätsprotokoll auswählen][9]

  **Aktivitätsprotokoll** enthält eine gefilterte Ansicht der früheren Vorgänge für diesen virtuellen Computer zusammen mit der Uhrzeit, dem Datum und dem Abonnement.

Sie können die Protokollinformationen herunterladen, indem Sie **Klicken Sie hier, um alle Elemente als CSV herunterzuladen** auswählen.

Ändern Sie die Filter, und wählen Sie **Anwenden** aus, um eine Suche und ein Protokoll zu erstellen.

## <a name="using-just-in-time-vm-access-via-rest-apis"></a>Verwenden von Just-In-Time-VM-Zugriff mit REST-APIs

Das Feature Just-In-Time-VM-Zugriff kann über die Azure Security Center-API verwendet werden. Sie können über diese API u.a. Informationen über konfigurierte virtuelle Computer erhalten, neue hinzufügen und Zugriff auf einen virtuellen Computer anfordern. Unter [Richtlinien für den Jit-Netzwerkzugriff](https://docs.microsoft.com/rest/api/securitycenter/jitnetworkaccesspolicies) finden Sie weitere Informationen zur Just-In-Time-REST-API.

## <a name="using-just-in-time-vm-access-via-powershell"></a>Verwenden von Just-In-Time-VM-Zugriff mit PowerShell 

Um die Lösung für den Just-In-Time-VM-Zugriff mit PowerShell zu verwenden, verwenden Sie die offiziellen Azure Security Center-PowerShell-Cmdlets und insbesondere `Set-AzureRmJitNetworkAccessPolicy`.

Im folgenden Beispiel werden auf einer bestimmten VM eine Richtlinie für den Just-In-Time-VM-Zugriff sowie die folgenden Einstellungen festgelegt:
1.  Schließen Sie die Ports 22 und 3389.
2.  Legen Sie für die Ports ein maximales Zeitfenster von drei Stunden fest, damit sie bei genehmigter Anforderung geöffnet werden können.
3.  Damit wird dem Benutzer, der Zugriff anfordert, ermöglicht, den Zugriff auf die Quell-IP-Adressen zu steuern und bei genehmigter Just-In-Time-Zugriffsanforderung eine Sitzung herzustellen.

Führen Sie hierfür Folgendes in PowerShell aus:

1.  Weisen Sie eine Variable zu, die die Just-In-Time-VM-Zugriffsrichtlinie für eine VM enthält:

        $JitPolicy = (@{
         id="/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
             number=22;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"},
             @{
             number=3389;
             protocol="*";
             allowedSourceAddressPrefix=@("*");
             maxRequestAccessDuration="PT3H"})})

2.  Fügen Sie einem Array die Just-In-Time-VM-Zugriffsrichtlinie hinzu:
    
        $JitPolicyArr=@($JitPolicy)

3.  Konfigurieren Sie die Just-In-Time-VM-Zugriffsrichtlinie auf der ausgewählten VM:
    
        Set-AzureRmJitNetworkAccessPolicy -Kind "Basic" -Location "LOCATION" -Name "default" -ResourceGroupName "RESOURCEGROUP" -VirtualMachine $JitPolicyArr 

### <a name="requesting-access-to-a-vm"></a>Anfordern des Zugriffs auf einen virtuellen Computer

Im folgenden Beispiel können Sie eine Just-In-Time-VM-Zugriffsanforderung für eine bestimmte VM sehen, in der das Öffnen von Port 22 für eine bestimmte IP-Adresse und für einen bestimmten Zeitraum angefordert wird:

Führen Sie die folgenden Schritte in PowerShell aus:
1.  Konfigurieren Sie die Eigenschaften für die VM-Zugriffsanforderung.

        $JitPolicyVm1 = (@{
          id="/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/VMNAME"
        ports=(@{
           number=22;
           endTimeUtc="2018-09-17T17:00:00.3658798Z";
           allowedSourceAddressPrefix=@("IPV4ADDRESS")})})
2.  Fügen Sie die Parameter für die VM-Zugriffsanforderung zu einem Array hinzu:

        $JitPolicyArr=@($JitPolicyVm1)
3.  Senden Sie die Zugriffsanforderung (mit der Ressourcen-ID, die Sie in Schritt 1 abgerufen haben).

        Start-AzureRmJitNetworkAccessPolicy -ResourceId "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Security/locations/LOCATION/jitNetworkAccessPolicies/default" -VirtualMachine $JitPolicyArr

Weitere Informationen finden Sie in der Dokumentation zu PowerShell-Cmdlets.


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel wurde Ihnen vermittelt, wie Sie mit Just-In-Time-VM-Zugriff im Security Center den Zugriff auf die virtuellen Azure-Computer steuern können.

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Festlegen von Sicherheitsrichtlinien:](security-center-azure-policy.md) Erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen:](security-center-recommendations.md) Erfahren Sie, wie Empfehlungen Ihnen beim Schutz Ihrer Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität:](security-center-monitoring.md) Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen:](security-center-managing-and-responding-alerts.md) Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Überwachen von Partnerlösungen:](security-center-partner-solutions.md) Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Security Center – Häufig gestellte Fragen:](security-center-faq.md) Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](https://blogs.msdn.microsoft.com/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.


<!--Image references-->
[1]: ./media/security-center-just-in-time/just-in-time-scenario.png
[2]: ./media/security-center-just-in-time/just-in-time.png
[10]: ./media/security-center-just-in-time/just-in-time-access.png
[3]: ./media/security-center-just-in-time/enable-just-in-time-access.png
[4]: ./media/security-center-just-in-time/request-access-to-a-vm.png
[5]: ./media/security-center-just-in-time/activity-log.png
[6]: ./media/security-center-just-in-time/default-ports.png
[7]: ./media/security-center-just-in-time/add-a-port.png
[8]: ./media/security-center-just-in-time/edit-policy.png
[9]: ./media/security-center-just-in-time/select-activity-log.png
