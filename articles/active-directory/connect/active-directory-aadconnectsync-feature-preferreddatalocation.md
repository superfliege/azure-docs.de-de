---
title: "Azure AD Connect-Synchronisierung: Konfigurieren des bevorzugten Datenspeicherorts für Multi-Geo Capabilities in Office 365 | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre Office 365-Benutzerressourcen mit der Azure AD Connect-Synchronisierung in der Nähe der Benutzer platzieren."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: billmath
ms.openlocfilehash: 021f009e66e57665a2252646b210f0e6dc55d33c
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="azure-ad-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Azure AD Connect-Synchronisierung: Konfigurieren des bevorzugten Datenspeicherorts für Office 365-Ressourcen
In diesem Thema wird ausführlich erläutert, wie Sie das PreferredDataLocation-Attribut in der Azure AD Connect-Synchronisierung konfigurieren. Wenn ein Kunde Multi-Geo Capabilities in Office 365 verwendet, wird mit diesem Attribut der geografische Standort der Office 365-Daten des Benutzers festgelegt. Die Benennungen **Region** und **geografischer Raum** werden synonym verwendet.

> [!IMPORTANT]
> Multi-Geo Capabilities befindet sich derzeit in der Vorschauphase. Wenn Sie am Vorschauprogramm teilnehmen möchten, wenden Sie sich an Ihren Microsoft-Vertreter.
>
>

## <a name="enable-synchronization-of-preferreddatalocation"></a>Aktivieren der Synchronisierung von „PreferredDataLocation“
Standardmäßig befinden sich die Office 365-Ressourcen für Ihre Benutzer im gleichen geografischen Raum wie Ihr Azure AD-Mandant. Wenn sich Ihr Mandant z.B. in Nordamerika befindet, befinden sich die Exchange-Postfächer der Benutzer ebenfalls in Nordamerika. Für ein multinationales Unternehmen ist dies möglicherweise nicht optimal. Der geografische Raum des Benutzers kann durch Festlegen des preferredDataLocation-Attributs definiert werden.

Durch das Festlegen dieses Attributs können Sie die Office 365-Ressourcen eines Benutzers, z.B. das Postfach und OneDrive, im selben geografischen Raum wie den Benutzer platzieren und verfügen immer noch über einen Mandanten für Ihre gesamte Organisation.

> [!IMPORTANT]
> Um Multi-Geo Capabilities nutzen zu können, müssen Sie über mindestens 5.000 Arbeitsplätze in Ihrem Office 365-Abonnement verfügen.
>
>

