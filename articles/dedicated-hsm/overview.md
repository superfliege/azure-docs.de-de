---
title: Was ist dediziertes HSM? | Microsoft-Dokumentation
description: Der Azure-Dienst für dedizierte HSMs bietet Schlüsselspeicherfunktionen innerhalb von Azure, die die Anforderungen der Zertifizierung „FIPS 140-2 Level 3“ erfüllen.
services: dedicated-hsm
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.date: 11/26/2018
ms.author: barclayn
ms.openlocfilehash: 92d77ec886a0f37c28f5e3031a7e14f63299c8aa
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52427113"
---
# <a name="what-is-dedicated-hsm"></a>Was ist dediziertes HSM?

Der Azure-Dienst für dedizierte HSMs bietet einen kryptografischen Schlüsselspeicher in Azure, der selbst strengste Sicherheitsanforderungen erfüllt. Dedizierte HSMs sind die optimale Lösung für Kunden, die Geräte mit der Zertifizierung „FIPS 140-2 Level 3“ sowie die vollständige und uneingeschränkte Kontrolle über die HSM-Appliance benötigen. Die HSM-Geräte werden weltweit in mehreren Azure-Regionen bereitgestellt und lassen sich problemlos als Gerätepaar bereitstellen und für hohe Verfügbarkeit konfigurieren. HSMs können zum Schutz vor regionalen Ausfällen auch regionsübergreifend bereitgestellt werden. Microsoft hat den Dienst für dedizierte HSMs mithilfe der Appliance [SafeNet Luna Network HSM 7 (Modell A790)](https://safenet.gemalto.com/data-encryption/hardware-security-modules-hsms/safenet-network-hsm/) von Gemalto implementiert. Dieses Gerät bietet ein Höchstmaß an Leistung und kryptografischen Integrationsoptionen. HSMs werden nach der Bereitstellung direkt mit dem virtuellen Netzwerk eines Kunden verbunden, und auf sie kann auch über lokale Anwendungen und Verwaltungstool zugegriffen werden. Hierzu ist eine Point-to-Site- oder eine Site-to-Site-VPN-Verbindung erforderlich. Kunden erhalten die Software und die Dokumentation für die Konfiguration und Verwaltung von HSM-Geräten über das Supportportal von Gemalto.

## <a name="why-use-azure-dedicated-hsm"></a>Argumente für die Verwendung des Azure-Diensts für dedizierte HSMs

### <a name="fips-140-2-level-3-compliance"></a>Konformität mit „FIPS 140-2 Level 3“

Aufgrund strenger branchenspezifischer Vorschriften muss in vielen Organisationen der kryptografische Schlüsselspeicher die Anforderungen von [FIPS 140-2 Level 3](https://csrc.nist.gov/publications/detail/fips/140/2/final) erfüllen. Der mehrinstanzenfähige Azure Key Vault-Dienst von Microsoft bietet derzeit nur eine Zertifizierung für „FIPS 140-2 Level 2“. Der Azure-Dienst für dedizierte HSMs ist besonders für Finanzdienstleister, Behörden und andere Kunden interessant, die die Anforderungen von „FIPS 140-2 Level 3“ erfüllen müssen.

### <a name="single-tenant-devices"></a>Geräte für einzelne Mandanten

Bei vielen unserer Kunden darf das kryptografische Speichergerät nur über einen einzelnen Mandanten verfügen. Der Azure-Dienst für dedizierte HSMs ermöglicht die Bereitstellung eines physischen Geräts über eines der weltweit verteilten Datencenter von Microsoft. Nach der Bereitstellung für einen Kunden hat nur noch dieser Kunde Zugriff auf das Gerät.

### <a name="full-administrative-control"></a>Vollständige administrative Kontrolle

Neben Geräten für einzelne Mandanten benötigen viele Kunden auch vollständige administrative Kontrolle sowie alleinigen Administratorzugriff. Nach der Bereitstellung des Geräts hat nur noch der jeweilige Kunde Zugriff auf die Administrator- oder Anwendungsebene. Microsoft hat nach dem ersten Zugriff des Kunden (bei dem auch das Kennwort geändert werden muss) keinerlei administrative Kontrolle mehr. Ab diesem Zeitpunkt ist der Kunde ein echter einzelner Mandant mit vollständiger administrativer Kontrolle und uneingeschränkter Anwendungsverwaltung. Über einen seriellen Port hat Microsoft zwar weiterhin Zugriff auf Telemetriedaten für die Hardwareüberwachung (Temperatur, Zustand der Stromversorgung, Lüfterzustand und Ähnliches), dabei handelt es sich aber nicht um eine Administratorrolle. Der Kunde hat die Möglichkeit, dies bei Bedarf zu deaktivieren. In diesem Fall erhält er allerdings proaktive Integritätswarnungen von Microsoft.

### <a name="high-performance"></a>Leistung

Das Gemalto-Gerät wurde aufgrund seiner breiten Unterstützung von Kryptografiealgorithmen, Betriebssystemen und APIs für diesen Dienst ausgewählt. Das bereitgestellte Modell bietet eine ausgezeichnete Leistung von 10.000 Vorgängen pro Sekunde für RSA-2048. Es unterstützt zehn Partitionen, die jeweils für individuelle Anwendungsinstanzen verwendet werden können. Das Gerät zeichnet sich durch geringe Wartezeit, hohe Kapazität und hohen Durchsatz aus.

### <a name="unique-cloud-based-offering"></a>Einzigartiges cloudbasiertes Angebot

Microsoft hat bei einigen Kunden einen ganz spezifischen Bedarf ausgemacht und ist der einzige Cloudanbieter, der neuen Kunden einen Dienst für dedizierte HSMs bietet, der die Anforderungen von „FIPS 140-2 Level 3“ erfüllt und über eine so umfangreiche Integration für cloudbasierte und lokale Anwendungen verfügt.

## <a name="is-azure-dedicated-hsm-right-for-you"></a>Ist der Azure-Dienst für dedizierte HSMs das Richtige für Sie?

Der Azure-Dienst für dedizierte HSMs ist ein spezieller Dienst, der die individuellen Anforderungen ganz bestimmter großer Organisationen erfüllt. Der Großteil der Azure-Kunden wird daher voraussichtlich keine Verwendung für diesen Dienst haben. In den meisten Fällen dürfte der Azure Key Vault-Dienst besser geeignet und kostengünstiger sein. Damit Sie leichter entscheiden können, ob der Dienst für Sie in Frage kommt, haben wir für Sie die folgenden Kriterien zusammengestellt.

### <a name="best-fit"></a>Optimal geeignet

Der Dienst eignet sich am besten für Lift & Shift-Szenarien, die einen direkten und alleinigen Zugriff auf HSM-Geräte erfordern. Beispiele:

- Migrieren von Anwendungen aus der lokalen Umgebung zu Azure Virtual Machines
- Migrieren von Anwendungen von Amazon AWS EC2 zu Azure Virtual Machines mit dem Dienst „AWS Cloud HSM Classic“ (Amazon bietet diesen Dienst nicht für neue Kunden an.)
- Ausführen von Software aus dem Handel in Azure Virtual Machines (etwa Apache/Ngnix SSL Offload, Oracle TDE und ADCS)

### <a name="not-a-fit"></a>Nicht geeignet

Für Microsoft-Clouddienste, die die Verschlüsselung mit von Kunden verwalteten Schlüsseln unterstützen (etwa Azure Information Protection, Azure Disk Encryption, Azure Data Lake Store, Azure Storage, Azure SQL, Office 365 Customer Key), steht keine Integration für den Azure-Dienst für dedizierte HSMs zur Verfügung.

### <a name="it-depends"></a>Abhängig vom Szenario

In vielen Szenarien kommt es darauf an, welche potenziell komplexe Kombination von Anforderungen erfüllt werden muss und welche Kompromisse möglich sind. Ein Beispiel wäre etwa „FIPS 140-2 Level 3“: Die Erfüllung dieser Anforderung ist häufig obligatorisch, was dediziertes HSM zur einzigen Option macht.  Wenn diese obligatorischen Anforderungen nicht relevant sind, hängt die Entscheidung zwischen Azure Key Vault und dediziertem HSM häufig von einer Mischung verschiedener Anforderungen ab. Beispiele:

- Neuer Code, der auf einem virtuellen Azure-Computer eines Kunden ausgeführt wird
- TDE von SQL Server auf einem virtuellen Azure-Computer
- Clientseitige Verschlüsselung von Azure Storage
- Always Encrypted für SQL Server und Azure SQL-Datenbank

## <a name="next-steps"></a>Nächste Schritte

Angesichts der überaus spezifischen Art dieses Diensts sollten Sie sich mit einigen der zentralen Konzepte in dieser Dokumentation sowie mit den Preisen, dem Support und den Vereinbarungen zum Servicelevel vertraut machen und sich anschließend das verfügbare Tutorial zur Bereitstellung von HSMs in einer vorhandenen virtuellen Netzwerkumgebung ansehen. [Integrationshandbücher von Gemalto](https://safenet.gemalto.com/partners/microsoft/) sowie Anleitungen für die Wahl der Bereitstellungsarchitektur sind ebenfalls hilfreich.

* [Hochverfügbarkeit](high-availability.md)
* [Physische Sicherheit](physical-security.md)
* [Netzwerk](networking.md)
* [Unterstützungsmöglichkeiten](supportability.md)
* [Überwachung](monitoring.md)
