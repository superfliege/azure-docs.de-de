---
title: Protokollieren des Netzwerkdatenverkehrs zu und von einem virtuellen Computer – Tutorial – Azure-Portal | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie den Netzwerkdatenverkehr zu und von einem virtuellen Computer mithilfe der Funktion für NSG-Flussprotokolle von Network Watcher protokollieren können.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to log the network traffic to and from a VM so I can analyze it for anomalies.
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: f010bebcf1130b3061c60987ffbd4e706a030773
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32776549"
---
# <a name="tutorial-log-network-traffic-to-and-from-a-virtual-machine-using-the-azure-portal"></a>Tutorial: Protokollieren des Netzwerkdatenverkehrs zu und von einem virtuellen Computer über das Azure-Portal

Mithilfe einer Netzwerksicherheitsgruppe (NSG) können Sie eingehenden Datenverkehr zu und ausgehenden Datenverkehr von einem virtuellen Computer filtern. Sie können den Netzwerkdatenverkehr, der über eine NSG verläuft, mithilfe der Funktion für NSG-Flussprotokolle von Network Watcher protokollieren. In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines virtuellen Computers mit einer Netzwerksicherheitsgruppe
> * Aktivieren von Network Watcher und Registrieren des Microsoft.Insights-Anbieters
> * Aktivieren eines Datenflussprotokolls für eine NSG mithilfe der Funktion für NSG-Flussprotokolle von Network Watcher
> * Herunterladen protokollierter Daten
> * Anzeigen protokollierter Daten

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-vm"></a>Erstellen eines virtuellen Computers

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Klicken Sie auf **Compute** und anschließend auf **Windows Server 2016 Datacenter** oder auf **Ubuntu Server 17.10 VM**.
3. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die Standardwerte für die übrigen Einstellungen, und klicken Sie auf **OK**:

    |Einstellung|Wert|
    |---|---|
    |Name|myVm|
    |Benutzername| Geben Sie den gewünschten Benutzernamen ein.|
    |Kennwort| Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    |Abonnement| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe| Klicken Sie auf **Neu erstellen**, und geben Sie **myResourceGroup** ein.|
    |Standort| Wählen Sie **USA, Osten** aus.|

4. Wählen Sie eine Größe für den virtuellen Computer aus, und klicken Sie dann auf **Auswählen**.
5. Übernehmen Sie unter **Einstellungen** alle Standardwerte, und klicken Sie auf **OK**.
6. Wählen Sie auf der Seite **Zusammenfassung** unter **Erstellen** die Option **Erstellen**, um die Bereitstellung des virtuellen Computers zu starten. Die Bereitstellung des virtuellen Computers dauert einige Minuten. Warten Sie, bis die Bereitstellung des virtuellen Computers abgeschlossen ist, bevor Sie mit den weiteren Schritten fortfahren.

Die Erstellung des virtuellen Computers dauert einige Minuten. Fahren Sie erst dann mit den weiteren Schritten fort, wenn die Erstellung des virtuellen Computers abgeschlossen ist. Beim Erstellen des virtuellen Computers im Portal wird auch eine Netzwerksicherheitsgruppe mit dem Namen **myVm-nsg** erstellt und der Netzwerkschnittstelle für den virtuellen Computer zugeordnet.

## <a name="enable-network-watcher"></a>Aktivieren von Network Watcher

