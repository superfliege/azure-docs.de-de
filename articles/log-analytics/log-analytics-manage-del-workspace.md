---
title: "Löschen eines Azure Log Analytics-Arbeitsbereichs | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie einen Log Analytics-Arbeitsbereich löschen, wenn Sie einen Arbeitsbereich in einem persönlichen Abonnement erstellt haben oder Ihr Arbeitsbereichsmodell neu strukturieren."
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 4f455e26f078360f17f8118f8b5b1db5bd75d473
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2018
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Löschen eines Log Analytics-Arbeitsbereichs mit dem Azure-Portal
In diesem Thema wird gezeigt, wie Sie mit dem Azure-Portal einen nicht mehr benötigten Log Analytics-Arbeitsbereich löschen. 

## <a name="to-delete-a-workspace"></a>So löschen Sie einen Arbeitsbereich 
Wenn Sie einen Log Analytics-Arbeitsbereich löschen, werden alle damit zusammenhängenden Daten innerhalb von 30 Tagen aus dem Dienst gelöscht.  Gehen Sie beim Löschen eines Arbeitsbereichs vorsichtig vor, da er unter Umständen wichtige Daten und Konfigurationen enthält, deren Löschung sich negativ auf Dienstvorgänge auswirken können.  
 
Wenn Sie Administrator sind und mehrere Benutzer mit dem Arbeitsbereich verknüpft sind, wird die Zuordnung zwischen den Benutzern und dem Arbeitsbereich aufgehoben. Wenn die Benutzer anderen Arbeitsbereichen zugeordnet sind, können sie Log Analytics mit diesen Arbeitsbereichen weiter nutzen. Wenn sie jedoch keinen anderen Arbeitsbereichen zugeordnet sind, müssen sie einen Arbeitsbereich erstellen, um Log Analytics verwenden zu können. 

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com) an. 
2. Klicken Sie im Azure-Portal auf **Alle Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.
3. Wählen Sie im Log Analytics-Abonnementbereich einen Arbeitsbereich aus, und klicken Sie dann oben im mittleren Bereich auf **Löschen**.<br><br> ![Löschoption im Bereich mit den Arbeitsbereichseigenschaften](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Wenn das Fenster mit einer Meldung angezeigt wird, in der Sie zum Bestätigen der Löschung des Arbeitsbereichs aufgefordert werden, klicken Sie auf **Ja**.<br><br> ![Bestätigen des Löschvorgangs für den Arbeitsbereich](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

