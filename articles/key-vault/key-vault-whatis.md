---
title: Was ist der Azure-Schlüsseltresor? Azure Key Vault | Microsoft-Dokumentation
description: Azure Key Vault schützt kryptografische Schlüssel und Geheimnisse, die von Cloudanwendungen und -diensten verwendet werden. Kunden können Authentifizierungsschlüssel, Schlüssel für Speicherkonten, Datenverschlüsselungsschlüssel, PFX-Dateien und Kennwörter verschlüsseln, indem sie Schlüssel verwenden, die durch Hardwaresicherheitsmodule (HSMs) geschützt werden.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: db2e2e80ecc98eba0f1bba18abad8b561b16036c
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745859"
---
# <a name="what-is-azure-key-vault"></a>Was ist der Azure-Schlüsseltresor?

Mit Azure Key Vault lassen sich folgende Probleme lösen:

- **Geheimnisverwaltung**: Azure Key Vault ermöglicht die sichere Speicherung und präzise Steuerung des Zugriffs auf Token, Kennwörter, Zertifikate, API-Schlüssel und andere Geheimnisse.
- **Schlüsselverwaltung**: Azure Key Vault kann auch als Schlüsselverwaltungslösung verwendet werden. Azure Key Vault vereinfacht das Erstellen und Verwalten der zur Verschlüsselung Ihrer Daten verwendeten Verschlüsselungsschlüssel. 
- **Zertifikatverwaltung**: Darüber hinaus können Sie mit dem Azure Key Vault-Dienst komfortabel öffentliche und private SSL-/TLS-Zertifikate (Secure Sockets Layer/Transport Layer Security) für die Verwendung mit Azure und Ihren internen verbundenen Ressourcen bereitstellen und verwalten. 
- **Speichern von Geheimnissen mit Unterstützung durch Hardwaresicherheitsmodule**: Die Geheimnisse und Schlüssel können per Software oder mit durch FIPS 140-2 Level 2 validierten HSMs geschützt werden.

## <a name="basic-concepts"></a>Grundlegende Konzepte

Azure Key Vault ist ein Tool zum sicheren Speichern und Zugreifen auf Geheimnisse. Als Geheimnis wird alles bezeichnet, für das Sie den Zugriff streng kontrollieren möchten, z.B. API-Schlüssel, Kennwörter oder Zertifikate. Ein **Tresor** ist logische Gruppe von Geheimnissen. Damit Sie jetzt Vorgänge mit Key Vault durchführen können, müssen Sie sich zunächst authentifizieren. 

Grundsätzlich gibt es drei Möglichkeiten für die Authentifizierung bei Key Vault:

1. **Mithilfe [verwalteter Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)** (**Empfohlen und Best Practice**): Wenn Sie eine App auf einer VM in Azure bereitstellen, können Sie Ihrer VM eine Identität zuweisen, die Zugriff auf Key Vault hat. Sie können Identitäten auch anderen Azure-Ressourcen zuweisen, die [hier](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) aufgeführt sind. Der Vorteil bei diesem Ansatz ist, dass die App bzw. der Dienst die Rotation des ersten Geheimnisses nicht verwaltet. Die Identität wird von Azure automatisch gedreht. 
2. **Mithilfe von Dienstprinzipal und Zertifikat:** Die zweite Option besteht darin, einen Dienstprinzipal und ein zugehöriges Zertifikat zu verwenden, das Zugriff auf Key Vault hat. Dies wird allerdings nicht empfohlen, da die Verantwortung für die Rotation des Zertifikats liegt beim Anwendungsbesitzer oder -entwickler.
3. **Mithilfe von Dienstprinzipal und Geheimnis:** Die dritte Option (keine bevorzugte Option) besteht darin, zur Authentifizierung beim Key Vault einen Dienstprinzipal und ein Geheimnis zu verwenden.

> [!NOTE]
> Diese Option sollte nicht verwendet werden, da es schwierig ist, das Bootstrapgeheimnis für die Authentifizierung bei Key Vault automatisch zu rotieren.

Im Anschluss finden Sie einige wichtige Begriffe:

