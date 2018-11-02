---
title: Azure IoT Hub – IP-Verbindungsfilter | Microsoft-Dokumentation
description: Verwenden der IP-Filterung zum Blockieren von Verbindungen von bestimmten IP-Adressen für Ihren Azure IoT Hub. Sie können Verbindungen von einzelnen IP-Adressen oder Bereichen von IP-Adressen blockieren.
author: rezasherafat
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: rezas
ms.openlocfilehash: 903f8284327d3d5b9ef386305a436ce44a8a11b2
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378101"
---
# <a name="use-ip-filters"></a>Verwenden von IP-Filtern

Die Sicherheit ist bei jeder IoT-Lösung, die auf Azure IoT Hub basiert, ein wichtiger Aspekt. Manchmal müssen Sie im Rahmen der Sicherheitskonfiguration die IP-Adressen explizit angeben, über die Geräte eine Verbindung herstellen können. Mit dem Feature *IP-Filter* können Sie Regeln konfigurieren, mit denen Datenverkehr von bestimmten IPv4-Adressen abgelehnt oder zugelassen wird.

## <a name="when-to-use"></a>Einsatzgebiete

Wenn es hilfreich ist, die IoT Hub-Endpunkte für bestimmte IP-Adressen zu blockieren, gibt es zwei spezifische Anwendungsfälle:

* Der IoT Hub sollte nur Datenverkehr aus einem angegebenen Bereich von IP-Adressen empfangen und den anderen Datenverkehr ablehnen. Ein Beispiel hierfür ist, wenn Sie IoT Hub mit [Azure Express Route](https://azure.microsoft.com/documentation/articles/expressroute-faqs/#supported-services) verwenden, um private Verbindungen zwischen IoT Hub und Ihrer lokalen Infrastruktur zu erstellen.

* Sie müssen Datenverkehr von IP-Adressen ablehnen, die vom IoT Hub-Administrator als verdächtig eingestuft wurden.

## <a name="how-filter-rules-are-applied"></a>Anwenden von Filterregeln

Die IP-Filterregeln werden auf IoT Hub-Dienstebene angewendet. Daher gelten die IP-Filterregeln für alle Verbindungen von Geräten und Back-End-Apps mit allen unterstützten Protokollen.

Alle Verbindungsversuche über eine IP-Adresse, die für eine IP-Ablehnungsregel in IoT Hub eine Übereinstimmung ergeben, werden mit dem Statuscode „401 – Nicht autorisiert“ und einer Beschreibung versehen. In der Antwortnachricht wird die IP-Regel nicht erwähnt.

## <a name="default-setting"></a>Standardeinstellung

Das Raster **IP-Filter** im Portal ist für ein IoT Hub standardmäßig leer. Diese Standardeinstellung bedeutet, dass Ihr Hub Verbindungen von allen IP-Adressen akzeptiert. Die Standardeinstellung entspricht einer Regel, bei der der IP-Adressbereich 0.0.0.0/0 zulässig ist.

![Standardeinstellungen der IP-Filterung von IoT Hub](./media/iot-hub-ip-filtering/ip-filter-default.png)

## <a name="add-or-edit-an-ip-filter-rule"></a>Hinzufügen oder Bearbeiten einer IP-Filterregel

Wenn Sie eine IP-Filterregel hinzufügen, werden Sie zum Eingeben der folgenden Werte aufgefordert:

* Einen **Namen für die IP-Filterregel**, bei dem es sich um eine eindeutige alphanumerische Zeichenfolge mit maximal 128 Zeichen handeln muss (Groß-/Kleinschreibung wird berücksichtigt). Nur alphanumerische ASCII 7-Bit-Zeichen und die Zeichen `{'-', ':', '/', '\', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '''}` sind zulässig.

* Wählen Sie als **Aktion** für die IP-Filterregel**reject** (Ablehnen) oder **accept** (Zulassen).

* Geben Sie eine einzelne IPv4-Adresse oder einen Block von IP-Adressen in CIDR-Notation ein. In CIDR-Notation steht 192.168.100.0/22 beispielsweise für die 1.024 IPv4-Adressen von 192.168.100.0 bis 192.168.103.255.

![Hinzufügen einer IP-Filterregel zu IoT Hub](./media/iot-hub-ip-filtering/ip-filter-add-rule.png)

Nach dem Speichern der Regel wird eine Warnung mit dem Hinweis angezeigt, dass der Updatevorgang läuft.

![Benachrichtigung über das Speichern einer IP-Filterregel](./media/iot-hub-ip-filtering/ip-filter-save-new-rule.png)

Die Option **Hinzufügen** ist deaktiviert, wenn Sie das Maximum von 10 IP-Filterregeln erreichen.

Sie können eine vorhandene Regel bearbeiten, indem Sie auf die Zeile mit der Regel doppelklicken.

> [!NOTE]
> Das Ablehnen von IP-Adressen kann andere Azure-Dienste (z. B. Azure Stream Analytics, Azure Virtual Machines oder den Geräte-Explorer im Portal) an der Interaktion mit dem IoT Hub hindern.

> [!WARNING]
> Wenn Sie Azure Stream Analytics (ASA) verwenden, um Nachrichten von einer IoT Hub-Instanz mit aktivierter IP-Filterung zu lesen, verwenden Sie den Event Hub-kompatiblen Namen und Endpunkt Ihrer IoT Hub-Instanz in der ASA-Verbindungszeichenfolge.

## <a name="delete-an-ip-filter-rule"></a>Löschen einer IP-Filterregel

Wählen Sie zum Löschen einer IP-Filterregel im Raster mindestens eine Filterregel aus, und klicken Sie auf **Löschen**.

![Löschen einer IP-Filterregel von IoT Hub](./media/iot-hub-ip-filtering/ip-filter-delete-rule.png)

## <a name="ip-filter-rule-evaluation"></a>Auswertung von IP-Filterregeln

IP-Filterregeln werden der Reihenfolge nach angewendet, und die erste Regel, die eine Übereinstimmung mit der IP-Adresse ergibt, bestimmt die Aktion (Zulassen oder Ablehnen).

Wenn Sie beispielsweise Adressen im Bereich 192.168.100.0/22 zulassen und alle anderen Adressen ablehnen möchten, sollte die erste Regel im Raster lauten, dass der Adressbereich 192.168.100.0/22 zulässig ist. Mit der nächsten Regel sollten alle Adressen abgelehnt werden, indem der Bereich 0.0.0.0/0 verwendet wird.

Sie können die Reihenfolge der IP-Filterregeln im Raster ändern, indem Sie auf die drei vertikal angeordneten Punkte am Anfang der Zeile klicken und Drag & Drop nutzen.

Klicken Sie auf **Speichern**, um die neue Reihenfolge der IP-Filterregeln zu speichern.

![Ändern der Reihenfolge der IP-Filterregeln von IoT Hub](./media/iot-hub-ip-filtering/ip-filter-rule-order.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Vorgangsüberwachung](iot-hub-operations-monitoring.md)
* [IoT Hub-Metriken](iot-hub-metrics.md)