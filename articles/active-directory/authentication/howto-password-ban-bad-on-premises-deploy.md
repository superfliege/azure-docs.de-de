---
title: Bereitstellen des Kennwortschutzes für Azure AD – Vorschauversion
description: Bereitstellen des Kennwortschutzes für Azure AD (Vorschauversion) zum lokalen Sperren ungültiger Kennwörter
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 10/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: efa684d75cd30dcbfc971d0ef0a3717cc15bd0e4
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2019
ms.locfileid: "55081278"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Vorschau: Bereitstellen des Kennwortschutzes für Azure AD

|     |
| --- |
| Azure AD-Kennwortschutz ist eine öffentliche Vorschaufunktion für Azure Active Directory. Weitere Informationen zu Vorschauversionen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Nachdem Sie nun erfahren haben, [wie Sie den Azure AD-Kennwortschutz für Windows Server Active Directory durchsetzen können](concept-password-ban-bad-on-premises.md), ist der nächste Schritt die Planung und Ausführung der Bereitstellung.

## <a name="deployment-strategy"></a>Bereitstellungsstrategie

Microsoft empfiehlt, dass mit allen Bereitstellungen im Überwachungsmodus begonnen wird. Der Überwachungsmodus ist die Standardeinstellung, bei der Kennwörter weiterhin festgelegt werden können und für alle, die blockiert würden, Einträge im Ereignisprotokoll erstellt werden. Sobald Proxyserver und DC-Agents vollständig im Überwachungsmodus bereitgestellt sind, sollte eine regelmäßige Überwachung durchgeführt werden. So lässt sich feststellen, welche Auswirkungen die Erzwingung von Kennwortrichtlinien auf Benutzer und die Umgebung hätte, wenn die Richtlinie durchgesetzt würde.

Während der Überwachungsphase erlangen viele Organisationen diese Erkenntnisse:

* Sie müssen bestehende Betriebsprozesse verbessern, um sicherere Kennwörter zu verwenden.
* Benutzer sind es gewohnt, regelmäßig unsichere Kennwörter zu wählen.
* Sie müssen Benutzer über bevorstehende Änderungen der Sicherheitsmaßnahmen und deren Auswirkungen auf sie informieren und erklären, wie sie sicherere Kennwörter wählen können.

Sobald das Feature für einen angemessenen Zeitraum im Überwachungsmodus ausgeführt wurde, kann die Erzwingungskonfiguration von **Überwachen** auf **Erzwingen** umgestellt werden, sodass sicherere Kennwörter erfordert werden. Eine gezielte Überwachung während dieser Zeit wird empfohlen.

## <a name="deployment-requirements"></a>Bereitstellungsanforderungen

* Alle Domänencontroller, auf denen der DC-Agent-Dienst für Azure AD-Kennwortschutz installiert werden soll, müssen Windows Server 2012 oder höher ausführen.

   > [!NOTE]
   > Server Core-basierte Betriebssysteme werden derzeit nicht unterstützt. Diese Unterstützung ist für die allgemeine Verfügbarkeit geplant.

* Alle Computer, auf denen der Azure AD-Kennwortschutz-Proxydienst installiert werden soll, müssen Windows Server 2012 R2 oder höher ausführen.

   > [!NOTE]
   > Server Core-basierte Betriebssysteme werden derzeit nicht unterstützt. Diese Unterstützung ist für die allgemeine Verfügbarkeit geplant.

* Alle Computer, auf denen Azure AD-Kennwortschutzkomponenten, einschließlich Domänencontrollern, installiert sind, müssen über eine Installation der Universal C Runtime verfügen.
Dies wird vorzugsweise erreicht, indem der Computer über Windows Update mit allen Patches versehen wird. Andernfalls kann ein geeignetes betriebssystemspezifisches Updatepaket installiert werden. Weitere Informationen finden Sie unter [Update for Universal C Runtime in Windows](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows) (Update für Universal C Runtime in Windows).
* Netzwerkkonnektivität muss zwischen mindestens einem Domänencontroller in jeder Domäne und mindestens einem Server bestehen, der den Azure AD-Kennwortschutz-Proxydienst hostet. Diese Konnektivität muss es dem Domänencontroller gestatten, auf den RPC-Endpunktzuordnungs-Port (135) und den RPC-Serverport auf dem Proxydienst zuzugreifen.  Der RPC-Serverport ist standardmäßig ein dynamischer RPC-Port, kann jedoch so konfiguriert werden (siehe unten), dass er einen statischen Port verwendet.
* Alle Computer, die den Proxydienst für den AD-Kennwortschutz hosten, müssen auf die folgenden Endpunkte zugreifen können:

    |Endpunkt |Zweck|
    | --- | --- |
    |`https://login.microsoftonline.com`|Authentifizierungsanforderungen|
    |`https://enterpriseregistration.windows.net`|Funktion für Azure AD-Kennwortschutz|

