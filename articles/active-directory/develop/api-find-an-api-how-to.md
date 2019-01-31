---
title: Suchen einer bestimmten API, die für eine benutzerdefinierte Anwendung benötigt wird | Microsoft-Dokumentation
description: Konfigurieren der Berechtigungen, die für den Zugriff auf eine bestimmte API in einer benutzerdefinierten Azure AD-Anwendung benötigt werden
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 8ca892341f064a0b2289e6415658c5d4e2d51ddc
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2019
ms.locfileid: "55157575"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Suchen einer bestimmten API, die für eine benutzerdefinierte Anwendung benötigt wird

Für den Zugriff auf APIs müssen Zugriffsbereiche und Rollen konfiguriert werden. Wenn Sie die Web-APIs einer Ressourcenanwendung für Clientanwendungen verfügbar machen möchten, müssen Sie Zugriffsbereiche und Rollen für die API konfigurieren. Wenn eine Clientanwendung auf eine Web-API zugreifen soll, müssen Sie Zugriffsberechtigungen für die API in der App-Registrierung konfigurieren.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Konfigurieren einer Ressourcenanwendung zum Bereitstellen von Web-APIs

Wenn Sie Ihre Web-API verfügbar machen, wird die API in der Liste **Hiermit wählen Sie eine API aus.** angezeigt, wenn Berechtigungen zu einer App-Registrierung hinzugefügt werden. Führen Sie zum Hinzufügen von Zugriffsbereichen die Schritte unter [Hinzufügen von Zugriffsbereichen zu Ihrer Ressourcenanwendung](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application) aus.

## <a name="configuring-a-client-application-to-access-web-apis"></a>Konfigurieren einer Clientanwendung für den Zugriff auf Web-APIs

Wenn Sie Ihrer App-Registrierung Berechtigungen hinzufügen, können Sie für verfügbar gemachte Web-APIs **API-Zugriff hinzufügen**. Führen Sie zum Zugriff auf Web-APIs die Schritte unter [So fügen Sie Anmeldeinformationen oder Zugriffsberechtigungen für Web-APIs hinzu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) aus.

## <a name="next-steps"></a>Nächste Schritte

-   [Integrieren von Anwendungen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Grundlegendes zum Azure Active Directory-Anwendungsmanifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


