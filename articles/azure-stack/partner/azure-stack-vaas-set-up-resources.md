---
title: 'Tutorial: Einrichten von Ressourcen für Validation-as-a-Service | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie Ressourcen für Validation-as-a-Service (VaaS) einrichten.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 0830deaaa1161880706c39aee741af06a41b4b6f
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426343"
---
# <a name="tutorial-set-up-resources-for-validation-as-a-service"></a>Tutorial: Einrichten von Ressourcen für Validation-as-a-Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Sie müssen eine Lösung erstellen. Eine VaaS-Lösung (Validation-as-a-Service) stellt eine Azure Stack-Lösung mit einer bestimmten Hardwareliste dar. Mithilfe der Lösung überprüfen Sie, ob Ihre Hardware die Ausführung von Azure Stack unterstützt. Führen Sie die Schritte in diesem Tutorial aus, um die Verwendung des Diensts mit Ihrer Lösung vorzubereiten.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten der Azure Active Directory-Instanz (Azure AD), um die Verwendung von VaaS vorzubereiten
> * Erstellen Sie ein Speicherkonto.

## <a name="configure-an-azure-ad-tenant"></a>Konfigurieren eines Azure AD-Mandanten

Für die Authentifizierung und Registrierung bei VaaS ist ein Azure AD-Mandant erforderlich. Die Funktionen für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) des Mandanten werden vom Partner verwendet, um die zur Verwendung von VaaS berechtigten Benutzer in der Partnerorganisation zu verwalten.

Registrieren Sie das Azure AD-Mandantenverzeichnis der Organisation (und nicht das für Azure Stack verwendete Azure AD-Mandantenverzeichnis), und definieren Sie eine Richtlinie für die Verwaltung der darin enthaltenen Benutzerkonten. Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

### <a name="create-a-tenant"></a>Erstellen eines Mandanten

Erstellen Sie zur Verwendung mit VaaS einen speziellen Mandanten mit einem aussagekräftigen Namen wie `ContosoVaaS@onmicrosoft.com`.

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) einen Azure AD-Mandanten, oder verwenden Sie einen vorhandenen Mandanten. <!-- For instructions on creating new Azure AD tenants, see [Get started with Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). -->

2. Fügen Sie Mitglieder Ihrer Organisation zum Mandanten hinzu. Diese Benutzer sind für das Anzeigen oder Planen von Tests mithilfe des Diensts verantwortlich. Nach Abschluss der Registrierung legen Sie die Zugriffsebene der Benutzer fest.
 
    Weisen Sie den Benutzern in Ihrem Mandanten eine der folgenden Rollen zu, um die Ausführung von Aktionen in VaaS für sie zu autorisieren:

    | Rollenname | BESCHREIBUNG |
    |---------------------|------------------------------------------|
    | Owner (Besitzer) | Verfügt über Vollzugriff auf alle Ressourcen. |
    | Leser | Kann alle Ressourcen anzeigen, aber nicht erstellen oder verwalten. |
    | Test Contributor (Testmitwirkender) | Kann Testressourcen erstellen und verwalten. |

    So weisen Sie Rollen in der Anwendung **Azure Stack Validation Service** (Azure Stack-Validierungsdienst) zu:

    1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
    2. Wählen Sie im Abschnitt **Identität** die Optionen **Alle Dienste** > **Azure Active Directory** aus.
    3. Klicken Sie auf **Unternehmensanwendungen** > **Azure Stack Validation Service** (Azure Stack-Validierungsdienst).
    4. Wählen Sie **Benutzer und Gruppen**. Auf dem Blatt **Azure Stack Validation Service– Users and group** (Azure Stack-Validierungsdienst – Benutzer und Gruppe) sind die zur Verwendung der Anwendung berechtigten Benutzer aufgeführt.
    5. Wählen Sie **+ Benutzer hinzufügen**, um einen Benutzer aus Ihrem Mandanten hinzuzufügen und eine Rolle zuzuweisen.
   
    Wenn Sie VaaS-Ressourcen und Aktionen in verschiedenen Gruppen innerhalb einer Organisation isolieren möchten, können Sie mehrere Azure AD-Mandantenverzeichnisse erstellen.

### <a name="register-your-tenant"></a>Registrieren Ihres Mandanten

Dieser Prozess autorisiert Ihren Mandanten in der Azure AD-Anwendung **Azure Stack Validation Service** (Azure Stack-Validierungsdienst).

