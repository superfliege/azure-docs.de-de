---
title: Behandeln von Problemen bei der Mandantenerstellung in Azure Active Directory B2C | Microsoft-Dokumentation
description: Hier finden Sie Informationen zu Problemen beim Erstellen eines Azure Active Directory-Mandanten oder eines Azure Active Directory B2C-Mandanten sowie entsprechende Lösungen.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1abd25f9c12c949b1c807aca3a4378ac493bd877
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2019
ms.locfileid: "54842357"
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Behandeln von Problemen beim Erstellen eines Azure Active Directory-Mandanten oder eines Azure Active Directory B2C-Mandanten 

## <a name="create-an-azure-ad-tenant"></a>Erstellen eines Azure AD-Mandanten
Falls die Erstellung eines Azure AD-Mandanten (Azure Active Directory) beim ersten Versuch nicht gelingt, versuchen Sie es erneut. Sollte das Problem weiterhin bestehen, wenden Sie sich an den Azure-Support.

## <a name="create-an-azure-ad-b2c-tenant"></a>Erstellen eines Azure AD B2C-Mandanten
Sollte beim [Erstellen eines Azure AD B2C-Mandanten (Azure Active Directory B2C)](active-directory-b2c-get-started.md) ein Problem auftreten, versuchen Sie Folgendes:

* Falls der Azure AD B2C-Mandant nicht in Ihrer Mandantenliste angezeigt wird, wiederholen Sie die Mandantenerstellung.
* Falls der Azure AD B2C-Mandant nicht in Ihrer Mandantenliste angezeigt wird und eine Fehlermeldung wie die folgende erscheint, löschen Sie den Mandanten, und erstellen Sie ihn erneut:

    „Die Erstellung des B2C-Mandanten "contosob2c" konnte nicht abgeschlossen werden. Weitere Informationen finden Sie unter [diesem Link](https://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409).“
* Es ist bekannt, dass es zu Problemen kommt, wenn Sie einen vorhandenen Azure AD B2C-Mandanten löschen und erneut mit dem gleichen Domänennamen erstellen. Bei der Erstellung eines neuen Azure AD B2C-Mandanten muss deshalb ein anderer Domänenname verwendet werden.
* Sollten diese Lösungen das Problem nicht beheben, wenden Sie sich an den Azure-Support. Weitere Informationen finden Sie unter [Senden von Supportanfragen für Azure AD B2C](active-directory-b2c-support.md).

