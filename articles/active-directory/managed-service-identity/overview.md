---
title: Verwaltete Dienstidentität für Azure-Ressourcen
description: Ein Überblick über die verwaltete Dienstidentität für Azure-Ressourcen.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 0232041d-b8f5-4bd2-8d11-27999ad69370
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: overview
ms.custom: mvc
ms.date: 03/28/2018
ms.author: daveba
ms.openlocfilehash: 1edd6f846d539b1f263877a5e0af107148513c6e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398384"
---
# <a name="what-is-managed-service-identity-for-azure-resources"></a>Was ist die verwaltete Dienstidentität (Managed Service Identity, MSI) für Azure-Ressourcen?

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Eine gängige Herausforderung beim Erstellen von Cloudanwendungen ist die Verwaltung der Anmeldeinformationen im Code, die für die Authentifizierung bei Clouddiensten erforderlich sind. Der Schutz dieser Anmeldeinformationen ist eine wichtige Aufgabe. Im Idealfall werden die Anmeldeinformationen nie auf Entwicklerarbeitsstationen angezeigt und auch nicht in die Quellcodeverwaltung eingecheckt. Azure Key Vault bietet eine Möglichkeit zum sicheren Speichern von Anmeldeinformationen, Geheimnissen und anderen Schlüsseln. Um diese abrufen zu können, muss sich Ihr Code jedoch bei Key Vault authentifizieren. 

Die verwaltete Dienstidentität in Azure Active Directory (Azure AD) löst dieses Problem. Dieses Feature stellt für Azure-Dienste eine automatisch verwaltete Identität in Azure AD bereit. Sie können diese Identität für die Authentifizierung bei jedem Dienst verwenden, der die Azure AD-Authentifizierung unterstützt, einschließlich Key Vault. Hierfür müssen keine Anmeldeinformationen im Code enthalten sein.

Die verwaltete Dienstidentität ist bei Azure AD mit einem Azure-Abonnement kostenlos. Es fallen keine zusätzlichen Kosten an.

## Wie funktioniert das Feature?<a name="how-does-it-work"></a>

Es gibt zwei Arten von verwalteten Identitäten:

- Eine **vom System zugewiesene Identität** wird direkt für eine Azure-Dienstinstanz aktiviert. Wenn die Identität aktiviert ist, erstellt Azure eine Identität für die Instanz in dem Azure AD-Mandanten, der vom Abonnement der Instanz als vertrauenswürdig eingestuft wird. Nach dem Erstellen der Identität werden die Anmeldeinformationen in der Instanz bereitgestellt. Der Lebenszyklus einer vom System zugewiesenen Identität ist direkt an die Azure-Dienstinstanz gebunden, für die sie aktiviert wurde. Wenn die Instanz gelöscht wird, bereinigt Azure automatisch die Anmeldeinformationen und die Identität in Azure AD.
- Eine **vom Benutzer zugewiesene Identität** wird als eigenständige Azure-Ressource erstellt. Azure erstellt eine Identität in dem Azure AD-Mandanten, der vom verwendeten Abonnement als vertrauenswürdig eingestuft wird. Nachdem die Identität erstellt wurde, kann sie einer oder mehreren Azure-Dienstinstanzen zugewiesen werden. Der Lebenszyklus einer vom Benutzer zugewiesenen Identität wird getrennt vom Lebenszyklus der Azure-Dienstinstanzen verwaltet, denen sie zugewiesen ist.

Ihr Code kann eine verwaltete Dienstidentität zum Anfordern von Zugriffstokens für Dienste verwenden, die die Azure AD-Authentifizierung unterstützen. Azure übernimmt die Weitergabe der von der Dienstinstanz verwendeten Anmeldeinformationen.

Das folgende Diagramm zeigt, wie verwaltete Dienstidentitäten mit virtuellen Azure-Computern funktionieren:

![Verwaltete Dienstidentitäten und Azure-VMs](media/overview/msi-vm-vmextension-imds-example.png)

### <a name="how-a-system-assigned-identity-works-with-an-azure-vm"></a>Funktionsweise einer vom System zugewiesenen Identität mit einer Azure-VM

1. Azure Resource Manager empfängt eine Anforderung zur Aktivierung der durch das System verwalteten Identität auf einem virtuellen Computer.
2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD für die Identität des virtuellen Computers. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.
3. Azure Resource Manager konfiguriert die Identität auf dem virtuellen Computer:
    1. Aktualisiert den Azure Instance Metadata Service-Identitätsendpunkt mit der Client-ID und dem Zertifikat des Dienstprinzipals.
    1. Stellt die VM-Erweiterung bereit und fügt die Client-ID und das Zertifikat des Dienstprinzipals hinzu. (Wird in Kürze als veraltet eingestuft.)
