---
title: Unterstützbarkeit – Azure Dedicated HSM | Microsoft-Dokumentation
description: Unterstützungsoptionen und Zuständigkeitsbereiche für Azure Dedicated HSM in verschiedenen Szenarios
services: dedicated-hsm
author: barclayn
manager: mbaldwin
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/07/2018
ms.author: barclayn
ms.openlocfilehash: 2ed6a79b8736a1d3b472e31ce643c0d1ee085bbb
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/08/2018
ms.locfileid: "53085908"
---
# <a name="azure-dedicated-hsm-supportability"></a>Azure-Dienst für dedizierte HSMs – Unterstützbarkeit

Der Azure-Dienst für dedizierte HSMs stellt ein physisches Gerät für die Verwendung durch einen Kunden mit umfassender administrativer Kontrolle und Verwaltungsverantwortung bereit. Ein [Gemalto SafeNet Luna 7 HSM Modell A790](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) wird zur Verfügung gestellt. Microsoft hat nach Bereitstellung durch den Kunden keinen administrativen Zugriff, abgesehen von der Zuordnung eines physischen seriellen Anschlusses für die Überwachungsrolle.  Ohne Zugriff kann Microsoft keine Verantwortung für die laufende Wartung auf Softwareebene oder Systemverwaltung übernehmen. Daher sind die Kunden für typische Betriebsaktivitäten verantwortlich.
Die Kunden sind vollständig verantwortlich für Anwendungen, die die HSMs nutzen, und sollten hinsichtlich Support bzw. Assistenz auf Beraterbasis mit Gemalto kooperieren. Aufgrund des Ausmaßes, in dem der Kunde für den ordnungsgemäßen Betrieb verantwortlich ist, kann Microsoft keinerlei Garantie für die Hochverfügbarkeit dieses Diensts bieten. Es liegt in der Verantwortung des Kunden, sicherzustellen, dass seine Anwendungen ordnungsgemäß konfiguriert sind, um Hochverfügbarkeit zu erzielen. Microsoft überwacht und wartet Geräteintegrität und Netzwerkkonnektivität.

## <a name="gemalto-support"></a>Gemalto-Support

