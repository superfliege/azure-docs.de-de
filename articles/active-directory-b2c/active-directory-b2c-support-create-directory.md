---
title: Behandeln von Problemen bei der Mandantenerstellung in Azure Active Directory B2C | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu Problemen beim Erstellen eines Azure Active Directory-Mandanten oder eines Azure Active Directory B2C-Mandanten sowie entsprechende Lösungen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 009f7ac2f7e614b7e07623e41888973f1a2b254d
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440979"
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Behandeln von Problemen beim Erstellen eines Azure Active Directory-Mandanten oder eines Azure Active Directory B2C-Mandanten 

## <a name="create-an-azure-ad-tenant"></a>Erstellen eines Azure AD-Mandanten
Falls die Erstellung eines Azure AD-Mandanten (Azure Active Directory) beim ersten Versuch nicht gelingt, versuchen Sie es erneut. Sollte das Problem weiterhin bestehen, wenden Sie sich an den Azure-Support.

## <a name="create-an-azure-ad-b2c-tenant"></a>Erstellen eines Azure AD B2C-Mandanten
Sollte beim [Erstellen eines Azure AD B2C-Mandanten (Azure Active Directory B2C)](active-directory-b2c-get-started.md) ein Problem auftreten, versuchen Sie Folgendes:

* Falls der Azure AD B2C-Mandant nicht in Ihrer Mandantenliste angezeigt wird, wiederholen Sie die Mandantenerstellung.
* Falls der Azure AD B2C-Mandant nicht in Ihrer Mandantenliste angezeigt wird und eine Fehlermeldung wie die folgende erscheint, löschen Sie den Mandanten, und erstellen Sie ihn erneut:

    „Die Erstellung des B2C-Mandanten "contosob2c" konnte nicht abgeschlossen werden. Weitere Informationen finden Sie unter [diesem Link](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409).“
* Es ist bekannt, dass es zu Problemen kommt, wenn Sie einen vorhandenen Azure AD B2C-Mandanten löschen und erneut mit dem gleichen Domänennamen erstellen. Bei der Erstellung eines neuen Azure AD B2C-Mandanten muss deshalb ein anderer Domänenname verwendet werden.
* Sollten diese Lösungen das Problem nicht beheben, wenden Sie sich an den Azure-Support. Weitere Informationen finden Sie unter [Senden von Supportanfragen für Azure AD B2C](active-directory-b2c-support.md).

