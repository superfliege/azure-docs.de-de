---
title: Konfigurieren von Regeln und Aktionen in Azure IoT Central | Microsoft-Dokumentation
description: In diesem Tutorial wird gezeigt, wie Sie als Ersteller telemetriebasierte Regeln und Aktionen in Ihrer Azure IoT Central-Anwendung konfigurieren.
author: ankitgupta
ms.author: ankitgup
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: af2aa8d7b01d973da400808fd3e97d0739693cd2
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236330"
---
# <a name="tutorial-configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>Tutorial: Konfigurieren von Regeln und Aktionen für Ihr Gerät in Azure IoT Central

In diesem Tutorial wird gezeigt, wie Sie als Ersteller telemetriebasierte Regeln und Aktionen in Ihrer Microsoft Azure IoT Central-Anwendung konfigurieren.

In diesem Tutorial erstellen Sie eine Regel, die eine E-Mail sendet, wenn die Temperatur in einer verbundenen Klimaanlage 90&deg; F überschreitet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer telemetriebasierten Regel
> * Hinzufügen einer Aktion

## <a name="prerequisites"></a>Voraussetzungen

Absolvieren Sie zuerst das Tutorial [1: Definieren eines neuen Gerätetyps in Ihrer Azure IoT Central-Anwendung](tutorial-define-device-type.md), um die Gerätevorlage **Connected Air Conditioner** (Verbundene Klimaanlage) zu erstellen.

## <a name="create-a-telemetry-based-rule"></a>Erstellen einer telemetriebasierten Regel

1. Klicken Sie zum Hinzufügen einer neuen telemetriebasierten Regel zu Ihrer Anwendung im linken Navigationsmenü auf **Device Explorer**:

    ![Device Explorer-Seite](media/tutorial-configure-rules/explorerpage.png)

    Die erstellte Gerätevorlage **Connected Air Conditioner (1.0.0)** und das erstellte Gerät **Connected Air Conditioner-1** aus dem vorherigen Tutorial werden angezeigt.

2. Wenn Sie mit dem Anpassen der verbundenen Klimaanlage beginnen möchten, wählen Sie das im vorherigen Tutorial erstellte Gerät aus:

    ![Seite der verbundenen Klimaanlage](media/tutorial-configure-rules/builderdevicelist.png)

3. Klicken Sie auf **Regeln**, um eine Regel in der Ansicht **Regeln** hinzuzufügen:

    ![Ansicht „Regeln“](media/tutorial-configure-rules/builderrulesview.png)

4. Klicken Sie auf **Neue Regel** und anschließend auf **Telemetrie**, um die schwellenwertbasierte Telemetrieregel zu erstellen.

5. Nutzen Sie beim Festlegen der Regel die Informationen in der folgenden Tabelle:

    | Einstellung     | Wert                          |
    | ----------- | ------------------------------ |
    | NAME        | Air conditioner temperature    |
    | Regel aktivieren | Andererseits                             |
    | Bedingung   | Temperature is greater than 90 |

    ![Temperaturregelbedingung](media/tutorial-configure-rules/buildertemperaturerule.png)

## <a name="add-an-action"></a>Hinzufügen einer Aktion

Beim Festlegen einer Regel definieren Sie auch eine Aktion, die bei Erfüllung der Regelbedingungen ausgeführt werden soll. In diesem Tutorial fügen Sie eine Aktion hinzu, um eine E-Mail als Benachrichtigung zu senden, dass die Regel ausgelöst wurde.

1. Scrollen Sie zum Hinzufügen einer **Aktion** im Bereich **Configure Telemetry Rule** (Telemetrieregel konfigurieren) nach unten, und klicken Sie neben **Aktionen** auf das Pluszeichen (**+**) und dann auf **E-Mail**:

    ![Temperaturregelaktion](media/tutorial-configure-rules/builderaddaction.png)

2. Nutzen Sie beim Festlegen der Aktion die Informationen in der folgenden Tabelle:

    | Einstellung   | Wert                          |
    | --------- | ------------------------------ |
    | To        | Ihre E-Mail-Adresse             |
    | Notizen     | Temperature in air conditioner exceeded threshold. |

    > [!NOTE]
    > Um eine E-Mail-Benachrichtigung zu erhalten, muss es sich bei der E-Mail-Adresse um eine [Benutzer-ID in der Anwendung](howto-administer.md) handeln, und dieser Benutzer muss sich mindestens einmal bei der Anwendung angemeldet haben.

    ![Aktion für Temperatur im Anwendungs-Generator](media/tutorial-configure-rules/buildertemperatureaction.png)

3. Wählen Sie **Speichern** aus. Ihre Regel ist auf der Seite **Regeln** aufgeführt:

    ![Regeln im Anwendungs-Generator](media/tutorial-configure-rules/builderrules.png)

## <a name="test-the-rule"></a>Testen der Regel

Die Regel wird kurz nach dem Speichern aktiv. Wenn die in der Regel festgelegten Bedingungen erfüllt werden, sendet Ihre Anwendung eine Nachricht an die in der Aktion angegebene E-Mail-Adresse.

![E-Mail-Aktion](media/tutorial-configure-rules/email.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Erstellen einer telemetriebasierten Regel
> * Hinzufügen einer Aktion

Sie haben eine schwellenwertbasierte Regel festgelegt. Als nächster Schritt wird das [Anpassen der Ansicht für Bediener](tutorial-customize-operator.md) empfohlen.

Weitere Informationen zu den verschiedenen Regeltypen in Azure IoT Central sowie zum Parametrisieren der Regeldefinition finden Sie in den folgenden Artikeln:
* [Erstellen einer Telemetrieregel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung](howto-create-telemetry-rules.md)
* [Erstellen eine Ereignisregel und Einrichten von Benachrichtigungen in Ihrer Azure IoT Central-Anwendung](howto-create-event-rules.md)

<!-- Next tutorials in the sequence -->