Eine Liste mit allen geografischen Räumen für Office 365 finden Sie auf der Seite [An welchem Ort befinden sich Ihre Daten?](https://aka.ms/datamaps).

Folgende geografische Räume stehen in Office 365 für Multi-Geo Capabilities zur Verfügung:

| geografischer Raum | preferredDataLocation-Wert |
| --- | --- |
| Asien-Pazifik | APC |
| Australien | AUS |
| Kanada | CAN |
| Europäische Union | EUR |
| Indien | IND |
| Japan | JPN |
| Südkorea | KOR |
| Vereinigtes Königreich | GBR |
| USA | NAM |

* Wenn ein geografische Raum nicht in dieser Tabelle aufgeführt wird (z.B. Südamerika), kann es nicht für Multi-Geo Capabilities verwendet werden.
* Die geografischen Räume Indien und Südkorea sind nur für Kunden mit Rechnungsadressen und in diesen geografischen Räumen erworbenen Lizenzen verfügbar.
* Nicht alle Office 365-Workloads unterstützen die Festlegung des geografischen Raums eines Benutzers.

Azure AD Connect unterstützt in Version 1.1.524.0 und höher die Synchronisierung des Attributs **PreferredDataLocation** für **Benutzerobjekte**. Genauer gesagt wurden folgende Änderungen eingeführt:

* Das Schema des Objekttyps **Benutzer** im Azure AD-Connector wird um das PreferredDataLocation-Attribut erweitert, das ein einwertiger Zeichenfolgentyp ist.
* Das Schema des Objekttyps **Person** in der Metaverse wird um das Attribut „PreferredDataLocation“ erweitert, das vom Typ „Zeichenfolge“ und einwertig ist.

Standardmäßig ist das PreferredDataLocation-Attribut für die Synchronisierung nicht aktiviert. Dieses Feature ist für größere Organisationen vorgesehen, da nicht jeder davon profitieren würde. Sie müssen darüber hinaus ein Attribut für die Office 365-Region Ihrer Benutzer angeben, da ein lokales Active Directory kein PreferredDataLocation-Attribut enthält. Dies ist für jede Organisation unterschiedlich.

> [!IMPORTANT]
> Derzeit kann in Azure AD das Attribut „PreferredDataLocation“ sowohl in synchronisierten Benutzerobjekten als auch Cloudbenutzerobjekten direkt mithilfe von Azure AD PowerShell konfiguriert werden. Nachdem Sie die Synchronisierung des Attributs „PreferredDataLocation“ aktiviert haben, dürfen Sie das Attribut nicht mehr in **synchronisierten Benutzerobjekten** mit Azure AD PowerShell konfigurieren, da Azure AD Connect diese basierend auf den Quellattributwerten im lokalen Active Directory überschreibt.

> [!IMPORTANT]
> Seit dem 1. September 2017 kann in Azure AD das PreferredDataLocation-Attribut in **synchronisierten Benutzerobjekten** nicht mehr direkt mithilfe von Azure AD PowerShell konfiguriert werden. Für die Konfiguration des PreferredLocation-Attributs in synchronisierten Benutzerobjekten müssen Sie Azure AD Connect verwenden.

Bevor Sie die Synchronisierung des Attributs „PreferredDataLocation“ aktivieren, müssen Sie folgende Schritte durchführen:

* Legen Sie zuerst fest, welches lokale Active Directory-Attribut als Quellattribut verwendet werden soll. Dieses muss vom Typ **einwertige Zeichenfolge** sein. In den Schritten unten wird eines der extensionAttributes verwendet.
* Wenn Sie das Attribut „PreferredDataLocation“ mithilfe von Azure AD PowerShell zuvor in vorhandenen synchronisierten Benutzerobjekten in Azure AD konfiguriert haben, müssen Sie ein **Backport** für die Attributwerte auf die entsprechenden Benutzerobjekte im lokalen Active Directory durchführen.

    > [!IMPORTANT]
    > Wenn Sie kein Backport für die Attributwerte auf die entsprechenden Benutzerobjekte im lokalen Active Directory durchführen, entfernt Azure AD Connect bei aktivierter Synchronisierung für das Attribut „PreferredDataLocation“ die vorhandenen Attributwerte in Azure AD.

* Es wird empfohlen, das Quellattribut jetzt in mindestens einigen lokalen AD-Benutzerobjekten zu konfigurieren, die später für die Überprüfung verwendet werden.

Die Schritte zum Aktivieren der Synchronisierung des Attributs „PreferredDataLocation“ können wie folgt zusammengefasst werden:

1. Deaktivieren des Synchronisierungsplaners und Sicherstellen, dass derzeit keine Synchronisierung ausgeführt wird
2. Hinzufügen des Quellattributs zum lokalen AD DS-Connectorschema
3. Hinzufügen von PreferredDataLocation zum Azure AD-Connectorschema
4. Erstellen einer Synchronisierungsregel für eingehende Daten zur Übertragung des Attributwerts aus dem lokalen Active Directory
5. Erstellen einer Synchronisierungsregel für ausgehende Daten zur Übertragung des Attributwerts an Azure AD
6. Ausführen des vollständigen Synchronisierungszyklus
7. Aktivieren des Synchronisierungsplaners
8. Überprüfen des Ergebnisses

> [!NOTE]
> Im restlichen Teil dieses Abschnitts werden diese Schritte ausführlich erläutert. Sie werden im Kontext einer Azure AD-Bereitstellung mit einer Topologie mit einer einzelnen Gesamtstruktur und ohne benutzerdefinierte Synchronisierungsregeln beschrieben. Wenn Sie über eine Topologie mit mehreren Gesamtstrukturen verfügen, benutzerdefinierte Synchronisierungsregeln konfiguriert haben oder einen Stagingserver verwenden, müssen Sie die Schritte entsprechend anpassen.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Schritt 1: Deaktivieren des Synchronisierungsplaners und Sicherstellen, dass derzeit keine Synchronisierung ausgeführt wird
Stellen Sie sicher, dass keine Synchronisierung ausgeführt wird, während Sie Synchronisierungsregeln aktualisieren. So vermeiden Sie, dass unbeabsichtigte Änderungen nach Azure AD exportiert werden. So deaktivieren Sie den integrierten Synchronisierungsplaner:

1. Starten Sie eine PowerShell-Sitzung auf dem Azure AD Connect-Server.
2. Deaktivieren Sie die geplante Synchronisierung durch Ausführung des Cmdlets `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Starten Sie **Synchronization Service Manager**, indem Sie zu **START** > **Synchronization Service** navigieren.
4. Wechseln Sie zur Registerkarte **Vorgänge**, und vergewissern Sie sich, dass kein Vorgang mit dem Status *In Arbeit* angezeigt wird.

![Synchronization Service Manager – Überprüfen, dass keine Vorgänge durchgeführt werden](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema"></a>Schritt 2: Hinzufügen des Quellattributs zum lokalen AD DS-Connectorschema
Nicht alle AD-Attribute werden in den lokale AD-Connectorbereich importiert. Wenn Sie ein Attribut ausgewählt haben, das in der Standardeinstellung nicht synchronisiert wird, müssen Sie es importieren. So fügen Sie das Quellattribut zur Liste der importierten Attribute hinzu:

1. Wechseln Sie in Synchronization Service Manager zur Registerkarte **Connectors**.
2. Klicken Sie mit der rechten Maustaste auf den **lokalen AD-Connector**, und wählen Sie **Eigenschaften** aus.
3. Navigieren Sie im Popupdialogfeld zur Registerkarte **Attribute auswählen**.
4. Stellen Sie sicher, dass das von Ihnen ausgewählte Quellattribut in der Attributliste aktiviert ist. Wenn das Attribut nicht angezeigt wird, klicken Sie auf das Kontrollkästchen „Alle anzeigen“.
5. Klicken Sie zum Speichern auf **OK**.

![Hinzufügen des Quellattributs zum lokalen AD-Connectorschema](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Schritt 3: Hinzufügen von PreferredDataLocation zum Azure AD-Connectorschema
Standardmäßig wird das PreferredDataLocation-Attribut nicht in den Azure AD Connect-Bereich importiert. So fügen Sie das Attribut „PreferredDataLocation“ zur Liste der importierten Attribute hinzu:

1. Wechseln Sie in Synchronization Service Manager zur Registerkarte **Connectors**.
2. Klicken Sie mit der rechten Maustaste auf den **Azure AD-Connector**, und wählen Sie **Eigenschaften** aus.
3. Navigieren Sie im Popupdialogfeld zur Registerkarte **Attribute auswählen**.
4. Wählen Sie das PreferredDataLocation-Attribut in der Attributliste aus.
5. Klicken Sie zum Speichern auf **OK**.

![Hinzufügen des Quellattributs zum Azure AD-Connectorschema](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Schritt 4: Erstellen einer Synchronisierungsregel für eingehende Daten zur Übertragung des Attributwerts aus dem lokalen Active Directory
Die Synchronisierungsregel für eingehende Daten ermöglicht die Übertragung des Attributwerts aus dem Quellattribut im lokalen Active Directory auf die Metaverse:

1. Starten Sie den **Synchronisierungsregel-Editor**, indem Sie zu **START** > **Synchronisierungsregel-Editor** navigieren.
2. Legen Sie den Suchfilter **Richtung** auf **Eingehend** fest.
3. Klicken Sie auf die Schaltfläche **Neue Regel hinzufügen**, um eine neue Regel für eingehende Daten zu erstellen.
4. Geben Sie auf der Registerkarte **Beschreibung** die folgende Konfiguration an:

    | Attribut | Wert | Details |
    | --- | --- | --- |
    | NAME | *Geben Sie einen Namen ein.* | Beispiel: *In from AD – User PreferredDataLocation* |
    | BESCHREIBUNG | *Geben Sie eine benutzerdefinierte Beschreibung an.* |  |
    | Verbundenes System | *Wählen Sie den lokalen AD-Connector aus.* |  |
    | Objekttyp des verbundenen Systems | **Benutzer** |  |
    | Metaverse-Objekttyp | **Person** |  |
    | Verknüpfungstyp | **Join** |  |
    | Rangfolge | *Wählen Sie eine Zahl zwischen 1 und 99 aus.* | Die Zahlen zwischen 1 und 99 sind für benutzerdefinierte Synchronisierungsregeln reserviert. Wählen Sie keinen Wert aus, der von einer anderen Synchronisierungsregel verwendet wird. |

5. Lassen Sie die **Bereichsfilter** leer, um alle Objekte einzubeziehen. Möglicherweise müssen Sie den Bereichsfilter entsprechend Ihrer Azure AD Connect-Bereitstellung anpassen.
6. Navigieren Sie zur Registerkarte **Transformation**, und implementieren Sie folgende Transformationsregel:

    | Flowtyp | Zielattribut | Quelle | Einmal anwenden | Zusammenführungstyp |
    | --- | --- | --- | --- | --- |
    |Direkt | PreferredDataLocation | Wählen Sie das Quellattribut aus. | Deaktiviert | Aktualisieren |

7. Klicken Sie auf **Hinzufügen**, um die Regel für eingehende Daten zu erstellen.

![Erstellen einer Synchronisierungsregel für eingehende Daten](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Schritt 5: Erstellen einer Synchronisierungsregel für ausgehende Daten zur Übertragung des Attributwerts an Azure AD
Die Synchronisierungsregel für ausgehende Daten ermöglicht die Übertragung des Attributwerts von der Metaverse auf das Attribut „PreferredDataLocation“ in Azure AD:

1. Navigieren Sie zum **Synchronisierungsregel-Editor**.
2. Legen Sie den Suchfilter **Richtung** auf **Ausgehend** fest.
3. Klicken Sie auf die Schaltfläche **Neue Regel hinzufügen**.
4. Geben Sie auf der Registerkarte **Beschreibung** die folgende Konfiguration an:

    | Attribut | Wert | Details |
    | ----- | ------ | --- |
    | NAME | *Geben Sie einen Namen ein.* | Beispiel: „Out to AAD – User PreferredDataLocation“ |
    | BESCHREIBUNG | *Geben Sie eine Beschreibung ein.* ||
    | Verbundenes System | *Wählen Sie den AAD-Connector aus.* ||
    | Objekttyp des verbundenen Systems | Benutzer ||
    | Metaverse-Objekttyp | **Person** ||
    | Verknüpfungstyp | **Join** ||
    | Rangfolge | *Wählen Sie eine Zahl zwischen 1 und 99 aus.* | Die Zahlen zwischen 1 und 99 sind für benutzerdefinierte Synchronisierungsregeln reserviert. Wählen Sie keinen Wert aus, der von einer anderen Synchronisierungsregel verwendet wird. |

5. Navigieren Sie zur Registerkarte **Bereichsfilter**, und fügen Sie eine **einzelne Bereichsfiltergruppe mit zwei Klauseln hinzu**:

    | Attribut | Operator | Wert |
    | --- | --- | --- |
    | sourceObjectType | EQUAL | Benutzer |
    | cloudMastered | NOTEQUAL | True |

    Der Bereichsfilter legt fest, auf welche Azure AD-Objekte diese Synchronisierungsregel für ausgehende Daten angewendet wird. In diesem Beispiel verwenden wir den Bereichsfilter aus der OOB-Synchronisierungsregel „Out to AD – User Identity“. Er verhindert, dass die Synchronisierungsregel auf Benutzerobjekte angewendet wird, die nicht über das lokale Active Directory synchronisiert wurden. Möglicherweise müssen Sie den Bereichsfilter entsprechend Ihrer Azure AD Connect-Bereitstellung anpassen.

6. Navigieren Sie zur Registerkarte **Transformation**, und implementieren Sie folgende Transformationsregel:

    | Flowtyp | Zielattribut | Quelle | Einmal anwenden | Zusammenführungstyp |
    | --- | --- | --- | --- | --- |
    | Direkt | PreferredDataLocation | PreferredDataLocation | Deaktiviert | Aktualisieren |

7. Klicken Sie auf **Hinzufügen**, um die Regel für ausgehende Daten zu erstellen.

![Erstellen einer Synchronisierungsregel für ausgehende Daten](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>Schritt 6: Ausführen des vollständigen Synchronisierungszyklus
Der vollständige Synchronisierungszyklus ist im Allgemeinen erforderlich, da wir sowohl zum AD- als auch zum Azure AD-Connectorschema neue Attribute hinzugefügt und benutzerdefinierte Synchronisierungsregeln eingeführt haben. Es wird empfohlen, die Änderungen vor dem Export nach Azure AD zu überprüfen. Anhand der folgenden Schritte können Sie die Änderungen überprüfen, während Sie die Schritte des vollständigen Synchronisierungszyklus manuell ausführen.

1. Führen Sie im **lokalen AD-Connector** den Schritt **Vollständiger Import** aus:

   1. Wechseln Sie in Synchronization Service Manager zur Registerkarte **Vorgänge**.
   2. Klicken Sie mit der rechten Maustaste auf den **lokalen AD-Connector**, und wählen Sie **Ausführen** aus.
   3. Wählen Sie im Popupdialogfeld **Vollständiger Import** aus, und klicken Sie auf **OK**.
   4. Warten Sie, bis der Vorgang abgeschlossen ist.

    > [!NOTE]
    > Sie können den Schritt „Vollständiger Import“ im lokalen AD-Connector überspringen, wenn das Quellattribut bereits in der Liste der importierten Attribute enthalten ist. Mit anderen Worten: Sie mussten bei [Schritt 2 zum Hinzufügen des Quellattributs zum lokalen AD-Connectorschemas](#step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema) keine Änderungen vornehmen.

2. Führen Sie im **Azure AD-Connector** den Schritt **Vollständiger Import** aus:

   1. Klicken Sie mit der rechten Maustaste auf den **Azure AD-Connector**, und wählen Sie **Ausführen...** aus.
   2. Wählen Sie im Popupdialogfeld **Vollständiger Import** aus, und klicken Sie auf **OK**.
   3. Warten Sie, bis der Vorgang abgeschlossen ist.

3. Überprüfen Sie die Änderungen an den Synchronisierungsregeln zu einem vorhandenen Benutzerobjekt:

Das Quellattribut aus dem lokalen Active Directory und der PreferredDataLocation aus Azure AD wurden in den jeweiligen Connectorbereich importiert. Bevor Sie mit dem Schritt „Vollständige Synchronisierung“ fortfahren, wird empfohlen, eine **Vorschau** zu einem vorhandenen Benutzerobjekt im lokalen AD-Connectorbereich auszuführen. In dem von Ihnen ausgewählten Objekt muss das Quellattribut aufgefüllt sein. Eine erfolgreiche **Vorschau**, bei dem „PreferredDataLocation“ in der Metaverse aufgefüllt ist, ist ein guter Indikator dafür, dass Sie die Synchronisierungsregeln korrekt konfiguriert haben. Informationen zum Durchführen einer **Vorschau** finden Sie im Abschnitt [Überprüfen der Änderungen](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Führen Sie im **lokalen AD-Connector** den Schritt **Vollständige Synchronisierung** aus:

   1. Klicken Sie mit der rechten Maustaste auf den **lokalen AD-Connector**, und wählen Sie **Ausführen** aus.
   2. Wählen Sie im Popupdialogfeld **Vollständige Synchronisierung** aus, und klicken Sie auf **OK**.
   3. Warten Sie, bis der Vorgang abgeschlossen ist.

5. Stellen Sie sicher, dass **Ausstehende Exporte** in Azure AD festgelegt ist, indem Sie wie folgt vorgehen:

   1. Klicken Sie mit der rechten Maustaste auf den **Azure AD-Connector**, und wählen Sie **Search Connector Space** (Connectorbereich durchsuchen) aus.
   2. Gehen Sie im Popupdialogfeld „Connectorbereich durchsuchen“ wie folgt vor:

      1. Legen Sie den **Bereich** auf **Ausstehender Export** fest.
      2. Aktivieren Sie alle drei Kontrollkästchen: **Hinzufügen, Ändern und Löschen**.
      3. Klicken Sie auf die Schaltfläche **Durchsuchen**, um die Liste von Objekten mit Änderungen, die exportiert werden sollen, abzurufen. Um die Änderungen für ein bestimmtes Objekt zu untersuchen, doppelklicken Sie auf das Objekt.
      4. Stellen Sie sicher, dass die Änderungen Ihren Erwartungen entsprechen.

6. Führen Sie im **Azure AD-Connector** den Schritt **Exportieren** aus.

   1. Klicken Sie mit der rechten Maustaste auf den **Azure AD-Connector**, und wählen Sie **Ausführen** aus.
   2. Wählen Sie im Popupdialogfeld „Connector ausführen“ den Schritt **Exportieren** aus, und klicken Sie auf **OK**.
   3. Warten Sie, bis der Export nach Azure AD abgeschlossen ist.

> [!NOTE]
> Sie werden möglicherweise feststellen, dass die Schritte „Vollständige Synchronisierung“ auf dem Azure AD-Connector und „Exportieren“ zum AD-Connector nicht enthalten sind. Diese Schritte sind nicht erforderlich, da die Attributwerte lediglich von dem lokalen Active Directory auf Azure AD übertragen werden.

## <a name="step-7-re-enable-sync-scheduler"></a>Schritt 7: Erneutes Aktivieren des Synchronisierungsplaners
So aktivieren Sie erneut den integrierten Synchronisierungsplaner:

1. Starten Sie eine PowerShell-Sitzung.
2. Aktivieren Sie die geplante Synchronisierung durch Ausführung des Cmdlets `Set-ADSyncScheduler -SyncCycleEnabled $true` wieder.

## <a name="step-8-verify-the-result"></a>Schritt 8: Überprüfen des Ergebnisses
Sie können nun die Konfiguration überprüfen und für Ihre Benutzer aktivieren.

1. Fügen Sie dem ausgewählte Attribut für einen Benutzer den geografischen Raum hinzu. Die Liste der verfügbaren geografischen Räume finden Sie in [dieser Tabelle](#enable-synchronization-of-preferreddatalocation).  
![AD-Attribut wurde einem Benutzer hinzugefügt](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Warten Sie, bis das Attribut mit Azure AD synchronisiert wurde.
3. Überprüfen Sie mithilfe von Exchange Online-PowerShell, ob die Postfachregion ordnungsgemäß festgelegt wurde.  
![Festlegen der Postfachregion für einen Benutzer in Exchange Online](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Sofern Ihr Mandant für die Verwendung dieses Features aktiviert wurde, wird das Postfach in den richtigen geografischen Raum verschoben. Dies können Sie überprüfen, indem Sie sich den Namen des Servers ansehen, auf dem sich das Postfach befindet.
4. Um sicherzustellen, dass diese Einstellung auf mehrere Postfächer angewendet wurde, verwenden Sie das Skript im [Technet-Katalog](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Dieses Skript enthält auch eine Liste der Serverpräfixe aller Office 365-Rechenzentren mit dem geografischen Raum, in dem sie sich befinden. Es kann im vorherigen Schritt als Referenz verwendet werden, um den Speicherort des Postfachs zu überprüfen.

## <a name="next-steps"></a>Nächste Schritte

**Weitere Informationen zu Multi-Geo Capabilities in Office 365:**

* Multi-Geo-Sitzungen auf der Ignite: https://aka.ms/MultiGeoIgnite
* Multi-Geo in OneDrive: https://aka.ms/OneDriveMultiGeo
* Multi-Geo in SharePoint Online: https://aka.ms/SharePointMultiGeo

**Weitere Informationen zum Konfigurationsmodell im Synchronisierungsmodul:**

* Weitere Informationen zum Konfigurationsmodell finden Sie unter [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md)(Grundlegendes zur deklarativen Bereitstellung).
* Weitere Informationen zur Ausdruckssprache finden Sie unter [Grundlegendes zu Ausdrücken für die deklarative Bereitstellung](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Übersichtsthemen**

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)
