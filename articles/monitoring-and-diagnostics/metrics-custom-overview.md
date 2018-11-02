---
title: Benutzerdefinierte Metriken in Azure Monitor
description: Erfahren Sie mehr zu benutzerdefinierten Metriken in Azure Monitor und wie diese modelliert werden.
author: ancav
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 1bdf1e1f5e58ecb0939d5876e0cef349e32de517
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344749"
---
# <a name="custom-metrics-in-azure-monitor"></a>Benutzerdefinierte Metriken in Azure Monitor

Wenn Sie Ressourcen und Anwendungen in Azure bereitstellen, sollten Sie mit dem Erfassen von Telemetriedaten beginnen, um Einblicke in deren Leistung und Integrität zu erhalten. Azure stellt Ihnen bei der Bereitstellung von Ressourcen einige sofort einsetzbare Metriken zur Verfügung. Diese werden als Standard- oder Plattformmetriken bezeichnet. Diese Metriken sind jedoch naturgemäß eingeschränkt. Möglicherweise möchten Sie einige benutzerdefinierte Leistungsindikatoren oder unternehmensspezifische Metriken sammeln, um eingehendere Erkenntnisse bereitzustellen.
Diese "benutzerdefinierten" Metriken können über Ihre Anwendungstelemetrie, einen auf Ihren Azure-Ressourcen ausgeführten Agent, oder sogar über ein außerhalb des Unternehmens befindliches Überwachungssystem gesammelt und direkt an Azure Monitor übermittelt werden. Nach der Veröffentlichung in Azure Monitor können Sie nach benutzerdefinierten Metriken für Ihre Azure-Ressourcen und -Anwendungen suchen, diese abfragen und Warnungen ausgeben, und zwar parallel zu den von Azure ausgegebenen Standardmetriken.

## <a name="send-custom-metrics"></a>Senden benutzerdefinierter Metriken
Benutzerdefinierte Metriken können über eine Vielzahl von Methoden an Azure Monitor gesendet werden.
- Instrumentieren Ihrer Anwendung mit dem Application Insights SDK und Senden benutzerdefinierter Telemetriedaten an Azure Monitor 
- Installieren der Windows-Diagnoseerweiterung auf Ihrer [Azure-VM](metrics-store-custom-guestos-resource-manager-vm.md), [VM-Skalierungsgruppe](metrics-store-custom-guestos-resource-manager-vmss.md), [klassischen VM](metrics-store-custom-guestos-classic-vm.md) oder Ihrem [klassischen Clouddienst](metrics-store-custom-guestos-classic-cloud-service.md) und Senden der Leistungsindikatoren an Azure Monitor 
- Installieren des [InfluxDB Telegraf Agent](metrics-store-custom-linux-telegraf.md) auf Ihrer Azure Linux VM und Senden der Metriken mithilfe des Ausgabe-Plug-Ins von Azure Monitor
- Direktes Senden von benutzerdefinierten Metriken [an die Azure Monitor-REST-API](metrics-store-custom-rest-api.md) https://<azureregion>.monitoring.azure.com/<AzureResourceID>/metrics

Wenn Sie benutzerdefinierte Metriken an Azure Monitor senden, muss jeder gemeldete Datenpunkt (oder Wert) die folgenden Informationen enthalten:

### <a name="authentication"></a>Authentifizierung
Zum Übermitteln von benutzerdefinierter Metriken an Azure Monitor muss die Entität über ein gültiges Azure Active Directory-Token im „Bearer“-Header der Anforderung verfügen. Es gibt einige unterstützte Möglichkeiten, einen gültigen Bearertoken zu erhalten:
1. [Verwaltete Identitäten für Azure-Ressourcen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview): Gibt einer Azure-Ressource selbst (etwa einer VM) eine Identität. MSI wurde entwickelt, um Ressourcenberechtigungen für bestimmte Vorgänge zu erteilen, damit z. B. eine Ressource Metriken über sich selbst ausgeben kann. Eine Ressource (oder ihre MSI) kann die Berechtigung „Herausgeber von Überwachungsmetriken“ für eine andere Ressource erhalten, sodass die verwaltete Dienstidentität auch für andere Ressourcen Metriken ausgeben kann.
2. [AAD-Dienstprinzipal](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals): Das Szenario hier ist, dass eine AAD-Anwendung (Dienst) mit Berechtigungen versehen werden kann, um Metriken zu einer Azure-Ressource auszugeben.
Azure Monitor überprüft das Anwendungstoken mithilfe von öffentlichen AAD-Schlüsseln, um die Anforderung zu authentifizieren. Die vorhandene Rolle „Herausgeber von Überwachungsmetriken“ verfügt bereits über diese Berechtigung, die im Azure-Portal verfügbar ist. Dem Dienstprinzipal kann die Rolle „Herausgeber von Überwachungsmetriken“ im erforderlichen Gültigkeitsbereich (Abonnement, Ressourcengruppe oder bestimmte Ressource) erteilt werden, je nachdem, für welche Ressourcen er benutzerdefinierte Metriken ausgibt.

