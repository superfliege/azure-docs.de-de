---
title: Ermittlungsfunktionen mit Notebooks in Azure Sentinel Preview | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Notebooks mit den Ermittlungsfunktionen von Azure Sentinel verwendet werden.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 63ce2be847017ed7e80fe5e573d5553311f6af2f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2019
ms.locfileid: "58107677"
---
# <a name="use-notebooks-to-hunt-for-anomalies"></a>Verwenden von Notebooks zur Ermittlung von Anomalien

> [!IMPORTANT]
> Azure Sentinel befindet sich momentan in der Public Preview.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel nutzt die Leistungsfähigkeit der interaktiven Jupyter-Notebooks, um Erkenntnisse und Aktionen zum Untersuchen oder Ermitteln von Anomalien in Ihrer Umgebung bereitzustellen. Azure Sentinel enthält vorab geladene Notebooks, die von den Sicherheitsanalysten von Microsoft entwickelt wurden. Jedes Notebook ist mit einem eigenständigen Workflow für einen bestimmten Anwendungsfall entworfen. In jedem Notebook sind Visualisierungen enthalten, um die Untersuchung von Daten und die Ermittlung von Bedrohungen zu beschleunigen. Sie können die integrierten Notebooks gemäß Ihrer Anforderungen anpassen, neue Notebooks von Grund auf neu erstellen oder Notebooks der Azure Sentinel-GitHub-Community importieren. Jupyter-Notebooks werden als Projekt in die Azure Notebooks-Seite importiert, wodurch Benutzer an einem Ort auf all ihre Azure Sentinel-Notebooks zugreifen können. Notebooks können mit einem Klick ausgeführt oder von Benutzern ihrer Umgebung entsprechend konfiguriert werden.

Durch die vollständig integrierte Funktion können Notebooks per Cloud Computing und in Cloudspeichern ausgeführt werden, ohne zugrunde liegenden Wartungsaufwand zu erzeugen. Aufgrund der Möglichkeit, das vorhandene Jupyter Notebooks-Ökosystem zu nutzen, können Sie Modelle beschaffen, ohne Kundendaten preiszugeben. 


Alle Notebooks werden in Azure Notebooks (derzeit als Preview verfügbar) gehostet. Dabei handelt es sich um eine interaktive Entwicklungsumgebung in der Azure-Cloud. Notebooks sind auf der ganzen Welt immer über jeden beliebigen Browser abrufbar.  Die integrierten Notebooks von Azure Sentinel für die Untersuchung und Ermittlung werden in ein Projekt geklont, das Ihnen gehört und an Ihre Umgebung angepasst werden kann. Die folgenden integrierten Notebooks gehören zu den beliebtesten:

- **Alert investigation and hunting** (Untersuchung und Ermittlung von Warnungen): Dieses interaktive Notebook ermöglicht die schnelle Selektierung verschiedener Warnungen, indem verwandte Aktivitäten abgerufen und die Warnungen um zugehörige Aktivitäten und Daten erweitert werden, aus denen die Warnungen generiert wurden.

- **Endpoint host guided hunting** (Endpunkthost-orientierte Ermittlung): Dieses Notebook ermöglicht Ihnen die Ermittlung von Anzeichen einer Sicherheitsverletzung durch Ausführen von Drilldowns in sicherheitsbezogene Aktivitäten, die mit einem Endpunkthost verknüpft sind.  

- **Office sign in guided hunting** (Office-Anmeldedaten-orientierte Ermittlung): Dieses Notebook ermöglicht die Ermittlung verdächtiger Anmeldedaten durch Visualisieren der geografischen Standorte von verdächtigen Protokollen und Anzeigen von ungewöhnlichen Anmeldedatenmustern, die aus Office 365-Daten abgeleitet werden.

## <a name="run-a-notebook"></a>Ausführen eines Notebooks
Im folgenden Beispiel wird das integrierte Notebook für die Suche nach ausführlichen Informationen zu Standortanomalien, um zu ermitteln, ob jemand von einem unerwarteten Standort aus auf Ihr Netzwerk zugreift.

1. Klicken Sie im Azure Sentinel-Portal auf **Notebooks**, und wählen Sie dann eines der integrierten Notebooks aus.
  
   ![Notebook auswählen](./media/notebooks/select-notebook.png)

3. Klicken Sie auf **Importieren**, um das GitHub-Repository in Ihr Azure Notebooks-Projekt zu klonen.
   ![Notebook importieren](./media/notebooks/import1.png)
4. Jedes Notebook führt Sie durch die einzelnen Schritte zum Durchführen einer Ermittlung oder Untersuchung. Modelle, Bibliotheken sowie andere Abhängigkeiten und Konfigurationen für die Konnektivität mit Azure Sentinel werden automatisch importiert, um die Ausführung mit nur einem Klick zu ermöglichen. Jeglicher Code und alle Bibliotheken, die zum Ausführen eines Notebooks erforderlich sind, werden vorab geladen. Sie können sofort damit beginnen, ein Notebook für Ihren Log Analytics-Arbeitsbereich auszuführen, ohne Konfigurationen vornehmen zu müssen.

   ![Repository importieren](./media/notebooks/import2.png)

5. Sie können alle bereitgestellten Beispielnotebooks untersuchen, anpassen und ausführen. Sie können sie als Bausteine für viele verschiedene Szenarios verwenden.

   ![Notebook auswählen](./media/notebooks/import3.png)



## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie eine Ermittlungsuntersuchung mithilfe von Notebooks in Azure Sentinel ausführen. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:

- [Proactively hunt for threats (Proaktive Ermittlung von Bedrohungen)](hunting.md)
- [Use bookmarks to save interesting information while hunting (Verwenden von Lesezeichen zum Speichern interessanter Informationen bei der Ermittlung)](bookmarks.md)