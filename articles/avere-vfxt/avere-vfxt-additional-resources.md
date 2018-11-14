---
title: Weitere Links zu Avere vFXT für Azure
description: Links zu weiteren Informationen zu Avere vFXT für Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 65e764d843f9e87adee4cf94c1d22b02db80eda0
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958833"
---
# <a name="additional-documentation"></a>Zusätzliche Dokumentation

Dieser Artikel enthält Links zu zusätzlichen Dokumentationen zur Verwaltungsoberfläche der Avere-Systemsteuerung und zu verwandten Themen. 

## <a name="avere-cluster-documentation"></a>Avere-Clusterdokumentation

Weitere Dokumente zum Avere-Cluster finden Sie auf der Website unter <http://library.averesystems.com/>. Diese Dokumente können Ihnen helfen, die Funktionen des Clusters und die Konfiguration seiner Einstellungen zu verstehen. 

* Das [Handbuch zur FXT-Clustererstellung](<http://library.averesystems.com/#fxt_cluster>) ist für Cluster konzipiert, die aus physischen Hardwareknoten bestehen, aber einige Informationen im Dokument sind auch für vFXT-Cluster relevant. Insbesondere neue vFXT-Clusteradministratoren können von den folgenden Abschnitten profitieren:
  * In [Anpassen von Support- und Überwachungseinstellungen](<http://library.averesystems.com/create_cluster/4_8/html/config_support.html#config-support>) wird erläutert, wie Sie die Uploadeinstellungen des Supports anpassen und die Remoteüberwachung aktivieren. 
  * [Konfigurieren von VServern und globalen Namespaces](<http://library.averesystems.com/create_cluster/4_8/html/config_vserver.html#config-vserver>) enthält Informationen zum Erstellen eines clientseitigen Namespace.
  * In [Konfigurieren von DNS für den Avere-Cluster](<http://library.averesystems.com/create_cluster/4_8/html/config_network.html#dns-overview>) wird erläutert, wie Sie Roundrobin-DNS konfigurieren.
  * In [Hinzufügen von Back-End-Speicher](<http://library.averesystems.com/create_cluster/4_8/html/config_core_filer.html#add-core-filer>) wird beschrieben, wie Sie Kernspeichereinheiten hinzuzufügen.

* Das [Handbuch zur Clusterkonfiguration](<http://library.averesystems.com/#operations>) ist eine vollständige Referenz der Einstellungen und Optionen für einen Avere-Cluster. Ein vFXT-Cluster verwendet eine Teilmenge dieser Optionen, aber die meisten der Konfigurationsseiten gelten dennoch.

* Im [Dashboard-Handbuch](<http://library.averesystems.com/#operations>) wird erläutert, wie die Features zur Clusterüberwachung der Avere-Systemsteuerung verwendet werden.

## <a name="vfxt-creation-and-management-documentation"></a>Dokumentation zur Erstellung und Verwaltung von vFXT

Eine vollständige Anleitung zum Verwenden von vfxt.py, dem Hilfsprogramm zum Erstellen und Verwalten von Cloudclustern, finden Sie in GitHub: [Verwalten von Cloudclustern mit vfxt.py](https://github.com/Azure/AvereSDK/blob/master/docs/README.md).  
