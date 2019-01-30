---
title: Authentifizieren eines Azure Stream Analytics-Auftrags für die Azure Data Lake Storage Gen1-Ausgabe
description: In diesem Artikel wird die Verwendung von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Azure Data Lake Storage Gen1-Ausgabe beschrieben.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/18/2019
ms.custom: seodec18
ms.openlocfilehash: 87c605feeab742ae589cf8d5d9a98c8e53ccf662
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2019
ms.locfileid: "54410454"
---
# <a name="authenticate-stream-analytics-to-azure-data-lake-storage-gen1-using-managed-identities-preview"></a>Authentifizieren von Stream Analytics bei Azure Data Lake Storage Gen1 mithilfe verwalteter Identitäten (Vorschau)

Azure Stream Analytics unterstützt die Authentifizierung über verwaltete Identitäten für die Azure Data Lake Storage Gen1-Ausgabe. Bei einer Identität handelt es sich um eine in Azure Active Directory registrierte verwaltete Anwendung, die einen bestimmten Stream Analytics-Auftrag repräsentiert und für die Authentifizierung bei einer Zielressource verwendet werden kann. Verwaltete Identitäten beseitigen die Einschränkungen benutzerbasierter Authentifizierungsmethoden, wie etwa die Notwendigkeit einer erneuten Authentifizierung aufgrund von Kennwortänderungen oder Token, die alle 90 Tage ablaufen. Darüber hinaus helfen verwaltete Identitäten bei der Automatisierung von Stream Analytics-Auftragsbereitstellungen, deren Ausgabe in Azure Data Lake Storage Gen1 erfolgt.

Lesen Sie den Blogbeitrag [Eight new features in Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/) (Acht neue Features in Azure Stream Analytics), um sich für diese Vorschau zu registrieren und mehr über neue Features zu erfahren.

Dieser Artikel veranschaulicht drei Methoden, um eine verwaltete Identität für einen Azure Stream Analytics-Auftrag zu aktivieren, dessen Ausgabe an Azure Data Lake Storage Gen1 erfolgt: das Azure-Portal, die Bereitstellung mithilfe von Azure Resource Manager-Vorlagen und Azure Stream Analytics-Tools für Visual Studio.

## <a name="azure-portal"></a>Azure-Portal

1. Erstellen Sie zunächst einen neuen Stream Analytics-Auftrag, oder öffnen Sie einen vorhandenen Auftrag im Azure-Portal. Wählen Sie in der Menüleiste auf der linken Bildschirmseite unter **Konfigurieren** die Option **Verwaltete Identität (Vorschau)**.

   ![Konfigurieren einer verwalteten Identität für Stream Analytics (Vorschau)](./media/stream-analytics-managed-identities-adls/stream-analytics-managed-identity-preview.png)

2. Wählen Sie im rechts angezeigten Fenster die Option **Vom System zugewiesene verwaltete Identität verwenden (Vorschau)**. Klicken Sie auf **Speichern**, um einen Dienstprinzipal für die Identität des Stream Analytics-Auftrags in Azure Active Directory zu erstellen. Der Lebenszyklus der neu erstellten Identität wird von Azure verwaltet. Wenn der Stream Analytics-Auftrag gelöscht wird, wird die zugeordnete Identität (also der Dienstprinzipal) von Azure automatisch ebenfalls gelöscht.

   Nach dem Speichern der Konfiguration wird die Objekt-ID (OIS) des Dienstprinzipals als Prinzipal-ID aufgeführt, wie hier gezeigt:

   ![Stream Analytics-Dienstprinzipal-ID](./media/stream-analytics-managed-identities-adls/stream-analytics-principal-id.png)
 
   Der Dienstprinzipal weist den gleichen Namen auf wie der Stream Analytics-Auftrag. Wenn der Name des Auftrags z.B. **MyASAJob** lautet, erhält auch der erstellte Dienstprinzipal den Namen **MyASAJob**.

3. Klicken Sie im Fenster mit den Ausgabeeigenschaften der ADLS Gen1-Ausgabesenke auf die Dropdownliste mit Authentifizierungsmodi, und wählen Sie **Verwaltete Identität (Vorschau)** aus.

4. Geben Sie die restlichen Eigenschaften an. Weitere Informationen zum Erstellen einer ADLS-Ausgabe finden Sie unter [Erstellen einer Data Lake Store-Ausgabe mit Stream Analytics](../data-lake-store/data-lake-store-stream-analytics.md). Wenn Sie fertig sind, klicken Sie auf **Speichern**.

   ![Konfigurieren von Azure Data Lake Storage](./media/stream-analytics-managed-identities-adls/stream-analytics-configure-adls.png)
 
