---
title: Behandeln von Problemen beim Bereitstellen von OpenShift in Azure | Microsoft-Dokumentation
description: Behandeln von Problemen beim Bereitstellen von OpenShift in Azure
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
ms.openlocfilehash: ea3664870480f6ed170972a5f52940dc4a852219
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshooting-openshift-deployment-in-azure"></a>Behandeln von Problemen beim Bereitstellen von OpenShift in Azure

Wenn der OpenShift-Cluster nicht erfolgreich bereitgestellt wird, können einige Aufgaben zur Problembehandlung ausgeführt werden können, um das Problem einzugrenzen. Zeigen Sie den Bereitstellungsstatus an, und vergleichen Sie ihn anhand der folgenden Liste der Exitcodes.

- Exitcode 3: Benutzername/Kennwort oder Organisations-ID/Aktivierungsschlüssel für Ihr Red Hat-Abonnement ist falsch
- Exitcode 4: Ihre Red Hat-Pool-ID ist falsch oder es sind keine Berechtigungen verfügbar
- Exitcode 5: Docker Thin-Poolvolume konnte nicht bereitgestellt werden
- Exitcode 6: Fehler bei der OpenShift-Clusterinstallation
- Exitcode 7: OpenShift-Clusterinstallation war erfolgreich, aber Fehler bei der Konfiguration des Azure-Cloudanbieters – Problem mit Masterkonfiguration auf Masterknoten
- Exitcode 8: OpenShift-Clusterinstallation war erfolgreich, aber Fehler bei der Konfiguration des Azure-Cloudanbieters – Problem mit Knotenkonfiguration auf Masterknoten
- Exitcode 9: OpenShift-Clusterinstallation war erfolgreich, aber Fehler bei der Konfiguration des Azure-Cloudanbieters – Problem mit Knotenkonfiguration auf Infra- oder App-Knoten
- Exitcode 10: OpenShift-Clusterinstallation war erfolgreich, aber Fehler bei der Konfiguration des Azure-Cloudanbieters – Masterknoten werden korrigiert oder Master kann nicht als „nicht planbar“ festgelegt werden
- Exitcode: 11: Fehler beim Bereitstellen der Metriken
- Exitcode: 12: Fehler beim Bereitstellen der Protokollierung

Für die Exitcodes 7 bis 10 wurde der OpenShift-Cluster installiert, aber es ist ein Fehler bei der Konfiguration des Azure-Cloudanbieters aufgetreten. Sie können SSH für den Masterknoten (Ursprung) oder geschützten Knoten (Containerplattform) ausführen und von dort SSH für jeden der Knoten im Cluster ausführen und die Probleme beheben.

Eine häufige Ursache für Fehler mit Exitcodes 7 bis 9 ist, dass der Dienstprinzipal nicht über die erforderlichen Berechtigungen für das Abonnement oder die Ressourcengruppe verfügt. Wenn dies tatsächlich das Problem ist, weisen Sie die richtigen Berechtigungen zu, und führen Sie das fehlgeschlagene Skript in allen nachfolgenden Skripts manuell erneut aus.
Achten Sie darauf, dass Sie den Dienst neu starten, bei dem ein Fehler aufgetreten ist (z. B. systemctl restart atomic-openshift-node.service), bevor Sie die Skripts erneut ausführen.

Führen Sie zur weiteren Problembehandlung SSH zum Masterknoten an Port 2200 (Ursprung) oder zum geschützten Knoten an Port 22 (Container-Plattform) aus. Sie müssen Root-Benutzer (sudo su) sein und dann zum folgenden Verzeichnis navigieren: /var/lib/waagent/custom-script/download

Es sollten Ordner namens „0“ und „1“ angezeigt werden. In jedem dieser Ordner sehen Sie zwei Dateien: „stderr“ und „stdout“. Sie können diese Dateien durchsuchen, um die Position zu ermitteln, an der der Fehler aufgetreten ist.
