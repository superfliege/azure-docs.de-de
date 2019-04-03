---
title: Korrigieren von geänderten Standardregeln – Azure AD Connect | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie geänderte Standardregeln korrigieren, die mit Azure AD Connect bereitgestellt werden.
services: active-directory
author: billmath
manager: daveba
editor: curtand
ms.reviewer: darora10
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f524e8cef3878816cec53575217bdb6d0fd9be7b
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500932"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Korrigieren von geänderten Standardregeln in Azure AD Connect

Azure AD Connect wird mit Standardregeln für die Synchronisierung ausgeliefert.  Diese Regeln gelten leider nicht universell für alle Unternehmen, und es kann Situationen geben, in denen Sie sie aufgrund Ihrer Anforderungen ändern müssen.

 Wenn Sie die Standardregeln geändert haben oder eine Änderung planen, dann nehmen Sie sich bitte einen Moment Zeit, um dieses Dokument zu lesen.

Dieses Dokument enthält 2 Beispiele für die häufigsten von Benutzern vorgenommenen Anpassungen, und erklärt die richtige Vorgehensweise dafür.

>[!NOTE] 
> Bestehende Standardregeln zu ändern, um eine erforderliche Anpassung zu erreichen, wird nicht unterstützt, weil diese Regeln dann in zukünftigen Versionen nicht mehr auf die neueste Version aktualisiert werden könnten. Dadurch würde verhindert, dass Sie benötigte Fehlerbehebungen und neue Features erhalten.  In diesem Dokument wird erklärt, wie Sie das gleiche Ergebnis erzielen können, ohne die bestehenden Standardregeln zu ändern. 

## <a name="how-to-identify-modified-default-rules"></a>Woran erkennt man geänderte Standardregeln?
Seit Version 1.3.7.0 von **Azure AD Connect** ist es nun einfach, geänderte Standardregel zu identifizieren. Sie können zu Apps auf dem Desktop gehen und auf **Synchronisierungsregel-Editor** klicken.

![Editor](media/how-to-connect-fix-default-rules/default1.png)

Im Editor werden alle geänderten Standardregeln mit einem Symbol vor dem Namen angezeigt, wie unten dargestellt:

![icon](media/how-to-connect-fix-default-rules/default2.png)

 Außerdem sehen Sie daneben eine deaktivierte Regel mit gleichem Namen, die die standardmäßige Standardregel ist:

