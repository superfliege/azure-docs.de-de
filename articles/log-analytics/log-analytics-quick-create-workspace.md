---
title: Erstellen eines Arbeitsbereichs in Azure Log Analytics | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie einen Log Analytics-Arbeitsbereich erstellen, um Verwaltungslösungen und die Datensammlung in Ihren cloudbasierten und lokalen Umgebungen zu aktivieren."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: magoedte
ms.openlocfilehash: 8259a97d28effa7bfa9cfb9d7cd9cd2a14c9d906
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Erstellen eines Log Analytics-Arbeitsbereichs im Azure-Portal
Im Azure-Portal können Sie einen Log Analytics-Arbeitsbereich einrichten. Dabei handelt es sich um eine einzigartige Log Analytics-Umgebung mit eigenem Datenrepository, eigenen Datenquellen und eigenen Lösungen.  Die in diesem Artikel beschriebenen Schritte sind erforderlich, wenn Sie Daten aus den folgenden Quellen sammeln möchten:

* Azure-Ressourcen im Abonnement
* Lokale Computer, die von System Center Operations Manager überwacht werden
* Gerätesammlungen aus System Center Configuration Manager 
* Diagnose- oder Protokolldaten aus dem Azure-Speicher

Informationen zu anderen Quellen in Ihrer Umgebung (etwa virtuelle Azure-Computer und Windows- oder Linux-Computer) finden Sie in folgenden Themen:

*  [Sammeln von Daten über virtuelle Azure-Computer](log-analytics-quick-collect-azurevm.md) 
*  [Sammeln von Daten von Linux-Computern, die in Ihrer Umgebung gehostet werden](log-analytics-quick-collect-linux-computer.md)
*  [Sammeln von Daten von Windows-Computern, die in Ihrer Umgebung gehostet werden](log-analytics-quick-collect-windows-computer.md)

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure-portal"></a>Anmelden beim Azure-Portal
Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) im Azure-Portal an. 

## <a name="create-a-workspace"></a>Erstellen eines Arbeitsbereichs
1. Klicken Sie im Azure-Portal unten links auf **Weitere Dienste**. Geben Sie in der Liste mit den Ressourcen **Log Analytics** ein. Sobald Sie mit der Eingabe beginnen, wird die Liste auf der Grundlage Ihrer Eingabe gefiltert. Wählen Sie **Log Analytics**.<br><br> ![Azure-Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Klicken Sie auf **Erstellen**, und wählen Sie anschließend Optionen für die folgenden Elemente aus:

  * Geben Sie einen Namen für den neuen **OMS-Arbeitsbereich** ein, wie z.B. *DefaultLAWorkspace*. 
  * Wählen Sie ein **Abonnement** aus, mit dem eine Verknüpfung erstellt werden soll, indem Sie in der Dropdownliste einen anderen Eintrag auswählen, falls der Standardeintrag nicht geeignet ist.
  * Wählen Sie für **Ressourcengruppe** eine vorhandene und bereits eingerichtete Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe.  
  * Wählen Sie einen verfügbaren **Standort** aus.  Weitere Informationen finden Sie auf der Seite zur [Verfügbarkeit von Log Analytics in den einzelnen Regionen](https://azure.microsoft.com/regions/services/).
  * In Log Analytics können Sie zwischen drei verschiedenen **Tarifen** wählen, aber wählen Sie für diesen Schnellstart den **kostenlosen** Tarif.  Weitere Informationen zu den einzelnen Tarifen finden Sie unter [Log Analytics – Preise](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-01.png)<br>  
3. Klicken Sie nach dem Bereitstellen der erforderlichen Informationen im Bereich **OMS-Arbeitsbereich** auf **OK**.  

Die Informationen werden überprüft, und der Arbeitsbereich wird erstellt. Sie können den Fortschritt im Menü unter **Benachrichtigungen** nachverfolgen. 

## <a name="next-steps"></a>Nächste Schritte
Sie besitzen einen verfügbaren Arbeitsbereich und können nun die Erfassung von Überwachungstelemetrie konfigurieren, Protokollsuchen zum Analysieren dieser Daten ausführen und eine Verwaltungslösung hinzufügen, um weitere daten- und analysebasierte Einblicke zu gewinnen. 

* Informationen zum Aktivieren der Datensammlung aus Azure-Ressourcen mit Azure-Diagnose oder Azure Storage finden Sie unter [Sammeln von Azure-Dienstprotokollen und Metriken zur Verwendung in Log Analytics](log-analytics-azure-storage.md).  
* [Fügen Sie System Center Operations Manager als Datenquelle hinzu](log-analytics-om-agents.md), um Daten von Agents zu sammeln, die an die Operations Manager-Verwaltungsgruppe berichten, und sie im Log Analytics-Arbeitsbereichsrepository zu speichern. 
* Stellen Sie eine Verbindung mit [Configuration Manager](log-analytics-sccm.md) her, um Computer zu importieren, die Sammlungen in der Hierarchie angehören.  
* Sehen Sie sich die verfügbaren [Verwaltungslösungen](/log-analytics-add-solutions.md) sowie Informationen dazu an, wie Sie eine Lösung zu Ihrem Arbeitsbereich hinzufügen bzw. daraus entfernen.