4. Wenn der virtuelle Computer über eine Identität verfügt, verwenden wir die Dienstprinzipalinformationen, um ihm Zugriff auf Azure-Ressourcen zu gewähren. Verwenden Sie zum Aufrufen von Azure Resource Manager die rollenbasierte Zugriffssteuerung (RBAC) in Azure AD, um dem VM-Dienstprinzipal die entsprechende Rolle zuzuweisen. Gewähren Sie Ihrem Code zum Aufrufen von Key Vault Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault.
5. Der auf dem virtuellen Computer ausgeführte Code kann ein Token von zwei Endpunkten anfordern, auf die nur von dem virtuellen Computer aus zugegriffen werden kann:

    - Identitätsendpunkt von Azure Instance Metadata Service (empfohlen): `http://169.254.169.254/metadata/identity/oauth2/token`
        - Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Verwenden Sie `resource=https://management.azure.com/` für die Authentifizierung bei Azure Resource Manager.
        - Der API-Versionsparameter gibt die IMDS-Version an. Verwenden Sie mindestens „api-version=2018-02-01“.
    - VM-Erweiterungsendpunkt (wird in Kürze als veraltet eingestuft): `http://localhost:50342/oauth2/token` 
        - Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Verwenden Sie `resource=https://management.azure.com/` für die Authentifizierung bei Azure Resource Manager.

6. Mit einem an Azure AD gerichteten Aufruf wird ein Zugriffstoken angefordert (siehe Schritt 5). Dabei werden die Client-ID und das Zertifikat verwendet, die in Schritt 3 konfiguriert wurden. Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.
7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.

### <a name="how-a-user-assigned-identity-works-with-an-azure-vm"></a>Funktionsweise einer vom Benutzer zugewiesenen Identität mit einer Azure-VM

1. Azure Resource Manager empfängt eine Anforderung zur Erstellung einer durch den Benutzer zugewiesenen Identität.
2. Azure Resource Manager erstellt einen Dienstprinzipal in Azure AD für die vom Benutzer zugewiesene Identität. Der Dienstprinzipal wird in dem Azure AD-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.
3. Azure Resource Manager empfängt eine Anforderung zur Konfiguration der durch den Benutzer verwalteten Identität auf einem virtuellen Computer:
    1. Aktualisiert den Azure Instance Metadata Service-Identitätsendpunkt mit der Client-ID und dem Zertifikat des Dienstprinzipals der vom Benutzer zugewiesenen Identität.
    1. Stellt die VM-Erweiterung bereit und fügt die Client-ID und das Zertifikat des Dienstprinzipals der vom Benutzer zugewiesenen Identität hinzu. (Wird in Kürze als veraltet eingestuft.)
4. Nachdem die vom Benutzer zugewiesene Identität erstellt wurde, verwenden wir die Dienstprinzipalinformationen, um der Identität Zugriff auf Azure-Ressourcen zu gewähren. Verwenden Sie zum Aufrufen von Azure Resource Manager die RBAC in Azure AD, um dem Dienstprinzipal der vom Benutzer zugewiesenen Identität die entsprechende Rolle zuzuweisen. Gewähren Sie Ihrem Code zum Aufrufen von Key Vault Zugriff auf das spezifische Geheimnis oder den spezifischen Schlüssel in Key Vault.

   > [!Note]
   > Sie können diesen Schritt auch vor Schritt 3 ausführen.

5. Der auf dem virtuellen Computer ausgeführte Code kann ein Token von zwei Endpunkten anfordern, auf die nur von dem virtuellen Computer aus zugegriffen werden kann:

    - Identitätsendpunkt von Azure Instance Metadata Service (empfohlen): `http://169.254.169.254/metadata/identity/oauth2/token`
        - Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Verwenden Sie `resource=https://management.azure.com/` für die Authentifizierung bei Azure Resource Manager.
        - Der Client-ID-Parameter gibt die Identität an, für die das Token angefordert wird. Dieser Wert ist erforderlich, um Mehrdeutigkeiten zu vermeiden, wenn sich auf einem einzelnen virtuellen Computer mehrere durch den Benutzer zugewiesene Identitäten befinden.
        - Der API-Versionsparameter gibt die Azure Instance Metadata Service-Version an. Verwenden Sie `api-version=2018-02-01` oder höher.

    - VM-Erweiterungsendpunkt (wird in Kürze als veraltet eingestuft): `http://localhost:50342/oauth2/token`
        - Der Ressourcenparameter gibt den Dienst an, an den das Token gesendet wird. Verwenden Sie `resource=https://management.azure.com/` für die Authentifizierung bei Azure Resource Manager.
        - Der Client-ID-Parameter gibt die Identität an, für die das Token angefordert wird. Dieser Wert ist erforderlich, um Mehrdeutigkeiten zu vermeiden, wenn sich auf einem einzelnen virtuellen Computer mehrere durch den Benutzer zugewiesene Identitäten befinden.
