---
title: Verwalten Ihr virtuelles Computerimage in Azure Marketplace | Microsoft Docs
description: "Ausführliche Anleitungen zum Verwalten Ihrer Abbild eines virtuellen Computers in Azure Marketplace nach der Erstveröffentlichung"
services: Azure Marketplace
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: cc8648d4-59c2-4678-b47d-992300677537
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/03/2016
ms.author: hascipio;
ms.openlocfilehash: e1f90650e71345957c2d353774cb8bef62c1868b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2017
---
# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Nach der Produktion-Handbuch für die virtuelle Maschine Angebote in Azure Marketplace
In diesem Artikel wird erläutert, wie Sie ein Angebot virtuellen Maschinen in Azure Marketplace aktualisieren können. Es führt Sie durch den Prozess ein vorhandenes Angebot einen oder mehrere neue SKUs hinzugefügt. Es führt Sie auch durch den Prozess zum Entfernen einer virtuellen Maschinen Angebot oder der SKU aus dem Marketplace.

Nachdem eine Angebot/SKU, in bereitgestellt wird der [Azure-Portal](http://portal.azure.com), Sie können nicht geändert werden die folgenden Textfelder:

* **Bieten Sie Bezeichner**: In Dienstpublishing-Portal, wechseln Sie zu **VMs** , und wählen Sie Ihr Angebot. Klicken Sie dann auf **VM-IMAGES** > **bieten Bezeichner**.
* **SKU-ID**: In Dienstpublishing-Portal, wechseln Sie zu **VMs** , und wählen Sie Ihr Angebot. Klicken Sie dann auf **SKUS** > **hinzufügen eine SKU**.
* **Verleger-Namespace**: In Dienstpublishing-Portal, wechseln Sie zu **VMs** > **Exemplarische Vorgehensweise** > **Teilen Sie uns über Ihr Unternehmen** (finden Sie unter "Schritt 2 Registrieren Sie Ihr Unternehmen") > **Verleger Namespace** > **Namespace**.

Nachdem das Angebot/SKU aufgeführt ist die [Marketplace](http://azure.microsoft.com/marketplace), kann nicht geändert werden die folgenden Textfelder ein:

* **Bieten Sie Bezeichner**: In Dienstpublishing-Portal, wechseln Sie zu **VMs** , und wählen Sie Ihr Angebot. Klicken Sie dann auf **VM-IMAGES** > **bieten Bezeichner**.
* **SKU-ID**: In Dienstpublishing-Portal, wechseln Sie zu **VMs** , und wählen Sie Ihr Angebot. Klicken Sie dann auf **SKUS** > **hinzufügen eine SKU**.
* **Verleger-Namespace**: In Dienstpublishing-Portal, wechseln Sie zu **VMs** > **Exemplarische Vorgehensweise** > **Teilen Sie uns über Ihr Unternehmen** (finden Sie unter "Schritt 2 Register") **Verleger Namespace** > **Namespace**.
* **Ports**: In Dienstpublishing-Portal, wechseln Sie zu **VMs** , und wählen Sie Ihr Angebot. Klicken Sie dann auf **VM-IMAGES** > **Öffnen von Ports**.
* **Ändern der aufgelisteten SKU(s) Preise**
* **Der aufgelisteten SKU(s) Abrechnung Modell geändert**
* **Entfernen der aufgelisteten SKU(s) Regionen Rechnungsadresse**
* **Ändern die Anzahl der Datenträger der aufgelisteten SKU(s)**

## <a name="update-the-technical-details-of-a-sku"></a>Aktualisieren Sie alle technischen Konfigurationsdetails einer SKU
Um eine neue Version der aufgeführten SKU hinzuzufügen, und veröffentlichen Ihr Angebot, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **VM-IMAGES** Registerkarte.
4. In der **SKUs** Abschnitt, und suchen Sie die SKU, die Sie aktualisieren möchten.
5. Fügen Sie eine neue Versionsnummer für die SKU hinzu, und klicken Sie auf die  **+**  Schaltfläche. Die neue Version sollte in einem Format X.Y.Z, wobei X, Y und Z ganze Zahlen sind. Versionsänderungen darf nur inkrementelle sein.
6. In der **Betriebssystem-VHD-URL** Feld, geben Sie die SAS-URI für das Betriebssystem-VHD erstellt, und speichern Sie die Änderungen.

   > [!IMPORTANT]
   > Sie können keine Inkrement/Anzahl der Datenträger von einer aufgelisteten SKU Dekrement. Sie müssen in diesem Fall erstellen Sie eine neue SKU. Ausführliche Anweisungen finden Sie im Abschnitt [hinzufügen einen neuen SKU unter einer aufgelisteten Angebot](#add-a-new-sku-under-a-listed-offer).
   >
   >
7. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![VM-Images](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="update-the-nontechnical-details-of-an-offer-or-a-sku"></a>Aktualisieren Sie die nicht-technische Details eines Angebots oder einer SKU
Sie können die-Kenntnisse benötigt aktualisieren (marketing, rechtliche, unterstützen, Kategorien) Details zu den live-Angebot oder der SKU auf dem Markt.

### <a name="update-the-offer-description-and-logos"></a>Aktualisieren Sie die Beschreibung des Angebots und logos
Veröffentlichen Ihr Angebot, und aktualisieren Sie die Angebotsdetails, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **MARKETING** Registerkarte.
4. Klicken Sie auf **Englisch (USA)**.
5. Klicken Sie auf die **DETAILS** Registerkarte. In der **Beschreibung** im Abschnitt zu, aktualisieren Sie das Angebot **Titel**, **Zusammenfassung**, und **LONG SUMMARY** und die Änderungen zu speichern.

   > [!NOTE]
   > Wenn Sie die Details für die SKU aktualisieren, achten Sie darauf, dass Sie diese Einschränkungen: 
   * Geben Sie doppelte Text nicht für das Angebot Beschreibung und die SKU-Beschreibung ein.
   * Geben Sie doppelte Text nicht für die SKU-Titel und das Angebot lange Zusammenfassung ein. 
   * Geben Sie doppelte Text nicht für die SKU-Titel und die Zusammenfassung des Angebots ein.
   >
   >

    ![Details](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)
6. In der **LOGOS** Teil der **DETAILS** Registerkarte, aktualisieren Sie die Logos beschrieben. Stellen Sie sicher, dass die Logos befolgen Sie die [Azure Marketplace-Richtlinien](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).

   > [!NOTE]
   > Ein Symbol Hero ist optional. Sie können auswählen, nicht auf ein Symbol Hero hochladen. Nachdem ein Symbol Hero hochgeladen wurde, besteht jedoch keine Funktion aus der Veröffentlichung löschen Portal. Führen Sie die [Hero Symbol Richtlinien](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
   >
   >
7. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![Logos](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="update-the-sku-description"></a>Aktualisiert die SKU-Beschreibung
Veröffentlichen Ihr Angebot, und aktualisieren Sie die Details für die SKU, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **MARKETING** Registerkarte.
4. Klicken Sie auf **Englisch (USA)**.
5. Klicken Sie auf die **Pläne** Registerkarte. In der **SKUs** im Abschnitt zu, aktualisieren Sie die SKU **Titel**, **Zusammenfassung**, und **Beschreibung** und die Änderungen zu speichern.

   > [!NOTE]
   > Wenn Sie die Details für die SKU aktualisieren, achten Sie darauf, dass Sie diese Einschränkungen: 
   * Geben Sie doppelte Text nicht für das Angebot Beschreibung und die SKU-Beschreibung ein. 
   * Geben Sie doppelte Text nicht für die SKU-Titel und das Angebot lange Zusammenfassung ein. 
   * Geben Sie doppelte Text nicht für die SKU-Titel und die Zusammenfassung des Angebots ein.
   >
   >
6. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![Pläne](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="change-existing-links-or-add-new-links"></a>Ändern von vorhandenen Links oder neue Links hinzufügen
Ändern von vorhandenen Links oder neue Links hinzufügen und dann erneut veröffentlichen, Ihr Angebot, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **MARKETING** Registerkarte.
4. Klicken Sie auf **Englisch (USA)**.
5. Klicken Sie auf die **LINKS** Registerkarte.
6. Um einen neuen Link Hinzufügen der **Links** auf **+ Link "hinzufügen"**. In der **Link hinzufügen** Dialogfeld Geben Sie den Link **Titel** und **URL** und die Änderungen zu speichern. Sie können einen beliebigen Link eingeben, der Informationen enthält, mit dem Kunden können.
7. Klicken Sie zum Aktualisieren oder Löschen eines vorhandenen links, wählen Sie den Link, und klicken Sie auf die **bearbeiten** Schaltfläche oder die **löschen** Schaltfläche.

   > [!NOTE]
   > Stellen Sie sicher, dass die Links, die in diesem Abschnitt eingegebene ordnungsgemäß funktionieren, da diese Links während der Produktion Anforderungsprozess validiert werden.
   >
   >
8. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![Links](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Link hinzufügen](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="change-an-existing-sample-image-or-add-a-new-sample-image"></a>Ändern Sie einer vorhandenen Beispielbild oder fügen Sie ein neues Abbild des Beispiels
Ändern einer vorhandenen Beispielbild oder neue Beispielbilder hinzufügen und dann erneut veröffentlichen, Ihr Angebot, gehen Sie folgendermaßen vor:

> [!NOTE]
> Nur eine Probe-Bild wird angezeigt, der [Azure-Portal](https://portal.azure.com).
>
>

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **MARKETING** Registerkarte.
4. Klicken Sie auf **Englisch (USA)**.
5. Klicken Sie auf die **BEISPIELBILDER** Registerkarte.
6. Hinzuzufügende ein neues Image des Beispiels, in der **Beispielbilder** auf **ein neues IMAGE hochladen** und speichern Sie die Änderungen.

   > [!NOTE]
   > Einschließen einer Beispiel-Image ist optional.
   >
7. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![Beispielbilder](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="update-the-legal-content"></a>Rechtlichen Inhalt aktualisieren
Um Inhalte zu aktualisieren und erneut veröffentlichen, Ihr Angebot, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **MARKETING** Registerkarte.
4. Klicken Sie auf **Englisch (USA)**.
5. Klicken Sie auf die **rechtliche** Registerkarte. In der **rechtliche** im Abschnitt zu, aktualisieren Sie Ihre Richtlinien/Nutzungsbedingungen. Geben oder fügen Sie die Richtlinien/Begriffe in der **NUTZUNGSBESTIMMUNGEN** Feld und die Änderungen zu speichern.
6. Die Begrenzung der Zeichenanzahl für die rechtlichen Bedingungen von Nutzen ist 1 Mio. Zeichen.
7. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
8. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![Rechtsabteilung](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="update-the-support-information"></a>Aktualisieren Sie die Supportinformationen
Veröffentlichen Ihr Angebot, und aktualisieren Sie die Supportinformationen, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **Unterstützung** Registerkarte.
4. In der **Engineering wenden Sie sich an** im Abschnitt zu, aktualisieren Sie die technischen Details des Kontakts. Diese Informationen werden für die interne Kommunikation zwischen dem Partner und Microsoft nur verwendet werden.
5. In der **Kundensupport** Abschnitt, die Support-Kontaktdaten zu aktualisieren und die **unterstützen URL**. Diese Informationen werden für die interne Kommunikation zwischen dem Partner und Microsoft nur verwendet werden.

   > [!NOTE]
   > Wenn Sie nur e-Mail-Support bereitstellen möchten, geben Sie eine dummy Telefonnummer in den **Kundensupport** Abschnitt. In diesem Fall wird die eingegebene e-Mail stattdessen verwendet.
   >
   >
6. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![Unterstützung](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="update-the-categories"></a>Aktualisieren Sie die Kategorien
Zum Aktualisieren der Abschnitt "Kategorien" für Ihr Angebot, und veröffentlichen Ihr Angebot, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **Kategorien** Registerkarte.
4. In der **Kategorien** Abschnitt, aktualisieren Sie die Kategorien für Ihr Angebot und die Änderungen zu speichern. Sie können bis zu fünf Kategorien für den Azure Marketplace-Katalog auswählen.
5. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
6. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![Abschnitt „Categories“](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="add-a-new-sku-under-a-listed-offer"></a>Hinzufügen eines neuen SKUs unter einer aufgelisteten Angebot
Um eine neue SKU in Ihrem live Angebot hinzuzufügen, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **SKUS** Registerkarte. Klicken Sie dann auf **hinzufügen eine SKU**. 
4. Klicken Sie im Dialogfeld Geben Sie einen **SKU Bezeichner** in Kleinbuchstaben. Wählen Sie die **bringen Sie Ihre eigene Lizenz (BYOL) Abrechnungsmodell** Kontrollkästchen, wenn Sie die neue SKU mit einem BYOL Abrechnungsmodell veröffentlichen möchten. Andernfalls deaktivieren Sie das Kontrollkästchen. Klicken Sie auf den Teilstrich zum Erstellen eines neuen SKUs. Wenn Sie das Abrechnungsmodell BYOL aus gewählt haben, wird das Abrechnungsmodell automatisch auf stündlich festgelegt. Wenn Sie die kostenlose 30-Tage-Testversion für das stündliche Abrechnungsmodell möchten, wählen Sie **einmonatigen** für **ist eine kostenlose Testversion verfügbar?** Wählen Sie andernfalls **keine Testversion**. (**Ist eine kostenlose Testversion verfügbar?**  erscheint nur, wenn Sie beim Erstellen der neuen SKUs BYOL ausgewählt haben.)

   > [!IMPORTANT]
   > **Dieser SKU aus dem Marketplace ausblenden, da sie immer über eine Projektmappenvorlage gekauft werden sollten** muss **Ja** *nur* , wenn Sie für die Veröffentlichung einer Projektmappenvorlage genehmigt sind. Diese Option muss, andernfalls immer **keine**.
   >
   >
4. Klicken Sie im Menü auf der linken Seite auf die **VM-IMAGES** Registerkarte, und ermitteln Sie, die neue SKU, die Sie erstellt haben.
5. Informationen zum Einrichten der neuen SKU finden Sie unter [erhalten-Zertifizierung für die VM-Image](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) Anleitungen.
6. Um Marketingmaterial für die neue SKU hinzufügen zu können, finden Sie unter [bieten Marketplace marketing Inhalt](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Für die neue SKU Preisinformationen finden Sie unter [legen Sie die Preise](marketplace-publishing-push-to-staging.md#step-2-set-your-prices).
8. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
9. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

    ![SKUs](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Hinzufügen einer SKUs](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="change-the-data-disk-count-for-a-listed-sku"></a>Ändern Sie die Anzahl der Datenträger für eine aufgelistete SKU
Sie können keine Inkrement/Anzahl der Datenträger von einer aufgelisteten SKU Dekrement. Sie müssen in diesem Fall erstellen Sie eine neue SKU. Ausführliche Anweisungen finden Sie im Abschnitt [hinzufügen einen neuen SKU unter einer aufgelisteten Angebot](#add-a-new-sku-under-a-listed-offer).

## <a name="delete-a-listed-offer-from-the-marketplace"></a>Löscht ein aufgelisteten Angebot aus dem Marketplace
Verschiedene Aspekte bei der eine Anforderung zum Entfernen einer live-Angebot durchgeführt werden müssen. So erhalten Sie Anleitung vom Supportteam, um ein aufgelisteten Angebot aus dem Marketplace entfernen, gehen Sie folgendermaßen vor:

1. Lösen Sie ein Support-Ticket für die [einen Vorfall erstellen](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681) Seite.

2. Wählen Sie **Problemtyp** als **verwalten Angebote**, und wählen Sie **Kategorie** als **ändern ein Angebot und/oder SKU bereits in der Produktion**.
3. Senden Sie die Anforderung.

Das Support-Team führt Sie durch den Löschvorgang Angebot-SKU.

> [!NOTE]
> Sie können das Angebot immer löschen, während es sich im Status "Entwurf" (aber nicht Staging oder Produktion) befindet. Auf der **Verlauf** auf **Entwurf verwerfen**.
>
>

## <a name="delete-a-listed-sku-from-the-marketplace"></a>Löschen einer aufgelisteten SKUs vom Marketplace
Um eine aufgelisteten SKU vom Marketplace zu löschen, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).

2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Bereich auf der linken Seite auf die **SKUS** Registerkarte.
4. Wählen Sie die SKU, die Sie löschen möchten, und klicken Sie auf die **löschen** Schaltfläche.
5. Wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** das Angebot im Marketplace nur erneut veröffentlichen.
6. Nachdem das Angebot im Marketplace veröffentlicht wird, wird die SKU von Marketplace und Azure-Portal gelöscht.

## <a name="delete-the-current-version-of-a-listed-sku-from-the-marketplace"></a>Löschen Sie die aktuelle Version von einer aufgelisteten SKU vom Marketplace
Um die aktuelle Version von einer aufgelisteten SKU vom Marketplace zu löschen, gehen Sie folgendermaßen vor: 

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).

2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **VM-IMAGES** Registerkarte.
4. Wählen Sie die SKU, dessen aktuelle Version, die Sie löschen möchten, und klicken Sie auf die **löschen** Schaltfläche.
5. Wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** das Angebot im Marketplace nur erneut veröffentlichen.
6. Nachdem das Angebot im Marketplace veröffentlicht Ruft ab, wird die aktuelle Version der aufgeführten SKU von Marketplace und Azure-Portal gelöscht. Die SKU wird dann auf die vorherige Version zurückgesetzt.

## <a name="revert-the-listing-price-to-production-values"></a>Wiederherstellen des Preis Angebot Produktions-Werte
Den Preis Angebot Produktion Werte zurücksetzen möchten, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).
2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **Preise** Registerkarte.
4. Wählen Sie eine Region, deren Preise, die Sie zurücksetzen möchten.

    ![Preise Regionen](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)
5. Für SKUs mit einem stündlichen Abrechnungsmodell zurückgesetzt werden in der Produktion für den ausgewählten Bereich der Preise für alle Kerne. Für SKUs mit einer BYOL Rechnungsmodell zu erstellen, die SKU verfügbar machen in der Region durch Aktivieren des Kontrollkästchens für die SKU in die **EXTERNALLY-LICENSED (BYOL) SKU Verfügbarkeit** Abschnitt.

    ![Preismodelle](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)
6. Klicken Sie auf **AUTOPRICE andere Märkte auf der Grundlage von Preisen IN US**.

   > [!NOTE]
   > Die Bezeichnung der Schaltfläche kann abhängig vom Bereich unterscheiden, die Sie auswählen. Da wir USA ausgewählt haben, wird die Schaltfläche beschriftet **AUTOPRICE andere Märkte basierend auf Preise IN Vereinigte Staaten**.
   >
   >

    ![Autoprice](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)
7. Auf Seite 1 des Assistenten Autoprice, wählen Sie die Basis Market und klicken Sie auf die **Pfeil** Schaltfläche.

    ![Basis market](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)
8. Auf Seite 2, Service-Pläne und Messwerte (Kernen) und klicken Sie auf die **Pfeil** Schaltfläche.

    ![Service-Pläne und Messgeräte](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)
9. Klicken Sie auf Seite 3 **ein-/ausschalten alle** alle Regionen auswählen. Oder Sie können die Kontrollkästchen für bestimmte Regionen manuell auswählen. Klicken Sie auf die **Pfeil** Schaltfläche.

    ![Wählen Sie die Märkte](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)
10. Seite 4, überprüfen Sie die Wechselkurse und klicken Sie auf **Fertig stellen**. Der Assistent setzt die Preise je nach der Auswahl.

11. Auf der **Preise** auf **Zusammenfassung und Änderungen an**.
    Für **Version anzeigen**Option **Entwurf**, und für **vergleichen mit**Option **Produktion**. Wenn Sie keinen Preis Unterschied angezeigt wird, zurückgesetzt, die Preise auf die Werte für die Produktion erfolgreich.

    ![Anzeigen der Zusammenfassung und Änderungen](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)
12. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
13. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

## <a name="revert-the-billing-model-to-production-values"></a>Das Abrechnungsmodell in Produktion Werte zurücksetzen
Das Abrechnungsmodell in Produktion Werte zurücksetzen möchten, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).

2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **SKUS** Registerkarte.
4. Klicken Sie auf die **bearbeiten** Schaltfläche, um das Abrechnungsmodell wiederherzustellen. Wählen Sie im Fenster, das geöffnet wird, oder deaktivieren Sie die **Abrechnung und Lizenzierung erfolgt extern von Azure (aka Bring Your eigenen-Lizenz)** Kontrollkästchen.

    ![Bearbeiten Sie die Abrechnung](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)
5. Führen Sie die Schritte in "Revert Angebot Preis Produktion Werte" in diesem Artikel.
6. Wechseln Sie zu der **veröffentlichen** Registerkarte, und klicken Sie auf **PUSH zum STAGING**. Detaillierte Anleitungen für Ihr Angebot in der Stagingumgebung testen finden Sie unter [testen Sie Ihr VM-Angebot für den Marketplace](marketplace-publishing-vm-image-test-in-staging.md).
7. Nachdem Sie Ihr Angebot in der Stagingumgebung getestet haben, wechseln Sie zu der **veröffentlichen** Registerkarte in die Publishing Portal. Klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

## <a name="revert-the-visibility-setting-of-a-listed-sku-to-the-production-value"></a>Zurücksetzen der Einstellung Sichtbarkeit einer aufgelisteten SKUs auf den Produktionswert
Gehen Sie folgendermaßen vor, um die Sichtbarkeit-Einstellung von einer aufgelisteten SKU auf den Produktionswert rückgängig zu machen:

1. Melden Sie sich bei der [Portal veröffentlichen](https://publish.windowsazure.com).

2. Wechseln Sie zu der **VMs** , und wählen Sie Ihr Angebot.
3. Klicken Sie im Menü auf der linken Seite auf die **SKUS** Registerkarte.
4. Wählen Sie die SKU, und die Einstellung der Sichtbarkeit der SKU auf den Produktionswert zurückgesetzt.

    ![Sichtbarkeit](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)
5. Nachdem Sie die Änderungen abgeschlossen sind, klicken Sie auf **PUSH Genehmigung zu Produktion anfordern** Ihr Angebot im Marketplace nur erneut veröffentlichen.

## <a name="see-also"></a>Weitere Informationen:
* [Erste Schritte: Veröffentlichen eines Angebots an Azure Marketplace](marketplace-publishing-getting-started.md)
* [Verstehen der Auszahlung reporting](marketplace-publishing-report-payout.md)
* [Ändern Sie Ihre Cloud Solution Provider Reseller incentive](marketplace-publishing-csp-incentive.md)
* [Problembehandlung für häufige Probleme beim Veröffentlichen in Marketplace](marketplace-publishing-support-common-issues.md)
* [Erhalten Sie Unterstützung als Verleger](marketplace-publishing-get-publisher-support.md)
* [Erstellen Sie ein VM-Image einer lokalen](marketplace-publishing-vm-image-creation-on-premise.md)
* [Erstellen eines virtuellen Computers unter Windows in der Azure-vorschauportal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
