---
title: Behandeln von Problemen beim Bereitstellen von OpenShift in Azure | Microsoft-Dokumentation
description: Behandeln von Problemen beim Bereitstellen von OpenShift in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: 35e554d3a9c7e7d56546ae9723c33eb59e906472
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/11/2017
---
# <a name="troubleshoot-openshift-deployment-in-azure"></a>Behandeln von Problemen beim Bereitstellen von OpenShift in Azure

Wenn Ihr OpenShift-Cluster nicht erfolgreich bereitgestellt wird, können Sie diese Aufgaben zur Problembehandlung ausprobieren, um das Problem einzugrenzen. Zeigen Sie den Bereitstellungsstatus an, und vergleichen Sie ihn anhand der folgenden Liste der Exitcodes:

- Exitcode 3: Benutzername/Kennwort oder Organisations-ID/Aktivierungsschlüssel für Ihr Red Hat-Abonnement ist falsch
- Exitcode 4: Ihre Red Hat-Pool-ID ist falsch oder es sind keine Berechtigungen verfügbar
- Exitcode 5: Docker Thin-Poolvolume konnte nicht bereitgestellt werden
- Exitcode 6: Fehler bei der OpenShift-Clusterinstallation
- Exitcode 7: OpenShift-Clusterinstallation war erfolgreich, aber Fehler bei der Konfiguration des Azure-Cloudlösungsanbieters – Problem mit Masterkonfiguration auf Masterknoten
- Exitcode 8: OpenShift-Clusterinstallation war erfolgreich, aber Fehler bei der Konfiguration des Azure-Cloudlösungsanbieters – Problem mit Knotenkonfiguration auf Masterknoten
- Exitcode 9: OpenShift-Clusterinstallation war erfolgreich, aber Fehler bei der Konfiguration des Azure-Cloudlösungsanbieters – Problem mit Knotenkonfiguration auf Infra- oder App-Knoten
- Exitcode 10: OpenShift-Clusterinstallation war erfolgreich, aber Fehler bei der Konfiguration des Azure-Cloudlösungsanbieters – Masterknoten werden korrigiert oder Master kann nicht als „nicht planbar“ festgelegt werden
- Exitcode 11: Fehler beim Bereitstellen der Metriken
- Exitcode 12: Fehler beim Bereitstellen der Protokollierung

Für die Exitcodes 7 bis 10 wurde der OpenShift-Cluster installiert, aber es ist ein Fehler bei der Konfiguration des Azure-Cloudlösungsanbieters aufgetreten. Sie können SSH für den Masterknoten (OpenShift Origin) oder den geschützten Knoten (OpenShift Container Platform) ausführen und von dort SSH für jeden der Clusterknoten ausführen, um die Probleme zu beheben.

Eine häufige Ursache für Fehler mit Exitcodes 7 bis 9 ist, dass der Dienstprinzipal nicht über die erforderlichen Berechtigungen für das Abonnement oder die Ressourcengruppe verfügt. Wenn dies das Problem ist, weisen Sie die richtigen Berechtigungen zu, und führen Sie das fehlgeschlagene Skript sowie alle nachfolgenden Skripts manuell erneut aus.

Achten Sie darauf, dass Sie den Dienst neu starten, bei dem ein Fehler aufgetreten ist (z. B. systemctl restart atomic-openshift-node.service), bevor Sie die Skripts erneut ausführen.

Führen Sie zur weiteren Problembehandlung SSH zum Masterknoten an Port 2200 (Origin) oder zum geschützten Knoten an Port 22 (Container Platform) aus. Sie müssen sich im Rootverzeichnis (sudo su) befinden und dann zum folgenden Verzeichnis navigieren: „/var/lib/waagent/custom-script/download“.

Hier sehen Sie die Ordner mit dem Namen „0“ und „1“. In jedem dieser Ordner sehen Sie zwei Dateien: „stderr“ und „stdout“. Durchsuchen Sie diese Dateien, um die Position zu ermitteln, an der der Fehler aufgetreten ist.
