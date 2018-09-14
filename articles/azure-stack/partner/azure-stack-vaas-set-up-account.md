---
title: Einrichten Ihres Azure Stack-Validation-as-a-Service-Kontos | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihr Validation-as-a-Service-Konto einrichten.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: b94d37d528739247cb4f94a17dadf3876c4db6bd
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158507"
---
# <a name="set-up-your-validation-as-a-service-account"></a>Einrichten Ihres Validation-as-a-Service-Kontos

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Validation-as-a-Service (VaaS) ist ein Azure-Dienst, der Microsoft Azure Stack-Partnern zur Verfügung steht, die über eine Co-Engineering-Vereinbarung mit Microsoft für das Entwerfen, Entwickeln, Validieren, Verkaufen, Bereitstellen und Unterstützen von Azure Stack-Lösungen auf dem Markt verfügen.

Erfahren Sie, wie Sie Ihr System für Validation-as-a-Service vorbereiten. Richten Sie die Azure Active Directory-Instanz ein, und führen Sie weitere erforderliche Aufgaben zur Vorbereitung auf VaaS durch. 

Zu diesen Aufgaben zählen:

- Erstellen eines Azure Storage-Blobs zum Speichern von Protokollen
- Bereitstellen des lokalen Agents
- Herunterladen von VMs mit dem Testimage auf die zu testende Azure Stack-Instanz

