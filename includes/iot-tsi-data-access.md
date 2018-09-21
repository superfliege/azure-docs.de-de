---
title: Includedatei
description: Includedatei
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: include
ms.date: 08/20/2018
ms.author: anshan
ms.custom: include file
ms.openlocfilehash: fb45ea02f365cf4e7b394e249f9b91a784e5469f
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46369942"
---
## <a name="grant-data-access"></a>Gewähren des Datenzugriffs

Führen Sie die folgenden Schritte aus, um einem Benutzerprinzipal Datenzugriff zu gewähren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Suchen Sie Ihre Time Series Insights-Umgebung. Geben Sie **Time Series** im **Suchfeld** ein. Wählen Sie **Time Series Environment** (Time Series-Umgebung) in den Suchergebnissen aus. 

3. Wählen Sie Ihre Time Series Insights-Umgebung in der Liste aus.

4. Wählen Sie **Datenzugriffsrichtlinien** und dann **+ Hinzufügen** aus.
    ![Verwalten der Time Series Insights-Quelle – Umgebung](media/iot-tsi-data-access/getstarted-grant-data-access1.png)

5. Wählen Sie **Benutzer auswählen** aus.  Suchen Sie den Benutzernamen oder die E-Mail-Adresse des Benutzers, den Sie hinzufügen möchten. Klicken Sie auf **Auswählen**, um die Auswahl zu bestätigen. 

    ![Verwalten der Time Series Insights-Quelle – Hinzufügen](media/iot-tsi-data-access/getstarted-grant-data-access2.png)

6. Wählen Sie **Rolle auswählen** aus. Wählen Sie die geeignete Zugriffsrolle für den Benutzer aus:
    - Wählen Sie **Mitwirkender** aus, wenn Sie dem Benutzer das Ändern von Verweisdaten und das Freigeben gespeicherter Abfragen und Perspektiven für andere Benutzer der Umgebung ermöglichen möchten. 
    - Wählen Sie andernfalls **Leser** aus, um dem Benutzer das Abfragen von Daten und das Speichern persönlicher (nicht freigegebener) Abfragen in der Umgebung zu ermöglichen.

    Wählen Sie **OK** aus, um die Rollenauswahl zu bestätigen.

    ![Verwalten der Time Series Insights-Quelle – Benutzer auswählen](media/iot-tsi-data-access/getstarted-grant-data-access3.png)

7. Wählen Sie auf der Seite **Benutzerrolle auswählen** die Option **OK** aus.

    ![Verwalten der Time Series Insights-Quelle – Rolle auswählen](media/iot-tsi-data-access/getstarted-grant-data-access4.png)

8. Auf der Seite **Datenzugriffsrichtlinien** sind die Benutzer und die Rollen der einzelnen Benutzer aufgeführt.

    ![Verwalten der Time Series Insights-Quelle – Ergebnisse](media/iot-tsi-data-access/getstarted-grant-data-access5.png)