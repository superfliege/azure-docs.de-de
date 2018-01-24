---
title: "Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure Active Directory"
description: "Ein Überblick über die verwaltete Dienstidentität für Azure-Ressourcen."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ms.reviewer: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 53577c8da5f82235284d1cb9e48f2d47254aa6bd
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
---
#  <a name="managed-service-identity-msi-for-azure-resources"></a>Verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Eine gängige Herausforderung beim Erstellen von Cloudanwendungen ist die Verwaltung der Anmeldeinformationen, die für die Authentifizierung bei Clouddiensten im Code enthalten sein müssen. Der Schutz dieser Anmeldeinformationen ist eine wichtige Aufgabe. Im Idealfall werden sie nie auf Entwicklerarbeitsstationen angezeigt oder in die Quellcodeverwaltung eingecheckt. Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen und anderen Schlüsseln und Geheimnissen. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. Mithilfe einer verwalteten Dienstidentität (Managed Service Identity, MSI) kann dieses Problem leichter gelöst werden, indem für Azure-Dienste eine automatisch verwaltete Identität in Azure Active Directory (Azure AD) bereitgestellt wird. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung einschließlich von Key Vault unterstützt. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Wenn Sie die verwaltete Dienstidentität für eine Azure-Dienstinstanz erstellen, erstellt Azure eine Identität in dem Azure AD-Mandanten, der von Ihrem Azure-Abonnement verwendet wird. Zusätzlich stellt Azure der Dienstinstanz die Anmeldeinformationen für die Identität bereit. Als Ergebnis kann der Code anschließend eine lokale Anforderung zum Abrufen von Zugriffstoken für Dienste senden, die die Azure AD-Authentifizierung unterstützen. Derweil übernimmt Azure die Weitergabe der von der Dienstinstanz verwendeten Anmeldeinformationen.

## <a name="how-do-i-enable-my-resources-to-use-a-managed-service-identity"></a>Wie aktiviere ich meine Ressourcen für die Verwendung einer verwalteten Dienstidentität?

Es stehen zwei Arten von verwalteten Dienstidentitäten zur Verfügung: *durch das System zugewiesene* und *vom Benutzer zugewiesene* Identitäten.

- Eine **durch das System zugewiesene** verwaltete Dienstidentität wird direkt auf einer Azure-Dienstinstanz aktiviert. Durch den Aktivierungsprozess erstellt Azure eine Identität für die Dienstinstanz im Azure AD-Mandanten und stellt die Anmeldeinformationen für die Identität auf der Dienstinstanz bereit. Der Lebenszyklus einer durch das System zugewiesenen verwalteten Dienstidentität ist direkt an die Azure-Dienstinstanz gebunden, auf der sie aktiviert wurde. Wenn die Dienstinstanz gelöscht wird, bereinigt Azure automatisch die Anmeldeinformationen und die Identität in Azure AD.

- Eine **durch den Benutzer zugewiesene** verwaltete Dienstidentität *(private Vorschau)* wird als eigenständige Azure-Ressource erstellt. Durch den Erstellungsprozess erstellt Azure eine Identität im Azure AD-Mandanten. Nachdem die Identität erstellt wurde, kann sie einer oder mehreren Azure-Dienstinstanzen zugewiesen werden. Eine durch den Benutzer zugewiesene verwaltete Dienstidentität kann von verschiedenen Azure-Dienstinstanzen verwendet werden, deshalb wird ihr Lebenszyklus separat verwaltet.

Es folgt ein Beispiel für die Funktionsweise einer durch das System zugewiesenen verwalteten Dienstidentität mit virtuellen Azure-Computern.

![Beispiel für MSI für virtuelle Computer](~/articles/active-directory/media/msi-vm-example.png)