5. Navigieren Sie zur Übersichtsseite für ADLS Gen1, und klicken Sie auf **Daten-Explorer**.

   ![Konfigurieren von Data Lake Storage – Übersicht](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-overview.png)

6. Klicken Sie im Bereich „Daten-Explorer“ auf **Zugriff**, und klicken Sie im Bereich „Zugriff“ auf **Hinzufügen**.

   ![Konfigurieren des Data Lake Storage-Zugriffs](./media/stream-analytics-managed-identities-adls/stream-analytics-adls-access.png)

7. Geben Sie im Textfeld im Bereich **Benutzer oder Gruppe auswählen** den Namen des Dienstprinzipals ein. Denken Sie daran, dass der Name des Dienstprinzipals auch der Name des zugehörigen Stream Analytics-Auftrags ist. Wenn Sie anfangen, den Namen einzugeben, wird er unterhalb des Textfelds angezeigt. Wählen Sie den Namen des gewünschten Dienstprinzipals aus, und klicken Sie auf **Auswählen**.

   ![Auswählen eines Dienstprinzipalnamens](./media/stream-analytics-managed-identities-adls/stream-analytics-service-principal-name.png)
 
8. Aktivieren Sie im Bereich **Berechtigungen** die Berechtigungen **Schreiben** und **Ausführen**, und weisen Sie diese der Option **Diesen Ordner und alle untergeordneten Ordner** zu. Klicken Sie dann auf **OK**.

   ![Auswählen von Schreib- und Ausführungsberechtigungen](./media/stream-analytics-managed-identities-adls/stream-analytics-select-permissions.png)
 
9. Der Dienstprinzipal wird im Bereich **Zugriff** unter **Zugewiesene Berechtigungen** aufgeführt, wie unten gezeigt. Sie können jetzt Ihren Stream Analytics-Auftrag starten.

   ![Stream Analytics-Zugriffsliste im Portal](./media/stream-analytics-managed-identities-adls/stream-analytics-access-list.png)

   Weitere Informationen zu den Dateisystemberechtigungen von Data Lake Storage Gen1 finden Sie unter [Zugriffssteuerung in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-access-control.md).

## <a name="stream-analytics-tools-for-visual-studio"></a>Stream Analytics-Tools für Visual Studio

1. Legen Sie in „JobConfig.json“ die Option **Systemseitig zugewiesene Identität verwenden** auf **True** fest.

   ![Stream Analytics-Auftrag – Konfiguration verwalteter Identitäten](./media/stream-analytics-managed-identities-adls/adls-mi-jobconfig-vs.png)

2. Klicken Sie im Fenster mit den Ausgabeeigenschaften der ADLS Gen1-Ausgabesenke auf die Dropdownliste mit Authentifizierungsmodi, und wählen Sie **Verwaltete Identität (Vorschau)** aus.

   ![ADLS-Ausgabe – verwaltete Identitäten](./media/stream-analytics-managed-identities-adls/adls-mi-output-vs.png)

3. Geben Sie die restlichen Eigenschaften an, und klicken Sie auf **Speichern**.

4. Klicken Sie im Abfrage-Editor auf **An Azure übermitteln**.

   Wenn Sie den Auftrag übermitteln, führen die Tools zwei Schritte aus:

   * In Azure Active Directory wird automatisch ein Dienstprinzipal für die Identität des Stream Analytics-Auftrags erstellt. Der Lebenszyklus der neu erstellten Identität wird von Azure verwaltet. Wenn der Stream Analytics-Auftrag gelöscht wird, wird die zugeordnete Identität (also der Dienstprinzipal) von Azure automatisch ebenfalls gelöscht.

   * **Schreib**- und **Ausführungsberechtigungen** für den im Auftrag verwendeten ADLS Gen1-Präfixpfad werden automatisch festgelegt und diesem Ordner sowie allen Unterordnern zugewiesen.

