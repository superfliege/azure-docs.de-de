---
title: Testen der interaktiven Featureüberprüfung mit Validation-as-a-Service in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Tests für die interaktive Featureüberprüfung für Azure Stack mit Validation-as-a-Service erstellen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: dc0c6a37e11574cef857a7af773a9d90bea57357
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/06/2018
ms.locfileid: "52972064"
---
# <a name="interactive-feature-verification-testing"></a>Testen der interaktiven Featureüberprüfung  

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Mit dem Testframework für die interaktive Featureüberprüfung können Sie Tests für Ihr System anfordern. Wenn Sie einen Test anfordern, bereitet Microsoft mithilfe des Frameworks Tests vor, die manuelle interaktive Schritte erfordern. Über das Framework kann Microsoft mehrere eigenständige automatisierte Tests verketten.

In diesem Artikel wird ein einfaches manuelles Szenario beschrieben. Der Test überprüft das Ersetzen eines Datenträgers in Azure Stack. Das Framework erfasst bei jedem Schritt Diagnoseprotokolle. Sie können Probleme debuggen, sobald sie gefunden werden. Das Framework ermöglicht auch die Weitergabe von Protokollen, die von anderen Tools oder Prozessen erzeugt wurden, sowie die Abgabe von Feedback für das Szenario.

## <a name="overview-of-a-test-pass"></a>Übersicht über einen Testdurchlauf

Ein Test für den Austausch eines Datenträgers ist ein gängiges Szenario. In diesem Beispiel umfasst der Test sieben Schritte:

1.  Erstellen eines neuen Workflows vom Typ **Testdurchlauf**
2.  Auswählen von **Disk Identification Test** (Datenträgeridentifizierungstest)
3.  Starten des Tests
4.  Auswählen der Aktionen im interaktiven Überprüfungsszenario
5.  Überprüfen des Ergebnisses des Szenarios
6.  Senden des Testergebnisses an Microsoft
7.  Überprüfen des Status im VaaS-Portal

## <a name="create-a-new-test-pass"></a>Erstellen eines neuen Testdurchlaufs

1.  Navigieren Sie zum [Azure Stack-Überprüfungsportal](https://www.azurestackvalidation.com), und melden Sie sich an.

2.  Erstellen Sie eine neue Lösung, oder wählen Sie eine bereits vorhandene Lösung aus.

3.  Wählen Sie auf der Kachel **Testdurchlauf** die Option **Starten** aus.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image1.png)

4.  Geben Sie einen Namen für den Workflow **Testdurchlauf** ein.

5.  Geben Sie die Umgebung sowie allgemeine Testparameter gemäß den Anweisungen im Artikel [Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack](azure-stack-vaas-parameters.md) ein.

6.  Die Diagnoseverbindungszeichenfolge muss dem Format entsprechen, das im Abschnitt [Testparameter](azure-stack-vaas-parameters.md#test-parameters) des Artikels [Allgemeine Workflowparameter für Validation-as-a-Service in Azure Stack](azure-stack-vaas-parameters.md) angegeben ist.

7.  Geben Sie die erforderlichen Parameter für den Test ein.

8.  Wählen Sie den Agent für die Ausführung des interaktiven Testlaufs aus.

> [!Note]  
> Zum Testen der interaktiven Featureüberprüfung für die Datenträgeridentifizierung müssen Domänenadministratorbenutzer und Kennwort angegeben werden.

![Alt text](media/azure-stack-vaas-interactive-feature-verification/image2.png)

## <a name="select-the-test"></a>Auswählen des Tests

1.  Wählen Sie **Disk Identification Test\<Version>** (Datenträgeridentifizierungstest<Version>) aus.

    > [!Note]  
    > Die Version des Tests erhöht sich, wenn Verbesserungen am Testbegleitmaterial vorgenommen werden. Sofern von Microsoft nicht anders angegeben, sollte immer die höchste Version verwendet werden.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image4.png)

2.  Wählen Sie **Bearbeiten** aus, um den Domänenadministratorbenutzer und das Kennwort anzugeben.

3.  Wählen Sie den passenden Testausführungs-Agent/die DVM aus, auf der der Test gestartet werden soll.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image5.png)

4.  Wählen **Übermitteln** aus, um den Test zu starten.

![Alt text](media/azure-stack-vaas-interactive-feature-verification/image6.png)

## <a name="start-the-test"></a>Starten des Tests

Die Aufforderungen des Tests für die Datenträgeridentifizierung werden auf dem Computer angezeigt, der den VaaS-Agent ausführt. Hierbei handelt es sich in der Regel um die DVM oder Jumpbox für die Azure Stack-Instanz.

![Alt text](media/azure-stack-vaas-interactive-feature-verification/image8.png)

## <a name="choose-the-actions"></a>Auswählen der Aktionen

1.  Unter den Links **Dokumentation** und **Überprüfung** finden Sie Anweisungen von Microsoft für dieses Szenario.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image9.png)

2.  Klicken Sie auf **Weiter**.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image10.png)

3.  Befolgen Sie die Anweisungen, um das Skript für die Vorüberprüfung auszuführen.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image11.png)

4.  Führen Sie nach erfolgreicher Ausführung des Skripts für die Vorüberprüfung das manuelle Szenario (Datenträgeraustausch) gemäß den Links **Dokumentation** und **Überprüfung** auf der Registerkarte **Informationen** durch.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image12.png)

5.  Lassen Sie das Dialogfeld während der Durchführung des manuellen Szenarios geöffnet.

6.  Folgen Sie nach Abschluss des manuellen Szenarios den Anweisungen zum Ausführen des Skripts für die Nachüberprüfung.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image13.png)

7.  Wählen Sie nach erfolgreichem Abschluss des manuellen Szenarios (Datenträgeraustausch) die Option **Weiter** aus.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image14.png)

> [!Important]  
> Wenn Sie das Fenster schließen, wird der Test vorzeitig beendet.

## <a name="check-the-status"></a>Überprüfen des Status

1.  Nach Abschluss des Tests werden Sie um Feedback gebeten.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image15.png)

2.  Diese Fragen helfen Microsoft dabei, die Erfolgsrate und die Veröffentlichungsqualität des Szenarios zu beurteilen.

## <a name="send-the-test-result"></a>Senden des Testergebnisses

1.  Fügen Sie alle Protokolldateien an, die Sie an Microsoft übermitteln möchten.

    ![Alt text](media/azure-stack-vaas-interactive-feature-verification/image16.png)

2.  Akzeptieren Sie die Lizenzbedingungen für die Feedbackübermittlung.

3.  Wählen Sie **Übermitteln** aus, um die Ergebnisse an Microsoft zu senden.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachen und Verwalten von Tests im VaaS-Portal](azure-stack-vaas-monitor-test.md)
