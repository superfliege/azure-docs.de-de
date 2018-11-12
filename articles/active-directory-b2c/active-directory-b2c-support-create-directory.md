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
ms.openlocfilehash: 1d510f51ed28c28c698437f905c4911a8c32e5ce
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234985"
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