* Ein globales Administratorkonto zum Registrieren des Azure AD-Kennwortschutz-Proxydiensts und der Gesamtstruktur bei Azure AD.
* Ein Konto mit Active Directory-Domänenadministratorrechten in der Gesamtstruktur-Stammdomäne, um die Windows Server Active Directory-Gesamtstruktur bei Azure AD zu registrieren.
* Alle Active Directory-Domänen, die die DC-Agent-Dienst-Software ausführen, müssen DFSR für die SYSVOL-Replikation verwenden.

## <a name="single-forest-deployment"></a>Bereitstellung in einer einzelnen Gesamtstruktur

Die Vorschauversion des Azure AD-Kennwortschutzes wird mit dem Proxydienst auf bis zu zwei Servern bereitgestellt, und der DC-Agent-Dienst kann inkrementell für alle Domänencontroller in der Active Directory-Gesamtstruktur bereitgestellt werden.

![Zusammenspiel der Komponenten des Azure AD-Kennwortschutzes](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>Herunterladen der Software

Es gibt zwei erforderliche Installer für den Azure AD-Kennwortschutz, die vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071) heruntergeladen werden können.

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Installieren und Konfigurieren des Azure AD-Kennwortschutz-Proxydiensts

1. Wählen Sie einen oder mehrere Server, um den Azure AD-Kennwortschutz-Proxydienst zu hosten.
   * Jeder dieser Dienste kann nur Kennwortrichtlinien für eine einzelne Gesamtstruktur bereitstellen. Außerdem muss der Hostcomputer mit einer Domäne (Stammdomäne und untergeordnete Domäne werden beide gleichermaßen unterstützt) in dieser Gesamtstruktur verbunden sein. Damit der Azure AD-Kennwortschutz-Proxydienst seine Aufgabe erfüllen kann, muss eine Netzwerkverbindung zwischen mindestens einem Domänencontroller in jeder Domäne der Gesamtstruktur und dem Hostcomputer mit dem Azure AD-Kennwortschutz-Proxydienst bestehen.
   * Sie haben die Möglichkeit, den Azure AD-Kennwortschutz-Proxydienst zu Testzwecken auf einem Domänencontroller zu installieren und auszuführen. Für den Domänencontroller ist dann aber eine Internetverbindung erforderlich.

   > [!NOTE]
   > Die öffentliche Vorschauversion unterstützt höchstens zwei (2) Proxyserver pro Gesamtstruktur.

2. Installieren Sie die Software für den Kennwortrichtlinien-Proxydienst mit dem MSI-Paket „AzureADPasswordProtectionProxy.msi“.
   * Nach der Softwareinstallation ist kein Neustart erforderlich. Die Softwareinstallation lässt sich mit Standard-MSI-Prozeduren automatisieren, z.B.: `msiexec.exe /i AzureADPasswordProtectionProxy.msi /quiet /qn`

      > [!NOTE]
      > Der Windows-Firewall-Dienst muss vor der Installation des MSI-Pakets AzureADPasswordProtectionProxy.msi ausgeführt werden, da ansonsten ein Installationsfehler auftritt. Wenn die Windows-Firewall dafür konfiguriert ist, nicht ausgeführt zu werden, besteht die Problemumgehung im vorübergehenden Aktivieren und Starten des Windows-Firewall-Diensts während des Installationsvorgangs. Die Proxysoftware ist nach der Installation nicht in bestimmter Weise von der Windows-Firewall-Software abhängig. Wenn Sie eine Drittanbieterfirewall verwenden, muss sie dennoch konfiguriert werden, um die Bereitstellungsanforderungen zu erfüllen (Zulassen des eingehenden Zugriffs an Port 135 und dem Proxy-RPC-Serverport, ob dynamisch oder statisch). [Siehe Bereitstellungsanforderungen](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements)