> [!NOTE]
> Wenn Sie einen AAD-Token anfordern, um benutzerdefinierte Metriken auszugeben, stellen Sie sicher, dass die Zielgruppe/Ressource, für die der Token angefordert wird, https://monitoring.azure.com/ entspricht (stellen Sie sicher, dass Sie das nachgestellte „/“ einbeziehen).

### <a name="subject"></a>Antragsteller
Diese Eigenschaft erfasst, für welche Azure-Ressourcen-ID die benutzerdefinierte Metrik gemeldet wird. Diese Informationen werden in der URL des jeweiligen API-Aufrufs codiert. Jede API kann nur metrische Werte für eine einzelne Azure-Ressource übermitteln.

> [!NOTE]
> Sie können keine benutzerdefinierten Metriken für die Ressourcen-ID einer Ressourcengruppe oder eines Abonnements ausgeben.
>
>

### <a name="region"></a>Region
Diese Eigenschaft erfasst, in welcher Azure-Region die Ressource, für die Sie Metriken senden, bereitgestellt wird. Metriken müssen an denselben regionalen Endpunkt von Azure Monitor ausgegeben werden wie die Region, in der die Ressource bereitgestellt wird. Benutzerdefinierte Metriken für eine VM, die in den USA (Westen) eingesetzt werden, müssen z. B. an den regionalen Endpunkt „WestUS“ von Azure Monitor gesendet werden. Die Regionsinformationen sind auch in der URL des API-Aufrufs codiert.

> [!NOTE]
> Während der öffentlichen Vorschau sind benutzerdefinierte Metriken nur in einer Teilmenge der Azure-Regionen verfügbar. Eine Liste der unterstützten Regionen ist in einem späteren Abschnitt dieses Artikels dokumentiert.
>
>

### <a name="timestamp"></a>Zeitstempel
Jeder Datenpunkt, der an Azure Monitor gesendet wird, muss mit einem Zeitstempel gekennzeichnet sein. Dieser Zeitstempel erfasst den Zeitpunkt (Datum/Uhrzeit), an dem der Metrikwert gemessen/erfasst wurde. Azure Monitor akzeptiert metrische Daten mit Zeitstempeln, die bis zu 20 Minuten in der Vergangenheit und bis zu 5 Minuten in der Zukunft liegen.

### <a name="namespace"></a>Namespace
Namespaces sind eine Möglichkeit, ähnliche Metriken zu kategorisieren oder zu gruppieren. Namespaces ermöglichen es Ihnen, Gruppen von Metriken zu isolieren, die unterschiedliche Erkenntnisse oder Leistungsindikatoren erfassen. Sie könnten z. B. einen Namespace namens *ContosoMemoryMetrics* verwenden, der verwendet wird, um Metriken zur Speicherbelegung zu verfolgen, die Ihre Anwendung profilieren, und einen anderen Namespace namens *ContosoAppTransaction*, der alle Metriken über Benutzertransaktionen in Ihrer Anwendung verfolgt.

### <a name="name"></a>NAME
Der Name der Metrik, die gemeldet wird. Normalerweise ist der Name ausreichend beschreibend, um zu erkennen, was gemessen wird. Eine Metrik, die die Anzahl der Bytes des auf einer bestimmten VM verwendeten Speichers misst, könnte z. B. einen metrischen Namen wie „Verwendete Arbeitsspeicherbytes“ aufweisen.

### <a name="dimension-keys"></a>Dimensionsschlüssel
Eine Dimension ist ein Schlüssel-Wert-Paar, das hilft, zusätzliche Merkmale der zu erfassenden Metrik zu beschreiben. Die zusätzlichen Merkmale ermöglichen die Erfassung weiterer Informationen über die Metrik, die umfangreichere Erkenntnisse ermöglichen. Die Metrik „Verwendete Arbeitsspeicherbytes“ könnte z. B. einen Dimensionsschlüssel namens „Prozess“ verwenden, der erfasst, wie viele Bytes des Arbeitsspeichers pro Prozess auf einer VM belegt werden. Auf diese Weise können Sie die Metrik filtern, um zu sehen, wie viele speicherspezifische Prozesse verwendet werden, oder um die fünf Prozesse mit der höchsten Speicherauslastung zu identifizieren.
Jede benutzerdefinierte Metrik kann über bis zu 10 Dimensionen verfügen.