- **Tenant:** Ein Mandant ist die Organisation, die eine bestimmte Instanz von Microsoft-Clouddiensten besitzt und verwaltet. Der Begriff am häufigsten verwendet, um speziell auf die Azure- und Office 365-Dienste einer Organisation zu verweisen.
- **Tresorbesitzer:** Ein Tresorbesitzer kann einen Schlüsseltresor erstellen und über Vollzugriff sowie über uneingeschränkte Kontrolle verfügen. Außerdem kann der Tresorbesitzer auch eine Überprüfung einrichten, um zu protokollieren, wer auf Geheimnisse und Schlüssel zugreift. Administratoren können den Lebenszyklus des Schlüssels steuern. Sie können zu einer neuen Version des Schlüssels wechseln, den Schlüssel sichern und ähnliche Aufgaben ausführen.
- **Tresorconsumer:** Ein Tresorconsumer kann Aktionen für die Objekte im Schlüsseltresor ausführen, wenn der Tresorbesitzer dem Consumer Zugriff gewährt. Welche Aktionen verfügbar sind, hängt von den gewährten Berechtigungen ab.
- **Ressource:** Eine Ressource ist ein verwaltbares Element, das über Azure verfügbar ist. Beispiele für häufig verwendete Ressourcen sind virtueller Computer, Speicherkonto, Web-App, Datenbank und virtuelles Netzwerk, aber es sind noch viele weitere Ressourcen vorhanden.
- **Ressourcengruppe**: Eine Ressourcengruppe ist ein Container, der verwandte Ressourcen für eine Azure-Lösung enthält. Die Ressourcengruppe kann alle Ressourcen für die Lösung oder nur die Ressourcen enthalten, die Sie als Gruppe verwalten möchten. Sie entscheiden in Abhängigkeit davon, was für Ihre Organisation am sinnvollsten ist, wie Sie die Ressourcen den Ressourcengruppen zuordnen möchten.
- **Dienstprinzipal**: Ein Azure-Dienstprinzipal ist eine Sicherheitsidentität, mit der von Benutzern erstellte Apps, Dienste und Automatisierungstools auf bestimmte Azure-Ressourcen zugreifen. Das Konzept lässt sich als Benutzeridentität (Benutzername und Kennwort oder Zertifikat) mit einer bestimmten Rolle und streng kontrollierten Berechtigungen beschreiben. Ein Dienstprinzipal sollte – im Gegensatz zu einer allgemeinen Benutzeridentität – nur für bestimmte Dinge zuständig sein. Wenn Sie ihr nur die Berechtigungen gewähren, die sie zum Ausführen ihrer Verwaltungsaufgaben benötigt, verbessert das die Sicherheit.
- **[Azure Active Directory (Azure AD):](../active-directory/active-directory-whatis.md)** Azure AD ist der Active Directory-Dienst für einen Mandanten. Jedes Verzeichnis verfügt über mindestens eine Domäne. Einem Verzeichnis können viele Abonnements zugeordnet sein, aber nur ein Mandant. 
- **ID des Azure-Mandanten:** Eine Azure-Mandanten-ID ist eine Möglichkeit zur eindeutigen Identifizierung einer Azure AD-Instanz in einem Azure-Abonnement.
- **Verwaltete Identitäten für Azure-Ressourcen:** Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Die Verwendung einer verwalteten Identität vereinfacht die Lösung dieses Problems, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure AD bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei Key Vault oder jedem anderen Dienst verwenden, der die Azure AD-Authentifizierung unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein. Weitere Informationen finden Sie in der nachfolgenden Abbildung und unter [Verwaltete Identitäten für Azure-Ressourcen – Übersicht](../active-directory/managed-identities-azure-resources/overview.md).

    ![Diagramm zur Funktionsweise von verwalteten Identitäten für Azure-Ressourcen](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Key Vault-Rollen

Die folgende Tabelle enthält Informationen dazu, wie Sie mithilfe des Schlüsseltresors die Anforderungen von Entwicklern und Sicherheitsadministratoren erfüllen können.

| Rolle | Problembeschreibung | Lösung durch den Azure-Schlüsseltresor |
| --- | --- | --- |
| Entwickler einer Azure-Anwendung |„Ich möchte eine Anwendung für Azure schreiben, die Schlüssel für Anmeldung und Verschlüsselung verwendet, aber ich möchte diese Schlüssel außerhalb der Anwendung verwalten, damit die Lösung auch für eine geografisch verteilte Anwendung geeignet ist. <br/><br/>Ich möchte Schlüssel und Geheimnisse schützen, ohne den Code dafür selbst zu schreiben. Außerdem sollen die Schlüssel auf einfache Weise und mit optimaler Leistung in meinen Anwendungen verwendbar sein.“ |√ Schlüssel werden in einem Tresor gespeichert und bei Bedarf über einen URI aufgerufen.<br/><br/> √ Schlüssel werden mit branchenüblichen Algorithmen, Schlüssellängen und Hardwaresicherheitsmodulen von Azure geschützt.<br/><br/>  √ Schlüssel werden in HSMs verarbeitet, die sich in den gleichen Azure-Datencentern befinden wie die Anwendungen. Durch diese Methode werden im Gegensatz zu einer Lösung, bei der die Schlüssel an einem anderen Ort (beispielsweise lokal) vorliegen, eine höhere Zuverlässigkeit und eine geringere Latenz erreicht. |
| Entwickler von SaaS-Lösungen (Software-as-a-Service) |"Ich möchte weder die Verantwortung noch die Haftung für Schlüssel und geheime Schlüssel für Kundenmandanten übernehmen. <br/><br/>Ich möchte, dass Kunden sowohl die Verantwortung als auch die Verwaltung für ihre Schlüssel übernehmen, sodass ich mich auf meine eigentlichen Aufgaben konzentrieren kann – die Bereitstellung der Softwarefunktionen.“ |√ Kunden können eigene Schlüssel in Azure importieren und diese verwalten. Wenn eine SaaS-Anwendung kryptografische Vorgänge unter Verwendung der Kundenschlüssel ausführen muss, führt der Schlüsseltresor diese Vorgänge im Auftrag der Anwendung aus. Die Anwendung hat keine Informationen über die Kundenschlüssel. |
| Sicherheitsbeauftragter (Chief Security Officer, CSO) |"Ich möchte sicherstellen, dass unsere Anwendungen den Anforderungen für FIPS 140-2 Level 2 HSMs für die sichere Schlüsselverwaltung entsprechen. <br/><br/>Ich möchte sicherstellen, dass meine Organisation die Kontrolle über den gesamten Lebenszyklus der Schlüssel behält und die Schlüsselverwendung überwachen kann. <br/><br/>Auch wenn wir verschiedene Azure-Dienste und -Ressourcen nutzen, möchte ich die Schlüssel über einen einzigen Speicherort in Azure verwalten." |√ HSMs sind FIPS 140-2 Level 2-zertifiziert.<br/><br/>√ Mit Key Vault sind Ihre Schlüssel für Microsoft nicht sichtbar und können auch nicht extrahiert werden.<br/><br/>√ Die Schlüsselverwendung wird nahezu in Echtzeit protokolliert.<br/><br/>√ Der Tresor stellt eine einzige Benutzeroberfläche bereit – unabhängig davon, über wie viele Tresore Sie in Azure verfügen, welche Regionen diese unterstützen und von welchen Anwendungen die Tresore verwendet werden. |

Jeder Benutzer mit einem Azure-Abonnement kann Schlüsseltresore erstellen und verwenden. Wenngleich der Schlüsseltresor insbesondere Entwicklern und Sicherheitsadministratoren Vorteile bietet, kann er durch einen Organisationsadministrator implementiert und verwaltet werden, der andere Azure-Dienste für eine Organisation verwaltet. So kann sich dieser Administrator beispielsweise mit einem Azure-Abonnement anmelden, einen Tresor für die Schlüsselspeicherung der Organisation erstellen und dann operative Vorgänge wie die folgenden übernehmen:

- Erstellen oder Importieren eines Schlüssels oder geheimer Informationen
- Widerrufen oder Löschen eines Schlüssels oder geheimer Informationen
- Autorisieren von Benutzern oder Anwendungen für den Zugriff auf den Schlüsseltresor, sodass sie die Schlüssel und geheimen Schlüssel verwalten oder verwenden können
- Konfigurieren der Schlüsselverwendung (z. B. Anmelden oder Verschlüsseln)
- Übersachen der Schlüsselverwendung

Dieser Administrator kann anschließend den Entwicklern die URIs für den Aufruf der Schlüssel aus ihren Anwendungen und den Sicherheitsadministratoren Protokollinformationen zur Schlüsselverwendung bereitstellen. 

![Übersicht über die Funktionsweise von Azure Key Vault][1]

Entwickler können die Schlüssel außerdem durch Verwendung von APIs direkt verwalten. Weitere Informationen finden Sie im [Entwicklerhandbuch für den Schlüsseltresor](key-vault-developers-guide.md).

## <a name="next-steps"></a>Nächste Schritte

Gewusst wie: [Schützen Ihres Tresors](key-vault-secure-your-key-vault.md)

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure-Tresorschlüssel ist in den meisten Regionen verfügbar. Weitere Informationen finden Sie auf der Seite [Preisübersicht für Schlüsseltresor](https://azure.microsoft.com/pricing/details/key-vault/).
