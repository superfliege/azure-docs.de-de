---
title: Tutorial – Konfigurieren von HTTPS in einer benutzerdefinierten Azure CDN-Domäne | Microsoft-Dokumentation
description: In diesem Tutorial wird beschrieben, wie Sie HTTPS in Ihrer benutzerdefinierten Azure CDN-Endpunktdomäne aktivieren und deaktivieren.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/12/2018
ms.author: rli
ms.custom: mvc
ms.openlocfilehash: a8f2da5a68552c35a55a7bbb764afc7b36af6962
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="tutorial-configure-https-on-an-azure-cdn-custom-domain"></a>Tutorial: Konfigurieren von HTTPS in einer benutzerdefinierten Azure CDN-Domäne

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

In diesem Lernprogramm wird veranschaulicht, wie Sie das HTTP-Protokoll für eine benutzerdefinierte Domäne aktivieren, die einem Azure CDN-Endpunkt (Content Delivery Network) zugeordnet ist. Durch die Verwendung des HTTPS-Protokolls in Ihrer benutzerdefinierten Domäne (z.B. „https:\//www.contoso.com“) stellen Sie sicher, dass Ihre sensiblen Daten per SSL-Verschlüsselung sicher zugestellt werden, wenn diese über das Internet gesendet werden. HTTPS sorgt für Vertrauen und bietet Authentifizierung und Angriffsschutz für Ihre Webanwendungen. Der Workflow zur Aktivierung von HTTPS wird durch die Möglichkeit zur Aktivierung per Klick sowie durch eine umfassende Zertifikatverwaltung vereinfacht – alles ohne zusätzliche Kosten.

Azure CDN unterstützt HTTPS standardmäßig für einen CDN-Endpunkt-Hostnamen. Wenn Sie also beispielsweise einen CDN-Endpunkt erstellen (z.B. „https:\//contoso.azureedge.net“), wird HTTPS automatisch aktiviert.  

Zu den wichtigsten Attributen des HTTPS-Features zählen unter anderem folgende:

- Keine zusätzliche Kosten: Zertifikatabruf und -verlängerung sind kostenlos, und für HTTPS-Datenverkehr fallen keine zusätzlichen Kosten an. Sie zahlen nur für den ausgehenden Datenverkehr des CDNs (nach GB).

- Unkomplizierte Aktivierung: Über das [Azure-Portal](https://portal.azure.com) ist die Bereitstellung mit einem einzelnen Klick möglich. Das Feature kann aber auch per REST-API oder mithilfe anderer Entwicklertools aktiviert werden.

- Umfassende Zertifikatverwaltung: Die gesamte Zertifikatbeschaffung und -verwaltung wird für Sie erledigt. Zertifikate werden automatisch bereitgestellt und vor Ablauf verlängert. Dadurch ist gewährleistet, dass keine Dienstunterbrechungen aufgrund eines abgelaufenen Zertifikats auftreten.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> - Aktivieren des HTTPS-Protokolls in Ihrer benutzerdefinierten Domäne
> - Überprüfen der Domäne
> - Deaktivieren des HTTPS-Protokolls in Ihrer benutzerdefinierten Domäne

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Schritte in diesem Tutorial ausführen können, müssen Sie zunächst ein CDN-Profil und mindestens einen CDN-Endpunkt erstellen. Weitere Informationen finden Sie unter [Erste Schritte mit Azure CDN](cdn-create-new-endpoint.md).

Zusätzlich müssen Sie Ihrem CDN-Endpunkt eine benutzerdefinierte Azure CDN-Domäne zuordnen. Weitere Informationen finden Sie unter [Tutorial: Hinzufügen einer benutzerdefinierten Domäne zum Azure CDN-Endpunkt](cdn-map-content-to-custom-domain.md).

## <a name="enable-the-https-feature"></a>Aktivieren des HTTPS-Features

Um HTTPS für eine benutzerdefinierte Domäne zu aktivieren, führen Sie die folgenden Schritte aus:

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum CDN-Profil **Azure CDN Standard von Verizon** oder **Azure CDN Premium von Verizon**.

2. Wählen Sie in der Liste mit den CDN-Endpunkten den Endpunkt aus, der Ihre benutzerdefinierte Domäne enthält.

    ![Liste mit Endpunkten](./media/cdn-custom-ssl/cdn-select-custom-domain-endpoint.png)

    Die Seite **Endpunkt** wird angezeigt.

3. Wählen Sie in der Liste mit den benutzerdefinierten Domänen die benutzerdefinierte Domäne aus, für die Sie HTTPS aktivieren möchten.

    ![Liste mit benutzerdefinierten Domänen](./media/cdn-custom-ssl/cdn-custom-domain.png)

    Die Seite **Benutzerdefinierte Domäne** wird angezeigt.

4. Wählen Sie **Ein**, um HTTPS zu aktivieren, und wählen Sie dann **Übernehmen**.

    ![HTTPS-Status benutzerdefinierter Domänen](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="validate-the-domain"></a>Überprüfen der Domäne

Wenn Sie bereits eine benutzerdefinierte Domäne verwenden, die Ihrem benutzerdefinierten Endpunkt mit einem CNAME-Eintrag zugeordnet ist, können Sie mit  
[Benutzerdefinierte Domäne ist Ihrem CDN-Endpunkt mit einem CNAME-Eintrag zugeordnet](#custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record) fortfahren. Fahren Sie andernfalls mit [Benutzerdefinierte Domäne ist Ihrem CDN-Endpunkt nicht zugeordnet](#custom-domain-is-not-mapped-to-your-cdn-endpoint) fort, falls der CNAME-Eintrag für Ihren Endpunkt nicht mehr vorhanden ist oder falls er die Unterdomäne „cdnverify“ enthält.

### <a name="custom-domain-is-mapped-to-your-cdn-endpoint-by-a-cname-record"></a>Benutzerdefinierte Domäne ist Ihrem CDN-Endpunkt mit einem CNAME-Eintrag zugeordnet

Nach dem Hinzufügen einer benutzerdefinierten Domäne zu Ihrem Endpunkt haben Sie einen CNAME-Eintrag in der DNS-Tabelle Ihrer Domänenregistrierungsstelle erstellt, um ihn Ihrem CDN-Endpunkt-Hostnamen zuzuordnen. Wenn dieser CNAME-Eintrag noch vorhanden ist und die cdnverify-Unterdomäne nicht enthält, wird er von der DigiCert-Zertifizierungsstelle (Certificate Authority, CA) verwendet, um den Besitz Ihrer benutzerdefinierten Domäne zu überprüfen. 

Ihr CNAME-Eintrag sollte im folgenden Format vorliegen, wobei *Name* Ihr benutzerdefinierter Domänenname und *Wert* Ihr CDN-Endpunkt-Hostname ist:

| NAME            | Typ  | Wert                 |
|-----------------|-------|-----------------------|
| www.contoso.com | CNAME | contoso.azureedge.net |

Weitere Informationen über CNAME-Einträge finden Sie unter [Erstellen Sie die CNAME-DNS-Einträge](https://docs.microsoft.com/en-us/azure/cdn/cdn-map-content-to-custom-domain#create-the-cname-dns-records).

Wenn Ihr CNAME-Eintrag im richtigen Format vorliegt, überprüft DigiCert automatisch Ihren benutzerdefinierten Domänennamen und fügt ihn dem SAN-Zertifikat hinzu. DigiCert sendet Ihnen keine Bestätigungs-E-Mail, und Sie müssen Ihre Anforderung nicht genehmigen. Das Zertifikat ist ein Jahr lang gültig und wird automatisch verlängert, bevor es abläuft. Fahren Sie mit [Warten auf die Weitergabe](#wait-for-propagation) fort. 

Die automatische Validierung dauert normalerweise einige Minuten. Öffnen Sie ein Supportticket, falls Ihre Domäne nicht innerhalb einer Stunde validiert wurde.

>[!NOTE]
>Wenn Sie über einen CAA-Datensatz (Certificate Authority Authorization) bei Ihrem DNS-Anbieter verfügen, muss dieser DigiCert als gültige Zertifizierungsstelle enthalten. Ein CAA-Datensatz ermöglicht es Domänenbesitzern, ihrem jeweiligen DNS-Anbieter mitzuteilen, welche Zertifizierungsstellen zum Ausstellen von Zertifikaten für ihre Domäne autorisiert sind. Wenn eine Zertifizierungsstelle einen Auftrag für ein Zertifikat für eine Domäne empfängt, für die ein CAA-Datensatz vorliegt und die Zertifizierungsstelle nicht als autorisierter Aussteller angegeben ist, darf sie das Zertifikat für die Domäne oder Unterdomäne nicht ausstellen. Informationen zum Verwalten von CAA-Einträgen finden Sie unter [Manage CAA records](https://support.dnsimple.com/articles/manage-caa-record/) (Verwalten von CAA-Einträgen). Ein Tool für CAA-Einträge finden Sie unter [CAA Record Helper](https://sslmate.com/caa/) (Hilfsprogramm für CAA-Einträge).

### <a name="custom-domain-is-not-mapped-to-your-cdn-endpoint"></a>Benutzerdefinierte Domäne ist Ihrem CDN-Endpunkt nicht zugeordnet

Wenn der CNAME-Eintrag für Ihren Endpunkt nicht mehr vorhanden ist oder die cdnverify-Unterdomäne enthält, folgen Sie den weiteren Anweisungen in diesem Schritt.

Nach der Aktivierung von HTTPS für Ihre benutzerdefinierte Domäne überprüft die DigiCert-Zertifizierungsstelle, ob die Domäne wirklich Ihnen gehört. Hierzu setzt sich DigiCert mit dem Registrant der Domäne in Verbindung, der in den [WHOIS](http://whois.domaintools.com/)-Registranteninformationen angegeben ist. Der Kontakt wird über die E-Mail-Adresse (Standardverfahren) oder die Telefonnummer in der WHOIS-Registrierung hergestellt. HTTPS wird für Ihre benutzerdefinierte Domäne erst nach Abschluss der Domänenüberprüfung aktiviert. Die Genehmigung der Domäne muss innerhalb von sechs Werktagen erfolgen. Anforderungen, die nicht innerhalb von sechs Werktagen genehmigt werden, werden automatisch abgebrochen. 

![WHOIS-Datensatz](./media/cdn-custom-ssl/whois-record.png)

DigiCert sendet zur Überprüfung auch eine E-Mail an zusätzliche E-Mail-Adressen. Falls die WHOIS-Registranteninformationen privat sind, vergewissern Sie sich, dass Sie die Bestätigung direkt über eine der folgenden Adressen vornehmen können:

admin@&lt;Ihr-Domänenname.com&gt;  
administrator@&lt;Ihr-Domänenname.com&gt;  
webmaster@&lt;Ihr-Domänenname.com&gt;  
hostmaster@&lt;Ihr-Domänenname.com&gt;  
postmaster@&lt;Ihr-Domänenname.com&gt;  

Sie sollten ähnlich wie im folgenden Beispiel nach wenigen Minuten eine E-Mail-Nachricht erhalten, in der Sie aufgefordert werden, die Anforderung zu genehmigen. Wenn Sie einen Spamfilter verwenden, fügen Sie admin@digicert.com der Positivliste hinzu. Sollten Sie nach 24 Stunden noch keine E-Mail erhalten haben, setzen Sie sich mit dem Microsoft-Support in Verbindung.
    
![E-Mail zur Bestätigung der Domäne](./media/cdn-custom-ssl/domain-validation-email.png)

Wenn Sie auf den Genehmigungslink klicken, werden Sie auf das folgende Onlineformular für die Genehmigung weitergeleitet: 
    
![Formular zur Bestätigung der Domäne](./media/cdn-custom-ssl/domain-validation-form.png)

Befolgen Sie die Anweisungen im Formular. Sie haben zwei Möglichkeiten zur Genehmigung:

- Sie können alle zukünftigen Aufträge genehmigen, die über dasselbe Konto und für dieselbe Stammdomäne (beispielsweise „contoso.com“) getätigt werden. Dies empfiehlt sich, wenn Sie planen, zusätzliche benutzerdefinierte Domänen für die gleiche Stammdomäne hinzuzufügen.

- Sie können lediglich den spezifischen Hostnamen aus dieser Anforderung genehmigen. Für nachfolgende Anforderungen ist eine zusätzliche Genehmigung erforderlich.

Nach der Genehmigung fügt DigiCert Ihren benutzerdefinierten Domänennamen dem SAN-Zertifikat hinzu. Das Zertifikat ist ein Jahr lang gültig und wird automatisch verlängert, bevor es abläuft.

## <a name="wait-for-propagation"></a>Warten auf die Weitergabe

Nach der Überprüfung des Domänennamens dauert es maximal 6 bis 8 Stunden, bis das HTTPS-Feature für die benutzerdefinierte Domäne aktiviert wird. Wenn der Prozess abgeschlossen ist, wird der HTTP-Status für benutzerdefinierte Domänen im Azure-Portal auf **Aktiviert** festgelegt, und die vier Schritte im Dialogfeld der benutzerdefinierten Domäne werden als abgeschlossen markiert. Die benutzerdefinierte Domäne ist jetzt bereit zur Verwendung von HTTPS.

![Dialogfeld „HTTPS aktivieren“](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Vorgangsstatus

Die folgende Tabelle zeigt den Status des Vorgangs zum Aktivieren von HTTPS. Nach der Aktivierung von HTTPS werden im Dialogfeld der benutzerdefinierten Domäne vier Schritte angezeigt. Beim Durchlaufen der einzelnen Schritte werden weitere Details zum jeweiligen Teilschritt angezeigt. Nicht alle diese Teilschritte werden vorkommen. Nachdem ein Schritt erfolgreich abgeschlossen wurde, wird neben diesem ein grünes Häkchen angezeigt. 

| Vorgangsschritt | Details zum Teilschritt des Vorgangs | 
| --- | --- |
| 1: Übermitteln der Anforderung | Übermitteln der Anforderung |
| | Die HTTPS-Anforderung wird übermittelt. |
| | Die HTTPS-Anforderung wurde erfolgreich übermittelt. |
| 2: Überprüfen der Domäne | Die Domäne wird automatisch überprüft, wenn sie per CNAME dem CDN-Endpunkt zugeordnet wird. Andernfalls wird an die E-Mail-Adresse, die im Registrierungsdatensatz (WHOIS-Registrant) Ihrer Domäne angegeben ist, eine Überprüfungsanforderung gesendet. Überprüfen Sie die Domäne schnellstmöglich. |
| | Der Besitz der Domäne wurde erfolgreich bestätigt. |
| | Die Überprüfungsanforderung für den Domänenbesitz ist abgelaufen (Kunde hat wahrscheinlich nicht innerhalb von 6 Tagen reagiert). HTTPS wird für Ihre Domäne nicht aktiviert. * |
| | Die Überprüfungsanforderung für den Domänenbesitz wurde vom Kunden zurückgewiesen. HTTPS wird für Ihre Domäne nicht aktiviert. * |
| 3: Zertifikatbereitstellung | Die Zertifizierungsstelle stellt zurzeit das Zertifikat aus, das zum Aktivieren von HTTPS für Ihre Domäne erforderlich ist. |
| | Das Zertifikat wurde ausgegeben und wird derzeit im CDN-Netzwerk bereitgestellt. Dies kann bis zu sechs Stunden dauern. |
| | Das Zertifikat wurde erfolgreich im CDN-Netzwerk bereitgestellt. |
| 4: Abschließen | HTTPS wurde in Ihrer Domäne erfolgreich aktiviert. |

\* Diese Meldung wird nur bei einem Fehler angezeigt. 

Sollte vor dem Übermitteln der Anforderung ein Fehler auftreten, wird die folgende Fehlermeldung angezeigt:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="clean-up-resources---disable-https"></a>Bereinigen von Ressourcen – Deaktivieren von HTTPS

In den obigen Schritten haben Sie das HTTPS-Protokoll für Ihre benutzerdefinierte Domäne aktiviert. Falls Sie Ihre benutzerdefinierte Domäne nicht mehr mit HTTPS verwenden möchten, können Sie HTTPS mit diesen Schritten deaktivieren:

### <a name="disable-the-https-feature"></a>Deaktivieren des HTTPS-Features 

1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum CDN-Profil **Azure CDN Standard von Verizon** oder **Azure CDN Premium von Verizon**.

2. Klicken Sie in der Liste mit den Endpunkten auf den Endpunkt, die Ihre benutzerdefinierte Domäne enthält.

3. Klicken Sie auf die benutzerdefinierte Domäne, für die Sie HTTPS deaktivieren möchten.

    ![Liste mit benutzerdefinierten Domänen](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Klicken Sie auf **Aus**, um HTTPS zu deaktivieren, und klicken Sie dann auf **Anwenden**.

    ![Dialogfeld „HTTPS für benutzerdefinierte Domänen“](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="wait-for-propagation"></a>Warten auf die Weitergabe

Nachdem das HTTPS-Feature für die benutzerdefinierte Domäne deaktiviert wurde, dauert es maximal 6 bis 8 Stunden, bis die Änderung wirksam wird. Nach Abschluss des Prozesses wird der HTTP-Status für benutzerdefinierte Domänen im Azure-Portal auf **Deaktiviert** festgelegt, und die drei Schritte im Dialogfeld der benutzerdefinierten Domäne werden als abgeschlossen markiert. Die benutzerdefinierte Domäne kann HTTPS nicht mehr verwenden.

![Dialogfeld „HTTPS deaktivieren“](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

#### <a name="operation-progress"></a>Vorgangsstatus

Die folgende Tabelle zeigt den Status des Vorgangs zum Deaktivieren von HTTPS. Nach der Deaktivierung von HTTPS werden im Dialogfeld der benutzerdefinierten Domäne drei Schritte angezeigt. Beim Durchlaufen der einzelnen Schritte werden weitere Details zum jeweiligen Schritt angezeigt. Nachdem ein Schritt erfolgreich abgeschlossen wurde, wird neben diesem ein grünes Häkchen angezeigt. 

| Vorgangsstatus | Vorgangsdetails | 
| --- | --- |
| 1: Übermitteln der Anforderung | Ihre Anforderung wird übermittelt. |
| 2: Aufheben der Zertifikatbereitstellung | Das Zertifikat wird gelöscht. |
| 3: Abschließen | Das Zertifikat wurde gelöscht. |

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

1. *Wer ist der Zertifikatanbieter, und welche Art von Zertifikat wird verwendet?*

    Microsoft verwendet ein SAN-Zertifikat (Subject Alternative Names; alternative Antragstellernamen) von DigiCert. Ein SAN-Zertifikat kann mehrere vollqualifizierte Domänennamen mit einem einzelnen Zertifikat schützen.

2. *Kann ich mein dediziertes Zertifikat verwenden?*
    
    Noch nicht, die Option ist aber geplant.

3. *Was passiert, wenn ich die Domänenüberprüfungs-E-Mail von DigiCert nicht erhalte?*

    Wenn Sie einen CNAME-Eintrag für Ihre benutzerdefinierte Domäne haben, der direkt auf Ihren Endpunkt-Hostnamen verweist (und Sie verwenden nicht den cdnverify-Unterdomänennamen), erhalten Sie keine Bestätigungs-E-Mail für die Domäne. Die Bestätigung erfolgt automatisch. Wenden Sie sich andernfalls an den Microsoft-Support, falls Sie nicht über einen CNAME-Eintrag verfügen und innerhalb von 24 Stunden keine E-Mail erhalten.

4. *Ist ein SAN-Zertifikat weniger sicher als ein dediziertes Zertifikat?*
    
    Ein SAN-Zertifikat bietet die gleichen Verschlüsselungs- und Sicherheitsstandards wie ein dediziertes Zertifikat. Zur Verbesserung der Serversicherheit verwenden alle ausgestellten SSL-Zertifikate SHA-256.

5. *Kann ich HTTPS für benutzerdefinierte Domänen mit Azure CDN von Akamai verwenden?*

    Dieses Feature ist derzeit nur mit Azure CDN von Verizon verfügbar. Microsoft arbeiter daran, dieses Feature in den nächsten Monaten auch für Azure CDN von Akamai zu unterstützen.

6. *Benötige ich einen CAA-Datensatz (Certificate Authority Authorization) bei meinem DNS-Anbieter?*

    Nein. Ein CAA-Datensatz ist momentan nicht erforderlich. Wenn Sie allerdings über einen solchen Datensatz verfügen, muss er DigiCert als gültige Zertifizierungsstelle enthalten.


## <a name="next-steps"></a>Nächste Schritte

Sie haben Folgendes gelernt:

> [!div class="checklist"]
> - Aktivieren des HTTPS-Protokolls in Ihrer benutzerdefinierten Domäne
> - Überprüfen der Domäne
> - Deaktivieren des HTTPS-Protokolls in Ihrer benutzerdefinierten Domäne

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie das Caching auf Ihrem CDN-Endpunkt konfigurieren.

> [!div class="nextstepaction"]
> [Steuern des Azure CDN-Zwischenspeicherverhaltens mit Cacheregeln](cdn-caching-rules.md)

