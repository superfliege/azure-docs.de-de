---
title: Behandeln von Problemen mit sicherem LDAP (LDAPS) in Azure AD Domain Services | Microsoft-Dokumentation
description: Behandeln von Problemen mit sicherem LDAP (LDAPS) für eine über Azure AD Domain Services verwaltete Domäne
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: maheshu
ms.openlocfilehash: 9713a06bbf6a61b316e061cb851721a3554cd632
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503671"
---
# <a name="troubleshoot-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Behandeln von Problemen mit sicherem LDAP (LDAPS) für eine über Azure AD Domain Services verwaltete Domäne

## <a name="connection-issues"></a>Verbindungsprobleme
Wenn Sie Probleme beim Herstellen einer Verbindung mit der verwalteten Domäne über sicheres LDAP haben:

* Die Ausstellerkette des Zertifikats für sicheres LDAP muss auf dem Client als vertrauenswürdig eingestuft werden. Sie könnten die Stammzertifizierungsstelle dem vertrauenswürdigen Stammzertifikatspeicher auf dem Client hinzufügen, um die Vertrauensstellung einzurichten.
* Stellen Sie sicher, dass der LDAP-Client (z.B. „ldp.exe“) eine Verbindung mit dem sicheren LDAP-Endpunkt mit einem DNS-Namen herstellt, nicht mit der IP-Adresse.
* Überprüfen Sie den DNS-Namen, mit dem der LDAP-Client eine Verbindung herstellt. Er muss in die öffentliche IP-Adresse für sicheres LDAP in der verwalteten Domäne aufgelöst werden.
* Stellen Sie sicher, dass das sichere LDAP-Zertifikat für die verwaltete Domäne den DNS-Namen im Attribut für den Antragsteller oder den alternativen Antragstellernamen aufweist.
* Die NSG-Einstellungen für das virtuelle Netzwerk müssen Datenverkehr über Port 636 aus dem Internet zulassen. Dieser Schritt gilt nur, wenn Sie den sicheren LDAP-Zugriff über das Internet aktiviert haben.


## <a name="need-help"></a>Sie brauchen Hilfe?
Wenn Sie weiterhin Probleme beim Herstellen einer Verbindung mit der verwalteten Domäne über sicheres LDAP haben, [bitten Sie das Produktteam](active-directory-ds-contact-us.md) um Hilfe. Nehmen Sie dabei die folgende Informationen auf, damit das Problem besser diagnostiziert werden kann:
* Einen Screenshot von „ldp.exe“ beim Herstellen einer Verbindung, die fehlschlägt.
* Die ID des Azure AD-Mandanten und den DNS-Domänennamen Ihrer verwalteten Domäne.
* Den genauen Benutzernamen, den Sie verwenden möchten.


## <a name="related-content"></a>Verwandte Inhalte
* [Erste Schritte mit Azure AD Domain Services](active-directory-ds-getting-started.md)
* [Verwalten einer durch Azure AD Domain Services verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)
* [LDAP-Abfragegrundlagen](https://technet.microsoft.com/library/aa996205.aspx)
* [Verwalten von Gruppenrichtlinien in einer durch Azure Active Directory Domain Services verwalteten Domäne](active-directory-ds-admin-guide-administer-group-policy.md)
* [Netzwerksicherheitsgruppen](../virtual-network/security-overview.md)
* [Erstellen einer Netzwerksicherheitsgruppe](../virtual-network/tutorial-filter-network-traffic.md)