3. Öffnen Sie ein PowerShell-Fenster als Administrator.
   * Die Software für den Azure AD-Kennwortschutz-Proxydienst enthält ein neues PowerShell-Modul namens „AzureADPasswordProtection“. Die folgenden Schritte basieren auf der Ausführung verschiedener Cmdlets aus diesem PowerShell-Modul und setzen voraus, dass Sie ein neues PowerShell-Fenster geöffnet und das neue Modul wie folgt importiert haben:
      * `Import-Module AzureADPasswordProtection`

      > [!NOTE]
      > Die Installationssoftware ändert die Umgebungsvariable „PSModulePath“ des Hostcomputers. Damit diese Änderung wirksam wird, sodass das PowerShell-Modul „AzureADPasswordProtection“ importiert und verwendet werden kann, müssen Sie möglicherweise ein komplett neues PowerShell-Konsolenfenster öffnen.

   * Stellen Sie sicher, dass der Dienst mit dem folgenden PowerShell-Befehl ausgeführt wird: `Get-Service AzureADPasswordProtectionProxy | fl`.
      * Das Ergebnis sollte einen **Status** ergeben, der das Ergebnis „Wird ausgeführt“ liefert.

4. Registrieren Sie den Proxy.
   * Sobald Schritt 3 abgeschlossen ist, wird der Azure AD-Kennwortschutz-Proxydienst auf dem Computer ausgeführt, verfügt aber noch nicht über die erforderlichen Anmeldeinformationen für die Kommunikation mit Azure AD. Die Registrierung bei Azure AD ist erforderlich, um dieses Feature mit dem PowerShell-Cmdlet `Register-AzureADPasswordProtectionProxy` zu aktivieren. Das Cmdlet erfordert globale Administratoranmeldeinformationen für Ihren Azure-Mandanten sowie lokale Active Directory-Domänenadministratorrechte in der Stammdomäne der Gesamtstruktur. Sobald sie für einen bestimmten Proxydienst erfolgreich war, sind auch weitere Aufrufe von `Register-AzureADPasswordProtectionProxy` erfolgreich, aber unnötig.
      * Das Cmdlet kann wie folgt ausgeführt werden:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionProxy -AzureCredential $tenantAdminCreds
         ```

         Das Beispiel funktioniert nur, wenn der aktuell angemeldete Benutzer auch ein Active Directory-Domänenadministrator für die Stammdomäne ist. Alternativ können die erforderlichen Anmeldeinformationen für die Domäne auch über den `-ForestCredential`-Parameter angegeben werden.

   > [!NOTE]
   > Dieser Vorgang kann fehlschlagen, wenn verstärkte Sicherheitskonfiguration für Internet Explorer aktiviert ist. Die Problemumgehung besteht darin, IESC zu deaktivieren, den Proxy zu registrieren und dann IESC erneut zu aktivieren.

   > [!NOTE]
   > Die Registrierung des Azure AD-Kennwortschutz-Proxydiensts muss für diesen Dienst nur einmalig ausgeführt werden. Der Proxydienst führt ab diesem Zeitpunkt automatisch alle weiteren notwendigen Wartungen durch. Sobald sie für eine bestimmte Gesamtstruktur erfolgreich war, sind auch weitere Aufrufe von „Register-AzureADPasswordProtectionProxy“ erfolgreich, aber unnötig.

   > [!TIP]
   > Beim ersten Ausführen dieses Cmdlets für einen bestimmten Azure-Mandanten kann es zu einer erheblichen Verzögerung (viele Sekunden) kommen, bevor das Cmdlet die Ausführung beendet. Solange kein Fehler gemeldet wird, stellt diese Verzögerung kein Problem dar.

5. Registrieren Sie die Gesamtstruktur.
   * Die lokale Active Directory-Gesamtstruktur muss mit den erforderlichen Anmeldeinformationen für die Kommunikation mit Azure unter Verwendung des PowerShell-Cmdlets `Register-AzureADPasswordProtectionForest` initialisiert werden. Das Cmdlet erfordert globale Administratoranmeldeinformationen für Ihren Azure-Mandanten sowie lokale Active Directory-Domänenadministratorrechte in der Stammdomäne der Gesamtstruktur. Dieser Schritt wird pro Gesamtstruktur einmal ausgeführt.
      * Das Cmdlet kann wie folgt ausgeführt werden:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $tenantAdminCreds
         ```

         Das Beispiel funktioniert nur, wenn der aktuell angemeldete Benutzer auch ein Active Directory-Domänenadministrator für die Stammdomäne ist. Alternativ können die erforderlichen Anmeldeinformationen für die Domäne auch über den ForestCredential-Parameter angegeben werden.

         > [!NOTE]
         > Dieser Vorgang kann fehlschlagen, wenn verstärkte Sicherheitskonfiguration für Internet Explorer aktiviert ist. Die Problemumgehung besteht darin, IESC zu deaktivieren, den Proxy zu registrieren und dann IESC erneut zu aktivieren.

         > [!NOTE]
         > Wenn mehrere Proxyserver in Ihrer Umgebung installiert sind, spielt es keine Rolle, welcher Proxyserver in der oben beschriebenen Prozedur angegeben wird.

         > [!TIP]
         > Beim ersten Ausführen dieses Cmdlets für einen bestimmten Azure-Mandanten kann es zu einer erheblichen Verzögerung (viele Sekunden) kommen, bevor das Cmdlet die Ausführung beendet. Solange kein Fehler gemeldet wird, stellt diese Verzögerung kein Problem dar.

   > [!NOTE]
   > Die Registrierung der Active Directory-Gesamtstruktur muss für die Gesamtstruktur nur einmalig durchgeführt werden. Die Domänencontroller-Agents, die in der Gesamtstruktur ausgeführt werden, führen ab diesem Zeitpunkt automatisch alle weiteren notwendigen Wartungen durch. Sobald sie für eine bestimmte Gesamtstruktur erfolgreich war, sind auch weitere Aufrufe von `Register-AzureADPasswordProtectionForest` erfolgreich, aber unnötig.

   > [!NOTE]
   > Damit `Register-AzureADPasswordProtectionForest` erfolgreich ist, muss mindestens ein Windows Server 2012-Domänencontroller oder höher in der Domäne des Proxyservers verfügbar sein. Es besteht aber nicht die Anforderung, dass die Software des DC-Agents vor diesem Schritt auf Domänencontrollern installiert wird.

