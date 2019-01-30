---
title: Cookieeinstellungen für den Anwendungsproxy – Azure Active Directory | Microsoft-Dokumentation
description: Azure Active Directory (Azure AD) umfasst Zugriffs- und Sitzungscookies für den Zugriff auf lokale Anwendungen über den Anwendungsproxy. In diesem Artikel erfahren Sie, wie Sie die Cookieeinstellungen verwenden und konfigurieren.
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: concept
ms.date: 01/16/2019
ms.author: barbkess
ms.reviewer: japere
ms.openlocfilehash: 9c5979357532bb29f8e3545db57aa32603763dc1
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54855600"
---
# <a name="cookie-settings-for-accessing-on-premises-applications-in-azure-active-directory"></a>Cookieeinstellungen für den Zugriff auf lokale Anwendungen in Azure Active Directory

Azure Active Directory (Azure AD) umfasst Zugriffs- und Sitzungscookies für den Zugriff auf lokale Anwendungen über den Anwendungsproxy. Finden Sie heraus, wie die Cookieeinstellungen für den Anwendungsproxy verwendet werden. 

## <a name="what-are-the-cookie-settings"></a>Wie lauten die Cookieeinstellungen?

Der [Anwendungsproxy](application-proxy.md) verwendet die folgenden Einstellungen für Zugriffs- und Sitzungscookies.

| Cookieeinstellung | Standard | Beschreibung | Empfehlungen |
| -------------- | ------- | ----------- | --------------- |
| Nur-HTTP-Cookie verwenden | **Nein** | Bei Festlegung von **Ja** kann der Anwendungsproxy das HTTPOnly-Flag in HTTP-Antwortheadern einschließen. Dieses Flag bietet zusätzliche Sicherheitsvorteile, z.B. wird das Kopieren oder Ändern von Cookies über ein clientseitiges Scripting (CSS) verhindert.<br></br><br></br>Vor der Unterstützung der HTTP-Only-Einstellung verschlüsselte der Anwendungsproxy Cookies und übermittelte sie über einen sicheren SSL-Kanal, um Schutz vor Änderungen bereitzustellen. | Verwenden Sie **Ja**, um von den zusätzlichen Sicherheitsvorteilen zu profitieren.<br></br><br></br>Verwenden Sie **Nein** für Clients oder Benutzer-Agents, die keinen Zugriff auf das Sitzungscookie benötigen. Verwenden Sie beispielsweise **Nein** für einen RDP- oder MTSC-Client, der über den Anwendungsproxy eine Verbindung mit einem Remotedesktop-Gatewayserver herstellt.|
| Sicheres Cookie verwenden | **Nein** | Bei Festlegung von **Ja** kann der Anwendungsproxy das Flag „Secure“ in HTTP-Antwortheadern einschließen. Sichere Cookies erhöhen die Sicherheit, weil sie über einen TLS-gesicherten Kanal wie z.B. HTTPS übertragen werden. Dadurch wird verhindert, dass Cookies aufgrund der Übertragung in Klartext durch Unbefugte eingesehen werden können. | Verwenden Sie **Ja**, um von den zusätzlichen Sicherheitsvorteilen zu profitieren.|
| Beständiges Cookie verwenden | **Nein** | Bei Festlegung auf **Ja** kann der Anwendungsproxy Zugriffscookies so festlegen, dass sie beim Schließen des Webbrowsers nicht ablaufen. Cookies bleiben erhalten, bis das Zugriffstoken abläuft oder der Benutzer die persistenten Cookies manuell löscht. | Verwenden Sie **Nein** wegen des Sicherheitsrisikos, das mit der Beibehaltung der Authentifizierung von Benutzern verbunden ist.<br></br><br></br>Wir empfehlen, **Ja** nur für ältere Anwendungen zu verwenden, die Cookies nicht zwischen Prozessen gemeinsam verwenden können. Es ist besser, Ihre Anwendung zur gemeinsamen Nutzung von Cookies zwischen Prozessen zu aktualisieren, als beständige Cookies zu verwenden. Angenommen, Sie benötigen beständige Cookies, damit ein Benutzer von einer SharePoint-Website aus Office-Dokumente in der Explorer-Ansicht öffnen kann. Ohne beständige Cookies kommt es bei diesem Vorgang möglicherweise zu Fehlern, wenn die Zugriffscookies nicht vom Browser, dem Explorer-Prozess und dem Office-Prozess gemeinsam verwendet werden. |

## <a name="set-the-cookie-settings---azure-portal"></a>Festlegen der Cookieeinstellungen – Azure-Portal
So legen Sie die Cookieeinstellungen im Azure-Portal fest

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Navigieren Sie zu **Azure Active Directory** > **Unternehmensanwendungen** > **Alle Anwendungen**.
3. Wählen Sie die Anwendung aus, für die Sie eine Cookieeinstellung aktivieren möchten.
4. Klicken Sie auf **Anwendungsproxy**.
5. Legen Sie unter **Zusätzliche Einstellungen** die Cookieeinstellung auf **Ja** oder **Nein** fest.
6. Klicken Sie zum Übernehmen der Änderungen auf **Speichern**. 

<!---

## View current cookie settings - PowerShell

To see the current cookie settings for the application, use this PowerShell command:  

```PowerShell
Get-AzureADApplicationProxyApplication -ObjectId <ObjectId> | fl * 
```

## Set cookie settings - PowerShell

In the following PowerShell commands, ```<ObjectId>``` is the ObjectId of the application. 

**Http-Only Cookie** 

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsHttpOnlyCookieEnabled $false 
```

**Secure Cookie**

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsSecureCookieEnabled $false 
```

**Persistent Cookies**

```PowerShell
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $true 
Set-AzureADApplicationProxyApplication -ObjectId <ObjectId> -IsPersistentCookieEnabled $false 
```

-->
