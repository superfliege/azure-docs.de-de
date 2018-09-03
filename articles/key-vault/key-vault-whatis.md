---
title: Was ist der Azure-Schlüsseltresor? | Microsoft-Dokumentation
description: Azure Key Vault unterstützt Sie dabei, kryptografische Schlüssel und Geheimnisse zu schützen, die von Cloudanwendungen und -diensten verwendet werden. Durch Verwenden des Schlüsseltresors können Kunden Schlüssel und geheime Schlüssel (beispielsweise Authentifizierungsschlüssel, Schlüssel für Speicherkonten, Datenverschlüsselungsschlüssel, PFX-Dateien und Kennwörter) verschlüsseln, indem sie durch Hardwaresicherheitsmodule (HSMs) geschützte Schlüssel verwenden.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2018
ms.author: barclayn
ms.openlocfilehash: 26828efedac9953ce1c7375fc62269e93019ce50
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094869"
---
# <a name="what-is-azure-key-vault"></a>Was ist der Azure-Schlüsseltresor?

Mit Azure Key Vault lassen sich folgende Probleme lösen:
- Azure Key Vault ermöglicht die sichere Speicherung und präzise Steuerung des Zugriffs auf Token, Kennwörter, Zertifikate, API-Schlüssel und andere Geheimnisse.
- Azure Key Vault kann auch als Schlüsselverwaltungslösung verwendet werden. Azure Key Vault vereinfacht das Erstellen und Verwalten der zur Verschlüsselung Ihrer Daten verwendeten Verschlüsselungsschlüssel. 
- Darüber hinaus können Sie mit dem Azure Key Vault-Dienst komfortabel öffentliche und private SSL-/TLS-Zertifikate (Secure Sockets Layer/Transport Layer Security) für die Verwendung mit Azure und Ihren internen verbundenen Ressourcen bereitstellen und verwalten. 
- Die Geheimnisse und Schlüssel können entweder per Software oder mit FIPS 140-2 Level 2 HSMs geschützt werden.

## <a name="basic-concepts"></a>Grundlegende Konzepte

