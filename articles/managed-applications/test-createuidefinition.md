---
title: Testen der Benutzeroberflächendefinition für Azure Managed Applications | Microsoft-Dokumentation
description: Hier wird beschrieben, wie Sie die Benutzeroberfläche zum Erstellen Ihrer Azure Managed Applications-Instanz über das Portal testen.
services: managed-applications
documentationcenter: na
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2018
ms.author: tomfitz
ms.openlocfilehash: 2c313538e297c5781b48fcfe9d0d5390f94c97f5
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043360"
---
# <a name="test-azure-portal-interface-for-your-managed-application"></a>Testen der Azure-Portalschnittstelle für die verwaltete Anwendung
Nach dem [Erstellen der Datei „createUiDefinition.json“](create-uidefinition-overview.md) für Ihre Azure Managed Applications-Instanz müssen Sie die Benutzeroberfläche testen. Um das Testen zu vereinfachen, verwenden Sie ein Skript, das Ihre Datei in das Portal lädt. Sie müssen Ihre verwaltete Anwendung nicht tatsächlich bereitstellen.

## <a name="prerequisites"></a>Voraussetzungen

* Die Datei **createUiDefinition.json**. Falls diese Datei nicht vorhanden ist, kopieren Sie die [Beispieldatei](https://github.com/Azure/azure-quickstart-templates/blob/master/test/template-validation-tests/sample-template/createUIDefinition.json), und speichern Sie sie lokal.

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="download-test-script"></a>Herunterladen des Testskripts

Kopieren Sie zum Testen der Oberfläche im Portal eins der folgenden Skripts auf den lokalen Computer:

* [PowerShell-Skript zum Querladen](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Azure CLI-Skript zum Querladen](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

## <a name="run-script"></a>Ausführen des Skripts

Führen Sie zum Anzeigen Ihrer Benutzeroberflächendatei im Portal das heruntergeladene Skript aus. Mit dem Skript wird ein Speicherkonto in Ihrem Azure-Abonnement erstellt und anschließend die Datei „createUiDefinition.json“ in das Speicherkonto hochgeladen. Anschließend wird das Portal geöffnet und die Datei aus dem Speicherkonto heruntergeladen.

Geben Sie einen Ort für das Speicherkonto und den Ordner mit der Datei „createUiDefinition.json“ an. Der Ort des Speicherkontos muss nur angegeben werden, wenn das Skript erstmals ausgeführt wird oder das Speicherkonto gelöscht wurde.

Verwenden Sie für PowerShell Folgendes:

```powershell
.\SideLoad-CreateUIDefinition.ps1 `
  -StorageResourceGroupLocation southcentralus `
  -ArtifactsStagingDirectory <path-to-folder-with-createuidefinition>
```

Verwenden Sie für die Azure-Befehlszeilenschnittstelle den folgenden Befehl:

```azurecli
./sideload-createuidef.sh \
  -l southcentralus \
  -a <path-to-folder-with-createuidefinition>
```

## <a name="test-your-interface"></a>Testen der Benutzeroberfläche

Das Skript öffnet eine neue Registerkarte im Browser. Das Portal mit der Schnittstelle zum Erstellen der verwalteten Anwendung wird angezeigt.

![Anzeigen des Portals](./media/test-createuidefinition/view-portal.png)

Öffnen Sie Web Developer Tools in Ihrem Browser, bevor Sie die Felder ausfüllen. In der **Konsole** werden wichtige Meldungen zur Benutzeroberfläche angezeigt.

![Klicken auf „Konsole“](./media/test-createuidefinition/select-console.png)

Wenn Ihre Benutzeroberflächendefinition einen Fehler enthält, wird die Beschreibung in der Konsole angezeigt.

![Anzeigen des Fehlers](./media/test-createuidefinition/show-error.png)

Geben Sie Werte für die Felder an. Anschließend werden die Werte angezeigt, die an die Vorlage übergeben werden.

![Anzeigen der Werte](./media/test-createuidefinition/show-json.png)

## <a name="test-your-solution-files"></a>Testen Ihrer Lösungsdateien

Sie haben überprüft, ob die Portalbenutzeroberfläche wie erwartet funktioniert. Vergewissern Sie sich nun, dass die Datei „createUiDefinition“ ordnungsgemäß in die Datei „mainTemplate.json“ integriert wurde. Sie können einen Überprüfungsskripttest ausführen, um den Inhalt der Lösungsdateien, einschließlich der Datei „createUiDefinition“, zu testen. Das Skript überprüft die JSON-Syntax, sucht in den Textfeldern nach regulären Ausdrücken und stellt sicher, dass die Ausgabewerte der Portalbenutzeroberfläche mit den Parametern der Vorlage übereinstimmen. Informationen zum Ausführen dieses Skripts finden Sie unter [Run static validation checks for templates](https://github.com/Azure/azure-quickstart-templates/tree/master/test/template-validation-tests) (Ausführen von statischen Überprüfungen für Vorlagen).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Portalbenutzeroberfläche überprüft haben, lesen Sie mehr darüber, wie Sie Ihre [verwaltete Azure-Anwendung im Marketplace verfügbar machen](publish-marketplace-app.md).