1. Senden Sie die folgenden Mandanteninformationen unter [vaashelp@microsoft.com](mailto:vaashelp@microsoft.com) an Microsoft.

    | Daten | BESCHREIBUNG |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Name der Organisation | Offizieller Organisationsname |
    | Name des Azure AD-Mandantenverzeichnisses | Registrierter Name des Azure AD-Mandantenverzeichnisses |
    | ID des Azure AD-Mandantenverzeichnisses | Dem Verzeichnis zugeordnete GUID des Azure AD-Mandantenverzeichnisses. Informationen dazu, wie Sie die ID Ihres Azure AD-Mandantenverzeichnisses ermitteln, finden Sie unter [Abrufen der Mandanten-ID](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). |

2. Warten Sie auf die Bestätigung des Teams für die Azure Stack-Überprüfung, um sicherzugehen, dass Ihr Mandant das VaaS-Portal verwenden kann.

### <a name="consent-to-the-vaas-application"></a>Einwilligung zur VaaS-Anwendung

Gewähren Sie als Azure AD-Administrator im Namen Ihres Mandanten der VaaS-Azure AD-Anwendung die erforderlichen Berechtigungen:

1. Melden Sie sich mit den Anmeldeinformationen eines globalen Administrators für den Mandanten beim [VaaS-Portal](https://azurestackvalidation.com/) an. 

2. Klicken Sie auf **Mein Konto**.

3. Akzeptieren Sie bei entsprechender Aufforderung die Bedingungen, um VaaS die aufgeführten Azure AD-Berechtigungen zu erteilen.

## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Während der Testausführung gibt VaaS Diagnoseprotokolle an ein Azure Storage-Konto aus. Das Speicherkonto kann nicht nur für Testprotokolle, sondern auch zum Hochladen der OEM-Erweiterungspakete für den Workflow für die Paketvalidierung verwendet werden.

Das Azure Storage-Konto wird in der öffentlichen Azure-Cloud und nicht in Ihrer Azure Stack-Umgebung gehostet.

1. Wählen Sie im Azure-Portal die Optionen **Alle Dienste** > **Storage** > **Speicherkonten** aus. Wählen Sie auf dem Blatt **Speicherkonten** die Option **Hinzufügen** aus.

2. Wählen Sie das Abonnement aus, in dem das Speicherkonto erstellt werden soll.

3. Wählen Sie unter **Ressourcengruppe** die Option **Neu erstellen** aus. Geben Sie einen Namen für die neue Ressourcengruppe ein.

4. Geben Sie einen Namen für Ihr Speicherkonto ein. Der gewählte Name muss folgende Eigenschaften aufweisen:
    - Er muss in Azure eindeutig sein.
    - Er muss zwischen 3 und 24 Zeichen lang sein.
    - Er darf nur Ziffern und Kleinbuchstaben enthalten.

5. Wählen Sie die Region **USA, Westen** für Ihr Speicherkonto aus.

    Damit für die Speicherung von Protokollen keine Netzwerkgebühren anfallen, kann das Azure Storage-Konto für die ausschließliche Nutzung der Region **USA, Westen** konfiguriert werden. Die Features für die Datenreplikation und die heiße Speicherebene sind für diese Daten nicht erforderlich. Wenn Sie eines dieser Features aktivieren, erhöhen sich die Kosten erheblich.

6. Übernehmen Sie mit Ausnahme von **Kontoart** für diese Einstellungen die Standardwerte:

    - Das Feld **Bereitstellungsmodell** ist standardmäßig auf **Resource Manager** festgelegt.
    - Das Feld **Leistung** ist standardmäßig auf **Standard** festgelegt.
    - Wählen Sie für **Kontoart** die Option **Blob Storage**.
    - Das Feld **Replikation** ist standardmäßig auf **Lokal redundanter Speicher (LRS)** festgelegt.
    - Das Feld **Zugriffsebene** ist standardmäßig auf **Heiß** festgelegt.

7. Klicken Sie auf **Überprüfen + erstellen**, um die Speicherkontoeinstellungen zu überprüfen und das Konto zu erstellen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Ihre Umgebung keine eingehenden Verbindungen zulässt, führen Sie die Schritte im Tutorial zum Bereitstellen des lokalen Agents zum Ausführen eines Tests für Ihre Hardware aus.

> [!div class="nextstepaction"]
> [Deploy the local agent](azure-stack-vaas-local-agent.md) (Bereitstellen des lokalen Agents)