![Standardregeln](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Häufige Anpassungen
Im Folgenden sind häufige Anpassungen der Standardregeln aufgeführt:

- [Ändern des Attributflusses](#changing-attribute-flow)
- [Ändern eines Bereichsfilters](#changing-scoping-filter)
- [Ändern einer Verknüpfungsbedingung](#changing-join-condition)

## <a name="before-changing-any-rules"></a>Vor dem Ändern von Regeln
- Deaktivieren Sie den Synchronisierungsplaner.  Der Synchronisierungsplaner wird standardmäßig alle 30 Minuten ausgeführt. Stellen Sie sicher, dass er nicht gestartet wird, während Sie Änderungen vornehmen und Probleme in Ihren neuen Regeln beheben. Um den Scheduler vorübergehend zu deaktivieren, starten Sie PowerShell, und führen Sie den Befehl `Set-ADSyncScheduler -SyncCycleEnabled $false` aus.
 ![Standardregeln](media/how-to-connect-fix-default-rules/default3.png)

- Die Änderung eines Bereichsfilters kann zum Löschen von Objekten im Zielverzeichnis führen. Bitte seien Sie vorsichtig, bevor Sie Änderungen an den Objektbereichen vornehmen. Es wird empfohlen, zunächst einen Stagingserver zu ändern, bevor Sie Änderungen am aktiven Server vornehmen.
- Führen Sie eine Vorschau für ein einzelnes Objekt aus, wie im Abschnitt [Überprüfen der Synchronisierungsregel](#validate-sync-rule) beschrieben, nachdem Sie eine neue Regel hinzugefügt haben.
- Führen Sie eine vollständige Synchronisierung durch, nachdem Sie eine neue Regel hinzugefügt oder eine benutzerdefinierte Synchronisierungsregel geändert haben. Diese Synchronisierung wendet neue Regeln auf alle Objekte an.

## <a name="changing-attribute-flow"></a>Ändern des Attributflusses
Es gibt 3 verschiedene Szenarien für den Attributfluss. Betrachten wir, wie Sie dies erreichen, ohne die standardmäßige Standardregeln zu ändern.
- Hinzufügen eines neuen Attributs
- Überschreiben des Werts des vorhandenen Attributs
- Vorhandenes Attribut nicht synchronisieren

### <a name="adding-new-attribute"></a>Hinzufügen eines neuen Attributs:
Wenn Sie feststellen, dass ein Attribut nicht aus dem Quellverzeichnis in das Zielverzeichnis fließt, können Sie [Azure AD Connect-Synchronisierung: Verzeichniserweiterungen](how-to-connect-sync-feature-directory-extensions.md) verwenden, um die neuen Attribute zu übergeben.

Beachten Sie, dass Sie immer zuerst [Azure AD Connect-Synchronisierung: Verzeichniserweiterungen](how-to-connect-sync-feature-directory-extensions.md) verwenden sollten, ein von Azure AD Connect bereitgestelltes Standardfeature. Wenn dieses Feature für Sie jedoch nicht funktioniert, führen Sie die folgenden Schritte aus, um ein Attribut zu übergeben, ohne die bestehende standardmäßige Standardssynchronisierungsregel zu ändern. Dazu fügen Sie zwei neue Synchronisierungsregeln hinzu.


#### <a name="add-an-inbound-sync-rule"></a>Hinzufügen einer Synchronisierungsregel für eingehenden Datenverkehr:
Eine Synchronisierungsregel für eingehenden Datenverkehr bedeutet, dass die Quelle für das Attribut ein Connectorbereich und das Ziel eine Metaverse ist. Um beispielsweise ein neues Attribut vom lokalen Active Directory an das Azure Active Directory zu übergeben, erstellen Sie eine neue Synchronisierungsregel für eingehenden Datenverkehr, indem Sie den **Synchronisierungsregel-Editor** starten, dann für „Richtung“ **Eingehend** auswählen und auf **Neue Regel hinzufügen** klicken. 

 ![Standardregeln](media/how-to-connect-fix-default-rules/default3a.png)

Folgen Sie bei der Benennung der Regel Ihrer eigenen Namenskonvention. Hier haben wir **Benutzerdefiniert eingehend von AD – Benutzer** verwendet. Das bedeutet, dass die Regel eine benutzerdefinierte Eingangsregel vom AD-Connectorbereich zur Metaverse ist. 

 ![Standardregeln](media/how-to-connect-fix-default-rules/default3b.png)

Geben Sie Ihre eigene Beschreibung für die Regel ein, um die zukünftige Wartung der Regel zu erleichtern. Geben Sie z.B. an, was das Ziel dieser Regel ist und warum sie benötigt wurde.
Ausgewähltes verbundenes System (die Gesamtstruktur) ist die Quelle des Attributs. Dann „Objekttyp des verbundenen Systems“ und „Metaverse-Objekttyp“.

Geben Sie den Rangfolgenwert zwischen 0 und 99 an (je niedriger die Zahl, desto höher die Rangfolge). Übernehmen Sie für andere Felder wie „Tag“, „Kennwortsynchronisierung aktivieren“ und „Deaktiviert“ die Standardeinstellungen.

Lassen Sie „Bereichsfilter“ leer, das bedeutet, dass die Regel für alle zwischen dem mit AD verbundenen System und der Metaverse verknüpften Objekte gilt.

Lassen Sie „Verknüpfungsregeln“ leer, was bedeutet, dass die Regel die in der standardmäßigen Standardregel definierte Verknüpfungsbedingung nutzt. Dies ist ein weiterer Grund, die standardmäßige Standardregel nicht zu deaktivieren/löschen, denn wenn es keine Verknüpfungsbedingung gibt, auf die zurückgegriffen werden kann, kann das Attribut nicht übergeben werden. 

Fügen Sie eine geeignete Transformation für Ihr Attribut hinzu. Sie können eine Konstante zuweisen, um einen konstanten Wert an Ihr Zielattribut zu übergeben, oder verwenden Sie eine direkte Zuordnung zwischen Quell- und Zielattribut, oder einen Ausdruck für das Attribut. Hier finden Sie verschiedene [expression-Funktionen](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference), die Sie verwenden können.

#### <a name="add-an-outbound-sync-rule"></a>Hinzufügen einer Synchronisierungsregel für ausgehenden Datenverkehr:
Bisher haben wir nur eine Synchronisationsregel für eingehenden Datenverkehr hinzugefügt. Dies ist nur die halbe Arbeit, da das Attribut noch nicht mit dem Zielverzeichnis verknüpft ist. Um das Attribut mit dem Zielverzeichnis zu verknüpfen, müssen Sie eine Ausgangsregel erstellen, d.h. die Quelle ist die Metaverse und das Ziel das verbundene System. Um eine Ausgangsregel zu erstellen, starten Sie den **Synchronisierungsregel-Editor**, ändern Sie die **Richtung** in **Ausgehend**, und klicken Sie auf **Neue Regel hinzufügen**. 

![Standardregeln](media/how-to-connect-fix-default-rules/default3c.png)

Wie in der Eingangsregel können Sie zur **Benennung** der Regel Ihre eigene Namenskonvention verwenden. Wählen Sie das **verbundene System** als Azure AD-Mandant und das verbundene Systemobjekt aus, auf das Sie den Attributwert festlegen möchten. Legen Sie die Rangfolge auf einen Wert zwischen 0 und 99 fest. 

![Standardregeln](media/how-to-connect-fix-default-rules/default3d.png)

Lassen Sie **Bereichsfilter** und **Verknüpfungsregeln** leer, und wählen Sie für die Transformation „Constant“ (Konstant), „Direct“ (Direkt) oder „expression“ (Ausdruck) aus. 

In diesem Beispiel wurde gezeigt, wie man ein neues Attribut für ein Benutzerobjekt von Active Directory an Azure Active Directory fließen lässt. Sie können diese Schritte verwenden, um jedem beliebigen Attribut jedes beliebigen Objekts eine Quelle und ein Ziel zuzuordnen.  Weitere Informationen finden Sie unter [Erstellen benutzerdefinierter Synchronisierungsregeln](how-to-connect-create-custom-sync-rule.md) und [Vorbereiten der Bereitstellung von Benutzern](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization).

### <a name="overriding-value-of-existing-attribute"></a>Überschreiben des Werts des vorhandenen Attributs
Möglicherweise möchten Sie den Wert eines bereits zugeordneten Attributs überschreiben. Wenn Sie z.B. in Azure AD für ein Attribut immer einen Nullwert festlegen möchten, können Sie dies tun, indem Sie nur eine Eingangsregel erstellen, wie im vorherigen Schritt beschrieben, und den konstanten Wert **AuthoritativeNull** für das Zielattribut übergeben. Beachten Sie, dass in diesem Fall „AuthoritativeNull“ anstelle von „Null“ verwendet wurde. Grund hierfür ist, dass ein Wert ungleich Null einen „Null“-Wert auch dann ersetzt, wenn er eine geringere Rangfolge (einen höheren Nummernwert in der Regel) aufweist. Ein „AuthoritativeNull“-Wert hingegen, wird die als „Null“ behandelt und nicht durch Werte ungleich Null aus anderen Regeln ersetzt. 

### <a name="dont-sync-existing-attribute"></a>Vorhandenes Attribut nicht synchronisieren
Wenn Sie ein Attribut von der Synchronisierung ausschließen möchten, können Sie die Attributfilterfunktion in Azure AD Connect verwenden. Starten Sie **Azure AD Connect** über das Desktopsymbol, und wählen Sie dann **Synchronisierungsoptionen anpassen** aus.

![Standardregeln](media/how-to-connect-fix-default-rules/default4.png)

 Stellen Sie sicher, dass **Azure AD-App und Attributfilterung** ausgewählt ist, und klicken Sie auf **Weiter**.

![Standardregeln](media/how-to-connect-fix-default-rules/default5.png)

Deaktivieren Sie die Attribute, die Sie von der Synchronisierung ausschließen möchten.

![Standardregeln](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="changing-scoping-filter"></a>Ändern eines Bereichsfilters
Die Azure AD Sync kümmert sich die meisten Objekte. Sie können den Bereich der Objekte verkleinern und festlegen, dass weniger Objekte auf unterstützte Weise exportiert werden, ohne die standardmäßigen Standardsynchronisierungsregeln zu ändern. Wenn Sie den Objektbereich der vergrößern möchten, können Sie die bestehende Regel **bearbeiten** klonen und die ursprüngliche Regel deaktivieren. Microsoft empfiehlt, den von Azure AD Connect konfigurierten Bereich nicht zu vergrößern. Wenn Sie den Objektbereich vergrößern, wird es für das Supportteam schwieriger, die Anpassungen zu verstehen und das Produkt zu unterstützen.

Hier wird beschrieben, wie Sie den Bereich der mit Azure AD synchronisierten Objekte verkleinern. Beachten Sie Folgendes: Wenn Sie den Bereich der synchronisierten **Benutzer** verkleinern, wird auch die Kennworthashsynchronisierung für die herausgefilterten Benutzer beendet. Wenn die Objekte bereits synchronisiert werden, werden die herausgefilterten Objekte nach der Verkleinerung des Bereichs aus dem Zielverzeichnis gelöscht. Gehen Sie daher bitte sehr sorgfältig mit Bearbeitungen des Bereichs um.
Hier werden die unterstützten Methoden zum Verkleinern des Bereichs der synchronisierten Objekte beschrieben.

- [cloudFiltered-Attribut](#cloudfiltered-attribute)
- [Filtern von Organisationseinheiten](#ou-filtering)

### <a name="cloudfiltered-attribute"></a>cloudFiltered-Attribut
Beachten Sie, dass dies kein Attribut ist, das in Active Directory festgelegt werden kann. Sie müssen den Wert dieses Attributs festlegen, indem Sie eine neue Eingangsregel hinzufügen, wie im Abschnitt **Überschreiben des Werts des vorhandenen Attributs** beschrieben. Anschließend können Sie dann die **Transformation** verwenden und dieses Attribut in der Metaverse auf **Expression** festlegen. Im folgenden Beispiel sollen alle Benutzer, deren Abteilungsname mit **HRD** (ohne Berücksichtigung der Groß-/Kleinschreibung) beginnt nicht synchronisiert werden:

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

Zunächst wird die Abteilung von der Quelle (Active Directory) in Kleinbuchstaben umgewandelt. Dann wurden mit der Left-Funktion die ersten 3 Zeichen mit „hrd“ verglichen. Wurde eine Übereinstimmung gefunden, wurde der Wert auf „True“ festgelegt, ansonsten auf „NULL“. Beachten Sie, dass der Wert NULL verwendet wird, sodass eine andere Regel mit geringerer Rangfolge (höherem Zahlenwert) den Wert mit anderer Bedingung überschreiben kann. Führen Sie eine Vorschau für ein Objekt aus, um die Synchronisierungsregel zu überprüfen, wie im Abschnitt [Überprüfen der Synchronisierungsregel](#validate-sync-rule) beschrieben.

![Standardregeln](media/how-to-connect-fix-default-rules/default7a.png)



### <a name="ou-filtering"></a>Filtern von Organisationseinheiten
Sie können eine oder mehrere Organisationseinheiten erstellen und die Objekte verschieben, die mit diesen Organisationseinheiten nicht synchronisiert werden sollen. Dann konfigurieren Sie die Filterung der Organisationseinheiten in Azure AD Connect, indem Sie **Azure AD Connect** über das Desktopsymbol starten und die Optionen wie unten gezeigt auswählen. Sie können die Filterung der Organisationseinheiten auch zum Zeitpunkt der Installation von Azure AD Connect konfigurieren. 

![Standardregeln](media/how-to-connect-fix-default-rules/default8.png)

Folgen Sie den Anweisungen des Assistenten, und deaktivieren Sie dann die Organisationseinheiten, die Sie nicht synchronisieren möchten.

![Standardregeln](media/how-to-connect-fix-default-rules/default9.png)

## <a name="changing-join-condition"></a>Ändern einer Verknüpfungsbedingung
Microsoft empfiehlt, die standardmäßigen, von Azure AD Connect konfigurierten Verknüpfungsbedingungen zu verwenden. Wenn Sie die standardmäßigen Verknüpfungsbedingungen ändern, wird es für das Supportteam schwieriger, die Anpassungen zu verstehen und das Produkt zu unterstützen.

## <a name="validate-sync-rule"></a>Überprüfen der Synchronisierungsregel
Sie können die neu hinzugefügte Synchronisierungsregel mit der Previewfunktion überprüfen, ohne den gesamten Synchronisierungszyklus auszuführen. Starten Sie die **Synchronization Service**-Benutzeroberfläche.

![Standardregeln](media/how-to-connect-fix-default-rules/default10.png)

Klicken Sie auf **Metaverse Search**, wählen Sie als Bereichsobjekt **Person** und dann **Klausel hinzufügen** aus, und nennen Sie Ihre Suchkriterien. Klicken Sie auf die Schaltfläche **Suchen**, und doppelklicken Sie auf das Objekt in den **Suchergebnissen**. Beachten Sie dass der Import und die Synchronisierung für die Gesamtstruktur ausgeführt wird, bevor Sie diesen Schritt ausführen, um sicherzustellen, dass die Daten in Azure AD Connect für dieses Objekt auf dem neuesten Stand sind.

![Standardregeln](media/how-to-connect-fix-default-rules/default11.png)



Wählen Sie **Connectors** aus, wählen Sie das Objekt im entsprechenden Connector (Gesamtstruktur) aus, klicken Sie auf **Eigenschaften...** .

![Standardregeln](media/how-to-connect-fix-default-rules/default12.png)

Klicken Sie auf **Vorschau...**.

![Standardregeln](media/how-to-connect-fix-default-rules/default13a.png)

Klicken Sie auf **Vorschau generieren** und **Importattributfluss** im linken Bereich.

![Standardregeln](media/how-to-connect-fix-default-rules/default14.png)
 
Hier werden Sie feststellen, dass die neu hinzugefügte Regel für das Objekt ausgeführt wird und das Attribut „cloudFiltered“ auf „True“ festgelegt hat.

![Standardregeln](media/how-to-connect-fix-default-rules/default15a.png)
 
Wie wird eine geänderte Regel mit der Standardregel verglichen?
Sie können beide Regeln separat als Textdateien exportieren. Diese Regeln werden als PowerShell-Skriptdatei exportiert. Sie können sie mit jedem Dateivergleichstool vergleichen, um die erfolgten Änderungen zu ermitteln. Hier in diesem Beispiel wurden die zwei Dateien mit windiff verglichen.
 
Sie können sehen, dass in der vom Benutzer geänderten Regel das Attribut „msExchMailboxGuid“ auf den Typ **Expression** anstelle von **Direct** geändert wurde, mit dem Wert **NULL** und der Option **ExecuteOnce**. Unterschiede „Identified“ und „Precedence“ können ignoriert werden. 

![Standardregeln](media/how-to-connect-fix-default-rules/default17.png)
 
Wir kann eine geänderte Standardregel korrigiert werden?
Um Ihre Regeln in die Standardeinstellungen zurück zu ändern, können Sie die geänderte Regel löschen, die Standardregel wie unten gezeigt aktivieren und dann eine **Vollständige Synchronisierung** ausführen. Bevor Sie dies tun, ergreifen Sie die oben genannten Korrekturmaßnahmen, damit die Anpassungen, die Sie zu erreichen versuchen, nicht verloren gehen## Weitere Schritte

## <a name="next-steps"></a>Nächste Schritte
- [Voraussetzungen für Azure AD Connect](how-to-connect-install-prerequisites.md) 
- [Express-Einstellungen](how-to-connect-install-express.md)
- [Benutzerdefinierte Einstellungen](how-to-connect-install-custom.md)

