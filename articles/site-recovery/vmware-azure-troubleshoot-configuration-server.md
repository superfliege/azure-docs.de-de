---
title: Beheben von Problemen mit dem Konfigurationsserver bei der Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: Dieser Artikel enthält Informationen zur Problembehandlung für die Bereitstellung des Konfigurationsserver bei der Notfallwiederherstellung von virtuellen VMware-Computern und physischen Servern in Azure mit Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 4faadc27648b0d944e61a4d390313a35b4ba8bfa
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52837731"
---
# <a name="troubleshoot-configuration-server-issues"></a>Behandeln von Problemen mit dem Konfigurationsserver

Dieser Artikel hilft Ihnen bei der Behandlung von Problemen bei der Bereitstellung und Verwaltung des [Azure Site Recovery](site-recovery-overview.md)-Konfigurationsservers. Der Konfigurationsserver wird beim Einrichten der Notfallwiederherstellung für lokale virtuelle VMware-Computer und physische Server in Azure mithilfe von Site Recovery verwendet. 

## <a name="installation-failures"></a>Fehler bei der Installation

| **Fehlermeldung** | **Empfohlene Maßnahme** |
|--------------------------|------------------------|
|ERROR   Failed to load Accounts. Fehler System.IO.IOException: Unable to read data from the transport connection when installing and registering the CS server. (FEHLER – Fehler beim Laden von Konten. Fehler: System.IO.IOException: Von der Übertragungsverbindung können keine Daten gelesen werden, wenn der CS-Server installiert und registriert wird.)| Vergewissern Sie sich, dass TLS 1.0 auf dem Computer aktiviert ist. |

## <a name="registration-failures"></a>Fehler bei der Registrierung

Zum Debuggen von Registrierungsfehlern können die Protokolle im Ordner „%ProgramData%\ASRLogs“ verwendet werden.

Sehen Sie sich zum Debuggen von Registrierungsfehlern die Protokolle im Ordner **%ProgramData%\ASRLogs** an.

| **Fehlermeldung** | **Empfohlene Maßnahme** |
|--------------------------|------------------------|
|**09:20:06**:InnerException.Type: SrsRestApiClientLib.AcsException,InnerException.<br>Meldung: ACS50008: SAML token is invalid. (SAML-Token ist ungültig.)<br>Ablaufverfolgungs-ID: 1921ea5b-4723-4be7-8087-a75d3f9e1072<br>Korrelations-ID: 62fea7e6-2197-4be4-a2c0-71ceb7aa2d97><br>Zeitstempel: **2016-12-12 14:50:08Z<br>** | Vergewissern Sie sich, dass die Uhrzeit Ihrer Systemuhr nicht mehr als 15 Minuten von der Ortszeit abweicht. Führen Sie das Installationsprogramm erneut aus, um die Registrierung durchzuführen.|
|**09:35:27**:DRRegistrationException while trying to get all disaster recovery vault for the selected certificate: : Threw Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException (DRRegistrationException beim Versuch, alle Tresore für Notfallwiederherstellung für das ausgewählte Zertifikat abzurufen: Exception.Type:Microsoft.DisasterRecovery.Registration.DRRegistrationException wurde ausgelöst), Exception.Message: ACS50008: SAML token is invalid. (SAML-Token ist ungültig.)<br>Ablaufverfolgungs-ID: e5ad1af1-2d39-4970-8eef-096e325c9950<br>Korrelations-ID: abe9deb8-3e64-464d-8375-36db9816427a<br>Zeitstempel: **2016-05-19 01:35:39Z**<br> | Vergewissern Sie sich, dass die Uhrzeit Ihrer Systemuhr nicht mehr als 15 Minuten von der Ortszeit abweicht. Führen Sie das Installationsprogramm erneut aus, um die Registrierung durchzuführen.|
|06:28:45:Fehler beim Erstellen des Zertifikats.<br>06:28:45:Setup kann nicht fortgesetzt werden. Ein Zertifikat kann nicht erstellt werden, das für die Authentifizierung für Site Recovery erforderlich ist. Führen Sie Setup erneut aus. | Vergewissern Sie sich, dass Sie das Setup als lokaler Administrator ausführen. |