6. Mit einem an Azure AD gerichteten Aufruf wird ein Zugriffstoken angefordert (siehe Schritt 5). Dabei werden die Client-ID und das Zertifikat verwendet, die in Schritt 3 konfiguriert wurden. Azure AD gibt ein JWT-Zugriffstoken (JSON Web Token) zurück.
7. Der Code sendet das Zugriffstoken in einem Aufruf eines Diensts, der die Azure AD-Authentifizierung unterstützt.

## <a name="how-can-i-use-managed-service-identities"></a>Wie kann ich verwaltete Dienstidentitäten verwenden?

In den folgenden Tutorials erfahren Sie mehr darüber, wie Sie verwaltete Dienstidentitäten für den Zugriff auf verschiedene Azure-Ressourcen verwenden können.

Erfahren Sie, wie Sie eine verwaltete Dienstidentität mit einer Windows-VM verwenden:

* [Zugreifen auf Azure Data Lake Store](tutorial-windows-vm-access-datalake.md)
* [Zugreifen auf Azure Resource Manager](tutorial-windows-vm-access-arm.md)
* [Zugreifen auf Azure SQL](tutorial-windows-vm-access-sql.md)
* [Zugreifen auf Azure Storage mithilfe eines Zugriffsschlüssels](tutorial-windows-vm-access-storage.md)
* [Zugreifen auf Azure Storage mithilfe von SAS-Anmeldeinformationen](tutorial-windows-vm-access-storage-sas.md)
* [Zugreifen auf eine Nicht-Azure AD-Ressource mithilfe von Azure Key Vault](tutorial-windows-vm-access-nonaad.md)

Erfahren Sie, wie Sie eine verwaltete Dienstidentität mit einer Linux-VM verwenden:

* [Zugreifen auf Azure Data Lake Store](tutorial-linux-vm-access-datalake.md)
* [Zugreifen auf Azure Resource Manager](tutorial-linux-vm-access-arm.md)
* [Zugreifen auf Azure Storage mithilfe eines Zugriffsschlüssels](tutorial-linux-vm-access-storage.md)
* [Zugreifen auf Azure Storage mithilfe von SAS-Anmeldeinformationen](tutorial-linux-vm-access-storage-sas.md)
* [Zugreifen auf eine Nicht-Azure AD-Ressource mithilfe von Azure Key Vault](tutorial-linux-vm-access-nonaad.md)

Erfahren Sie, wie Sie eine verwaltete Dienstidentität mit anderen Azure-Diensten verwenden:

* [Azure App Service](/azure/app-service/app-service-managed-service-identity)
* [Azure-Funktionen](/azure/app-service/app-service-managed-service-identity)
* [Azure Service Bus](../../service-bus-messaging/service-bus-managed-service-identity.md)
* [Azure Event Hubs](../../event-hubs/event-hubs-managed-service-identity.md)
* [Azure API Management](../../api-management/api-management-howto-use-managed-service-identity.md)

## Welche Azure-Dienste unterstützen das Feature?<a name="which-azure-services-support-managed-service-identity"></a>

Verwaltete Dienstidentitäten können für die Authentifizierung bei Diensten verwendet werden, die die Azure AD-Authentifizierung unterstützen. Eine Liste der Azure-Dienste, die die verwaltete Dienstidentität unterstützen, finden Sie unter [Dienste, die die verwaltete Dienstidentität unterstützen](services-support-msi.md).

## <a name="next-steps"></a>Nächste Schritte

Informationen zu den ersten Schritten mit der verwalteten Dienstidentität finden Sie in den folgenden Schnellstartanleitungen:

* [Verwenden einer verwalteten Dienstidentität eines virtuellen Windows-Computers für den Zugriff auf Resource Manager](tutorial-windows-vm-access-arm.md)
* [Zugreifen auf Azure Resource Manager mithilfe der verwalteten Dienstidentität eines virtuellen Linux-Computers](tutorial-linux-vm-access-arm.md)