1. Azure Resource Manager empfängt eine Nachricht zur Aktivierung der verwalteten Dienstidentität auf einem virtuellen Computer.
2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD, um die Identität des virtuellen Computers darzustellen. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.
3. Azure Resource Manager konfiguriert die Details des Dienstprinzipals in der MSI-VM-Erweiterung des virtuellen Computers. Dieser Schritt umfasst das Konfigurieren der Client-ID und des Zertifikats, die von der Erweiterung zum Abrufen von Zugriffstokens von Azure AD verwendet werden.
4. Da die Dienstprinzipalidentität des virtuellen Computers nun bekannt ist, kann diesem der Zugriff auf Azure-Ressourcen erteilt werden. Müsste Ihr Code zum Beispiel Azure Resource Manager aufrufen, würden Sie dem Dienstprinzipal des virtuellen Computers die entsprechende Rolle mithilfe der rollenbasierten Zugriffssteuerung (RBAC) in Azure AD zuweisen.  Wenn Ihr Code Key Vault aufrufen müsste, würden Sie dem Code den Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault gewähren.
5. Der auf dem virtuellen Computer ausgeführte Code fordert ein Token von einem lokalen Endpunkt an, der von der MSI-VM-Erweiterung gehostet wird: http://localhost:50342/oauth2/token. Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Wenn der Code beispielsweise bei Azure Resource Manager authentifiziert werden soll, würden Sie Folgendes verwenden: „resource=https://management.azure.com/“.
6. Die MSI-VM-Erweiterung verwendet die konfigurierte Client-ID und das Zertifikat, um ein Zugriffstoken von Azure AD anzufordern.  Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.
7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.

Dieses Diagramm veranschaulicht auch das folgende Beispiel für die Funktionsweise einer durch den Benutzer zugewiesenen verwalteten Dienstidentität mit Azure Virtual Machines.

![Beispiel für MSI für virtuelle Computer](~/articles/active-directory/media/msi-vm-example.png)

1. Azure Resource Manager empfängt eine Nachricht zur Erstellung einer durch den Benutzer zugewiesenen Dienstidentität.
2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD, der die MSI-Identität repräsentiert. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.
3. Azure Resource Manager empfängt eine Nachricht zum Konfigurieren der Details des Dienstprinzipals in der MSI-VM-Erweiterung eines virtuellen Computers. Dieser Schritt umfasst das Konfigurieren der Client-ID und des Zertifikats, die von der Erweiterung zum Abrufen von Zugriffstokens von Azure AD verwendet werden.
4. Da die MSI-Dienstprinzipalidentität nun bekannt ist, kann der Zugriff auf Azure-Ressourcen erteilt werden. Müsste Ihr Code zum Beispiel Azure Resource Manager aufrufen, würden Sie dem MSI-Dienstprinzipal mithilfe der rollenbasierten Zugriffssteuerung (RBAC) die geeignete Rolle in Azure AD zuweisen. Wenn Ihr Code Key Vault aufrufen müsste, würden Sie dem Code den Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault gewähren. Hinweis: Schritt 3 ist zum Ausführen von Schritt 4 nicht erforderlich. Sobald eine verwaltete Dienstidentität vorhanden ist, kann dieser Ressourcenzugriff erteilt werden – unabhängig davon, ob die Konfiguration auf einer VM erfolgt oder nicht.
5. Der auf dem virtuellen Computer ausgeführte Code fordert ein Token von einem lokalen Endpunkt an, der von der MSI-VM-Erweiterung gehostet wird: http://localhost:50342/oauth2/token. Der Client-ID-Parameter gibt den Namen der MSI-Identität an, die verwendet werden soll. Zusätzlich gibt der Ressourcenparameter den Dienst an, an den das Token gesendet wird. Wenn der Code beispielsweise bei Azure Resource Manager authentifiziert werden soll, würden Sie Folgendes verwenden: „resource=https://management.azure.com/“.
6. Die MSI-VM-Erweiterung überprüft, ob das Zertifikat für die angeforderte Client-ID konfiguriert wurde, und fordert ein Zugriffstoken aus Azure AD an. Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.
7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.

