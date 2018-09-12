---
title: Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack| Microsoft-Dokumentation
description: Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: c50e4b5c9eb81c9386e2cb0db96a88de70dcb9e9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157802"
---
# <a name="workflow-common-parameters-for-azure-stack-validation-as-a-service"></a>Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Die allgemeinen Parameter umfassen Werte, wie z.B. Umgebungsvariablen und Benutzeranmeldeinformationen, die für sämtliche Tests in Validation-as-a-Service (VaaS) erforderlich sind. Sie definieren diese Werte auf Workflowebene. Sie speichern die Werte bei der Erstellung oder Änderung eines Workflows. Zur geplanten Zeit lädt der Workflow die Werte für den Test. 

## <a name="environment-parameters"></a>Umgebungsparameter

Umgebungsparameter beschreiben die Azure Stack-Umgebung, die getestet wird. Diese Werte müssen durch Generieren und Hochladen Ihrer Konfigurationsdatei für den Stempel `&lt;link&gt;. [How to get the stamp info link].` bereitgestellt werden

| Parametername | Erforderlich | Typ | BESCHREIBUNG |
|----------------------------------|----------|------|---------------------------------------------------------------------------------------------------------------------------------|
| Azure Stack-Build | Erforderlich |  | Buildnummer der Azure Stack-Bereitstellung (z.B. 1.0.170330.9) |
| OEM-Version | JA |  | Die Versionsnummer des OEM-Pakets, die während der Azure Stack-Bereitstellung verwendet wird. |
| OEM-Signatur | JA |  | Die Signatur des OEM-Pakets, die während der Azure Stack-Bereitstellung verwendet wird. |
| ID des AAD-Mandanten | Erforderlich |  | Die Mandanten-GUID für Azure Active Directory, die während der Azure Stack-Bereitstellung angegeben wird.|
| Region | Erforderlich |  | Die Region der Azure Stack-Bereitstellung. |
| Resource Manager-Endpunkt des Mandanten | Erforderlich |  | Endpunkt für Azure Resource Manager-Vorgänge des Mandanten (z.B. https://management.<ExternalFqdn>) |
| Resource Manager-Endpunkt des Administrators | JA |  | Endpunkt für Azure Resource Manager-Vorgänge des Mandanten (z.B. https://adminmanagement.<ExternalFqdn>) |
| Externer FQDN | JA |  | Externer vollqualifizierter Domänenname, der als Suffix für Endpunkte verwendet wird (Beispiel: local.azurestack.external oder redmond.contoso.com.) |
| Anzahl von Knoten | JA |  | Die Anzahl der Knoten für die Bereitstellung. |

## <a name="test-parameters"></a>Testparameter

Allgemeine Testparameter umfassen vertrauliche Informationen, die nicht in Konfigurationsdateien gespeichert werden können und manuell bereitgestellt werden müssen.

| Parametername | Erforderlich | Typ | BESCHREIBUNG |
|--------------------------------|------------------------------------------------------------------------------|------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mandantenbenutzername | Erforderlich |  | Der Azure Active Directory-Mandantenadministrator, der entweder bereits bereitgestellt wurde oder im AAD-Verzeichnis vom Dienstadministrator bereitgestellt werden muss. Weitere Einzelheiten zur Bereitstellung eines Mandantenkontos finden Sie unter [Erste Schritte mit Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). Dieser Wert wird von dem Test zur Durchführung von Vorgängen auf Mandantenebene verwendet, wie z.B. zum Bereitstellen von Vorlagen für die Bereitstellung von Ressourcen (virtuelle Computer, Speicherkonten usw.) und die Ausführung von Workloads. Dieser Wert wird von dem Test zur Durchführung von Vorgängen auf Mandantenebene verwendet, wie z.B. zum Bereitstellen von Vorlagen für die Bereitstellung von Ressourcen (virtuelle Computer, Speicherkonten usw.) und die Ausführung von Workloads. |
| Mandantenkennwort | Erforderlich |  | Das Kennwort für den Mandantenbenutzer. |
| Benutzername des Dienstadministrators | Erforderlich: Lösungs- und Paketvalidierung<br>Nicht erforderlich: Testdurchlauf |  | Der Azure Active Directory-Administrator des Mandanten des AAD-Verzeichnisses, der während der Azure Stack-Bereitstellung angegeben wird. |
| Kennwort des Dienstadministrators | Erforderlich: Lösungs- und Paketvalidierung<br>Nicht erforderlich: Testdurchlauf |  | Das Kennwort für den Dienstadministratorbenutzer. |
| Benutzername des Cloudadministrators | Erforderlich |  | Das Azure Stack-Domänenadministratorkonto (z.B. „contoso\cloudadmin“). Suchen Sie in der Konfigurationsdatei nach Benutzerrolle = „CloudAdmin“, und wählen Sie in der Konfigurationsdatei den Wert in dem Tag „UserName“ aus. |
| Kennwort des Cloudadministrators | Erforderlich |  | Das Kennwort für den Cloudadministratorbenutzer. |
| Diagnoseverbindungszeichenfolge | Erforderlich |  | Ein SAS-URI zu einem Azure Storage-Konto, in das während der Testausführung Diagnoseprotokolle kopiert werden. Anweisungen zum Generieren des SAS-URIs finden Sie unter [Einrichten eines BLOB-Speicherkontos](azure-stack-vaas-set-up-account.md). |


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Validation-as-a-Service in Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
