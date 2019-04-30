---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: a3b6616edbe1678cb18f78ec9025fc0286bf124f
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684474"
---
Beachten Sie die folgenden bewährten Methoden:

- Der Verwaltungsdienst kann keine vorhandenen Kennwörter abrufen, er kann sie nur über das Azure-Portal zurücksetzen. Es wird empfohlen, alle Kennwörter an einem sicheren Ort zu speichern, damit Sie ein Kennwort nicht zurücksetzen müssen, wenn Sie es vergessen haben. Wenn Sie ein Kennwort zurücksetzen, vergessen Sie nicht, alle Benutzer zuvor zu benachrichtigen.
- Auf die Windows PowerShell-Oberfläche Ihres Geräts können Sie remote über HTTP zugreifen. Aus Sicherheitsgründen empfiehlt es sich, HTTP nur in vertrauenswürdigen Netzwerken zu verwenden.
- Gerätekennwörter müssen sicher und gut geschützt sein. Berücksichtigen Sie die [bewährten Methoden für Kennwörter](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices#enable-password-management).