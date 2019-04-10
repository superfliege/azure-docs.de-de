---
title: Bereitstellen von Cloudabhängigkeiten der Azure IoT OPC UA-Geräteverwaltung | Microsoft-Dokumentation
description: Informationen zum Bereitstellen von Azure OPC Twin-Abhängigkeiten.
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: conceptual
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: cdd89293328da47c8e338bb6fca8c7b93dd84f97
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759219"
---
# <a name="deploying-dependencies-for-local-development"></a>Bereitstellen von Abhängigkeiten für die lokale Entwicklung

In diesem Artikel wird erläutert, wie Sie ausschließlich die für die lokale Entwicklung und das lokale Debuggen erforderlichen Azure-Plattformdienste bereitstellen.   Am Ende haben Sie eine Ressourcengruppe mit allen für die lokale Entwicklung und das lokale Debuggen erforderlichen Elementen erstellt.

## <a name="deploy-azure-platform-services"></a>Bereitstellen von Azure-Plattformdiensten

1. Stellen Sie sicher, dass die PowerShell- und [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-1.1.0)-Erweiterungen installiert sind.  Öffnen Sie eine Eingabeaufforderung oder ein Terminal, und führen Sie Folgendes aus:

   ```bash
   git clone https://github.com/Azure/azure-iiot-components
   cd azure-iiot-components
   ```

   ```bash
   deploy -type local
   ```

2. Befolgen Sie die Anweisungen, um der Ressourcengruppe für die Bereitstellung einen Namen zuzuweisen.  Mit dem Skript werden ausschließlich die Abhängigkeiten für diese Ressourcengruppe in Ihrem Azure-Abonnement, jedoch nicht die Microservices zugewiesen.  Außerdem wird mit dem Skript eine Anwendung in Azure Active Directory registriert.  Diese ist zur Unterstützung der OAUTH-basierten Authentifizierung erforderlich.  Die Bereitstellung kann einige Minuten dauern.

3. Nach Abschluss des Skripts können Sie die ENV-Datei speichern.  Die ENV-Umgebungsdatei ist die Konfigurationsdatei für alle Dienste und Tools, die Sie auf dem Entwicklungscomputer ausführen möchten.  

## <a name="troubleshooting-deployment-failures"></a>Beheben von Problemen bei der Bereitstellung

### <a name="resource-group-name"></a>Ressourcengruppenname

Verwenden Sie einen kurzen und einfachen Namen für die Ressourcengruppe.  Der Name wird auch zum Benennen von Ressourcen verwendet, daher muss er den Namenskonventionen für Ressourcen entsprechen.  

### <a name="azure-active-directory-aad-registration"></a>Azure Active Directory-Registrierung (AAD)

Über das Bereitstellungsskript wird versucht, die AAD-Anwendungen in Azure Active Directory zu registrieren.  Je nach Ihren Rechten für den ausgewählten AAD-Mandanten können dabei Fehler auftreten.   Es gibt 3 Möglichkeiten:

1. Wenn Sie in einer Liste mit AAD-Mandanten einen Mandanten ausgewählt haben, starten Sie das Skript neu, und wählen Sie einen anderen Mandanten aus.
2. Alternativ können Sie einen privaten AAD-Mandanten bereitstellen, das Skript neu starten und diesen Mandanten verwenden.
3. Fahren Sie ohne Authentifizierung fort.  Da Sie Ihre Microservices lokal ausführen, ist dies möglich. Produktionsumgebungen werden jedoch nicht simuliert.  

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie Dienste zur OPC-Geräteverwaltung erfolgreich in einem vorhandenen Projekt bereitgestellt haben, empfiehlt sich nun folgender nächster Schritt:

> [!div class="nextstepaction"]
> [Weitere Informationen zum Bereitstellen von Modulen zur OPC-Geräteverwaltung](howto-opc-twin-deploy-modules.md)