6. Optional: Konfigurieren Sie den Azure AD-Kennwortschutz-Proxydienst so, dass er auf einem bestimmten Port lauscht.
   * Von der DC-Agent-Software für den Azure AD-Kennwortschutz wird auf den Domänencontrollern RPC über TCP verwendet, um mit dem Azure AD-Kennwortschutz-Proxydienst zu kommunizieren. Standardmäßig lauscht der Azure AD-Kennwortschutz-Proxydienst für die Kennwortrichtlinie auf jedem verfügbaren dynamischen RPC-Endpunkt. Wenn es aufgrund von Netzwerktopologie oder Firewallanforderungen erforderlich ist, kann der Dienst stattdessen so konfiguriert werden, dass er auf einem bestimmten TCP-Port lauscht.
      * Verwenden Sie das Cmdlet `Set-AzureADPasswordProtectionProxyConfiguration`, um den Dienst für die Ausführung unter einem statischen Port zu konfigurieren.
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Sie müssen den Dienst beenden und neu starten, damit die Änderungen wirksam werden.

      * Um den Dienst so zu konfigurieren, dass er unter einem dynamischen Port läuft, verwenden Sie die gleiche Prozedur, setzen „StaticPort“ dabei aber wieder auf Null:
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Sie müssen den Dienst beenden und neu starten, damit die Änderungen wirksam werden.

   > [!NOTE]
   > Der Azure AD-Kennwortschutz-Proxydienst erfordert einen manuellen Neustart nach jeder Änderung der Portkonfiguration. Es ist nicht notwendig, die auf Domänencontrollern laufende DC-Agent-Dienstsoftware neu zu starten, nachdem solche Konfigurationsänderungen vorgenommen wurden.

   * Die aktuelle Konfiguration des Diensts kann mit dem Cmdlet `Get-AzureADPasswordProtectionProxyConfiguration` abgefragt werden, wie das folgende Beispiel zeigt:

      ```
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0 
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Installieren des DC-Agent-Diensts für den Azure AD-Kennwortschutz

* Installieren Sie die DC-Agent-Dienstsoftware für den Azure AD-Kennwortschutz mithilfe des MSI-Pakets `AzureADPasswordProtectionDCAgent.msi`:
   * Die Softwareinstallation erfordert einen Neustart beim Installieren und Deinstallieren, da die Kennwortfilter-DLLs aufgrund der Betriebssystemanforderungen nur bei einem Neustart geladen oder entladen werden.
   * Es bestehe die Möglichkeit, den DC-Agent-Dienst auf einem Computer zu installieren, der noch kein Domänencontroller ist. In diesem Fall wird der Dienst gestartet und ausgeführt, ist aber ansonsten inaktiv, bis der Computer zum Domänencontroller höher gestuft wird.

   Die Softwareinstallation lässt sich mit Standard-MSI-Prozeduren automatisieren, z.B.: `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > Der Beispielbefehl „msiexec“ führt zu einem sofortigen Neustart; dies kann durch die Angabe des `/norestart`-Flags vermieden werden.

