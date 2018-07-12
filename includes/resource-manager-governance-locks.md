---
title: Includedatei
description: Includedatei
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: a69a739f36854cacd6b361ca2bd17d904e9c4c96
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38753682"
---
Ressourcensperren verhindern, dass Benutzer in Ihrer Organisation versehentlich wichtige Ressourcen löschen oder ändern. Im Gegensatz zur rollenbasierten Zugriffssteuerung wenden Ressourcensperren eine Einschränkung auf alle Benutzer und Rollen an. 

Sie können die Sperrebene auf **CanNotDelete** oder **ReadOnly** festlegen. Im Portal werden die Sperrebenen als **Löschen** und **Schreibgeschützt** angezeigt.

* **CanNotDelete** bedeutet, dass autorisierte Benutzer weiterhin eine Ressource lesen und ändern, aber nicht löschen können. 
* **ReadOnly** bedeutet, dass autorisierte Benutzer eine Ressource zwar lesen, aber nicht löschen oder aktualisieren können. Mit dieser Sperre erzielen Sie einen ähnlichen Effekt wie durch die Beschränkung sämtlicher autorisierter Benutzer auf die Berechtigungen der **Leserolle**. 

> [!TIP]
> Seien Sie beim Anwenden einer **ReadOnly**-Sperre vorsichtig. Einige Vorgänge, die wie Lesevorgänge aussehen, erfordern tatsächlich zusätzliche Aktionen. Das Festlegen einer **ReadOnly**-Sperre für ein Speicherkonto hindert beispielsweise alle Benutzer am Auflisten der Schlüssel. Der Vorgang zum Auflisten von Schlüsseln wird über eine POST-Anforderung behandelt, da die zurückgegebenen Schlüssel für Schreibvorgänge zur Verfügung stehen. Das Festlegen einer **ReadOnly**-Sperre für eine App Service-Ressource verhindert, dass der Server-Explorer von Visual Studio Dateien für die Ressource anzeigen kann, da für diese Interaktion Schreibzugriff erforderlich ist.

Wenn Sie eine Sperre in einem übergeordneten Bereich anwenden, erben alle Ressourcen in diesem Bereich die entsprechende Sperre. Auch Ressourcen, die Sie später hinzufügen, erben die Sperre aus dem übergeordneten Element. Die restriktivste Sperre in der Vererbung hat Vorrang.

Resource Manager-Sperren gelten nur für Vorgänge auf der Verwaltungsebene (also für Vorgänge, die an `https://management.azure.com` gesendet werden). Die Verarbeitung ressourceneigener Funktionen wird durch die Sperren nicht eingeschränkt. Die Ressourcenänderungen sind eingeschränkt, die Ressourcenvorgänge jedoch nicht. Beispielsweise verhindert eine ReadOnly-Sperre für eine SQL-Datenbank das Löschen oder Ändern der Datenbank. Sie verhindert jedoch nicht das Erstellen, Aktualisieren oder Löschen von Daten in der Datenbank. Datentransaktionen sind zulässig, da diese Vorgänge nicht an `https://management.azure.com` gesendet werden.

### <a name="who-can-create-or-delete-locks-in-your-organization"></a>Personen in Ihrer Organisation, die Sperren erstellen oder löschen können
Zum Erstellen oder Löschen von Verwaltungssperren benötigen Sie Zugriff auf `Microsoft.Authorization/locks/*`-Aktionen. Unter den integrierten Rollen können nur **Besitzer** und **Benutzerzugriffsadministrator** diese Aktionen ausführen.