Kunden, die den Dienst für dedizierte HSMs nutzen, benötigen einen Supportvertrag mit Gemalto. Kunden erhalten im Rahmen ihres Supportvertrags Anleitungen, Support und Dienste direkt von Gemalto. Gemalto bietet Support über das [Kundensupportportal](https://supportportal.gemalto.com/csm/).
Gemalto stellt alle Softwarekomponenten bereit, die erforderlich sind, um das HSM zu verwenden (z.B. Clientzugriffsoftware und SDKs). Es wird auch Support zur Konfiguration geleistet, und Beratungsdienste für Entwurf, Entwicklung und Bereitstellung von Anwendungen mit dem SafeNet Luna 7 HSM werden angeboten.

### <a name="software-components"></a>Softwarekomponenten

Diverse Softwarekomponenten werden bei der Konfiguration des HSM-Geräts verwendet:

* Clientsoftware
* SDK
* Tools

### <a name="guidance"></a>Anleitungen

Gemalto stellt im [Kundensupportportal](https://supportportal.gemalto.com/csm/) Leitfäden für Verwaltung und Konfiguration bereit. Nach der Anmeldung mit einer gültigen Kunden-ID stehen diese Dokumente zum Herunterladen zur Verfügung. Gemalto bietet auch eine Reihe von Integrationshandbüchern, um Kunden mit verschiedenen Szenarien und Softwareintegrationen zu unterstützen. Weitere Informationen finden Sie auf der [Gemalto-Partnerwebsite für Microsoft](https://safenet.gemalto.com/partners/microsoft/).

### <a name="support"></a>Support

Alle Probleme auf Softwareebene oder Fragen in Bezug auf die Verwendung der HSMs als Teil des Diensts für dedizierte HSMs sollten direkt an Gemalto gemeldet bzw. gestellt werden. Gemalto ist bezüglich aller oben aufgeführten Softwarekomponenten und jeder benutzerdefinierten HSM-Konfiguration nach der Bereitstellung ansprechbar. Weitere Informationen finden Sie im [Gemalto-Kundensupportportal](https://supportportal.gemalto.com/csm/).

### <a name="consulting-services"></a>Beratungsdienste

Um Unterstützung zu Entwurf, Entwicklung und Bereitstellung von benutzerdefinierten Anwendungen zu erhalten, die das HSM verwenden, wenden Sie sich an Ihren Gemalto-Kundenbetreuer.

## <a name="microsoft-support"></a>Microsoft-Support

Microsoft ist dafür verantwortlich, sicherzustellen, dass physische HSM-Geräte für die exklusive Verwendung eines einzelnen Kunden erreichbar und in einem funktionsfähigen Zustand sind. Die Kunden sind verantwortlich für die Administration und Verwaltung des Geräts. Unter die Verantwortung von Microsoft fällt:

* Sicherstellen, dass das Gerät mit Strom versorgt und gekühlt wird
* Warten eines funktionsfähigen Zustands des HSM (z.B. Problemlösungsszenarios)
* Über das Netzwerk kann auf das Gerät zugegriffen werden.

Probleme wie die folgenden sollten an Microsoft gemeldet werden:

* Ausfälle von Komponenten
* Vollständiger Geräteausfall
* Netzwerkzugriffsprobleme
* Probleme bei der Bereitstellung und Bereitstellungsaufhebung.

Microsoft hat über den physischen seriellen Anschluss Zugriff auf das Gerät über eine Überwachungsrolle (d.h. keine administrative Rolle), die grundlegende Integritätsdatentelemetrie ermöglicht.  Dies ermöglicht Microsoft, den Kunden bei Problemen proaktiv zu benachrichtigen, es sei denn, der Kunde entscheidet sich, diese Berechtigung einzuschränken. 

### <a name="provisioning-and-decommissioning"></a>Bereitstellung und Bereitstellungsaufhebung

Sobald ein Kunde über eine genehmigte Registrierung für den dedizierten HSM-Dienst verfügt, kann er HSM-Ressourcen erstellen (derzeit über PowerShell oder die Befehlszeilenschnittstelle und nicht im Azure-Portal). Die Ressource durchläuft einen Zuordnungsprozess, in dem ein physisches Gerät in einer angegebenen Region dem vorab definierten virtuellen Netzwerk (VNET) eines Kunden zugeordnet wird. Wenn das Gerät in einem VNET angezeigt wird, kann der Kunde darauf zugreifen und es gemäß den Anforderungen weiter konfigurieren. Die Kunden greifen mit Gemalto-Clientsoftware und -Tools auf ihre dedizierten HSMs zu. Die Ressourcenerstellung wird von Microsoft unterstützt. Für die benutzerdefinierte Konfiguration und darüber hinaus bietet Gemalto Support. (Gemalto-Support siehe oben). Wenn ein Kunde die Verwendung eines HSM beendet hat, muss es zurückgesetzt (oder auf null gesetzt) werden, um Datenpersistenz auszuschließen. Beim Zurücksetzen des Geräts werden alle benutzerdefinierten Konfigurationen und Daten entfernt. Microsoft hebt die Zuordnung des Geräts auf und gibt es in makellosem Zustand an den Pool zurück. Dies bedeutet, dass das Gerät bei Rückgabe an den Pool keine Anzeichen früherer Kundenaktivitäten mehr aufweist. 

### <a name="hardware-issues"></a>Hardwareprobleme

Das HSM-Gerät verfügt über redundante und austauschbare Stromversorgungs- und Lüftereinheiten. Das Entfernen der Lüftereinheit löst ein Manipulationsereignis aus, wenn das Gerät beim Entfernen eingeschaltet ist. Beim Ausfall einer Komponente verwendet Microsoft das am besten geeignete Verfahren, um das Problem auf Komponentenebene so zu behandeln, dass nur eine minimale Unterbrechung auftritt und das niedrigste Risiko für die Dienstverfügbarkeit der Kunden besteht.
Bei jedem schwerwiegenderen Ausfall des Geräts wird dieses Gerät durch ein frisches aus dem freien Pool ersetzt. Der Kunde fügt das neue Gerät einfach in das vorhandene Hochverfügbarkeitspaar ein, damit es synchronisiert und wieder in einen voll betriebsfähigen Zustand versetzt wird. Die Daten enthaltenden Komponenten des ausgefallenen Geräts werden entfernt und vor Ort im Rechenzentrum zerstört. Nur das Gehäuse wird zum Recycling an Gemalto zurückgegeben.


### <a name="networking-issues"></a>Netzwerkprobleme

Wenn Kunden Netzwerkprobleme beim Zugriff auf das HSM-Gerät feststellen, sollten sie sich an den Microsoft-Support wenden. Ein einfacher Netzwerkzugriffstest ist, mit SSH eine Verbindung mit dem HSM-Gerät herzustellen. Wenn dies nicht gelingt, wenden Sie sich an den Microsoft-Support.

## <a name="service-level-expectations-for-support"></a>Servicelevelerwartungen für den Support

Microsoft-Support-Servicelevel siehe [Azure-Supportplan](https://azure.microsoft.com/support/plans/).
Gemalto-Support-Servicelevel siehe [Gemalto Support Essentials](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Nächste Schritte

Der Benutzer sollte vor der Bereitstellung von Geräten, dem Anwendungsentwurf und der Bereitstellung mit Schlüsselkonzepten wie hohe Verfügbarkeit und Sicherheit gut vertraut sein.

* [Bereitstellungsarchitektur](deployment-architecture.md)
* [Hochverfügbarkeit](high-availability.md)
* [Physische Sicherheit](physical-security.md)
* [Netzwerk](networking.md)
* [Überwachung](monitoring.md)