Nach der Installation auf einem Domänencontroller und dem Neustart ist der Installationsvorgang der DC-Agent-Software für den Azure AD-Kennwortschutz abgeschlossen. Eine andere Konfiguration ist weder erforderlich noch möglich.

## <a name="multiple-forest-deployments"></a>Bereitstellungen in mehreren Gesamtstrukturen

Für die Bereitstellung des Azure AD-Kennwortschutzes in mehreren Gesamtstrukturen gibt es keine zusätzlichen Anforderungen. Jede Gesamtstruktur wird unabhängig voneinander konfiguriert, wie im Abschnitt über die Bereitstellung in einer einzelnen Gesamtstruktur beschrieben. Jeder Azure AD-Kennwortschutzproxy kann nur Domänencontroller unterstützen, mit denen er verknüpft ist. Unabhängig von der in Active Directory konfigurierten Vertrauensstellung hat die Azure AD-Kennwortschutzsoftware in einer bestimmten Gesamtstruktur keine Informationen über eine Azure AD-Kennwortschutzsoftware, die in einer anderen Gesamtstruktur bereitgestellt ist.

## <a name="read-only-domain-controllers"></a>Schreibgeschützte Domänencontroller

Kennwortänderungen werden niemals auf schreibgeschützten Domänencontrollern (RODCs) verarbeitet und gespeichert, sondern an beschreibbare Domänencontroller weitergeleitet. Eine Installation der DC-Agent-Software auf RODCs ist daher nicht erforderlich.

## <a name="high-availability"></a>Hochverfügbarkeit

Bei der Hochverfügbarkeit des Azure AD-Kennwortschutzes geht es hauptsächlich darum, die Verfügbarkeit der Proxyserver zu gewährleisten, wenn Domänencontroller in einer Gesamtstruktur versuchen, neue Richtlinien oder andere Daten aus Azure herunterzuladen. Die öffentliche Vorschauversion unterstützt höchstens zwei Proxyserver pro Gesamtstruktur. Jeder DC-Agent verwendet bei der Entscheidung, welcher Proxyserver aufgerufen werden soll, einen einfachen Roundrobin-Algorithmus, und überspringt Proxyserver, die nicht antworten.
Die üblichen Probleme, die mit Hochverfügbarkeit einhergehen, werden durch das Konzept der DC-Agent-Software verringert. Der DC-Agent verwaltet einen lokalen Cache, in dem sich die zuletzt heruntergeladene Kennwortrichtlinie befindet. Selbst wenn alle registrierten Proxyserver aus irgendeinem Grund nicht mehr verfügbar sind, setzen die DC-Agents ihre zwischengespeicherten Kennwortrichtlinien durch. Eine angemessene Aktualisierungshäufigkeit für Kennwortrichtlinien in einer umfangreichen Bereitstellung liegt für gewöhnlich in der Größenordnung von Tagen, nicht Stunden oder weniger.   Kurze Ausfälle der Proxyserver haben daher keinen wesentlichen Einfluss auf die Ausführung des Features für den Azure AD-Kennwortschutz oder die damit verbundenen Vorteile in puncto Sicherheit.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun die für den Azure AD-Kennwortschutz erforderlichen Dienste auf Ihren lokalen Servern installiert haben, lesen Sie den Artikel zu [Konfiguration nach der Installation und Sammeln von Berichtsinformationen](howto-password-ban-bad-on-premises-operations.md), um Ihre Bereitstellung abzuschließen.

[Konzeptionelle Übersicht über den Azure AD-Kennwortschutz](concept-password-ban-bad-on-premises.md)