### <a name="dimension-values"></a>Dimensionswerte
Wenn Sie einen metrischen Datenpunkt melden, gibt es für jeden Dimensionsschlüssel der zu meldenden Metrik einen entsprechenden Dimensionswert. Wenn Sie z. B. den von der ContosoApp auf Ihrer VM belegten Arbeitsspeicher melden möchten:

* Der metrische Name wäre *Verwendete Arbeitsspeicherbytes*.
* Der Dimensionsschlüssel wäre *Prozess*.
* Der Dimensionswert wäre *ContosoApp.exe*.

Wenn Sie einen Metrikwert veröffentlichen, können Sie nur einen einzigen Dimensionswert pro Dimensionsschlüssel angeben. Wenn Sie dieselbe Speicherauslastung für mehrere Prozesse auf der VM erfassen, können Sie mehrere Metrikwerte für diesen Zeitstempel melden. Jeder Metrikwert würde einen anderen Dimensionswert für den Dimensionsschlüssel „Prozess“ angeben.

### <a name="metric-values"></a>Metrikwerte
Azure Monitor speichert alle Metriken in Granularitätsintervallen von einer Minute. Uns ist bewusst, dass für eine Metrik unter Umständen mehrere Stichproben erfasst (z. B. CPU-Auslastung) oder für viele Einzelereignisse während einer bestimmten Minute gemessen werden müssen (z. B. Transaktionswartezeit während der Anmeldung) innerhalb einer angegebenen Minute. Sie können die Werte vorab lokal aggregieren und ausgeben, um die Anzahl der Rohwerte einzuschränken, die Sie in Azure Monitor ausgeben und bezahlen müssen:

* Min: Der beobachtete Mindestwert aus allen Stichproben/Messungen während der Minute.
* Max: Der beobachtete Höchstwert aus allen Stichproben/Messungen während der Minute.
* Sum: Die Summierung aller beobachteten Werte aus allen Stichproben/Messungen während der Minute.
* Count: Die Anzahl der Stichproben/Messungen, die während der Minute durchgeführt wurden.

Wenn es z. B. innerhalb einer bestimmten Minute vier Anmeldetransaktionen für Ihre App gab und sich daraus die folgenden resultierenden gemessenen Wartezeiten für jede dieser Transaktionen ergeben:

|Transaktion 1|Transaktion 2|Transaktion 3|Transaktion 4|
|---|---|---|---|
|7 ms|4 ms|13 ms|16 ms|
|

Die sich daraus ergebende metrische Veröffentlichung für Azure Monitor lautet:
* Min: 4
* Max: 16
* Sum: 40
* Count: 4

Wenn Ihre Anwendung nicht in der Lage ist, vorab die lokale Aggregierung durchzuführen und jede einzelne Stichprobe oder jedes einzelne Ereignis sofort nach der Erfassung ausgeben muss, können Sie die Rohmesswerte ausgeben.
So würden Sie z. B. jedes Mal, wenn in Ihrer App eine Anmeldetransaktion stattgefunden hat, eine Metrik mit nur einer einzigen Messung für Azure Monitor veröffentlichen. Für eine Anmeldetransaktion, die 12 ms dauerte, würde sich also die folgende metrische Veröffentlichung ergeben:
* Min: 12
* Max: 12
* Sum: 12
* Count: 1

Dieser Prozess ermöglicht es Ihnen, mehrere Werte für dieselbe Kombination von Metrik und Dimension während einer bestimmten Minute auszugeben. Azure Monitor nimmt dann alle für eine bestimmte Minute ausgesandten Rohwerte und aggregiert sie.

### <a name="sample-custom-metric-publication"></a>Beispiel einer benutzerdefinierten metrischen Veröffentlichung
Im folgenden Beispiel erstellen Sie eine benutzerdefinierte Metrik namens „Verwendete Arbeitsspeicherbytes“ unter dem metrischen Namespace „Arbeitsspeicherprofil“ für einen virtuellen Computer. Die Metrik weist eine einzelne Dimension namens „Prozess“ auf. Für den angegebenen Zeitstempel werden Metrikwerte für zwei verschiedene Prozesse ausgegeben:

```json
{
    "time": "2018-08-20T11:25:20-7:00",
    "data": {

      "baseData": {

        "metric": "Memory Bytes in Use",
        "namespace": "Memory Profile",
        "dimNames": [
          "Process"        ],
        "series": [
          {
            "dimValues": [
              "ContosoApp.exe"
            ],
            "min": 10,
            "max": 89,
            "sum": 190,
            "count": 4
          },
          {
            "dimValues": [
              "SalesApp.exe"
            ],
            "min": 10,
            "max": 23,
            "sum": 86,
            "count": 4
          }
        ]
      }
    }
  }
```
> [!NOTE]
> Application Insights, die Microsoft Azure-Diagnoseerweiterung und der InfluxData Telegraf Agent sind bereits so konfiguriert, dass sie Metrikwerte für den richtigen regionalen Endpunkt ausgeben und alle oben genannten Eigenschaften in jeder Ausgabe enthalten.
>
>

