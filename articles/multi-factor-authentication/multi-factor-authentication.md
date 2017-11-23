---
title: "Informationen zur Überprüfung in zwei Schritten in Azure MFA | Microsoft-Dokumentation"
description: "Was ist Azure Multi-Factor Authentication (MFA), warum wird MFA verwendet, weitere Informationen über den Client für die Multi-Factor Authentication sowie die verschiedenen verfügbaren Methoden und Versionen. "
keywords: "Einführung in die MFA, MFA-Übersicht, Was ist MFA"
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: richagi
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: joflore
ms.openlocfilehash: a928344dfc41687c92e7845bc304a05122ab8a92
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a>Was ist Azure Multi-Factor Authentication?
Die Überprüfung in zwei Schritten ist eine Authentifizierungsmethode, die die Verwendung mehrerer Überprüfungsmethoden erfordert und eine wichtige zweite Sicherheitsebene für Benutzeranmeldungen und Transaktionen bietet. Dies funktioniert durch das Anfordern von zwei oder mehr der folgenden Verifizierungsmethoden:

* Etwas, das Sie wissen (normalerweise ein Kennwort)
* Etwas, das Sie haben (ein vertrautes Gerät, das nicht leicht dupliziert werden kann, wie ein Telefon)
* Etwas, das Sie sind (biometrisch)

<center>![Benutzername und Kennwort](./media/multi-factor-authentication/pword.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Zertifikate](./media/multi-factor-authentication/phone.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smartphone](./media/multi-factor-authentication/hware.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Smartcard](./media/multi-factor-authentication/smart.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Virtuelle Smartcard](./media/multi-factor-authentication/vsmart.png)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Benutzername und Kennwort](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) ist die Lösung von Microsoft für die Überprüfung in zwei Schritten. Azure MFA hilft beim Schützen des Zugriffs auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen von Benutzern an ein einfaches Anmeldeverfahren. Sie bietet eine sichere Authentifizierung mit verschiedenen Überprüfungsmethoden, einschließlich Telefonanruf, SMS oder mobile App.

## <a name="why-use-azure-multi-factor-authentication"></a>Warum sollten Sie Azure Multi-Factor Authentication verwenden?
Heute sind mehr Menschen immer häufiger verbunden als je zuvor. Über Smartphones, Tablets, Laptops und PCs haben Benutzer verschiedene Möglichkeiten für den Zugriff auf ihre Konten und Anwendungen, sodass sie von überall aus eine Verbindung herstellen und ständig verbunden bleiben können.

Azure Multi-Factor Authentication ist eine benutzerfreundliche, skalierbare und zuverlässige Lösung, die eine zweite Methode zur Authentifizierung zum Schutz der Benutzer darstellt.

| ![Benutzerfreundlich](./media/multi-factor-authentication/simple.png) | ![Skalierbar](./media/multi-factor-authentication/scalable.png) | ![Immer geschützt](./media/multi-factor-authentication/protected.png) | ![Zuverlässig](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Benutzerfreundlich** |**Skalierbar** |**Immer geschützt** |**Zuverlässig** |

* **Benutzerfreundlich** : Azure Multi-Factor Authentication ist einfach einzurichten und zu verwenden. Dank des zusätzlichen Schutzes von Azure Multi-Factor Authentication können Benutzer ihre eigenen Geräte verwalten. Und das Beste: Azure Multi-Factor Authentication kann in der Regel ganz einfach mit wenigen Klicks eingerichtet werden.
* **Skalierbar** : Azure Multi-Factor Authentication nutzt die Vorteile der Cloud und wird nahtlos in Ihr lokales Active Directory und in benutzerdefinierte Apps integriert. Von diesem Schutz profitieren sogar Ihre umfangreichen unternehmenskritischen Szenarien.
* **Immer geschützt** : Azure Multi-Factor Authentication bietet sichere Authentifizierung unter Verwendung der strengsten Branchenstandards.
* **Zuverlässig:** Microsoft garantiert für Azure Multi-Factor Authentication eine Verfügbarkeit von 99,9 %. Der Dienst gilt als nicht verfügbar, wenn keine Überprüfungsanforderungen für die Überprüfung in zwei Schritten empfangen oder verarbeitet werden können.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Nächste Schritte

- Informationen zur [Funktionsweise von Azure Multi-Factor Authentication](multi-factor-authentication-how-it-works.md)

- Informieren Sie sich zu den verschiedenen [Versionen und Verbrauchsmethoden für Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md).