## <a name="create-an-azure-active-directory-tenant-id"></a>Erstellen einer Azure Active Directory-Mandanten-ID

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) einen Azure Active Directory-Mandanten, oder verwenden Sie einen vorhandenen Mandanten.

    Es wird empfohlen, zur Verwendung mit VaaS einen speziellen Mandanten mit einem aussagekräftigen Namen wie „ContosoVaaS@onmicrosoft.com“ zu erstellen. Die Funktionen für die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) des Mandanten werden vom Partner verwendet, um die zur Verwendung von VaaS berechtigten Benutzer in der Partnerorganisation zu verwalten.  
    
    Weitere Informationen finden Sie unter [Verwalten Ihres Azure AD-Verzeichnisses](https://docs.microsoft.com/azure/active-directory/active-directory-administer).

    > [!Note]  
    > Weitere Informationen zum Erstellen neuer Azure Active Directory-Mandanten finden Sie unter [Get started with Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad) (Erste Schritte mit Azure AD).

2. Fügen Sie Mitglieder Ihrer Organisation hinzu, die für die Verwendung des Diensts für den Mandanten verantwortlich sein sollen. Weisen Sie jedem Benutzer im Mandanten eine der folgenden Rollen zu, um die Zugriffsebene für VaaS zu steuern:

    | Rollenname | BESCHREIBUNG |
    |---------------------|------------------------------------------|
    | Owner (Besitzer) | Verfügt über Vollzugriff auf alle Ressourcen. |
    | Leser | Kann alle Ressourcen anzeigen, aber nicht bearbeiten. |
    | Test Contributor (Testmitwirkender) | Kann Testressourcen verwalten. |
    | Catalog Contributor (Katalogmitwirkender) | Kann Ressourcen für die Lösungsveröffentlichung verwalten. |

## <a name="set-up-your-tenant"></a>Einrichten Ihres Mandanten

Richten Sie Ihren Mandanten in der Anwendung **Azure Stack Validation Service** (Azure Stack-Validierungsdienst) ein. 

1. Senden Sie die folgenden Informationen zum Mandanten unter vaashelp@microsoft.com an Microsoft.

    | Daten | BESCHREIBUNG |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | Name der Organisation | Offizieller Organisationsname. |
    | Name des Azure AD-Mandantenverzeichnisses | Registrierter Name des Azure AD-Mandantenverzeichnisses. |
    | ID des Azure AD-Mandantenverzeichnisses | Dem Verzeichnis zugeordnete GUID des Azure AD-Mandantenverzeichnisses.<br> Informationen dazu, wie Sie die ID Ihres Azure AD-Mandantenverzeichnisses ermitteln, finden Sie unter [Abrufen der Mandanten-ID](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id). |

    

2. Sie erhalten vom Azure Stack-Team eine Bestätigung, dass Ihr Mandant das VaaS-Portal verwenden kann.

3. Melden Sie sich mit den Anmeldeinformationen eines globalen Administrators für den Mandanten beim [VaaS-Portal](https://azurestackvalidation.com/
) an. Klicken Sie auf **Mein Konto**.

    ![Anmelden beim VaaS-Portal](media/vaas_portalsignin.png)

3. Sie werden von der Website aufgefordert, Zugriff für VaaS zu gewähren. Akzeptieren Sie die Bedingungen, um fortzufahren.

## <a name="assign-user-roles"></a>Zuweisen von Benutzerrollen

So weisen Sie eine Benutzerrolle zu:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie in der Gruppe **Identität** auf **Alle Dienste** > **Azure Active Directory**.
3. Klicken Sie auf **Unternehmensanwendungen** > **Azure Stack Validation Service** (Azure Stack-Validierungsdienst).
4. Wählen Sie **Benutzer und Gruppen**. Auf dem Blatt **Azure Stack Validation Service– Users and group** (Azure Stack-Validierungsdienst – Benutzer und Gruppe) sind die zur Verwendung der Anwendung berechtigten Benutzer aufgeführt.
5. Klicken Sie auf **+ Benutzer hinzufügen**, um eine Zuweisung hinzuzufügen.

## <a name="create-an-azure-storage-blob-to-store-logs"></a>Erstellen eines Azure Storage-Blobs zum Speichern von Protokollen

VaaS erstellt bei der Ausführung der Validierungstests Diagnoseprotokolle. Sie benötigen einen Speicherort bzw. eine Azure Blob-Dienst-SAS-URL zum Speichern Ihrer Protokolle. Das Speicherkonto kann auch zum Speichern der OEM-Anpassungspakete verwendet werden.

In den folgenden Schritten werden das Einrichten und Generieren eines SAS-URI (Storage-as-a-Service) für ein Azure-Speicherkonto erläutert, und Sie erfahren, wo Sie das Speicherkonto beim Starten der Tests im VaaS-Portal angeben.

### <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

1. Befolgen Sie zum Erstellen eines Speicherkontos die Anweisungen unter [Speicherkonto erstellen](../../storage/common/storage-quickstart-create-account.md).

2. Wählen Sie bei der Auswahl des Speicherkontotyps den Kontotyp **Blob-Speicher** aus.

### <a name="generate-a-sas-url-for-the-storage-account"></a>Generieren einer SAS-URL für das Speicherkonto

1. Navigieren Sie zu dem Speicherkonto, das Sie in den obigen Schritten erstellt haben.

2. Wählen Sie auf dem Blatt unter **Einstellungen** die Option **Shared Access Signature** aus.

3. Aktivieren Sie unter **Zulässige Dienste** nur die Option **Blob** (deaktivieren Sie alle anderen Optionen).

4. Aktivieren Sie unter **Zugelassene Ressourcentypen** die Optionen **Dienst**, „**Container“ und **Objekt**.

5. Aktivieren Sie unter **Zugelassene Berechtigungen** die Optionen **Lesen**, **Schreiben**, **Auflisten**, **Hinzufügen** und **Erstellen** (deaktivieren Sie alle anderen Optionen).

6. Legen Sie die **Startzeit** auf die aktuelle Zeit und die **Endzeit** auf drei Monate nach der aktuellen Zeit fest.

7. Aktivieren Sie **Generate SAS and connection string** (SAS und Verbindungszeichenfolge generieren), und speichern Sie die für **SAS-URL für Blob-Dienst** angezeigte Zeichenfolge.

> [!Note]  
> Die SAS-URL läuft zu der beim Generieren der URL festgelegten Endzeit ab. Stellen Sie sicher, dass die URL für einen ausreichenden Zeitraum gültig ist, bevor Sie sie zum Debuggen an das Produktteam weitergeben. Wenn Sie Tests planen, sollte die URL für mehr als 30 Tage gültig sein.

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie den lokalen VaaS-Agent, um Ihre Hardware zu überprüfen. Anweisungen hierzu finden Sie unter [Bereitstellen des lokalen Agents und Testen von VMs](azure-stack-vaas-test-vm.md).
- Erfahren Sie mehr zu [Validation-as-a-Service in Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).