---
title: Festlegen der Zugriffssteuerung in Azure Cosmos DB | Microsoft Docs
description: Erfahren Sie, wie die Zugriffssteuerung in Azure Cosmos DB festgelegt wird.
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: fae3af3f-4d6e-46d8-9d9b-b80a4218add9
ms.service: cosmos-db
ms.topic: article
ms.date: 02/06/2018
ms.author: mimig
ms.openlocfilehash: 5ef4a12c8229d8801a5d9749514a69c2c1e19499
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="access-control-in-azure-cosmos-db"></a>Zugriffssteuerung in Azure Cosmos DB

Um Ihrem Benutzerkonto Azure Cosmos DB-Kontoleserzugriff hinzuzufügen, lassen Sie den Besitzer eines Abonnements die folgenden Schritte im Azure-Portal ausführen.

1. Öffnen Sie das Azure-Portal, und wählen Sie Ihr Azure Cosmos DB-Konto aus.
2. Klicken Sie auf die Registerkarte **Zugriffssteuerung (IAM)** aus, und klicken Sie dann auf **Hinzufügen**.
3. Wählen Sie im Bereich **Berechtigungen hinzufügen** im Feld **Rolle** die Option **Cosmos DB-Rolle "Kontoleser"** aus.
4. Wählen Sie im Feld **Zugriff zuweisen zu** die Option **Azure AD-Benutzer, -Gruppe oder -Anwendung** aus.
5. Wählen Sie den Benutzer, die Gruppe oder die Anwendung aus, dem oder der Sie Zugriff gewähren möchten.  Sie können das Verzeichnis nach Anzeigename, E-Mail-Adresse oder Objektbezeichner durchsuchen.
    Der ausgewählten Benutzer, die Gruppe oder die Anwendung wird in der Liste der ausgewählten Elemente angezeigt.
6. Klicken Sie auf **Speichern**.

Die Entität kann jetzt Azure Cosmos DB-Ressourcen lesen.
