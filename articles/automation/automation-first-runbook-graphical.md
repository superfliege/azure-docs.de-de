---
title: Mein erstes grafisches Runbook in Azure Automation | Microsoft Docs
description: "Tutorial, in dem Sie sich mit dem Erstellen, Testen und Veröffentlichen eines einfachen grafischen Runbooks vertraut machen können."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
keywords: Runbooks, Runbookvorlage, Runbookautomatisierung, Azure-Runbook
ms.assetid: dcb88f19-ed2b-4372-9724-6625cd287c8a
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/17/2017
ms.author: magoedte;bwren
ms.openlocfilehash: ab4ca2bf37e30bff66de7acc3932e3cb89b6e93b
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/14/2017
---
# <a name="my-first-graphical-runbook"></a>Mein erstes grafisches Runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-Workflow](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 

In diesem Tutorial werden die Schritte zur Erstellung eines [grafischen Runbooks](automation-runbook-types.md#graphical-runbooks) in Azure Automation beschrieben. Wir beginnen mit einem einfachen Runbook, das getestet und veröffentlicht wird. Dabei erfahren Sie, wie Sie den Status des Runbookauftrags nachverfolgen. Anschließend wird das Runbook geändert, um damit tatsächlich Azure-Ressourcen zu verwalten. Im vorliegenden Fall soll ein virtueller Azure-Computer gestartet werden. Zum Abschluss des Tutorials werden Runbookparameter und bedingte Verknüpfungen hinzugefügt, um die Stabilität des Runbooks zu erhöhen.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Azure-Abonnement. Wenn Sie noch kein Abonnement haben, können Sie Ihre [MSDN-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oder sich für ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) registrieren.
* [Automation-Konto](automation-offering-get-started.md) dient zur Aufbewahrung des Runbooks und zur Authentifizierung gegenüber Azure-Ressourcen. Dieses Konto muss über die Berechtigung zum Starten und Beenden des virtuellen Computers verfügen.
* Einen virtuellen Azure-Computer. Da dieser Computer gestartet und beendet wird, sollte es sich nicht um einen virtuellen Computer in der Produktionsumgebung handeln.

## <a name="step-1---create-runbook"></a>Schritt 1: Erstellen eines Runbooks
Erstellen Sie zunächst ein einfaches Runbook, das den Text *Hello World* ausgibt.

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.

   Die Seite des Automation-Kontos bietet einen kurzen Überblick über die Ressourcen des Kontos. Sie sollten bereits über einige Objekte verfügen. Bei den meisten Objekten handelt es sich um die Module, die automatisch in einem neuen Automation-Konto enthalten sind. Darunter müsste sich auch das in den [Voraussetzungen](#prerequisites)erwähnte Anmeldeinformationsobjekt befinden.

2. Klicken Sie unter **PROZESSVERWALTUNG** auf **Runbooks**, um die Liste mit den Runbooks zu öffnen.
3. Erstellen Sie ein neues Runbook, indem Sie auf die Schaltfläche **Runbook hinzufügen** und anschließend auf **Neues Runbook erstellen** klicken.
4. Nennen Sie das Runbook *MyFirstRunbook-Graphical*.
5. Da Sie in diesem Fall ein [grafisches Runbook](automation-graphical-authoring-intro.md) erstellen, legen Sie den **Runbooktyp** auf **Grafisch** fest.<br> ![Neues Runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Klicken Sie auf **Erstellen** , um das Runbook zu erstellen und den grafischen Editor zu öffnen.

## <a name="step-2---add-activities-to-the-runbook"></a>Schritt 2: Hinzufügen von Aktivitäten zum Runbook
Mithilfe des Bibliotheksteuerelements auf der linken Seite des Editors können Sie Aktivitäten auswählen und Ihrem Runbook hinzufügen. Sie fügen ein **Write-Output**-Cmdlet hinzu, um Text aus dem Runbook auszugeben.

1. Klicken Sie im Bibliotheksteuerelement auf das Textfeld für die Suche, und geben Sie **Write-Output**ein. Die folgende Abbildung zeigt die Suchergebnisse: <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
2. Führen Sie einen Bildlauf zum Ende der Liste durch. Sie können entweder mit der rechten Maustaste auf **Write-Output** klicken und **Zur Canvas hinzufügen** auswählen oder auf die Auslassungspunkte neben dem Cmdlet klicken und **Zur Canvas hinzufügen** auswählen.
3. Klicken Sie in der Canvas auf die Aktivität **Write-Output**. Dadurch wird das Blatt für die Konfigurationssteuerung geöffnet, auf dem Sie die Aktivität konfigurieren können.
4. Die **Bezeichnung** ist standardmäßig auf den Namen des Cmdlets festgelegt, kann aber in einen aussagekräftigeren Wert geändert werden. Legen Sie ihn auf *Write Hello World to output*fest.
5. Klicken Sie auf **Parameter** , um Parameterwerte für das Cmdlet anzugeben.

   Einige Cmdlets verfügen über mehrere Parametersätze, und Sie müssen auswählen, welchen Sie verwenden möchten. **Write-Output** besitzt in diesem Fall lediglich einen einzelnen Parametersatz, sodass Sie keinen auswählen müssen. <br> ![Write-Output-Eigenschaften](media/automation-first-runbook-graphical/write-output-properties-b.png)

6. Wählen Sie den **InputObject** -Parameter aus. Dieser Parameter dient zum Angeben des Texts, der an den Ausgabestream gesendet werden soll.
7. Wählen Sie im Dropdownfeld **Datenquelle** die Option **PowerShell-Ausdruck** aus. Das Dropdownfeld **Datenquelle** enthält verschiedene Quellen, die Sie zum Auffüllen eines Parameterwerts verwenden.

   Sie können Ausgaben von solchen Quellen (etwa eine andere Aktivität, ein Automation-Objekt oder ein PowerShell-Ausdruck) verwenden. In diesem Fall soll lediglich *Hello World* ausgegeben werden. Hierzu können Sie einen PowerShell-Ausdruck verwenden und eine Zeichenfolge angeben.

8. Geben Sie im Feld **Ausdruck** die Zeichenfolge *"Hello World"* ein, und klicken Sie dann zweimal auf **OK**, um zum Zeichenbereich zurückzukehren.<br> ![PowerShell Expression](media/automation-first-runbook-graphical/expression-hello-world.png)
9. Klicken Sie auf **Speichern**, um das Runbook zu speichern.<br> ![Runbook speichern](media/automation-first-runbook-graphical/runbook-toolbar-save-revised20165.png)

## <a name="step-3---test-the-runbook"></a>Schritt 3: Testen des Runbooks
Bevor Sie das Runbook für die Verwendung in der Produktionsumgebung veröffentlichen, sollten Sie sich vergewissern, dass es ordnungsgemäß funktioniert. Beim Testen eines Runbooks führen Sie die **Entwurfsversion** des Runbooks aus und sehen sich interaktiv die Ausgabe an.

1. Klicken Sie auf **Testbereich**, um das Blatt „Test“ zu öffnen.<br> ![Testbereich](media/automation-first-runbook-graphical/runbook-toolbar-test-revised20165.png)
2. Klicken Sie auf **Starten** , um den Test zu starten. Andere Optionen dürften nicht zur Verfügung stehen.
3. Ein [Runbookauftrag](automation-runbook-execution.md) wird erstellt, und der dazugehörige Status wird angezeigt.

   Der Auftrag besitzt zunächst den Status *In der Warteschlange*. Hiermit wird angegeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.

4. Nach Abschluss des Runbookauftrags wird die Ausgabe angezeigt. In diesem Fall wird *Hello World* angezeigt.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
5. Schließen Sie das Testblatt, um zum Zeichenbereich zurückzukehren.

## <a name="step-4---publish-and-start-the-runbook"></a>Schritt 4: Veröffentlichen und Starten des Runbooks
Das erstellte Runbook befindet sich immer noch im Entwurfsmodus. Es muss veröffentlicht werden, damit es in der Produktionsumgebung ausgeführt werden kann. Beim Veröffentlichen eines Runbooks wird die vorhandene veröffentlichte Version durch die Entwurfsversion überschrieben. In diesem Fall ist noch keine veröffentlichte Version vorhanden, da Sie das Runbook gerade erst erstellt haben.

1. Klicken Sie auf **Veröffentlichen**, um das Runbook zu veröffentlichen, und bestätigen Sie den Vorgang mit **Ja**.<br> ![Veröffentlichen](media/automation-first-runbook-graphical/runbook-toolbar-publish-revised20166.png)
2. Wenn Sie nach links scrollen, um das Runbook auf dem Blatt **Runbooks** anzuzeigen, zeigt das Runbook **Veröffentlicht** als **Erstellungsstatus**.
3. Führen Sie wieder einen Bildlauf nach rechts durch, um das Blatt für **MyFirstRunbook**anzuzeigen.

   Mit den Optionen am oberen Rand können wir das Runbook starten, den Start für einen späteren Zeitpunkt planen oder einen [Webhook](automation-webhooks.md) erstellen, um den Start über einen HTTP-Aufruf zu ermöglichen.

4. Klicken Sie zum Starten des Runbooks auf **Starten** und anschließend auf **Ja**.<br> ![Runbook starten](media/automation-first-runbook-graphical/runbook-controls-start-revised20165.png)
5. Für den erstellten Runbookauftrag wird ein Auftragsblatt geöffnet. Vergewissern Sie sich, dass der **Auftragsstatus** den Wert **Abgeschlossen** hat.
7. Wenn der Runbookstatus *Abgeschlossen*lautet, klicken Sie auf **Ausgabe**. Das Blatt **Ausgabe** wird geöffnet, und der Text *Hello World* wird in dem Bereich angezeigt.
8. Schließen Sie das Ausgabeblatt.
9. Klicken Sie auf **Alle Protokolle**, um das Blatt „Datenströme“ für den Runbookauftrag zu öffnen. Im Ausgabedatenstrom sollte nur *Hello World* angezeigt werden. Hier können aber auch andere Datenströme für einen Runbookauftrag (wie etwa „Ausführlich“ und „Fehler“) angezeigt werden, sofern das Runbook Schreibvorgänge dafür durchführt.
10. Schließen Sie die Blätter „Alle Protokolle“ und „Auftrag“, um zum Blatt „MyFirstRunbook“ zurückzukehren.
11. Wenn Sie alle Aufträge für das Runbook anzeigen möchten, schließen Sie das Blatt **Auftrag**, und klicken Sie unter **RESSOURCEN** auf **Aufträge**. Dadurch werden alle von diesem Runbook erstellten Aufträge aufgeführt. Hier ist nur ein einzelner Auftrag aufgeführt, da Sie den Auftrag bislang erst einmal ausgeführt haben.
12. Wenn Sie auf diesen Auftrag klicken, wird wieder der Auftragsbereich geöffnet, den Sie sich beim Starten des Runbooks angesehen haben. So können Sie bereits ausgeführte Aufträge öffnen und Details zu jedem Auftrag anzeigen, der für ein bestimmtes Runbook erstellt wurde.

## <a name="step-5---create-variable-assets"></a>Schritt 5: Erstellen variabler Ressourcen
Sie haben Ihr Runbook inzwischen zwar getestet und veröffentlicht, bislang ist es aber noch nicht sonderlich hilfreich. Sie möchten damit ja eigentlich Azure-Ressourcen verwalten. Bevor Sie das Runbook für die Authentifizierung konfigurieren, erstellen Sie eine Variable zum Speichern der Abonnement-ID, auf die Sie nach dem Einrichten der Authentifizierungsaktivität in Schritt 6 verweisen können. Das Einfügen eines Verweises in den Abonnementkontext ermöglicht Ihnen die einfache Nutzung mehrerer Abonnements. Kopieren Sie Ihre Abonnement-ID aus der Option „Abonnements“ im Navigationsbereich, bevor Sie fortfahren.

1. Klicken Sie auf dem Blatt „Automatisierungskonten“ unter **FREIGEGEBENE RESSOURCEN** auf **Variablen**.
2. Klicken Sie auf **Variable hinzufügen**.<br>![Automation-Variable](media/automation-first-runbook-graphical/create-new-subscriptionid-variable.png)
4. Geben Sie auf dem Blatt „Neue Variable“ im Feld **Name** die Zeichenfolge **AzureSubscriptionId** und im Feld **Wert** Ihre Abonnement-ID ein. Behalten Sie unter **Typ** die Option *Zeichenfolge* sowie den Standardwert für **Verschlüsselung** bei.  
5. Klicken Sie auf **Erstellen** , um die Variable zu erstellen.  

## <a name="step-6---add-authentication-to-manage-azure-resources"></a>Schritt 6: Hinzufügen von Authentifizierungsfunktionen für die Verwaltung von Azure-Ressourcen
Nachdem Sie nun über eine Variable für Ihre Abonnement-ID verfügen, können Sie Ihr Runbook für die Authentifizierung mit den Anmeldeinformationen für das ausführende Konto konfigurieren, die unter [Voraussetzungen](#prerequisites) angegeben sind. Fügen Sie hierzu der Canvas das **Objekt** für die Verbindung mit dem ausführenden Azure-Konto und das Cmdlet **Add-AzureRMAccount** hinzu.  

1. Navigieren Sie wieder zu Ihrem Runbook, und klicken Sie auf dem Blatt „MyFirstRunbook“ auf **Bearbeiten**.<br> ![Runbook bearbeiten](media/automation-first-runbook-graphical/runbook-controls-edit-revised20165.png)
2. **Write Hello World to output** wird nicht mehr benötigt. Klicken Sie daher auf die Auslassungspunkte und anschließend auf **Löschen**.
3. Erweitern Sie im Bibliotheksteuerelement die Optionen **OBJEKTE** und **Verbindungen**, und fügen Sie der Canvas das Element **AzureRunAsConnection** hinzu, indem Sie auf **Zur Canvas hinzufügen** klicken.
5. Geben Sie im Bibliotheksteuerelement in das Textfeld für die Suche die Zeichenfolge **Add-AzureRmAccount** ein.
6. Fügen Sie **Fügen Sie-AzureRmAccount** dem Zeichenbereich hinzu.<br> ![Add-AzureRMAccount](media/automation-first-runbook-graphical/search-powershell-cmdlet-addazurermaccount.png)
7. Zeigen Sie auf **Get Run As Connection** , bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil zu **Add-AzureRmAccount**. Bei dem soeben erstellten Pfeil handelt es sich um eine *Verknüpfung*. Das Runbook wird mit **Get Run As Connection** gestartet und führt dann **Add-AzureRmAccount** aus.<br> ![Verknüpfung zwischen Aktivitäten erstellen](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
8. Wählen Sie im Zeichenbereich **Add-AzureRmAccount** aus, und geben Sie im Konfigurationssteuerungsbereich im Textfeld **Bezeichnung** den Text **Login to Azure** ein.
9. Klicken Sie auf **Parameter** , um das Konfigurationsblatt für Aktivitätsparameter anzuzeigen.
10. Da **Add-AzureRmAccount** über mehrere Parametersätze verfügt, müssen Sie einen davon auswählen, um Parameterwerte angeben zu können. Klicken Sie auf **Parametersatz**, und wählen Sie dann den Parametersatz **ServicePrincipalCertificate** aus.
11. Wenn Sie den Parametersatz auswählen, werden die Parameter auf dem Konfigurationsblatt für die Aktivitätsparameter angezeigt. Klicken Sie auf **APPLICATIONID**.<br> ![Azure RM-Kontoparameter hinzufügen](media/automation-first-runbook-graphical/add-azurermaccount-params.png)
12. Wählen Sie auf dem Blatt „Parameterwert“ für die **Datenquelle** die Option **Aktivitätsausgabe** und dann in der Liste **Get Run As Connection** aus. Geben Sie im Textfeld **Feldpfad** den Text **ApplicationId** ein, und klicken Sie auf **OK**. Geben Sie den Namen der Eigenschaft für den Feldpfad an, da die Aktivität ein Objekt mit mehreren Eigenschaften ausgibt.
13. Klicken Sie auf **CERTIFICATETHUMBPRINT**, und wählen Sie auf dem Blatt „Parameterwert“ für die **Datenquelle** die Option **Aktivitätsausgabe**. Wählen Sie in der Liste die Option **Get Run As Connection** aus, und geben Sie im Textfeld **Feldpfad** den Text **CertificateThumbprint** ein. Klicken Sie anschließend auf **OK**.
14. Klicken Sie auf **SERVICEPRINCIPAL**, und wählen Sie auf dem Blatt „Parameterwert“ für die **Datenquelle** die Option **ConstantValue** aus. Klicken Sie auf die Option **True** und dann auf **OK**.
15. Klicken Sie auf **TENANTID**, und wählen Sie auf dem Blatt „Parameterwert“ für die **Datenquelle** die **Aktivitätsausgabe**. Wählen Sie in der Liste die Option **Get Run As Connection** aus, und geben Sie im Textfeld **Feldpfad** den Text **TenantId** ein. Klicken Sie anschließend zweimal auf **OK**.  
16. Geben Sie im Bibliotheksteuerelement in das Textfeld für die Suche die Zeichenfolge **Set-AzureRmContext** ein.
17. Fügen Sie **Set-AzureRmContext** dem Zeichenbereich hinzu.
18. Wählen Sie im Zeichenbereich die Option **Set-AzureRmContext** aus, und geben Sie im Konfigurationssteuerungsbereich im Feld **Bezeichnung** die Zeichenfolge **Specify Subscription Id** ein.
19. Klicken Sie auf **Parameter** , um das Konfigurationsblatt für Aktivitätsparameter anzuzeigen.
20. Da **Set-AzureRmContext** über mehrere Parametersätze verfügt, müssen Sie einen davon auswählen, um Parameterwerte angeben zu können. Klicken Sie auf **Parametersatz**, und wählen Sie anschließend den Parametersatz **SubscriptionId** aus.  
21. Wenn Sie den Parametersatz auswählen, werden die Parameter auf dem Konfigurationsblatt für die Aktivitätsparameter angezeigt. Klicken Sie auf **SubscriptionID**
22. Wählen Sie auf dem Blatt „Parameterwert“ für die **Datenquelle** die Option **Variablenasset** und in der Liste die Option **AzureSubscriptionId** aus. Klicken Sie anschließend zweimal auf **OK**.   
23. Zeigen Sie auf **Login to Azure** , bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil auf **Specify Subscription Id**.

Ihr Runbook sollte nun wie folgt aussehen:  <br>![Konfiguration der Runbook-Authentifizierung](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="step-7---add-activity-to-start-a-virtual-machine"></a>Schritt 7: Hinzufügen einer Aktivität zum Starten eines virtuellen Computers
In diesem Schritt fügen Sie eine Aktivität vom Typ **Start-AzureRmVM** hinzu, um einen virtuellen Computer zu starten. Sie können einen beliebigen virtuellen Computer in Ihrem Azure-Abonnement auswählen. Sie werden den Namen vorerst innerhalb des Cmdlets hartcodieren.

1. Geben Sie im Bibliotheksteuerelement in das Textfeld für die Suche die Zeichenfolge **Start-AzureRm** ein.
2. Fügen Sie **Start-AzureRmVM** dem Zeichenbereich hinzu, und ziehen Sie das Element anschließend unter **Specify Subscription Id**.
3. Zeigen Sie auf **Specify Subscription Id** , bis am unteren Rand der Form ein Kreis erscheint. Klicken Sie auf den Kreis, und ziehen Sie den Pfeil zu **Start-AzureRmVM**.
4. Wählen Sie **Start-AzureRmVM**aus. Klicken Sie auf **Parameter** und anschließend auf **Parametersatz**, um die Sätze für **Start-AzureRmVM** anzuzeigen. Wählen Sie den Parametersatz **ResourceGroupNameParameterSetName** aus. Neben **ResourceGroupName** und **Name** wird jeweils ein Ausrufezeichen angezeigt. Das bedeutet, dass es sich dabei um erforderliche Parameter handelt. Beachten Sie außerdem, dass für beide ein Zeichenfolgenwert erwartet wird.
5. Wählen Sie **Name**. Wählen Sie **PowerShell-Ausdruck** als **Datenquelle** aus, und geben Sie den Namen des virtuellen Computers (in doppelten Anführungszeichen) ein, der mit diesem Runbook gestartet werden soll. Klicken Sie auf **OK**.<br>![Start-AzureRmVM – Wert für den Namensparameter](media/automation-first-runbook-graphical/runbook-startvm-nameparameter.png)
6. Wählen Sie **ResourceGroupName**aus. Wählen Sie unter **PowerShell-Ausdruck** die Zeichenfolge **Datenquelle** aus, und geben Sie den Namen der Ressourcengruppe in doppelten Anführungszeichen ein. Klicken Sie auf **OK**.<br> ![Start-AzureRmVM-Parameter](media/automation-first-runbook-graphical/startazurermvm-params.png)
7. Klicken Sie auf den Testbereich, um das Runbook zu testen.
8. Klicken Sie auf **Starten** , um den Test zu starten. Vergewissern Sie sich nach Abschluss des Tests, dass der virtuelle Computer gestartet wurde.

Ihr Runbook sollte nun wie folgt aussehen:  <br>![Konfiguration der Runbook-Authentifizierung](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="step-8---add-additional-input-parameters-to-the-runbook"></a>Schritt 8: Hinzufügen zusätzlicher Eingabeparameter für das Runbook
Das Runbook startet den virtuellen Computer derzeit in der Ressourcengruppe, die Sie im Cmdlet **Start-AzureRmVM** angegeben haben. Es wäre aber hilfreich, wenn wir beides beim Starten des Runbooks angeben könnten. Zu diesem Zweck werden dem Runbook als Nächstes Eingabeparameter hinzugefügt.

1. Öffnen Sie den grafischen Editor, indem Sie im Bereich **MyFirstRunbook** auf **Bearbeiten** klicken.
2. Klicken Sie auf **Eingabe und Ausgabe** und anschließend auf **Eingabe hinzufügen**, um den Eingabeparameterbereich für das Runbook zu öffnen.<br> ![Runbookeingabe und -ausgabe](media/automation-first-runbook-graphical/runbook-toolbar-InputandOutput-revised20165.png)
3. Geben Sie als *Name* die Zeichenfolge **VMName**an. Behalten Sie für **Typ** die Einstellung *string* bei, aber ändern Sie **Obligatorisch** in *Ja*. Klicken Sie auf **OK**.
4. Erstellen Sie einen zweiten obligatorischen Eingabeparameter namens *ResourceGroupName*, und klicken Sie anschließend auf **OK**, um den Bereich **Eingabe und Ausgabe** zu schließen.<br> ![Runbook-Eingabeparameter](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
5. Wählen Sie die Aktivität **Start-AzureRmVM** aus, und klicken Sie anschließend auf **Parameter**.
6. Ändern Sie die **Datenquelle** für **Name** in **Runbookeingabe**, und wählen Sie anschließend **VMName** aus.<br>
7. Ändern Sie die **Datenquelle** für **ResourceGroupName** in **Runbookeingabe**, und wählen Sie anschließend **ResourceGroupName** aus.<br> ![Start-AzureVM – Parameter](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
8. Speichern Sie das Runbook, und öffnen Sie den Testbereich. Nun können Sie Werte für die beiden Eingabevariablen angeben, die im Test verwendet werden.
9. Schließen Sie den Testbereich.
10. Klicken Sie auf **Veröffentlichen** , um die neue Version des Runbooks zu veröffentlichen.
11. Beenden Sie den virtuellen Computer, den Sie im vorherigen Schritt gestartet haben.
12. Klicken Sie auf **Starten** , um das Runbook zu starten. Geben Sie **VMName** und **ResourceGroupName** für den virtuellen Computer ein, den Sie starten möchten.<br> ![Start Runbook](media/automation-first-runbook-graphical/runbook-start-inputparams.png)
13. Vergewissern Sie sich nach Abschluss des Runbooks, dass der virtuelle Computer gestartet wurde.

## <a name="step-9---create-a-conditional-link"></a>Schritt 9: Erstellen einer bedingten Verknüpfung
In diesem Schritt passen Sie das Runbook so an, dass nur dann versucht wird, den virtuellen Computer zu starten, wenn dieser noch nicht gestartet ist. Zu diesem Zweck fügen Sie dem Runbook ein **Get-AzureRmVM**-Cmdlet hinzu, das den Instanzebenenstatus des virtuellen Computers abruft. Anschließend fügen Sie ein PowerShell-Workflow-Codemodul namens **Get Status** mit einem PowerShell-Codeausschnitt hinzu, mit dem ermittelt wird, ob der virtuelle Computer ausgeführt wird oder beendet ist. Eine bedingte Verknüpfung des Moduls **Get Status** führt **Start-AzureRmVM** nur dann aus, wenn der aktuelle Ausführungszustand angibt, dass der virtuelle Computer beendet ist. Schließlich geben Sie mithilfe des PowerShell-Cmdlets „Write-Output“ noch eine Nachricht aus, die darüber informiert, ob der virtuelle Computer erfolgreich gestartet wurde.

1. Öffnen Sie **MyFirstRunbook** im grafischen Editor.
2. Entfernen Sie die Verknüpfung zwischen **Specify Subscription Id** und **Start-AzureRmVM**, indem Sie auf die Verknüpfung klicken und die *ENTF-TASTE* drücken.
3. Geben Sie im Bibliotheksteuerelement in das Textfeld für die Suche die Zeichenfolge **Get-AzureRm** ein.
4. Fügen Sie dem Zeichenbereich **Get-AzureRmVM** hinzu.
5. Wählen Sie **Get-AzureRmVM** und anschließend **Parametersatz** aus, um die Sätze für **Get-AzureRmVM** anzuzeigen. Wählen Sie den Parametersatz **GetVirtualMachineInResourceGroupNameParamSet** aus. Neben **ResourceGroupName** und **Name** wird jeweils ein Ausrufezeichen angezeigt. Das bedeutet, dass es sich dabei um erforderliche Parameter handelt. Beachten Sie außerdem, dass für beide ein Zeichenfolgenwert erwartet wird.
6. Wählen Sie unter **Datenquelle** für **Name** die Option **Runbookeingabe** und anschließend **VMName** aus. Klicken Sie auf **OK**.
7. Wählen Sie unter **Datenquelle** für **ResourceGroupName** die Option **Runbookeingabe** und anschließend **ResourceGroupName** aus. Klicken Sie auf **OK**.
8. Wählen Sie unter **Datenquelle** als **Status** die Option **Konstanter Wert** aus, und klicken Sie anschließend auf **True**. Klicken Sie auf **OK**.
9. Erstellen Sie eine Verknüpfung zwischen **Specify Subscription Id** und **Get-AzureRmVM**.
10. Erweitern Sie im Bibliotheksteuerelement den Bereich **Runbooksteuerung**, und fügen Sie dem Zeichenbereich **Code** hinzu.  
11. Erstellen Sie eine Verknüpfung zwischen **Get-AzureRmVM** und **Code**.  
12. Klicken Sie auf **Code**, und ändern Sie im Konfigurationsbereich die Bezeichnung in **Get Status**.
13. Wählen Sie den Parameter **Code** aus, um das Blatt **Code-Editor** anzuzeigen.  
14. Fügen Sie im Code-Editor den folgenden Codeausschnitt ein:
    
     ```powershell
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```
15. Erstellen Sie eine Verknüpfung zwischen **Get Status** und **Start-AzureRmVM**.<br> ![Runbook mit Codemodul](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
16. Wählen Sie die Verknüpfung aus, und legen Sie **Bedingung anwenden** im Konfigurationsbereich auf **Ja** fest. Beachten Sie, dass die Verknüpfung nun als gestrichelte Linie dargestellt wird. Das bedeutet, dass die Aktivität nur ausgeführt wird, wenn die Bedingung erfüllt ist.  
17. Geben Sie für den **Bedingungsausdruck** Folgendes ein: *$ActivityOutput['Get Status'] -eq "Stopped"*. Nun wird **Start-AzureRmVM** nur dann ausgeführt, wenn der virtuelle Computer beendet ist.
18. Erweitern Sie im Bibliotheksteuerelement den Knoten **Cmdlets** und anschließend **Microsoft.PowerShell.Utility**.
19. Fügen Sie dem Zeichenbereich zweimal **Write-Output** hinzu.<br> ![Runbook mit Write-Output](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
20. Klicken Sie im ersten Steuerelement vom Typ **Write-Output** auf **Parameter**, und legen Sie die **Bezeichnung** auf *Notify VM Started* fest.
21. Legen Sie die **Datenquelle** für **InputObject** auf **PowerShell-Ausdruck** fest, und geben Sie den Ausdruck *"$VMName successfully started."* ein.
22. Klicken Sie im zweiten Steuerelement vom Typ **Write-Output** auf **Parameter**, und legen Sie die **Bezeichnung** auf *Notify VM Start Failed* fest.
23. Legen Sie die **Datenquelle** für **InputObject** auf **PowerShell-Ausdruck** fest, und geben Sie den Ausdruck *"$VMName could not start."* ein.
24. Verknüpfen Sie **Start AzureRmVM** mit **Notify VM Started** und **Notify VM Start Failed**.
25. Wählen Sie die Verknüpfung mit **Notify VM Started** aus, und legen Sie **Bedingung anwenden** auf **True** fest.
26. Geben Sie als **Bedingungsausdruck** Folgendes ein: *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -eq $true*. Dieses Write-Output-Steuerelement wird nur ausgeführt, wenn der virtuelle Computer erfolgreich gestartet wurde.
27. Wählen Sie die Verknüpfung mit **Notify VM Start Failed** aus, und legen Sie **Bedingung anwenden** auf **True** fest.
28. Geben Sie als **Bedingungsausdruck** Folgendes ein: *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -ne $true*. Dieses Write-Output-Steuerelement wird nur ausgeführt, wenn der virtuelle Computer nicht erfolgreich gestartet wurde.
29. Speichern Sie das Runbook, und öffnen Sie den Testbereich.
30. Wenn Sie nun das Runbook starten, während der virtuellen Computer beendet ist, wird der virtuelle Computer gestartet.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur grafischen Inhaltserstellung finden Sie unter [Grafische Erstellung in Azure Automation](automation-graphical-authoring-intro.md)
* Erste Schritte mit PowerShell-Runbooks werden in [Mein erstes PowerShell-Runbook](automation-first-runbook-textual-powershell.md)
* Informationen über die ersten Schritte mit PowerShell-Workflow-Runbooks finden Sie unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)

