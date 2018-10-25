---
title: Windows Defender Advanced Threat Protection mit Azure Security Center
description: Dieses Dokument stellt die Integration von Azure Security Center und Windows Defender Advanced Threat Protection vor.
services: security-center
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: barclayn
ms.openlocfilehash: e7bb7f0c4cb63390b91012e071589611991a2866
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068331"
---
# <a name="windows-defender-advanced-threat-protection-with-azure-security-center"></a>Windows Defender Advanced Threat Protection mit Azure Security Center

Azure Security Center erweitert das Angebot an Cloudworkloadschutz-Plattformen um die Integration in [Windows Defender Advanced Threat Protection](https://www.microsoft.com/en-us/WindowsForBusiness/windows-atp) (ATP).
Diese Änderung bietet umfassende Funktionen für Endpoint Detection and Response (EDR). Durch die ATP-Integration in Windows Defender können Sie Anomalien erkennen. Sie können auch erweiterte Angriffe auf Serverendpunkte erkennen und darauf reagieren, die von Azure Security Center überwacht werden.

Azure Security Center-Kunden können nun die Features von Windows Defender ATP verwenden:

- **Erkennungssensoren für Sicherheitsverletzungen der nächsten Generation**: Windows Defender ATP-Sensoren für Windows-Server erfassen eine breite Palette von verhaltensbezogenen Signalen.

- **Auf Analysen basierende, cloudgesteuerte Erkennung von Sicherheitsverletzungen**: Windows Defender ATP passt sich schnell an sich ändernde Bedrohungen an. Das Produkt verwendet fortschrittliche Analysen und Big Data. Windows Defender ATP wird durch die Leistungsfähigkeit von Intelligent Security Graph mit Signalen aus Windows, Azure und Office verstärkt, um unbekannte Bedrohungen zu erkennen. Das Produkt stellt aussagekräftige Warnmeldungen zur Verfügung und ermöglicht eine schnelle Reaktion.

- **Bedrohungsinformationen**: Windows Defender ATP identifiziert Angriffstools, -techniken und -verfahren. Wenn diese erkannt werden, werden Warnungen generiert. Das Produkt verwendet Daten, die von Microsoft-Bedrohungsspezialisten und Sicherheitsteams generiert werden, ergänzt durch Informationen von Partnern.

Diese Funktionen sind ab sofort in Azure Security Center verfügbar:

- **Automatisiertes Onboarding**: Der Windows Defender ATP-Sensor wird automatisch für Windows-Server aktiviert, die in Azure Security Center integriert sind.

- **Zentralisierte Benutzeroberfläche**: Die Azure Security Center-Konsole zeigt Windows Defender ATP-Warnungen an.

- **Detaillierte Untersuchung des Computers**: Azure Security Center-Kunden können auf die Windows Defender ATP-Konsole zugreifen, um den Umfang einer Sicherheitsverletzung durch eine detaillierte Untersuchung zu erkennen.

![Azure Security Center zeigt eine Liste der Warnungen und allgemeine Informationen zu jeder Warnung an.](media/security-center-wdatp/image1.png)

Sie können die Warnung in Azure Security Center [untersuchen](security-center-investigation.md):

![Das Warnungsuntersuchungsdashboard in Azure Security Center](media/security-center-wdatp/image2.png)

Sie können die Warnung weiter untersuchen, indem Sie sie in Windows Defender ATP pivotieren. Dort werden zusätzliche Informationen wie der Warnprozessbaum und die Ereignisgrafik angezeigt. Sie können auch eine detaillierte Computerzeitachse verwenden, die jedes Verhalten über einen historischen Zeitraum von bis zu sechs Monaten anzeigt.

![Windows Defender ATP-Seite mit Detailinformationen zu einer Warnung](media/security-center-wdatp/image3.png)

## <a name="platform-support"></a>Plattformunterstützung

Dieses Feature unterstützt die Erkennung unter Windows Server 2012 R2 und Windows Server 2016.

Nur die Server in Abonnements für die Standarddienstebene werden unterstützt.

## <a name="onboarding-instructions"></a>Onboardinganweisungen

Um anzuzeigen, ob die ATP-Integration von Windows Defender aktiviert ist, wählen Sie **Security Center** > **Sicherheitsrichtlinie** > **Abonnement** > **Einstellungen bearbeiten** aus.

  ![Azure Security Center-Richtlinienverwaltung](media/security-center-wdatp/policy-management.png)

Hier können Sie die Integrationen anzeigen, die zurzeit aktiviert sind.

  ![Einstellungsseite von Azure Security Center Threat Detection mit aktivierter Windows Defender ATP-Integration](media/security-center-wdatp/enable-integrations.png)

- Wenn Sie die Server bereits in die Standardebene von Azure Security Center integriert haben, müssen Sie keine weiteren Maßnahmen ergreifen. Azure Security Center integriert die Server automatisch in Windows Defender ATP. Dies kann bis zu 24 Stunden dauern.

- Wenn Sie die Server noch nie in die Standardebene von Azure Security Center integriert haben, integrieren Sie sie wie gewohnt in Azure Security Center.

- Gehen Sie folgendermaßen vor, wenn Sie die Server über Windows Defender ATP integriert haben:
  - Verwenden Sie die Anleitungen in der Dokumentation zum [Aufheben der Integration von Servercomputern](https://go.microsoft.com/fwlink/p/?linkid=852906).
  - Integrieren Sie diese Server in Azure Security Center.

## <a name="access-to-the-windows-defender-atp-portal"></a>Zugreifen auf das Windows Defender ATP-Portal

Befolgen Sie die Anweisungen unter [Zuweisen von Benutzerzugriff auf das Portal](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/assign-portal-access-windows-defender-advanced-threat-protection).

## <a name="set-the-firewall-configuration"></a>Festlegen der Firewallkonfiguration

Wenn Sie einen Proxy oder eine Firewall verwenden, der bzw. die anonymen Datenverkehr blockiert: Da ein Windows Defender ATP-Sensor aus dem Systemkontext eine Verbindung herstellt, stellen Sie sicher, dass anonymer Datenverkehr zulässig ist. Befolgen Sie die Anweisungen unter [Aktivieren des Zugriffs auf Windows Defender ATP-Dienst-URLs im Proxyserver](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-proxy-internet-windows-defender-advanced-threat-protection#enable-access-to-windows-defender-atp-service-urls-in-the-proxy-server).

## <a name="test-the-feature"></a>Testen des Features

So generieren Sie eine unbedenkliche Windows Defender ATP-Testwarnung:

1. Verwenden Sie Remotedesktop, um auf eine Windows Server 2012 R2-VM oder eine Windows Server 2016-VM zuzugreifen.  Öffnen Sie ein Eingabeaufforderungsfenster.

2. Kopieren Sie an der Eingabeaufforderung den folgenden Befehl, und führen Sie ihn aus. Das Eingabeaufforderungsfenster wird automatisch geschlossen.

    ```
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-WDATP-test\\invoice.exe'); Start-Process 'C:\\test-WDATP-test\\invoice.exe'
    ```

   ![Ein Eingabeaufforderungsfenster mit dem oben gezeigten Befehl](media/security-center-wdatp/image4.jpeg)

3. Wenn der Befehl erfolgreich ist, sehen Sie eine neue Warnung im Azure Security Center-Dashboard und im Windows Defender ATP-Portal. Die Anzeige dieser Warnung kann einige Minuten dauern.

4. Um die Warnung in Security Center zu überprüfen, navigieren Sie zu **Sicherheitswarnungen** >  **Verdächtige PowerShell-Befehlszeile**.

5. Wählen Sie im Untersuchungsfenster den Link aus, um zum Windows Defender ATP-Portal zu navigieren.

## <a name="next-steps"></a>Nächste Schritte

- [Festlegen von Sicherheitsrichtlinien in Azure Security Center:](security-center-policies.md) Hier erfahren Sie, wie Sie Sicherheitsrichtlinien für Ihre Azure-Abonnements und -Ressourcengruppen konfigurieren.
- [Verwalten von Sicherheitsempfehlungen in Azure Security Center](security-center-recommendations.md): Hier erfahren Sie, wie Empfehlungen Ihnen beim Schutz der Azure-Ressourcen helfen.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