Jeder Azure-Dienst, der verwaltete Dienstidentitäten unterstützt, verfügt über eine eigene Methode dafür, wie der Code ein Zugriffstoken erhält. In den Tutorials zu den jeweiligen Diensten finden Sie die einzelnen Methoden zum Abrufen eines Tokens.

## <a name="try-managed-service-identity"></a>Verwaltete Dienstidentitäten testen

Testen Sie ein Tutorial für verwaltete Dienstidentitäten, um mehr über End-to-End-Szenarios für den Zugriff auf verschiedene Azure-Ressourcen zu erfahren:
<br><br>
| Für MSI aktivierte Ressourcen | Lernen Sie Folgendes: |
| ------- | -------- |
| Azure-VM (Linux)   | [Zugreifen auf Azure Resource Manager mit einer Linux-VM-MSI](msi-tutorial-linux-vm-access-arm.md) |
|                    | [Zugreifen auf Azure Storage über einen Zugriffsschlüssel mit einer Linux-VM-MSI](msi-tutorial-linux-vm-access-storage.md) |

## <a name="which-azure-services-support-managed-service-identity"></a>Welche Azure-Dienste unterstützen verwaltete Dienstidentitäten?

Azure-Dienste, die verwaltete Dienstidentitäten unterstützen, können MSI zur Authentifizierung bei Diensten verwenden, die die Azure AD-Authentifizierung unterstützen.  MSI und die Azure AD-Authentifizierung werden derzeit in Azure integriert.  Sie sollten sich regelmäßig über Aktualisierungen informieren.

### <a name="azure-services-that-support-managed-service-identity"></a>Azure-Dienste, die verwaltete Dienstidentitäten unterstützen

Die folgenden Azure-Dienste unterstützen verwaltete Dienstidentitäten.

| Dienst | Status | Datum | Konfigurieren | Abrufen von Token |
| ------- | ------ | ---- | --------- | ----------- |
| Azure Virtual Machines | Vorschau | September 2017 | [Azure-CLI](msi-qs-configure-cli-windows-vm.md)<br>[Azure-Ressourcen-Manager-Vorlagen](msi-qs-configure-template-windows-vm.md) | [Bash/Curl](msi-how-to-use-vm-msi-token.md#get-a-token-using-curl)<br>[HTTP/REST](msi-how-to-use-vm-msi-token.md#get-a-token-using-http) |

### <a name="azure-services-that-support-azure-ad-authentication"></a>Azure-Dienste, die die Azure AD-Authentifizierung unterstützen

Die folgenden Dienste unterstützen die Azure AD-Authentifizierung und wurden mit Clientdiensten getestet, die verwaltete Dienstidentitäten verwenden.

| Dienst | Ressourcen-ID | Status | Datum | Zuweisen des Zugriffs |
| ------- | ----------- | ------ | ---- | ------------- |
| Azure Resource Manager | https://management.azure.com/ | Verfügbar | September 2017 | [Azure-CLI](msi-howto-assign-access-CLI.md) |
| Azure Key Vault | https://vault.azure.net/ | Verfügbar | September 2017 | |
| Azure Data Lake | https://datalake.azure.net/ | Verfügbar | September 2017 | |
| Azure SQL | https://database.windows.net/ | Verfügbar | Oktober 2017 | |

## <a name="how-much-does-managed-service-identity-cost"></a>Wie viel kosten verwaltete Dienstidentitäten?

Verwaltete Dienstidentitäten sind im Lieferumfang von Azure Active Directory Free enthalten. Dies ist die Standardbereitstellung für Azure-Abonnements.  Für verwaltete Dienstidentitäten fallen keine zusätzlichen Kosten an.

## <a name="support-and-feedback"></a>Support und Feedback

Wir freuen uns darauf, von Ihnen zu hören.

* Stellen Sie Fragen zu Stack Overflow mit dem Tag [azure-msi](http://stackoverflow.com/questions/tagged/azure-msi).
* Fordern Sie Funktionen an, oder geben Sie Feedback im [Azure AD-Feedbackforum für Entwickler](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences).






