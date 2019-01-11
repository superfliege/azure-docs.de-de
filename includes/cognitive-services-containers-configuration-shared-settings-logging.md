---
author: diberry
ms.author: diberry
ms.service: cognitive-services
ms.topic: include
ms.date: 01/02/2019
ms.openlocfilehash: c8235fa65a3be91956ffad731d47488a852e42a2
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977350"
---
Die `Logging`-Einstellungen dienen zur Verwaltung der ASP.NET Core-Protokollierungsunterstützung für Ihren Container. Sie können für Ihren Container die gleichen Konfigurationseinstellungen und Werte verwenden wie für eine ASP.NET Core-Anwendung. 

Der Container unterstützt folgende Protokollanbieter:

|Anbieter|Zweck|
|--|--|
|[Console](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#console-provider)|Der ASP.NET Core-Protokollierungsanbieter `Console`. Alle ASP.NET Core-Konfigurationseinstellungen und Standardwerte für diesen Protokollanbieter werden unterstützt.|
|[Debuggen](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#debug-provider)|Der ASP.NET Core-Protokollierungsanbieter `Debug`. Alle ASP.NET Core-Konfigurationseinstellungen und Standardwerte für diesen Protokollanbieter werden unterstützt.|
|[Datenträger](#disk-logging)|Der JSON-Protokollanbieter. Dieser Protokollanbieter schreibt Protokolldaten in die Ausgabeeinbindung.|

### <a name="disk-logging"></a>Datenträgerprotokollierung

Der Protokollanbieter `Disk` unterstützt folgende Konfigurationseinstellungen:  

| NAME | Datentyp | BESCHREIBUNG |
|------|-----------|-------------|
| `Format` | Zeichenfolge | Das Ausgabeformat für Protokolldateien.<br/> **Hinweis:** Dieser Wert muss auf `json` festgelegt werden, um den Protokollanbieter zu aktivieren. Wenn dieser Wert bei der Containerinstanziierung angegeben wird, ohne eine Ausgabeeinbindung anzugeben, tritt ein Fehler auf. |
| `MaxFileSize` | Ganze Zahl  | Die maximale Größe einer Protokolldatei (in MB). Wenn die Größe der aktuellen Protokolldatei diesen Wert erreicht oder übersteigt, wird vom Protokollanbieter eine neue Protokolldatei erstellt. Bei Angabe von „-1“ wird die Größe der Protokolldatei nur durch die maximal zulässige Dateigröße für die Ausgabeeinbindung begrenzt (sofern vorhanden). Der Standardwert ist 1. |

Weitere Informationen zum Konfigurieren der ASP.NET Core-Protokollierungsunterstützung finden Sie unter [Protokollierung in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#settings-file-configuration).
