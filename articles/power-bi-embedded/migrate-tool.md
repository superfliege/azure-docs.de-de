---
title: Verwenden des Power BI Embedded-Migrationstools | Microsoft-Dokumentation
description: "Mit dem Power BI Embedded-Migrationstool können Sie Ihre Berichte aus den Power BI-Arbeitsbereichssammlungen in Power BI Embedded kopieren."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: 0b7b5089045daf6dd88fcd84e316b2bd44f8c927
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-power-bi-embedded-migration-tool"></a>Verwenden des Power BI Embedded-Migrationstools

Mit dem Power BI Embedded-Migrationstool können Sie Ihre Berichte aus den Power BI-Arbeitsbereichssammlungen in Power BI Embedded kopieren.

Die Migration der Inhalte aus Ihren Arbeitsbereichssammlungen zum Power BI-Dienst kann parallel zu Ihrer aktuellen Lösung erfolgen und erfordert keine Downtime.

## <a name="limitations"></a>Einschränkungen

* Gepushte DataSets können nicht heruntergeladen werden und müssen mithilfe der Power BI-REST-APIs für den Power BI-Dienst neu erstellt werden.
* PBIX-Dateien, die vor dem 26. November 2016 importiert wurden, können nicht heruntergeladen werden.

## <a name="download"></a>Download

