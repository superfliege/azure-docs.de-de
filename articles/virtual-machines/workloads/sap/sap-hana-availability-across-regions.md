---
title: Verfügbarkeit von SAP HANA in verschiedenen Azure-Regionen | Microsoft-Dokumentation
description: Übersicht über Überlegungen zur Verfügbarkeit beim Ausführen von SAP HANA auf Azure-VMs
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/26/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 052394884f85d527caa88ff6c9464af669f47bb5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-across-azure-regions"></a>SAP HANA-Verfügbarkeit in den Azure-Regionen
In diesem Artikel werden Szenarien zur SAP HANA-Verfügbarkeit in unterschiedlichen Azure-Regionen beschrieben und erläutert. Da zwischen getrennten Azure-Regionen größere Abstände liegen, gibt es besondere Überlegungen, die in diesem Artikel aufgeführt sind.

## <a name="motivation-to-deploy-across-multiple-azure-regions"></a>Motivation für die Bereitstellung in mehreren Azure-Regionen
Unterschiedliche Azure-Regionen sind durch einen größere Abstand voneinander getrennt. Abhängig von der geopolitischen Region kann es sich dabei um Hunderte von Meilen oder selbst mehrere Tausend Meilen handeln, etwa in den USA. Aufgrund des Abstands zwischen den Azure-Regionen treten beim Netzwerkdatenverkehr zwischen Ressourcen in zwei verschiedenen Azure-Regionen erhebliche Roundtrip-Netzwerklatenzen auf. Diese sind so bedeutend, dass ein synchroner Datenaustausch zwischen zwei SAP HANA-Instanzen bei normaler SAP-Workload ausgeschlossen ist. Andererseits sehen Sie sich häufig der Tatsache gegenüber, dass für die Entfernung zwischen Ihrem primären Rechenzentrum und einem sekundären Rechenzentrum eine definierte Anforderung besteht, um bei Naturkatastrophen, die ein größeres Gebiet betreffen, Verfügbarkeit zu gewährleisten. Beispiele sind die Orkane, die im September und Oktober 2017 in der Karibik und im Großraum Florida auftraten. Auch in anderen Fällen liegt als Anforderung zumindest ein Mindestabstand vor. In den meisten Kundenfällen erfordert dieser definierte Mindestabstand, dass eine Verfügbarkeit in mehreren [Azure-Regionen](https://azure.microsoft.com/regions/) eingeplant werden muss. Da die Entfernung zwischen zwei Azure-Regionen zu groß für die Verwendung des synchronen Replikationsmodus von HANA ist, sind Sie durch die Anforderungen von RTO und RPO möglicherweise gezwungen, Verfügbarkeitskonfigurationen in einer Region bereitzustellen und diese dann um weitere Bereitstellungen in einer zweiten Region zu ergänzen.

Ein weiterer Aspekt, der bei diesen Konfigurationen berücksichtigt werden muss, betrifft Failover und die Clientumleitung. Es kann angenommen werden, dass ein Failover zwischen SAP HANA-Instanzen in zwei verschiedenen Azure-Regionen immer manuell erfolgen. Da der Replikationsmodus für die SAP HANA-Systemreplikation auf asynchron festgelegt ist, ist es möglich, dass die in der primären HANA-Instanz committeten Daten noch nicht bei der sekundären HANA-Instanz angekommen sind. Aus diesem Grund kann automatisches Failover für Konfigurationen mit asynchroner Replikation nicht akzeptiert werden. Selbst bei manuell kontrolliertem Failover, etwa bei einer Failoverübung, müssen Sie sicherstellen, dass alle auf der Primärseite committeten Daten die sekundären Instanz erreichen, bevor manuell zur anderen Azure-Region übergegangen wird.
 
Da Sie in den Azure-VNETs, die in der zweiten Azure-Region bereitgestellt werden, mit einem anderen IP-Adressbereich arbeiten, muss entweder die Konfiguration der SAP HANA-Clients geändert werden, oder Sie müssen bevorzugt Schritte einrichten, um die Namensauflösung zu ändern, sodass die Clients an die IP-Adresse des neuen sekundären Servers umgeleitet werden. Der SAP-Artikel unter [Wiederherstellung der Clientverbindung nach einer Übernahme](https://help.sap.com/doc/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c93a723ceedc45da9a66ff47672513d3.html) beschreibt dies ausführlicher.   

## <a name="simple-availability-between-two-azure-regions"></a>Einfache Verfügbarkeit zwischen zwei Azure-Regionen
In diesem Szenario haben Sie sich entschieden, keine Verfügbarkeitskonfiguration in einer einzelnen Region einzurichten. Es besteht jedoch die Anforderung, dass die Workload bei einem Notfall verarbeitet werden muss. Einen typischen Fall für Systeme dieser Art bilden Nicht-Produktionssysteme. Sie können zwar einen Systemausfall für einen halben oder sogar einen ganzen Tag tolerieren, aber keinesfalls zulassen, dass das System 48 Stunden oder länger nicht verfügbar ist. Damit die Einrichtung nicht zu kostenaufwendig wird, führen Sie auf dem virtuellen Computer ein anderes, noch weniger wichtiges System aus, das als Ziel fungiert, oder Sie geben dem virtuellen Computer in der sekundären Region eine geringere Größe und legen fest, Daten nicht vorab zu laden. Da das Failover manuell erfolgt und viele weitere Schritte umfasst, um auch ein Failover des vollständigen Anwendungsstapels auszuführen, ist die zusätzliche Zeit zum Herunterfahren des virtuellen Computers, Ändern seiner Größe und Neustarten des virtuellen Computers akzeptabel.

> [!NOTE]
> Auch ohne Daten vorab in das HANA-Systemreplikationsziel zu laden, benötigen Sie mindestens 64 GB Speicher und darüber hinaus ausreichend Speicher, um die Rowstore-Daten im Speicher der Zielinstanz beizubehalten.

![Zwei virtuelle Computer über zwei Regionen](./media/sap-hana-availability-two-region/two_vm_HSR_async_2regions_nopreload.PNG)

> [!NOTE]
> In dieser Konfiguration können Sie nicht RPO = 0 angeben, da Ihr HANA System im asynchronen Modus repliziert wird. Wenn Sie RPO = 0 bereitstellen müssen, ist dies nicht die Konfiguration Ihrer Wahl.

Eine kleine Änderung der Konfiguration könnte darin bestehen, das Vorabladen von Daten zu konfigurieren. Angesichts der manuellen Ausführung des Failovers und der Tatsache, dass Anwendungsebenen ebenfalls in die zweite Region verschoben werden müssen, ist es jedoch möglicherweise nicht sinnvoll, Daten vorab zu laden. 

## <a name="combine-availability-within-one-region-and-across-regions"></a>Kombinieren von Verfügbarkeit in einer Region und regionsübergreifender Verfügbarkeit 
Eine Kombination der regionsinternen und regionsübergreifenden Verfügbarkeit kann folgendermaßen motiviert sein:

- Voraussetzung für RPO = 0 in einer Azure-Region.
- Unzulässigkeit, dass der globale Betrieb des Unternehmens von einer großen Naturkatastrophe, die eine größere Region betrifft, eingeschränkt wird. Beispiele sind die Orkane, die in den letzten Jahren in der Karibik auftraten.
- Forderungen von Abständen zwischen primären und sekundären Standorten durch Vorschriften, die eindeutig die Möglichkeiten von Azure-Verfügbarkeitszonen überschreiten.

 
In solchen Fällen konfigurieren Sie in der Terminologie von SAP eine [SAP HANA-Multitier-Systemreplikationskonfiguration](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/ca6f4c62c45b4c85a109c7faf62881fc.html) mit HANA-Systemreplikation. Die Architektur sieht folgendermaßen aus:

![Drei virtuelle Computer über zwei Regionen](./media/sap-hana-availability-two-region/three_vm_HSR_async_2regions_ha_and_dr.PNG)

Diese Konfiguration bietet RPO = 0 mit kleinen RTO-Zeiten in der primären Region und außerdem ein sinnvolles RPO bei einem Wechsel in die zweite Region. Die RTO-Zeiten in der zweiten Region sind davon abhängig, ob Sie das Vorabladen von Daten konfigurieren. In vielen Kundenfällen wird auf dem virtuellen Computer in der sekundären Region ein Testsystem ausgeführt. Aufgrund dieser Nutzung können die Daten nicht vorab geladen werden.

> [!NOTE]
> Da Sie für die HANA-Systemreplikation von Ebene 1 auf Ebene 2 (synchrone Replikation in der primären Region) den Betriebsmodus für Protokollwiedergabe verwenden, darf die Replikation zwischen Ebene 2 und Ebene 3 (Replikation zum sekundären Standort) nicht im Betriebsmodus delta_datashipping ausgeführt werden. Details zu Betriebsmodi und einigen Einschränkungen sind durch SAP unter [Operation Modes for SAP HANA System Replication](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/627bd11e86c84ec2b9fcdf585d24011c.html) (Betriebsmodi für die SAP HANA Systemreplikation) dokumentiert. 

## <a name="next-steps"></a>Nächste Schritte
Eine ausführliche Anleitung zur Einrichtung einer solchen Konfiguration in Azure finden Sie in folgenden Artikeln:

- [Einrichten der SAP HANA-Systemreplikation auf virtuellen Azure-Computern](sap-hana-high-availability.md)
- [Your SAP on Azure – Part 4 – High Availability for SAP HANA using System Replication](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/) (SAP für Azure – Teil 4 – Hochverfügbarkeit für SAP HANA mit Systemreplikation)

 



 
  
