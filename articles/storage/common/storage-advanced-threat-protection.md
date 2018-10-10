---
title: Überwachen von Bedrohungen in Azure Storage
description: Es wird beschrieben, wie Sie die Advanced Threat Protection von Azure Storage zum Erkennen von Anomalien in Bezug auf die Kontoaktivität erkennen und Benachrichtigungen über potenziell schädliche Zugriffsversuche auf Ihr Konto erhalten.
services: storage
author: rmatchoro
ms.service: storage
ms.topic: article
ms.date: 09/24/2018
ms.author: ronmat
ms.manager: shaik
ms.openlocfilehash: 8b2ca2d5d6418d68cab847df80fc437e468249ed
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995641"
---
# <a name="azure-storage-advanced-threat-protection"></a>Azure Storage Advanced Threat Protection

Bei Advanced Threat Protection von Azure Storage werden Anomalien in Bezug auf die Kontoaktivität erkannt, und Sie erhalten Benachrichtigungen über potenziell schädliche Zugriffsversuche auf Ihr Konto. Aufgrund dieser Schutzebene können Sie Bedrohungen begegnen, ohne dass Sie ein Sicherheitsexperte sein oder Systeme für die Überwachung der Sicherheit verwalten müssen.

Bedrohungen werden erkannt, indem Sicherheitswarnungen definiert werden, die beim Auftreten von Anomalien in Bezug auf Aktivitäten ausgelöst werden. Diese Warnungen sind in [Azure Security Center](https://azure.microsoft.com/services/security-center/) integriert und enthalten Details zu verdächtigen Aktivitäten und Empfehlungen zur Untersuchung und Eindämmung von Bedrohungen. 

> [!NOTE]
> Azure Storage Advanced Threat Protection ist derzeit nur für den Blob-Dienst verfügbar. Sicherheitswarnungen sind in Azure Security Center integriert und werden per E-Mail an Abonnementadministratoren gesendet.

Bei Azure Storage Advanced Threat Protection werden Diagnoseprotokolle von an den Blob-Dienst gesendeten Lese-, Schreib- und Löschanforderungen erfasst, um Bedrohungen zu erkennen. Für die Untersuchung der Warnungen von Advanced Threat Protection müssen Sie [Diagnoseprotokolle konfigurieren](storage-monitor-storage-account.md#configure-logging), um alle Protokollebenen für den Blob-Dienst zu aktivieren.

## <a name="set-up-advanced-threat-protection-in-the-portal"></a>Einrichten von Advanced Threat Protection im Portal

1. Starten Sie das Azure-Portal unter [https://portal.azure.com](https://portal.azure.com/).

2. Navigieren Sie zur Konfigurationsseite des Azure Storage-Kontos, das Sie schützen möchten. Wählen Sie auf der Seite **Einstellungen** die Option **Advanced Threat Protection**.

3. Gehen Sie auf dem Konfigurationsblatt **Advanced Threat Protection** wie folgt vor:
    * Legen Sie *Advanced Threat Protection* auf **EIN** fest.
    * Klicken Sie auf **Speichern**, um die neue oder aktualisierte Advanced Threat Protection-Richtlinie zu speichern.

![Aktivieren von Azure Storage Advanced Threat Protection](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

## <a name="explore-anomalies"></a>Untersuchen von Anomalien

Wenn für die Speicheraktivität Anomalien auftreten, erhalten Sie eine E-Mail-Benachrichtigung mit Informationen zum verdächtigen Sicherheitsereignis. Einzelheiten des Ereignisses sind:

* Art der Anomalie
* Speicherkontoname
* Speichertyp
* Ereigniszeit

Die E-Mail enthält auch Details zu möglichen Ursachen und empfohlenen Aktionen zur Untersuchung und Eindämmung der potenziellen Bedrohung.

![Azure Storage Advanced Threat Protection – Warnungs-E-Mail](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

Sie können Ihre aktuellen Sicherheitswarnungen in Azure Security Center über die [Kachel „Sicherheitswarnungen“](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts) anzeigen und verwalten. Durch Klicken auf eine bestimmte Warnung werden Details und Aktionen zum Untersuchen der aktuellen Bedrohung und zum Begegnen zukünftiger Bedrohungen bereitgestellt.

![Azure Storage Advanced Threat Protection – Warnungs-E-Mail](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Protection-Warnungen

Warnungen werden bei ungewöhnlichen und potenziell schädlichen Zugriffsversuchen oder Exploit-Vorgängen für Speicherkonten generiert. Diese Ereignisse können die folgenden Warnungen auslösen:

* **Access from unusual location** (Zugriff von einem ungewöhnlichen Ort): Diese Warnung wird ausgelöst, wenn sich am Zugriffsmuster eines Speicherkontos etwas ändert. Ein Beispiel hierfür ist der Fall, in dem eine Person von einem ungewöhnlichen geografischen Ort aus auf ein Speicherkonto zugreift. In einigen Fällen erkennt die Warnung eine legitime Aktion (eine neue Anwendung oder Wartungsvorgänge von Entwicklern). In anderen Fällen erkennt die Warnung eine schädliche Aktion (ehemaliger Mitarbeiter, externer Angreifer usw.).

* **Unusual data extraction** (Ungewöhnliche Datenextraktion): Diese Warnung wird ausgelöst, wenn sich das Muster der Datenextraktion für ein Speicherkonto ändert. Ein Beispiel hierfür ist der Fall, in dem eine Person in einem Speicherkonto auf eine ungewöhnliche Datenmenge zugreift. In einigen Fällen wird die Warnung auch bei einer legitimen Aktion (Wartungsaktivitäten) ausgelöst. In anderen Fällen erkennt die Warnung eine schädliche Aktion (Herausfilterung von Daten/Sicherheitsverletzung, unberechtigte Datenübertragung).

* **Unusual anonymous access** (Ungewöhnlicher anonymer Zugriff): Diese Warnung wird ausgelöst, wenn sich am Zugriffsmuster eines Speicherkontos etwas ändert. Ein Beispiel ist der anonyme Zugriff auf ein Speicherkonto. In einigen Fällen erkennt die Warnung auch einen legitimen Zugriff per öffentlichem Lesezugriff. In anderen Fällen erkennt die Warnung einen unberechtigten Zugriff, bei dem der öffentliche Lesezugriff auf einen Container und seine Blobs ausgenutzt wird.

* **Unexpected delete** (Unerwarteter Löschvorgang): Diese Warnung wird ausgelöst, wenn basierend auf der Verlaufsanalyse des Speicherkontos darin ein oder mehrere unerwartete Löschvorgänge durchgeführt werden. Ein Beispiel hierfür ist die Durchführung eines *DeleteBlob*-Vorgangs mit einer neuen Anwendung und von einer neuen IP-Adresse. In einigen Fällen erkennt die Warnung auch eine legitime Aktion (beispielsweise hat der Administrator auf einer Dienstreise einen anderen Browser genutzt). In anderen Fällen erkennt die Warnung eine schädliche Aktion (Angreifer löscht Daten). 
 
* **Access permission change** (Änderung der Zugriffsberechtigung): Diese Warnung wird ausgelöst, wenn für ein Speicherkonto eine unerwartete Änderung der Zugriffsberechtigung auftritt. Es kann beispielsweise sein, dass eine Person die Zugriffsberechtigung für ein Speicherkonto geändert hat, indem eine neue Anwendung und eine neue IP-Adresse verwendet wurden. In einigen Fällen erkennt die Warnung auch eine legitime Aktion (beispielsweise hat der Administrator auf einer Dienstreise einen anderen Browser genutzt). In anderen Fällen erkennt die Warnung eine schädliche Aktion (beispielsweise erhöht ein Angreifer die Berechtigungen eines Kontos, auf das er sich Zugriff verschafft hat). 

* **Upload Azure Cloud Service package** (Upload eines Azure-Clouddienstpakets): Diese Warnung wird ausgelöst, wenn es zu einem unerwarteten Upload eines Azure-Clouddienstpakets (*CSPKG*-Datei) in ein Speicherkonto kommt. Ein Beispiel hierfür ist das Hochladen einer *CSPKG*-Datei von einer neuen IP-Adresse. In einigen Fällen erkennt die Warnung auch eine legitime Aktion. In anderen Fällen erkennt die Warnung eine schädliche Aktion (z.B. das Hochladen eines Clouddienstpakets als Vorbereitung auf die Bereitstellung eines schädlichen Diensts).    
   

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Protokollen in Azure Storage-Konten](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Weitere Informationen zu [Azure Security Center](../../security-center/security-center-intro.md)