Azure Key Vault ist ein Tool zum sicheren Speichern und Zugreifen auf Geheimnisse. Als Geheimnis wird alles bezeichnet, für das Sie den Zugriff streng kontrollieren möchten, z.B. API-Schlüssel, Kennwörter oder Zertifikate.
Im Anschluss finden Sie einige wichtige Begriffe:
- **Mandant:** Ein Mandant ist die Organisation, die eine bestimmte Instanz von Microsoft-Clouddiensten besitzt und verwaltet. Der Begriff am häufigsten verwendet, um speziell auf die Azure- und Office 365-Dienste einer Organisation zu verweisen.
- **Tresorbesitzer:** Ein Tresorbesitzer kann einen Schlüsseltresor erstellen und über Vollzugriff sowie über uneingeschränkte Kontrolle verfügen. Außerdem kann der Tresorbesitzer auch eine Überprüfung einrichten, um zu protokollieren, wer auf Geheimnisse und Schlüssel zugreift. Administratoren können den Lebenszyklus des Schlüssels steuern. Sie können zu einer neuen Version des Schlüssels wechseln, den Schlüssel sichern und ähnliche Aufgaben ausführen.
- **Ressource:** Eine Ressource ist ein verwaltbares Element, das über Azure verfügbar ist. Beispiele für häufig verwendete Ressourcen sind virtueller Computer, Speicherkonto, Web-App, Datenbank und virtuelles Netzwerk, aber es sind noch viele weitere Ressourcen vorhanden.
- **Ressourcengruppe:** Eine Azure-Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Die Ressourcengruppe kann alle Ressourcen für die Lösung oder nur die Ressourcen enthalten, die Sie als Gruppe verwalten möchten. Sie entscheiden in Abhängigkeit davon, was für Ihre Organisation am sinnvollsten ist, wie Sie die Ressourcen den Ressourcengruppen zuordnen möchten.
- **Tresorconsumer:** Ein Tresorconsumer kann Aktionen für die Objekte im Schlüsseltresor ausführen, wenn der Tresorbesitzer dem Consumer Zugriff gewährt. Welche Aktionen verfügbar sind, hängt von den gewährten Berechtigungen ab.
- **[Azure Active Directory (Azure AD):](../active-directory/active-directory-whatis.md)** Azure AD ist der Active Directory-Dienst für einen Mandanten. Jedes Verzeichnis verfügt über mindestens eine Domäne. Einem Verzeichnis können viele Abonnements zugeordnet sein, aber nur ein Mandant. 
- **Azure-Mandanten-ID:** Eine Azure-Mandanten-ID ist eine Möglichkeit zur eindeutigen Identifizierung einer Azure AD-Instanz in einem Azure-Abonnement.
- **Verwaltete Dienstidentität:** Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code aber bei Key Vault authentifizieren. Mithilfe der verwalteten Dienstidentität kann dieses Problem leichter gelöst werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure AD bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei Key Vault oder jedem anderen Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. Weitere Informationen finden Sie im Artikel [Was ist die verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen?](../active-directory/managed-service-identity/overview.md).

    ![Diagramm der Funktionsweise der verwalteten Dienstidentität](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Key Vault-Rollen

Die folgende Tabelle enthält Informationen dazu, wie Sie mithilfe des Schlüsseltresors die Anforderungen von Entwicklern und Sicherheitsadministratoren erfüllen können.

| Rolle | Problembeschreibung | Lösung durch den Azure-Schlüsseltresor |
| --- | --- | --- |
| Entwickler einer Azure-Anwendung |„Ich möchte eine Anwendung für Azure schreiben, die Schlüssel für Anmeldung und Verschlüsselung verwendet, aber ich möchte diese Schlüssel außerhalb der Anwendung verwalten, damit die Lösung auch für eine geografisch verteilte Anwendung geeignet ist. <br/><br/>Ich möchte Schlüssel und Geheimnisse schützen, ohne den Code dafür selbst zu schreiben. Außerdem sollen die Schlüssel auf einfache Weise und mit optimaler Leistung in meinen Anwendungen verwendbar sein.“ |√ Schlüssel werden in einem Tresor gespeichert und bei Bedarf über einen URI aufgerufen.<br/><br/> √ Schlüssel werden mit branchenüblichen Algorithmen, Schlüssellängen und Hardwaresicherheitsmodulen von Azure geschützt.<br/><br/> √ Schlüssel werden in HSMs verarbeitet, die sich in den gleichen Azure-Datencentern befinden wie die Anwendungen. Durch diese Methode werden im Gegensatz zu einer Lösung, bei der die Schlüssel an einem anderen Ort (beispielsweise lokal) vorliegen, eine höhere Zuverlässigkeit und eine geringere Latenz erreicht. |
| Entwickler von SaaS-Lösungen (Software-as-a-Service) |"Ich möchte weder die Verantwortung noch die Haftung für Schlüssel und geheime Schlüssel für Kundenmandanten übernehmen. <br/><br/>Ich möchte, dass Kunden sowohl die Verantwortung als auch die Verwaltung für ihre Schlüssel übernehmen, sodass ich mich auf meine eigentlichen Aufgaben konzentrieren kann – die Bereitstellung der Softwarefunktionen.“ |√ Kunden können eigene Schlüssel in Azure importieren und diese verwalten. Wenn eine SaaS-Anwendung kryptografische Vorgänge unter Verwendung der Kundenschlüssel ausführen muss, führt der Schlüsseltresor diese Vorgänge im Auftrag der Anwendung aus. Die Anwendung hat keine Informationen über die Kundenschlüssel. |
| Sicherheitsbeauftragter (Chief Security Officer, CSO) |"Ich möchte sicherstellen, dass unsere Anwendungen den Anforderungen für FIPS 140-2 Level 2 HSMs für die sichere Schlüsselverwaltung entsprechen. <br/><br/>Ich möchte sicherstellen, dass meine Organisation die Kontrolle über den gesamten Lebenszyklus der Schlüssel behält und die Schlüsselverwendung überwachen kann. <br/><br/>Auch wenn wir verschiedene Azure-Dienste und -Ressourcen nutzen, möchte ich die Schlüssel über einen einzigen Speicherort in Azure verwalten." |√ HSMs sind FIPS 140-2 Level 2-zertifiziert.<br/><br/>√ Mit Key Vault sind Ihre Schlüssel für Microsoft nicht sichtbar und können auch nicht extrahiert werden.<br/><br/>√ Die Schlüsselverwendung wird nahezu in Echtzeit protokolliert.<br/><br/>√ Der Tresor stellt eine einzige Benutzeroberfläche bereit – unabhängig davon, über wie viele Tresore Sie in Azure verfügen, welche Regionen diese unterstützen und von welchen Anwendungen die Tresore verwendet werden. |

Jeder Benutzer mit einem Azure-Abonnement kann Schlüsseltresore erstellen und verwenden. Wenngleich der Schlüsseltresor insbesondere Entwicklern und Sicherheitsadministratoren Vorteile bietet, kann er durch einen Organisationsadministrator implementiert und verwaltet werden, der andere Azure-Dienste für eine Organisation verwaltet. So kann sich dieser Administrator beispielsweise mit einem Azure-Abonnement anmelden, einen Tresor für die Schlüsselspeicherung der Organisation erstellen und dann operative Vorgänge wie die folgenden übernehmen:

* Erstellen oder Importieren eines Schlüssels oder geheimer Informationen
* Widerrufen oder Löschen eines Schlüssels oder geheimer Informationen
* Autorisieren von Benutzern oder Anwendungen für den Zugriff auf den Schlüsseltresor, sodass sie die Schlüssel und geheimen Schlüssel verwalten oder verwenden können
* Konfigurieren der Schlüsselverwendung (z. B. Anmelden oder Verschlüsseln)
* Übersachen der Schlüsselverwendung

Dieser Administrator kann anschließend den Entwicklern die URIs für den Aufruf der Schlüssel aus ihren Anwendungen und den Sicherheitsadministratoren Protokollinformationen zur Schlüsselverwendung bereitstellen. 

![Übersicht über den Azure-Schlüsseltresor][1]

Entwickler können die Schlüssel außerdem durch Verwendung von APIs direkt verwalten. Weitere Informationen finden Sie im [Entwicklerhandbuch für den Schlüsseltresor](key-vault-developers-guide.md).

## <a name="next-steps"></a>Nächste Schritte

Ein Tutorial mit ersten Schritten für Administratoren finden Sie unter [Erste Schritte mit dem Azure-Schlüsseltresor](key-vault-get-started.md).

Weitere Informationen zur Verwendungsprotokollierung für Key Vault finden Sie unter [Azure-Schlüsseltresor-Protokollierung](key-vault-logging.md).

Weitere Informationen zur Verwendung von Schlüsseln und Geheimnissen mit Azure Key Vault finden Sie unter [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure-Tresorschlüssel ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie auf der Seite [Preisübersicht für Schlüsseltresor](https://azure.microsoft.com/pricing/details/key-vault/).