## <a name="custom-metric-definitions"></a>Benutzerdefinierte Metrikdefinitionen
Es ist nicht erforderlich, eine benutzerdefinierte Metrik in Azure Monitor vorab zu definieren, bevor sie ausgegeben wird. Da jeder veröffentlichte metrische Datenpunkt Informationen zu Namespace, Name und Dimension enthält, wird beim ersten Senden einer benutzerdefinierten Metrik an Azure Monitor automatisch eine Metrikdefinition erstellt. Diese metrische Definition ist dann für jede Ressource sichtbar, für die die Metrik über die Metrikdefinitionen ausgegeben wurde.

> [!NOTE]
> Azure Monitor unterstützt noch nicht die Definition von „Einheiten“ für eine benutzerdefinierte Metrik.

## <a name="using-custom-metrics"></a>Verwenden von benutzerdefinierten Metriken
Nachdem benutzerdefinierte Metriken an Azure Monitor übermittelt wurden, können Sie sie über das Azure-Portal durchsuchen, über die REST-APIs von Azure Monitor abfragen oder Warnungen für sie erstellen, damit Sie benachrichtigt werden können, wenn bestimmte Bedingungen erfüllt sind.
### <a name="browse-your-custom-metrics-via-the-azure-portal"></a>Durchsuchen Ihrer benutzerdefinierten Metriken über das Azure-Portal
1.  [Navigieren Sie zum Azure-Portal](https://portal.azure.com).
2.  Wählen Sie das Blatt „Überwachen“ aus.
3.  Klicken Sie auf „Metriken“.
4.  Wählen Sie eine Ressource aus, für die Sie benutzerdefinierte Metriken gesendet haben.
5.  Wählen Sie den Namespace der Metrik für Ihre benutzerdefinierte Metrik aus.
6.  Wählen Sie die benutzerdefinierte Metrik aus.

## <a name="supported-regions"></a>Unterstützte Regionen
Während der öffentlichen Vorschau ist die Möglichkeit, benutzerdefinierte Metriken zu veröffentlichen, nur in einer Teilmenge von Azure-Regionen verfügbar. Das bedeutet, dass Metriken nur für Ressourcen in einer der unterstützten Regionen veröffentlicht werden können. Die folgende Tabelle enthält die unterstützten Azure-Regionen für benutzerdefinierte Metriken und die entsprechenden Endpunkte, auf denen Metriken für Ressourcen in diesen Regionen veröffentlicht werden sollten.

|Azure-Region|Präfix des regionalen Endpunkts|
|---|---|
|USA (Ost)|https://eastus.monitoring.azure.com/|
|USA Süd Mitte|https://southcentralus.monitoring.azure.com/|
|USA, Westen-Mitte|https://westcentralus.monitoring.azure.com/|
|USA, Westen 2|https://westus2.monitoring.azure.com/|
|Asien, Südosten|https://southeastasia.monitoring.azure.com/|
|Nordeuropa|https://northeurope.monitoring.azure.com/|
|Europa, Westen|https://westeurope.monitoring.azure.com/|

## <a name="quotas-and-limits"></a>Kontingente und Grenzwerte
Azure Monitor hat die folgenden Nutzungslimits für benutzerdefinierte Metriken auferlegt.

|Category (Kategorie)|Begrenzung|
|---|---|
|Aktive Zeitreihen/Abonnements/Region|50.000|
|Dimensionsschlüssel pro Metrik|10|
|Zeichenkettenlänge für metrische Namespaces, metrische Namen, Dimensionsschlüssel und Dimensionswerte|256 Zeichen|
Eine aktive Zeitreihe ist definiert als eine beliebige eindeutige Kombination aus Metrik, Dimensionsschlüssel und Dimensionswert, deren Metrikwerte in den letzten 12 Stunden veröffentlicht wurden.

## <a name="next-steps"></a>Nächste Schritte
Verwenden benutzerdefinierter Metriken von verschiedenen Diensten 
 - [Virtueller Computer](metrics-store-custom-guestos-resource-manager-vm.md)
 - [VM-Skalierungsgruppe](metrics-store-custom-guestos-resource-manager-vmss.md)
 - [Virtueller Computer (klassisch)](metrics-store-custom-guestos-classic-vm.md)
 - [Virtueller Linux-Computer mit Telegraf Agent](metrics-store-custom-linux-telegraf.md)
 - [REST-API](metrics-store-custom-rest-api.md)
 - [Clouddienst (klassisch)](metrics-store-custom-guestos-classic-cloud-service.md)
 