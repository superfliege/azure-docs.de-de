---
title: Was ist der Azure AD Connect-Administrator-Agent – Azure AD Connect | Microsoft-Dokumentation
description: Beschreibt die Tools, die zum Synchronisieren und Überwachen Ihrer lokalen Umgebung mit Azure AD verwendet werden.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/03/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e33143626e136523b4af086e841b92e9ad30fa86
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/15/2019
ms.locfileid: "59577608"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>Was ist der Azure AD Connect-Administrator-Agent? 
Der Azure AD Connect-Administrator-Agent ist eine neue Komponente von Azure Active Directory Connect, die auf einem Azure Active Directory Connect-Server installiert ist. Er wird zum Sammeln spezifischer Daten aus der Active Directory-Umgebung verwendet, die einem Microsoft-Supporttechniker bei der Problembehandlung helfen, wenn Sie eine Supportanfrage senden.

Nach der Installation wartet der Azure AD Connect-Administrator-Agent auf bestimmte angeforderte Daten aus Azure Active Directory, ruft diese aus der Synchronisierungsumgebung ab und sendet sie an Azure Active Directory, wo sie dem Microsoft-Supporttechniker zur Verfügung gestellt werden.

Die Informationen, die der Azure AD Connect-Administrator-Agent aus der Umgebung abruft, werden in keiner Weise gespeichert. Sie werden lediglich dem Microsoft-Supporttechniker angezeigt, damit dieser die von Ihnen gesendete Supportanfrage zu Azure Active Directory Connect untersuchen und mögliche Probleme behandeln kann.

## <a name="how-is-the-azure-ad-connect-admin-agent-installed-on-the-azure-ad-connect-server"></a>Wie wird der Azure AD Connect-Administrator-Agent auf dem Azure AD Connect-Server installiert? 
Nach der Installation des Administrator-Agents werden die folgenden beiden neuen Programme auf Ihrem Server in der Systemsteuerung in der Liste „Programme hinzufügen/entfernen“ angezeigt: 

![Administrator-Agent](media/whatis-aadc-admin-agent/adminagent1.png)

Entfernen oder deinstallieren Sie die Programme nicht, da sie ein wichtiger Bestandteil der Azure AD Connect-Installation sind.

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>Welche Daten in meinem Synchronisierungsdienst werden dem Microsoft-Supporttechniker angezeigt?
Wenn Sie eine Supportanfrage senden, kann der Microsoft-Supporttechniker für einen bestimmten Benutzer die relevanten Daten in Active Directory, den Azure Active Directory-Connectorbereich im Azure Active Directory Connect-Server und die Metaversedaten im Azure Active Directory Connect-Server sehen.

Der Microsoft-Supporttechniker kann in Ihrem System weder Daten ändern, noch Kennwörter sehen.

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>Was geschieht, wenn ich nicht möchte, dass der Microsoft-Supporttechniker auf meine Daten zugreift? 
 
Wenn der Microsoft-Supporttechniker bei einer Supportanfrage nicht auf Ihre Daten zugreifen soll, können Sie die Dienstkonfigurationsdatei wie nachstehend beschrieben ändern und so den Zugriff deaktivieren: 

1.  Öffnen Sie im Editor **C:\Programme\Microsoft Azure AD Connect Administration Agent\AzureADConnectAdministrationAgentService.exe.config**.
2.  Deaktivieren Sie die Einstellung **UserDataEnabled**, wie unten dargestellt. Wenn die Einstellung **UserDataEnabled** vorhanden und auf „true“ festgelegt ist, setzen Sie diese auf „false“. Wenn die Einstellung nicht vorhanden ist, fügen Sie diese, wie unten gezeigt, hinzu.    

    ```xml
    <appSettings>
      <add key="TraceFilename" value="ADAdministrationAgent.log" />
      <add key="UserDataEnabled" value="false" />
    </appSettings>
    ```

3.  Speichern Sie die Konfigurationsdatei.
4.  Starten Sie den Dienst des Azure AD Connect-Administrator-Agents neu, wie unten dargestellt.

![Administrator-Agent](media/whatis-aadc-admin-agent/adminagent2.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).