5. Sie können die Resource Manager-Vorlagen mit der folgenden Eigenschaft mithilfe des [Stream Analytics CI.CD Nuget-Pakets](https://www.nuget.org/packages/Microsoft.Azure.StreamAnalytics.CICD/) Version 1.5.0 oder höher auf einem Buildcomputer (außerhalb von Visual Studio) erstellen. Führen Sie die Schritte zur Bereitstellung von Resource Manager-Vorlagen im nächsten Abschnitt durch, um den Dienstprinzipal abzurufen und über PowerShell Zugriff darauf gewähren.

## <a name="resource-manager-template-deployment"></a>Resource Manager-Vorlagenbereitstellung

1. Sie können eine *Microsoft.StreamAnalytics/streamingjobs*-Ressource mit einer verwalteten Identität erstellen, indem Sie die folgende Eigenschaft in den Ressourcenabschnitt Ihrer Resource Manager-Vorlage einfügen:

   ```json
   "Identity": {
   "Type": "SystemAssigned",
   },
   ```

   Diese Eigenschaft weist Azure Resource Manager an, die Identität für Ihren Azure Stream Analytics-Auftrag zu erstellen und zu verwalten.

   **Beispielauftrag**

   ```json
   { 
   "Name": "AsaJobWithIdentity", 
   "Type": "Microsoft.StreamAnalytics/streamingjobs", 
   "Location": "West US",
   "Identity": {
     "Type": "SystemAssigned", 
     }, 
   "properties": {
      "sku": {
       "name": "standard"
       },
   "outputs": [
         {
           "name": "string",
           "properties":{
             "datasource": {        
               "type": "Microsoft.DataLake/Accounts",
               "properties": {
                 "accountName": “myDataLakeAccountName",
                 "filePathPrefix": “cluster1/logs/{date}/{time}",
                 "dateFormat": "YYYY/MM/DD",
                 "timeFormat": "HH",
                 "authenticationMode": "Msi"
                 }
                 
   }
   ```
  
   **Antwort des Beispielauftrags**

   ```json
   { 
   "Name": "mySAJob", 
   "Type": "Microsoft.StreamAnalytics/streamingjobs", 
   "Location": "West US",
   "Identity": {
   "Type": "SystemAssigned",
    "principalId": "GUID", 
    "tenantId": "GUID", 
   }, 
   "properties": {
           "sku": {
             "name": "standard"
           },
   }
   ```

   Notieren Sie sich die Prinzipal-ID der Auftragsantwort, um Zugriff auf die angeforderte ADLS-Ressource zu gewähren.

   Die **Mandanten-ID** ist die ID des Azure Active Directory-Mandanten, in dem der Dienstprinzipal erstellt wurde. Der Dienstprinzipal wird in dem Azure-Mandanten erstellt, der von diesem Abonnement als vertrauenswürdig eingestuft wird.

   Der **Typ** gibt den Typ der verwalteten Identität an, wie im Abschnitt mit den Typen verwalteter Identitäten erläutert. Es wird nur der vom System zugewiesene Typ unterstützt.

2. Gewähren Sie Zugriff auf den Dienstprinzipal über PowerShell. Um über PowerShell Zugriff auf den Dienstprinzipal zu gewähren, führen Sie folgenden Befehl aus:

   ```powershell
   Set-AzureRmDataLakeStoreItemAclEntry -AccountName <accountName> -Path <Path> -AceType User -Id <PrinicpalId> -Permissions <Permissions>
   ```

   Die **PrincipalId** ist die Objekt-ID des Dienstprinzipals und wird im Portal aufgeführt, sobald der Dienstprinzipal erstellt wurde. Wenn Sie den Auftrag mithilfe einer Resource Manager-Vorlagenbereitstellung erstellt haben, wird die Objekt-ID in der Identity-Eigenschaft der Auftragsantwort aufgeführt.

   **Beispiel**

   ```powershell
   PS > Set-AzureRmDataLakeStoreItemAclEntry -AccountName "adlsmsidemo" -Path / -AceType
   User -Id 14c6fd67-d9f5-4680-a394-cd7df1f9bacf -Permissions WriteExecute
   ```

   Weitere Informationen zum oben gezeigten PowerShell-Befehl finden Sie in der Dokumentation zu [Set-AzureRmDataLakeStoreItemAclEntry](https://docs.microsoft.com/powershell/module/azurerm.datalakestore/set-azurermdatalakestoreitemaclentry?view=azurermps-6.8.1&viewFallbackFrom=azurermps-4.2.0#optional-parameters).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Data Lake Store-Ausgabe mit Stream Analytics](../data-lake-store/data-lake-store-stream-analytics.md)
* [Lokales Testen von Stream Analytics-Abfragen mit Visual Studio](stream-analytics-vs-tools-local-run.md)
* [Lokales Testen von Livedaten mithilfe von Azure Stream Analytics-Tools für Visual Studio](stream-analytics-live-data-local-testing.md) 
