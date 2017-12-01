---
title: "Blueprint zur Azure-Zahlungsverarbeitung – Verschlüsselungsanforderungen"
description: PCI-DSS-Anforderung 4
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 43f75ba9-cb4e-49ab-b3f4-09e48310bc18
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 3eb5b663558c2a68c13368b179ff942dd3c53716
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="encryption-requirements-for-pci-dss-compliant-environments"></a>Verschlüsselungsanforderungen für PCI-DSS-konforme Umgebungen 
## <a name="pci-dss-requirement-4"></a>PCI-DSS-Anforderung 4

**Verschlüsselung der Übertragung der Daten von Karteninhabern über offene öffentliche Netzwerke**

> [!NOTE]
> Diese Anforderungen werden vom [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als Teil des [PCI Data Security Standard (DSS) Version 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss) festgelegt. Informationen zu Testverfahren und Anleitungen für die einzelnen Anforderungen finden Sie im PCI-DSS.

Vertrauliche Informationen müssen während der Übertragung über Netzwerke, die für böswillige Benutzer leicht zugänglich sind, verschlüsselt werden. Falsch konfigurierte Drahtlosnetzwerke und Sicherheitsrisiken in älteren Verschlüsselungs- und Authentifizierungsprotokollen sind weiterhin Ziele für böswillige Benutzer, die diese Sicherheitsrisiken ausnutzen, um privilegierten Zugriff auf Umgebungen mit Daten von Karteninhabern zu erlangen.

## <a name="pci-dss-requirement-41"></a>PCI-DSS-Anforderung 4.1

**4.1** Verwenden Sie starke Kryptografie und Sicherheitsprotokolle (z. B. TLS, IPSEC, SSH usw.), um sensible Daten von Karteninhabern während der Übertragung über offene, öffentliche Netzwerke einschließlich der folgenden zu schützen:
- Nur vertrauenswürdige Schlüssel und Zertifikate werden akzeptiert.
- Das verwendete Protokoll unterstützt nur sichere Versionen oder Konfigurationen.
- Die Verschlüsselungsstärke eignet sich für die verwendete Verschlüsselungsmethodik. 

> [!NOTE]
> Bei Verwendung von SSL/frühem TLS müssen die Anforderungen in Anhang A2 erfüllt werden.
>
> Beispiele für offene, öffentliche Netzwerke sind u. a.:
> - Das Internet
> - Drahtlostechnologien einschließlich 802.11 und Bluetooth
> - Mobilfunktechnologien, z. B. Global System for Mobile Communications (GSM), Code Division Multiple Access (CDMA)
> - General Packet Radio Service (GPRS)
> - Satellitenkommunikation


**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Der Contoso Webstore ist eine PaaS-Lösung, die starke Kryptografie für die Bereitstellung wie folgt implementiert:<br /><br />[Azure Storage](https://azure.microsoft.com/services/storage/) verwendet Folgendes, um die Anforderungen für ruhende Daten zu erfüllen:<br /><br /><ul><li>[Azure Storage Service Encryption (SSE) für ruhende Daten](/azure/storage/storage-service-encryption)</li><li>SQL-Datenbank: Eine PaaS-SQL-Datenbank-Instanz wird zur Veranschaulichung von Sicherheitsmaßnahmen für die Datenbank verwendet. Weitere Informationen finden Sie in den [PCI-Anleitungen für Azure SQL-Datenbank](payment-processing-blueprint.md#azure-sql-database).</li><li>[Azure Disk Encryption (Bitlocker)](/azure/security/azure-security-disk-encryption)</li></ul>Die Verwendung von Azure Key Vault entspricht den Azure Government-, PCI-DSS- und HIPAA-Anforderungen.|



### <a name="pci-dss-requirement-411"></a>PCI-DSS-Anforderung 4.1.1

**4.1.1** Stellen Sie sicher, dass für Drahtlosnetzwerke, die Daten von Karteninhabern übertragen oder mit der Umgebung der Karteninhaberdaten verbunden sind, die bewährten Methoden der Branche (z. B. IEEE 802.11i) genutzt werden, um starke Verschlüsselung für die Authentifizierung und Übertragung zu implementieren.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | WLAN und SNMP sind nicht in der Lösung implementiert.|



## <a name="pci-dss-requirement-42"></a>PCI-DSS-Anforderung 4.2

**4.2** Senden Sie niemals ungeschützte PANs über Messagingtechnologien für Endbenutzer (z. B. E-Mail, Sofortnachrichten, SMS, Chat usw.).

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Im Contoso Webstore sind keine Messaginglösungen implementiert, die nicht geschützte PAN-Daten (Primary Account Number) senden können.|



## <a name="pci-dss-requirement-43"></a>PCI-DSS-Anforderung 4.3

**4.3** Stellen Sie sicher, dass Sicherheitsrichtlinien und betriebliche Verfahren für die Verschlüsselung der Übertragungen von Daten von Karteninhabern dokumentiert und verwendet werden sowie allen beteiligten Parteien bekannt sind.

**Zuständigkeiten:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Anbieter<br />(Microsoft&nbsp;Azure)** | Nicht zutreffend |
| **Kunde<br />(PCI&#8209;DSS&nbsp;-Plan)** | Kunden sind für die Dokumentation und Verschlüsselung von Übertragungen verantwortlich, die Daten von Karteninhabern enthalten.|