Wenn Sie bereits über eine aktivierte Network Watcher-Instanz in der Region „USA, Osten“ verfügen, fahren Sie mit [Registrieren des Insights-Anbieters](#register-insights-provider) fort.

1. Klicken Sie im Portal auf **Alle Dienste**. Geben Sie im Feld **Filter** die Zeichenfolge *Network Watcher* ein. Wenn **Network Watcher** in den Ergebnissen angezeigt wird, wählen Sie ihn aus.
2. Wählen Sie die **Regionen** aus, um sie zu erweitern, und wählen Sie dann **...** rechts von **USA, Osten** aus (siehe folgende Abbildung):

    ![Aktivieren von Network Watcher](./media/network-watcher-nsg-flow-logging-portal/enable-network-watcher.png)

3. Klicken Sie auf **Network Watcher aktivieren**.

## <a name="register-insights-provider"></a>Registrieren des Insights-Anbieters

Für die NSG-Datenflussprotokollierung ist der **Microsoft.Insights**-Anbieter erforderlich. Führen Sie zum Registrieren des Anbieters die folgenden Schritte aus:

1. Wählen Sie oben links im Portal die Option **Alle Dienste** aus. Geben Sie im Feld „Filter“ den Text *Abonnements* ein. Wenn **Abonnements** in den Suchergebnissen angezeigt wird, wählen Sie diesen Eintrag aus.
2. Wählen Sie in der Liste der Abonnements das Abonnement aus, für das Sie den Anbieter aktivieren möchten.
3. Klicken Sie unter **EINSTELLUNGEN** auf **Ressourcenanbieter**.
4. Überprüfen Sie, ob der **STATUS** für den **microsoft.insights**-Anbieter **Registriert** lautet (siehe folgende Abbildung). Lautet der Status **Nicht registriert**, wählen Sie rechts neben dem Anbieter die Option **Registrieren** aus.

    ![Registrieren des Anbieters](./media/network-watcher-nsg-flow-logging-portal/register-provider.png)

## <a name="enable-nsg-flow-log"></a>Aktivieren des NSG-Flussprotokolls

1. NSG-Flussprotokolldaten werden in einem Azure Storage-Konto gespeichert. Zum Erstellen eines Azure Storage-Kontos wählen Sie oben links im Portal die Option **+ Ressource erstellen** aus.
2. Wählen Sie die Option **Speicher** und anschließend **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.
3. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die verbleibenden Standardeinstellungen, und wählen Sie dann **Erstellen** aus.

    | Einstellung        | Wert                                                        |
    | ---            | ---   |
    | Name           | Länge von 3 bis 24 Zeichen, darf nur Kleinbuchstaben und Ziffern enthalten und muss für alle Azure Storage-Konten eindeutig sein.                                                               |
    | Standort       | Wählen Sie **USA, Osten** aus.                                           |
    | Ressourcengruppe | Wählen Sie **Vorhandene verwenden** und dann **myResourceGroup** aus. |

    Das Erstellen des Speicherkontos kann etwa eine Minute dauern. Fahren Sie erst dann mit den weiteren Schritten fort, wenn das Speicherkonto erstellt wurde. Wenn Sie kein Speicherkonto erstellen, sondern ein vorhandenes Konto verwenden, vergewissern Sie sich, dass Sie ein Speicherkonto auswählen, für das **Alle Netzwerke** (Standard) unter **Firewalls und virtuelle Netzwerken** in **EINSTELLUNGEN** ausgewählt ist.
4. Wählen Sie oben links im Portal die Option **Alle Dienste** aus. Geben Sie im Feld **Filter** die Zeichenfolge *Network Watcher* ein. Wählen Sie **Network Watcher** aus, wenn der Begriff in den Suchergebnissen angezeigt wird.
5. Wählen Sie unter **PROTOKOLLE** die Option **NSG-Flussprotokolle** aus (siehe folgende Abbildung):

    ![NSGs](./media/network-watcher-nsg-flow-logging-portal/nsgs.png)

6. Wählen Sie in der Liste der Netzwerksicherheitsgruppen die Gruppe mit dem Namen **myVm-nsg** aus.
7. Wählen Sie unter **Flowprotokolleinstellungen** die Option **Ein** aus.
8. Wählen Sie das in Schritt 3 erstellte Speicherkonto aus.
9. Setzen Sie **Aufbewahrung (Tage)** auf 5, und klicken Sie dann auf **Speichern**.

## <a name="download-flow-log"></a>Herunterladen des Flussprotokolls

1. Wählen Sie in Network Watcher im Portal die Option **NSG-Flussprotokolle** unter **PROTOKOLLE** aus.
2. Wählen Sie **Sie können Flowprotokolle aus konfigurierten Speicherkonten herunterladen** aus (siehe folgende Abbildung):

  ![Herunterladen von Flussprotokollen](./media/network-watcher-nsg-flow-logging-portal/download-flow-logs.png)

3. Wählen Sie das in Schritt 2 unter [Aktivieren des NSG-Flussprotokolls](#enable-nsg-flow-log) konfigurierte Speicherkonto aus.
4. Wählen Sie unter **BLOB-DIENST** die Option **Container** und dann den Container **insights-logs-networksecuritygroupflowevent** aus (siehe folgende Abbildung):

    ![Auswählen des Containers](./media/network-watcher-nsg-flow-logging-portal/select-container.png)
5. Navigieren Sie durch die Ordnerhierarchie, bis Sie zu einer Datei „PT1H.json“ gelangen (siehe folgende Abbildung):

    ![Protokolldatei](./media/network-watcher-nsg-flow-logging-portal/log-file.png)

    Protokolldateien werden in einer Ordnerhierarchie gespeichert, die der folgenden Namenskonvention folgt: https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json

6. Wählen Sie **...** rechts neben der Datei „PT1H.json“ und dann **Herunterladen** aus.

## <a name="view-flow-log"></a>Anzeigen des Flussprotokolls

Der folgende JSON-Code ist ein Beispiel für den Inhalt der Datei „PT1H.json“ für jeden Datenfluss, für den Daten protokolliert werden:

```json
{
    "time": "2018-05-01T15:00:02.1713710Z",
    "systemId": "<Id>",
    "category": "NetworkSecurityGroupFlowEvent",
    "resourceId": "/SUBSCRIPTIONS/<Id>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MYVM-NSG",
    "operationName": "NetworkSecurityGroupFlowEvents",
    "properties": {
        "Version": 1,
        "flows": [{
            "rule": "UserRule_default-allow-rdp",
            "flows": [{
                "mac": "000D3A170C69",
                "flowTuples": ["1525186745,192.168.1.4,10.0.0.4,55960,3389,T,I,A"]
            }]
        }]
    }
}
```

Bei dem Wert für **mac** in der vorherigen Ausgabe handelt es sich um die MAC-Adresse der Netzwerkschnittstelle, die beim Erstellen des virtuellen Computers erstellt wurde. Die durch Kommas getrennten Informationen für **flowTuples** sind wie folgt:

| Beispieldaten | Angabe   | Erklärung                                                                              |
| ---          | ---                    | ---                                                                                      |
| 1525186745   | Zeitstempel             | Der Zeitstempel für den Zeitpunkt, zu dem der Datenfluss auftrat, im UNIX EPOCHE-Format. Im vorherigen Beispiel wird das Datum in den 1. Mai 2018 um 14:59:05 Uhr GMT konvertiert.                                                                                    |
| 192.168.1.4  | Quell-IP-Adresse      | Die Quell-IP-Adresse, von der der Datenfluss stammte.
| 10.0.0.4     | Ziel-IP-Adresse | Die Ziel-IP-Adresse, für die der Datenfluss bestimmt war. 10.0.0.4 ist die private IP-Adresse des virtuellen Computers, den Sie unter [Erstellen eines virtuellen Computers](#create-a-vm) erstellt haben.                                                                                 |
| 55960        | Quellport            | Der Quellport, von dem der Datenfluss stammte.                                           |
| 3389         | Zielport       | Der Zielport, für den der Datenfluss bestimmt war. Da der Datenverkehr für Port 3389 bestimmt war, wurde der Datenfluss anhand der Regel namens **UserRule_default-allow-rdp** in der Protokolldatei verarbeitet.                                                |
| T            | Protokoll               | Gibt an, ob das Protokoll des Datenflusses TCP (T) oder UDP (U) war.                                  |
| I            | Richtung              | Gibt an, ob es sich um eingehenden (I) oder ausgehenden (O) Datenverkehr handelte.                                     |
| A            | Aktion                 | Gibt an, ob es sich um zulässigen (A) oder verweigerten (D) Datenverkehr handelte.                                           |

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die NSG-Datenflussprotokollierung für eine Netzwerksicherheitsgruppe aktivieren. Sie haben außerdem erfahren, wie Sie in einer Datei protokollierte Daten herunterladen und anzeigen. Die Rohdaten in der JSON-Datei sind möglicherweise schwer zu interpretieren. Zur visuellen Darstellung der Daten können Sie die [Datenverkehrsanalyse](traffic-analytics.md) von Network Watcher, Microsoft [PowerBI](network-watcher-visualize-nsg-flow-logs-power-bi.md) und andere Tools verwenden.