Sie können das Beispiel für das Migrationstool von [GitHub](https://github.com/Microsoft/powerbi-migration-sample) herunterladen. Sie können entweder eine ZIP-Datei des Repositorys herunterladen oder es lokal klonen. Nach dem Herunterladen können Sie die Datei *powerbi-migration-sample.sln* in Visual Studio öffnen, um das Migrationstool zu erstellen und auszuführen.

## <a name="migration-plans"></a>Migrationspläne

In einem Migrationsplan, der einfach nur Metadaten umfasst, wird der Inhalt in Power BI-Arbeitsbereichssammlungen katalogisiert und festgelegt, wie sie in Power BI Embedded veröffentlicht werden sollen.

### <a name="start-with-a-new-migration-plan"></a>Starten mit einem neuen Migrationsplan

Bei einem Migrationsplan handelt es sich um die Metadaten der in Power BI-Arbeitsbereichssammlungen verfügbaren Elemente, die dann in Power BI Embedded verschoben werden sollen. Der Migrationsplan wird als XML-Datei gespeichert.

Sie erstellen zunächst einen neuen Migrationsplan. Gehen Sie wie folgt vor, um einen neuen Migrationsplan zu erstellen.

1. Wählen Sie **Datei** > **New Migration Plan** (Neuer Migrationsplan) aus.

    ![New Migration Plan (Neuer Migrationsplan)](media/migrate-tool/migrate-tool-plan.png)

2. Wählen Sie im Dialogfeld **Select Power BI Workspace Collections Resource Group** (Power BI-Arbeitsbereichssammlungen-Ressourcengruppe auswählen) die Dropdownliste **Umgebung** und darin die Option „prod“ aus.

3. Sie werden aufgefordert, sich anzumelden. Verwenden Sie dazu die Anmeldeinformationen für Ihr Azure-Abonnement.

    > [!IMPORTANT]
    > Sie melden sich bei Power BI **nicht** über Ihr Office 365-Organisationskonto an.

4. Wählen Sie das Azure-Abonnement aus, in dem Ihre Ressource mit den Power BI-Arbeitsbereichssammlungen gespeichert ist.

    ![Auswählen des Azure-Abonnements](media/migrate-tool/migrate-tool-select-resource-group.png)

5. Wählen Sie unterhalb der Abonnementliste die **Ressourcengruppe**, die Ihre Arbeitsbereichssammlungen enthält, und dann **Auswählen** aus.

    ![Auswählen der Ressourcengruppe](media/migrate-tool/migrate-tool-select-resource-group2.png)

6. Wählen Sie **Analysieren** aus. Dadurch wird der Bestand an Elementen in Ihrem Azure-Abonnement angezeigt, sodass Sie Ihren Plan starten können.

    ![Auswählen der Schaltfläche „Analysieren“](media/migrate-tool/migrate-tool-analyze-group.png)

    > [!NOTE]
    > Der Analysevorgang kann je nach Anzahl der Arbeitsbereichssammlungen und Umfang der Inhalte in den Arbeitsbereichssammlungen einige Minuten dauern.

7. Nach Abschluss des Analysevorgangs**** werden Sie aufgefordert, den Migrationsplan zu speichern.

Damit haben Sie den Migrationsplan mit Ihrem Azure-Abonnement verbunden. Es folgen Informationen zum Ablauf beim Arbeiten mit dem Migrationsplan. Dieser umfasst die Schritte Analysieren und Planen der Migration, Herunterladen, Erstellen von Gruppen und Hochladen.

### <a name="save-your-migration-plan"></a>Speichern des Migrationsplans

Sie können den Migrationsplan zur späteren Verwendung speichern. Dadurch wird eine XML-Datei erstellt, die alle Informationen im Migrationsplan enthält.

Gehen Sie wie folgt vor, um den Migrationsplan zu speichern.

1. Wählen Sie **Datei** > **Save Migration Plan** (Migrationsplan speichern) aus.

    ![Menüoption „Save Migration Plan“ (Migrationsplan speichern)](media/migrate-tool/migrate-tool-save-plan.png)

2. Geben Sie einen Namen für die Datei an, oder verwenden Sie den generierten Dateinamen, und wählen Sie **Speichern** aus.

### <a name="open-an-existing-migration-plan"></a>Öffnen eines vorhandenen Migrationsplans

Sie können einen gespeicherten Migrationsplan öffnen und die Arbeit daran fortsetzen.

Gehen Sie wie folgt vor, um den vorhandenen Migrationsplan zu öffnen.

1. Wählen Sie **Datei** > **Open Existing Migration Plan** (Vorhandenen Migrationsplan öffnen) aus.

    ![Menüoption „Open Existing Migration Plan“ (Vorhandenen Migrationsplan öffnen)](media/migrate-tool/migrate-tool-open-plan.png)

2. Wählen Sie die Migrationsdatei und dann **Öffnen** aus.

## <a name="step-1-analyze-and-plan-migration"></a>Schritt 1: Analysieren und Planen der Migration

Auf der Registerkarte **Analyze & Plan Migration** (Migration analysieren und planen) werden die aktuellen Inhalte in der Ressourcengruppe Ihres Azure-Abonnements angezeigt.

![Registerkarte „Analyze & Plan Migration“ (Migration analysieren und planen)](media/migrate-tool/migrate-tool-step1.png)

Wir sehen uns *SampleResourceGroup* als Beispiel an.

### <a name="paas-topology"></a>PaaS-Topologie

Dabei handelt es sich um eine Auflistung Ihrer *Ressourcengruppe > Arbeitsbereichssammlungen > Arbeitsbereiche*. Für die Ressourcengruppe und die Arbeitsbereichssammlungen wird jeweils ein Anzeigename angezeigt. Für die Arbeitsbereiche wird jeweils eine GUID angezeigt.

Die Elemente in der Liste werden außerdem in einer Farbe und mit einer Zahl im Format (#/#) angezeigt. Hiermit wird die Anzahl der Berichte angegeben, die heruntergeladen werden können.

Schwarz bedeutet, dass alle Berichte heruntergeladen werden können. Rot bedeutet, dass einige Berichte nicht heruntergeladen werden können. Die linke Zahl gibt die Gesamtanzahl der Berichte an, die heruntergeladen werden können. Die rechte Zahl gibt die Gesamtanzahl der Berichte in der Gruppierung an.

Sie können ein Element in der PaaS-Topologie auswählen, um die Berichte im Berichtsbereich anzuzeigen.

### <a name="reports"></a>Berichte

Im Berichtsbereich sind die verfügbaren Berichte aufgeführt. Zudem ist angegeben, ob sie jeweils heruntergeladen oder nicht heruntergeladen werden können.

![Liste der Berichte in Power BI-Arbeitsbereichssammlungen](media/migrate-tool/migrate-tool-analyze-reports.png)

### <a name="target-structure"></a>Target Structure (Zielstruktur)

In **Target Structure** (Zielstruktur) geben Sie im Tool an, an welchen Ort Elemente heruntergeladen werden und wie sie hochgeladen werden.

#### <a name="download-plan"></a>Download Plan (Downloadplan)

Es wird automatisch ein Pfad erstellt. Sie können diesen Pfad bei Bedarf ändern. Wenn Sie den Pfad ändern, müssen Sie **Update paths** (Pfade aktualisieren) auswählen.

**Damit wird der Download nicht ausgeführt.** Es wird lediglich die Struktur angegeben, in die die Berichte heruntergeladen werden.

#### <a name="upload-plan"></a>Upload Plan (Uploadplan)

Hier können Sie ein Präfix für die App-Arbeitsbereiche angeben, die im Power BI-Dienst erstellt werden. Nach dem Präfix folgt die GUID für den in Azure vorhandenen Arbeitsbereich.

![Angeben des Gruppennamenpräfixes](media/migrate-tool/migrate-tool-upload-plan.png)

**Dadurch werden die Gruppen im Power BI-Dienst nicht erstellt.** Stattdessen wird lediglich die Namensstruktur für die Gruppen definiert.

Wenn Sie das Präfix ändern, müssen Sie **Generate Upload Plan** (Uploadplan generieren) auswählen.

Bei Bedarf können Sie mit der rechten Maustaste auf eine Gruppe klicken und die Gruppe im Uploadplan direkt umbenennen.

![Kontextmenüoption „Gruppe umbenennen“](media/migrate-tool/migrate-tool-upload-report-rename-item.png)

> [!NOTE]
> Der Name der *Gruppe* darf keine Leerzeichen und keine ungültigen Zeichen enthalten.

## <a name="step-2-download"></a>Schritt 2: Download

Auf der Registerkarte **Download** wird die Liste der Berichte und zugeordneten Metadaten angezeigt. Zudem werden der Exportstatus sowie der vorherige Exportstatus angezeigt.

![Registerkarte „Download“](media/migrate-tool/migrate-tool-download-tab.png)

Sie haben zwei Möglichkeiten.

* Wählen Sie bestimmte Berichte und dann **Ausgewählte Elemente herunterladen** aus.
* Wählen Sie **Alle herunterladen** aus.

![Schaltfläche „Ausgewählte Elemente herunterladen“](media/migrate-tool/migrate-tool-download-options.png)

Bei einem erfolgreich ausgeführten Download wird der Status *Fertig* angezeigt, der angibt, dass die PBIX-Datei vorhanden ist.

Wählen Sie nach Abschluss des Downloads die Registerkarte **Gruppen erstellen** aus.

## <a name="step-3-create-groups"></a>Schritt 3: Erstellen von Gruppen

Nach dem Herunterladen der verfügbaren Berichte können Sie zur Registerkarte **Gruppen erstellen** wechseln. Auf dieser Registerkarte werden basierend auf dem erstellten Migrationsplan die App-Arbeitsbereiche im Power BI-Dienst erstellt. Der App-Arbeitsbereich wird mit dem Namen erstellt, den Sie auf der Registerkarte **Upload** in **Analyze & Plan Migration** (Migration analysieren und planen) angegeben haben.

![Registerkarte „Gruppen erstellen“](media/migrate-tool/migrate-tool-create-groups.png)

Zum Erstellen der App-Arbeitsbereiche können Sie entweder **Create Selected Groups** (Ausgewählte Gruppen erstellen) oder **Create All Missing Groups** (Alle fehlenden Gruppen erstellen) auswählen.

Wenn Sie eine dieser Optionen auswählen, werden Sie aufgefordert, sich anzumelden. *Sie verwenden Ihre Anmeldeinformationen für den Power BI-Dienst, in dem die App-Arbeitsbereiche erstellt werden sollen.*

![Fenster für Power BI-Anmeldung](media/migrate-tool/migrate-tool-create-group-sign-in.png)

Dadurch wird der App-Arbeitsbereich im Power BI-Dienst erstellt. Die Berichte werden jedoch nicht in den App-Arbeitsbereich hochgeladen.

Sie können überprüfen, ob der App-Arbeitsbereich erstellt wurde, indem Sie sich bei Power BI anmelden und überprüfen, ob der Arbeitsbereich vorhanden ist. Sie werden feststellen, dass der Arbeitsbereich keine Elemente enthält.

![App-Arbeitsbereich im Power BI-Dienst](media/migrate-tool/migrate-tool-app-workspace.png)

Nach dem Erstellen des Arbeitsbereichs können Sie zur Registerkarte **Upload** wechseln.

## <a name="step-4-upload"></a>Schritt 4: Upload

Auf der Registerkarte **Upload** werden die Berichte in den Power BI-Dienst hochgeladen. Es werden die Liste der Berichte, die wir auf der Registerkarte „Download“ heruntergeladen haben, sowie der auf dem Migrationsplan basierende Zielgruppenname angezeigt.

![Registerkarte „Upload“](media/migrate-tool/migrate-tool-upload-tab.png)

Sie können ausgewählte Berichte oder alle Berichte hochladen. Zudem können Sie den Uploadstatus zurücksetzen, um Elemente erneut hochzuladen.

Sie haben auch die Möglichkeit, auszuwählen, wie vorgegangen wird, wenn ein Bericht mit dem gleichen Namen vorhanden ist. Sie haben die Wahl zwischen **Abbrechen**, **Ignorieren** und **Überschreiben**.

![Dropdownliste für Vorgehensweise, wenn ein Bericht mit gleichem Namen vorhanden ist](media/migrate-tool/migrate-tool-upload-report-same-name.png)

![Hochladen ausgewählter Ergebnisse](media/migrate-tool/migrate-tool-upload-selected.png)

### <a name="duplicate-report-names"></a>Duplizieren von Berichtsnamen

Wenn ein Bericht mit dem gleichen Namen vorhanden ist, Sie jedoch wissen, dass es sich um einen anderen Bericht handelt, müssen Sie den **TargetName** des Berichts ändern. Sie können den Namen ändern, indem Sie die XML-Datei des Migrationsplans manuell bearbeiten.

Für die Änderung müssen Sie das Migrationstool schließen und anschließend das Tool und den Migrationsplan erneut öffnen.

Im Beispiel oben ist bei einem der geklonten Berichte ein Fehler aufgetreten, weil ein Bericht mit dem gleichen Namen vorhanden ist. In der XML-Datei des Migrationsplans wird Folgendes angezeigt.

```
<ReportMigrationData>
    <PaaSWorkspaceCollectionName>SampleWorkspaceCollection</PaaSWorkspaceCollectionName>
    <PaaSWorkspaceId>4c04147b-d8fc-478b-8dcb-bcf687149823</PaaSWorkspaceId>
    <PaaSReportId>525a8328-b8cc-4f0d-b2cb-c3a9b4ba2efe</PaaSReportId>
    <PaaSReportLastImportTime>1/3/2017 2:10:19 PM</PaaSReportLastImportTime>
    <PaaSReportName>cloned</PaaSReportName>
    <IsPushDataset>false</IsPushDataset>
    <IsBoundToOldDataset>false</IsBoundToOldDataset>
    <PbixPath>C:\MigrationData\SampleResourceGroup\SampleWorkspaceCollection\4c04147b-d8fc-478b-8dcb-bcf687149823\cloned-525a8328-b8cc-4f0d-b2cb-c3a9b4ba2efe.pbix</PbixPath>
    <ExportState>Done</ExportState>
    <LastExportStatus>OK</LastExportStatus>
    <SaaSTargetGroupName>SampleMigrate</SaaSTargetGroupName>
    <SaaSTargetGroupId>6da6f072-0135-4e6c-bc92-0886d8aeb79d</SaaSTargetGroupId>
    <SaaSTargetReportName>cloned</SaaSTargetReportName>
    <SaaSImportState>Failed</SaaSImportState>
    <SaaSImportError>Report with the same name already exists</SaaSImportError>
</ReportMigrationData>
```

Für das fehlerhafte Element können wir den Namen von SaaSTargetReportName ändern.

```
<SaaSTargetReportName>cloned2</SaaSTargetReportName>
```

Dann können wir den Plan im Migrationstool erneut öffnen und den entsprechenden Bericht hochladen.

In Power BI können wir dann sehen, dass die Berichte und Datasets in den App-Arbeitsbereich hochgeladen wurden.

![Berichtsliste im Power BI-Dienst](media/migrate-tool/migrate-tool-upload-app-workspace.png)

<a name="upload-local-file"></a>
### <a name="upload-a-local-pbix-file"></a>Hochladen einer lokalen PBIX-Datei

Sie können eine lokale Version einer Power BI Desktop-Datei hochladen. Schließen Sie dazu das Tool, bearbeiten Sie die XML-Datei, und geben Sie den vollständigen Pfad zu der lokalen PBIX-Datei in der **PbixPath**-Eigenschaft an.

```
<PbixPath>[Full Path to PBIX file]</PbixPath>
```

Öffnen Sie nach dem Bearbeiten der XML-Datei erneut den Plan im Migrationstool, und laden Sie den Bericht hoch.

<a name="directquery-reports"></a>
### <a name="directquery-reports"></a>DirectQuery-Berichte

Sie müssen die Verbindungszeichenfolge für DirectQuery-Berichte aktualisieren. Dies kann über *powerbi.com* erfolgen, oder Sie können die Verbindungszeichenfolge programmgesteuert von Power BI Embedded (Paas) abfragen. Ein Beispiel finden Sie unter [Extrahieren der DirectQuery-Verbindungszeichenfolge aus einem PaaS-Bericht](migrate-code-snippets.md#extract-directquery-connection-string-from-power-bi-workspace-collections).

Sie können dann die Verbindungszeichenfolge für das DataSet im Power BI-Dienst aktualisieren und die Anmeldeinformationen für die Datenquelle festlegen. In den folgenden Beispielen finden Sie die entsprechende Vorgehensweise.

* [Aktualisieren der DirectQuery-Verbindungszeichenfolge in Power BI Embedded](migrate-code-snippets.md#update-directquery-connection-string-in-power-bi-embedded)
* [Festlegen von DirectQuery-Anmeldeinformationen in Power BI Embedded](migrate-code-snippets.md#set-directquery-credentials-in-power-bi-embedded)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Ihre Berichte von Power BI-Arbeitsbereichssammlungen zu Power BI Embedded migriert wurden, können Sie nun die Anwendung aktualisieren und die Berichte in diesen App-Arbeitsbereich einbetten.

Weitere Informationen finden Sie unter [Migrieren von Power BI-Arbeitsbereichssammlungsinhalten zu Power BI Embedded](migrate-from-power-bi-workspace-collections.md).

Weitere Fragen? [Fragen Sie die Power BI-Community](http://community.powerbi.com/).