---
title: "Azure Active Directory Domain Services: Aktivieren der eingeschränkten Kerberos-Delegierung | Microsoft-Dokumentation"
description: "Aktivieren der eingeschränkten Kerberos-Delegierung in verwalteten Azure Active Directory Domain Services-Domänen"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: maheshu
ms.openlocfilehash: 0235944ef89cab7af152664651711edd5e80e632
ms.sourcegitcommit: 3ee36b8a4115fce8b79dd912486adb7610866a7c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="configure-kerberos-constrained-delegation-kcd-on-a-managed-domain"></a>Konfigurieren der eingeschränkten Kerberos-Delegierung (KCD) in einer verwalteten Domäne
Viele Anwendungen müssen im Kontext des Benutzers auf Ressourcen zugreifen. Active Directory unterstützt einen Mechanismus, die so genannte Kerberos-Delegierung, die diesen Anwendungsfall ermöglicht. Darüber hinaus können Sie die Delegierung einschränken, sodass im Kontext des Benutzers nur auf bestimmte Ressourcen zugegriffen werden kann. In Azure AD Domain Services verwaltete Domänen unterscheiden sich insofern von herkömmlichen Active Directory-Domänen, als dass sie sicherer gesperrt sind.

Dieser Artikel veranschaulicht die Konfiguration der eingeschränkten Kerberos-Delegierung in einer verwalteten Azure AD Domain Services-Domäne.

## <a name="kerberos-constrained-delegation-kcd"></a>Eingeschränkte Kerberos-Delegierung (KCD)
Die Kerberos-Delegierung ermöglicht einem Konto den Identitätswechsel zu einem anderen Sicherheitsprinzipal (z.B. einem Benutzer) für den Zugriff auf Ressourcen. Stellen Sie sich eine Webanwendung vor, die im Kontext eines Benutzers auf eine Back-End-Web-API zugreift. In diesem Beispiel nimmt die Webanwendung (die im Kontext eines Dienstkontos oder eines Computerkontos ausgeführt wird) beim Zugriff auf die Ressource (Back-End-Web-API) die Identität des Benutzers an. Die Kerberos-Delegierung ist unsicher, da sie nicht die Ressourcen einschränkt, auf die das Konto mit dem Identitätswechsel im Kontext des Benutzers zugreifen kann.

Die eingeschränkte Kerberos-Delegierung (KCD) schränkt die Dienste/Ressourcen ein, an denen der angegebene Server im Namen eines Benutzers Aktionen durchführen kann. Die herkömmliche KCD erfordert Domänenadministratorrechte zum Konfigurieren eines Domänenkontos für einen Dienst und beschränkt das Konto auf eine einzelne Domäne.

Mit der herkömmlichen KCD sind auch einige Probleme verbunden. Wenn der Domänenadministrator kontobasierte KCD für den Dienst konfiguriert hat, hatte der Dienstadministrator in früheren Betriebssystemversionen keine praktische Möglichkeit herauszufinden, welche Front-End-Dienste an die Ressourcendienste delegierten, die sich in ihrem Besitz befanden. Jeder Front-End-Dienst, der an einen Ressourcendienst delegieren konnte, bot einen potenziellen Angriffspunkt. Wenn ein Server kompromittiert wurde, der einen Front-End-Dienst hostete, und wenn dieser Server für die Delegierung an Ressourcendienste konfiguriert war, wurden möglicherweise auch die Ressourcendienste kompromittiert.

> [!NOTE]
> In einer verwalteten Azure AD Domain Services-Domäne besitzen Sie keine Domänenadministratorberechtigungen. Aus diesem Grund **kann die herkömmliche KCD nicht in einer verwalteten Domäne konfiguriert werden**. Verwenden Sie die ressourcenbasierte KCD entsprechend der Beschreibung in diesem Artikel. Dieser Mechanismus ist zudem sicherer.
>
>

## <a name="resource-based-kerberos-constrained-delegation"></a>Ressourcenbasierte eingeschränkte Kerberos-Delegierung
Seit Windows Server 2012 haben Dienstadministratoren die Möglichkeit, eingeschränkte Delegierung für den Dienst zu konfigurieren. In diesem Modell kann der Back-End-Dienstadministrator bestimmten Front-End-Diensten die Verwendung von KCD erlauben oder verweigern. Dieses Modell wird als **ressourcenbasierte eingeschränkte Kerberos-Delegierung** bezeichnet.

Die ressourcenbasierte KCD wird mithilfe von PowerShell konfiguriert. Sie verwenden die Cmdlets „Set-ADComputer“ oder „Set-ADUser-Cmdlets“, je nachdem, ob es sich beim Konto für den Identitätswechsel um ein Computerkonto oder ein Benutzerkonto/Dienstkonto handelt.

### <a name="configure-resource-based-kcd-for-a-computer-account-on-a-managed-domain"></a>Konfigurieren der ressourcenbasierten KCD für ein Computerkonto in einer verwalteten Domäne
Angenommen, Sie haben eine Web-App, die auf dem Computer „contoso100-Webapp.contoso100.com“ ausgeführt wird. Sie muss im Kontext von Domänenbenutzern auf die Ressource zugreifen (eine Web-API, die auf „contoso100-api.contoso100.com“ ausgeführt wird). Hier wird gezeigt, wie Sie die ressourcenbasierte KCD für dieses Szenario einrichten würden.

```powershell
$ImpersonatingAccount = Get-ADComputer -Identity contoso100-webapp.contoso100.com
Set-ADComputer contoso100-api.contoso100.com -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

### <a name="configure-resource-based-kcd-for-a-user-account-on-a-managed-domain"></a>Konfigurieren der ressourcenbasierten KCD für ein Benutzerkonto in einer verwalteten Domäne
Angenommen, Sie haben eine Web-App, die als Dienstkonto „appsvc“ ausgeführt wird, und diese muss im Kontext von Domänenbenutzern auf die Ressource zugreifen (eine Web-API, die als Dienstkonto „backendsvc“ ausgeführt wird). Hier wird gezeigt, wie Sie die ressourcenbasierte KCD für dieses Szenario einrichten würden.

```powershell
$ImpersonatingAccount = Get-ADUser -Identity appsvc
Set-ADUser backendsvc -PrincipalsAllowedToDelegateToAccount $ImpersonatingAccount
```

## <a name="related-content"></a>Verwandte Inhalte
* [Azure AD-Domänendienste – Leitfaden zu den ersten Schritten](active-directory-ds-getting-started.md)
* [Übersicht über die eingeschränkte Kerberos-Delegierung](https://technet.microsoft.com/library/jj553400.aspx)
