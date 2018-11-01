---
title: Problembehandlung bei Domänen- und SSL-Zertifikatsproblemen in Azure-Web-Apps | Microsoft-Dokumentation
description: Problembehandlung bei Domänen- und SSL-Zertifikatsproblemen in Azure-Web-Apps
services: app-service\web
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: e10c6580c29784f8d56a31e267c47ec08894113b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418363"
---
# <a name="troubleshoot-domain-and-ssl-certificate-problems-in-azure-web-apps"></a>Problembehandlung bei Domänen- und SSL-Zertifikatsproblemen in Azure-Web-Apps

In diesem Artikel werden häufige Probleme aufgelistet, die beim Konfigurieren einer Domäne oder eines SSL-Zertifikats für Ihre Azure-Web-Apps auftreten können. Darüber hinaus werden die möglichen Ursachen und Lösungen für diese Probleme beschrieben.

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/support/options/) auf, und wählen Sie die Option **Support erhalten**.

## <a name="certificate-problems"></a>Probleme mit Zertifikaten

### <a name="you-cant-add-an-ssl-certificate-binding-to-a-web-app"></a>Sie können keine SSL-Zertifikatbindung zu einer Web-App hinzufügen. 

#### <a name="symptom"></a>Symptom

Wenn Sie eine SSL-Bindung hinzufügen, erhalten Sie die folgende Fehlermeldung:

„Fehler beim Hinzufügen der SSL-Bindung. Das Zertifikat für den vorhandenen VIP kann nicht festgelegt werden, da ein anderer VIP dieses Zertifikat bereits verwendet.“

#### <a name="cause"></a>Ursache

Dieses Problem kann auftreten, wenn Sie mehrere IP-basierte SSL-Bindungen für die gleiche IP-Adresse bei mehreren Web-Apps haben. Beispiel: Web-App A hat ein IP-basiertes SSL mit einem altem Zertifikat. Web-App B hat für dieselbe IP-Adresse ein IP-basiertes SSL mit einem neuem Zertifikat. Wenn Sie die SSL-Bindung der Web-App mit dem neuen Zertifikat aktualisieren, tritt dieser Fehler auf, da dieselbe IP-Adresse für eine andere App verwendet wird. 

#### <a name="solution"></a>Lösung 

Verwenden Sie zur Problembehebung eine der folgenden Methoden:

- Löschen Sie die IP-basierte SSL-Bindung bei der Web-App, die das alte Zertifikat verwendet. 
- Erstellen Sie eine neue IP-basierte SSL-Bindung, die das neue Zertifikat verwendet.

### <a name="you-cant-delete-a-certificate"></a>Sie können ein Zertifikat nicht löschen. 

#### <a name="symptom"></a>Symptom

Wenn Sie versuchen, ein Zertifikat zu löschen, erhalten Sie die folgende Fehlermeldung:

„Das Zertifikat kann nicht gelöscht werden, da es derzeit in einer SSL-Bindung verwendet wird. Die SSL-Bindung muss entfernt werden, bevor Sie das Zertifikat löschen können.“

#### <a name="cause"></a>Ursache

Dieses Problem kann auftreten, wenn das Zertifikat von einer anderen Web-App verwendet wird.

#### <a name="solution"></a>Lösung

Entfernen Sie die SSL-Bindung für dieses Zertifikat aus den Web-Apps. Versuchen Sie anschließend, das Zertifikat zu löschen. Wenn Sie das Zertifikat immer noch nicht löschen können, löschen Sie den Cache Ihres Internet-Browsers, und öffnen Sie das Azure-Portal erneut in einem neuen Browserfenster. Versuchen Sie anschließend, das Zertifikat zu löschen.

### <a name="you-cant-purchase-an-app-service-certificate"></a>Sie können ein App Service-Zertifikat nicht erwerben. 

#### <a name="symptom"></a>Symptom
Sie können über das Azure-Portal kein [Azure App Service-Zertifikat](./web-sites-purchase-ssl-web-site.md) erwerben.

#### <a name="cause-and-solution"></a>Ursache und Lösung
Dieses Problem kann aus einem der folgenden Gründe auftreten:

- Der App Service-Plan ist „Free“ oder „Shared“. In diesen Tarifen wird SSL nicht unterstützt. 

    **Lösung**: Führen Sie ein Upgrade für den App Service-Plan der Web-App auf „Standard“ aus.

- Für das Abonnement ist keine gültige Kreditkarte angegeben.

    **Lösung**: Fügen Sie Ihrem Abonnement eine gültige Kreditkarte hinzu. 

- Der Erwerb eines App Service-Zertifikats wird vom Abonnementangebot (z. B. Microsoft Student) nicht unterstützt.  

    **Lösung**: Führen Sie ein Upgrade für Ihr Abonnement aus. 

- Für das Abonnement wurde die Obergrenze von Käufen erreicht, die für ein Abonnement zulässig sind.

    **Lösung**: Für App Service-Zertifikate gilt bei Abonnements des Typs „Nutzungsbasierte Bezahlung“ und „Enterprise Agreement“ ein Limit von zehn (10) Zertifikatskäufen. Bei anderen Abonnementtypen sind nur drei (3) Käufe zulässig. Wenden Sie sich an den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um dieses Limit zu erhöhen.
- Das App Service-Zertifikat wurde als betrugsverdächtig gekennzeichnet. Sie erhalten die folgende Fehlermeldung: „Ihr Zertifikat wurde als betrugsverdächtig gekennzeichnet. Die Anforderung wird aktuell geprüft. Wenn das Zertifikat nicht innerhalb von 24 Stunden verwendbar ist, wenden Sie sich an den Azure-Support.“

    **Lösung**: Wenn das Zertifikat als betrugsverdächtig gekennzeichnet und das Problem nach 24 Stunden noch nicht behoben ist, führen Sie die folgenden Schritte aus:

    1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
    2. Wechseln Sie zu **App Service-Zertifikate**, und wählen Sie das Zertifikat aus.
    3. Wählen Sie **Zertifikatkonfiguration** > **Schritt 2: Überprüfen** > **Domänenüberprüfung** aus. In diesem Schritt wird eine E-Mail-Nachricht an den Azure-Zertifikatanbieter gesendet mit der Bitte, das Problem zu beheben.

## <a name="domain-problems"></a>Probleme mit Domänen

### <a name="you-purchased-an-ssl-certificate-for-the-wrong-domain"></a>Sie haben ein SSL-Zertifikat für die falsche Domäne erworben.

#### <a name="symptom"></a>Symptom

Sie haben ein App Service-Zertifikat für die falsche Domäne erworben. Sie können das Zertifikat nicht so aktualisieren, dass die richtige Domäne verwendet wird.

#### <a name="solution"></a>Lösung

Löschen Sie dieses Zertifikat, und erwerben Sie anschließend ein neues Zertifikat.

Wenn sich das aktuelle Zertifikat, in dem die falsche Domäne verwendet wird, im Status „Ausgestellt“ befindet, wird Ihnen dieses Zertifikat auch in Rechnung gestellt. App Service-Zertifikate sind nicht erstattbar. Sie können sich jedoch an den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) wenden und prüfen, ob es andere Optionen gibt. 

### <a name="an-app-service-certificate-was-renewed-but-the-web-app-shows-the-old-certificate"></a>Ein App Service-Zertifikat wurde erneuert, aber in der Web-App wird das alte Zertifikat angezeigt 

#### <a name="symptom"></a>Symptom

Das App Service-Zertifikat wurde erneuert, doch die Web-App, die das App Service-Zertifikat verwendet, nutzt weiterhin das alte Zertifikat. Darüber hinaus haben Sie eine Warnmeldung erhalten, dass das HTTPS-Protokoll erforderlich ist.

#### <a name="cause"></a>Ursache 
Der Web-Apps-Feature von Azure App Service führt alle acht Stunden einen Hintergrundauftrag aus und synchronisiert die Zertifikatsressource, wenn Änderungen vorgenommen wurden. Wenn Sie ein Zertifikat aktualisieren oder wechseln, ruft die Anwendung manchmal weiterhin das alte Zertifikat und nicht das neu aktualisierte Zertifikat ab. Dies liegt daran, dass der Auftrag zum Synchronisieren der Zertifikatsressource noch nicht ausgeführt wurde. 
 
#### <a name="solution"></a>Lösung

Sie können eine Synchronisierung des Zertifikats erzwingen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Wählen Sie **App Service-Zertifikate** aus, und wählen Sie dann das Zertifikat aus.
2. Wählen Sie **Erstellung neuer Schlüssel und Synchronisierung** aus, und wählen Sie dann **Synchronisieren** aus. Die Synchronisierung nimmt einige Zeit in Anspruch. 
3. Wenn die Synchronisierung abgeschlossen ist, wird folgende Benachrichtigung angezeigt: „Alle Ressourcen wurden erfolgreich mit dem neuesten Zertifikat aktualisiert“.

### <a name="domain-verification-is-not-working"></a>Die Domänenüberprüfung funktioniert nicht 

#### <a name="symptom"></a>Symptom 
Für das App Service-Zertifikat ist zuerst eine Überprüfung der Domäne erforderlich, bevor das Zertifikat verwendet werden kann. Wenn Sie **Überprüfen** auswählen, schlägt der Vorgang fehl.

#### <a name="solution"></a>Lösung
Überprüfen Sie Ihre Domäne manuell durch Hinzufügen eines TXT-Eintrags:
 
1.  Wechseln Sie zum DNS-Anbieter (Domain Name Service), der Ihre Domäne (Domänennamen) hostet.
2.  Fügen Sie einen TXT-Eintrag für Ihre Domäne hinzu, in dem der Wert des Domänentokens verwendet wird, das im Azure-Portal angezeigt wird. 

Warten Sie einige Minuten, bis die DNS-Daten weitergegeben wurden, und wählen Sie dann die Schaltfläche **Aktualisieren** aus, um die Überprüfung auszulösen. 

Als Alternative können Sie die HTML-Webseitenmethoden verwenden, um Ihre Domäne manuell zu überprüfen. Diese Methode ermöglicht es der Zertifizierungsstelle, den Domänenbesitz für die Domäne zu bestätigen, für die das Zertifikat ausgestellt ist.

1.  Erstellen Sie eine HTML-Datei namens „{Domänenüberprüfungstoken}.html“. Der Inhalt dieser Datei muss der Wert des Domänenüberprüfungstokens sein.
3.  Laden Sie diese Datei in das Stammverzeichnis des Webservers hoch, der Ihre Domäne hostet.
4.  Wählen Sie **Aktualisieren** aus, um den Status des Zertifikats zu prüfen. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.

Beispiel: Sie erwerben ein Standardzertifikat für azure.com mit dem Domänenüberprüfungstoken „1234abcd“. Eine Webanforderung, die für http://azure.com/1234abcd.html gestellt wird, sollte „1234abcd“ zurückgeben. 

> [!IMPORTANT]
> Bei einer Zertifikatbestellung muss der Domänenüberprüfungsvorgang innerhalb von 15 Tagen abgeschlossen sein. Nach 15 Tagen wird das Zertifikat von der Zertifizierungsstelle verweigert, und das Zertifikat wird Ihnen nicht in Rechnung gestellt. Löschen Sie in diesem Fall das Zertifikat, und versuchen Sie es erneut.
>
> 

### <a name="you-cant-purchase-a-domain"></a>Sie können keine Domäne erwerben.

#### <a name="symptom"></a>Symptom
Sie können im Azure-Portal keine Domäne aus der Web Apps- oder App Service-Domäne erwerben.

#### <a name="cause-and-solution"></a>Ursache und Lösung

Dieses Problem tritt aus einem der folgenden Gründe auf:

- Es ist keine Kreditkarte für das Azure-Abonnement angegeben, oder die Kreditkarte ist ungültig.

    **Lösung**: Fügen Sie Ihrem Abonnement eine gültige Kreditkarte hinzu.

- Sie sind nicht der Besitzer des Abonnements, sodass Sie nicht berechtigt sind, eine Domäne zu erwerben.

    **Lösung:** [Weisen Sie Ihrem Konto die Rolle „Besitzer“](../role-based-access-control/role-assignments-portal.md) zu. Oder wenden Sie sich an den Abonnementadministrator, um die Berechtigung zum Erwerben einer Domäne zu erhalten.
- Sie haben das Limit für den Erwerb von Domänen für Ihr Abonnement erreicht. Das aktuelle Limit beläuft sich auf 20 Domänenkäufe.

    **Lösung**: Wenden Sie sich an den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um eine Erhöhung des Limits anzufordern.
- Der Kauf einer App Service-Domäne wird von Ihrem Azure-Abonnementtyp nicht unterstützt.

    **Lösung**: Führen Sie ein Upgrade für Ihr Azure-Abonnement auf einen anderen Abonnementtyp aus, z. B. auf ein Abonnement mit nutzungsbasierter Zahlung.

### <a name="you-cant-add-a-host-name-to-a-web-app"></a>Sie können einer Web-App keinen Hostnamen hinzufügen. 

#### <a name="symptom"></a>Symptom

Wenn Sie einen Hostnamen hinzufügen, schlägt die Domänenvalidierung und -überprüfung fehl.

#### <a name="cause"></a>Ursache 

Dieses Problem tritt aus einem der folgenden Gründe auf:

- Sie haben keine Berechtigung zum Hinzufügen eines Hostnamens.

    **Lösung**: Bitten Sie den Abonnementadministrator, Ihnen die Berechtigung zum Hinzufügen eines Hostnamens zu erteilen.
- Der Besitz der Domäne konnte nicht überprüft werden.

    **Lösung**: Vergewissern Sie sich, dass Ihr CNAME- oder A-Eintrag richtig konfiguriert ist. Erstellen Sie einen CNAME-Eintrag oder einen A-Eintrag, um der Web-App eine benutzerdefinierte Domäne zuzuordnen. Wenn Sie eine Stammdomäne verwenden möchten, müssen Sie einen A- und einen TXT-Eintrag verwenden:

    |Eintragstyp|Host|Verweist auf|
    |------|------|-----|
    |Eine Datei|@|IP-Adresse für eine Web-App|
    |TXT|@|<App-Name>.azurewebsites.net|
    |CNAME|www|<App-Name>.azurewebsites.net|

### <a name="dns-cant-be-resolved"></a>DNS kann nicht aufgelöst werden

#### <a name="symptom"></a>Symptom

Sie haben die folgende Fehlermeldung erhalten:

„Der DNS-Eintrag konnte nicht gefunden werden.“

#### <a name="cause"></a>Ursache
Dieses Problem tritt aus einem der folgenden Gründe auf:

- Die Gültigkeitsdauer (Time to Live, TTL) ist noch nicht abgelaufen. Überprüfen Sie die DNS-Konfiguration für Ihre Domäne, um den TTL-Wert zu bestimmen, und warten Sie dann bis zum Ablauf der Gültigkeitsdauer.
- Die DNS-Konfiguration ist fehlerhaft.

#### <a name="solution"></a>Lösung
- Warten Sie 48 Stunden, bis sich dieses Problem von selbst löst.
- Wenn Sie die Einstellung für die Gültigkeitsdauer (TTL) in Ihrer DNS-Konfiguration ändern können, ändern Sie den Wert auf 5 Minuten, um festzustellen, ob das Problem dadurch behoben wird.
- Überprüfen Sie mithilfe von [WhatsmyDNS.net](https://www.whatsmydns.net/), ob Ihre Domäne auf die IP-Adresse der Web-App verweist. Wenn dies nicht der Fall ist, konfigurieren Sie den A-Eintrag mit der richtigen IP-Adresse der Web-App.

### <a name="you-need-to-restore-a-deleted-domain"></a>Sie müssen eine gelöschte Domäne wiederherstellen 

#### <a name="symptom"></a>Symptom
Die Domäne wird nicht mehr im Azure-Portal angezeigt.

#### <a name="cause"></a>Ursache 
Die Besitzer des Abonnements hat die Domäne möglicherweise versehentlich gelöscht.

#### <a name="solution"></a>Lösung
Wurde Ihre Domäne vor weniger als sieben Tagen gelöscht, wurde der Löschvorgang für die Domäne noch nicht gestartet. In diesem Fall können Sie dieselbe Domäne erneut im Azure-Portal unter demselben Abonnement erwerben. (Achten Sie darauf, dass Sie den genauen Domänennamen in das Suchfeld eingeben.) Diese Domäne wird Ihnen nicht erneut in Rechnung gestellt. Wenn die Domäne vor mehr als sieben Tagen gelöscht wurde, wenden Sie sich an den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um Hilfe beim Wiederherstellen der Domäne zu erhalten.

### <a name="a-custom-domain-returns-a-404-error"></a>Eine benutzerdefinierte Domäne gibt einen 404-Fehler zurück. 

#### <a name="symptom"></a>Symptom

Wenn Sie mithilfe des benutzerdefinierten Domänennamens zur Website navigieren, erhalten Sie die folgende Fehlermeldung:

„Fehler 404 – Web-App wurde nicht gefunden.“


#### <a name="cause-and-solution"></a>Ursache und Lösung

**Ursache 1** 

Der benutzerdefinierten Domäne, die Sie konfiguriert haben, fehlt ein CNAME- oder ein A-Eintrag. 

**Lösung für Ursache 1**

- Wenn Sie einen A-Eintrag hinzugefügt haben, stellen Sie sicher, dass auch ein TXT-Eintrag hinzugefügt wird. Weitere Informationen finden Sie unter [Erstellen des A-Eintrags](./app-service-web-tutorial-custom-domain.md#create-the-a-record).
- Wenn Sie die Stammdomäne für Ihre Web-App nicht verwenden müssen, empfiehlt es sich, dass Sie anstelle eines A-Eintrags einen CNAME-Eintrag verwenden.
- Verwenden Sie nicht gleichzeitig einen CNAME-Eintrag und einen A-Eintrag für dieselbe Domäne. Dies kann einen Konflikt verursachen und verhindern, dass die Domäne aufgelöst wird. 

**Ursache 2** 

Im Internet-Browser ist möglicherweise immer noch die alte IP-Adresse für Ihre Domäne im Cache gespeichert. 

**Lösung für Ursache 2**

Löschen Sie den Browser-Cache. Bei Windows-Geräten können Sie den Befehl `ipconfig /flushdns` ausführen. Überprüfen Sie mithilfe von [WhatsmyDNS.net](https://www.whatsmydns.net/), ob Ihre Domäne auf die IP-Adresse der Web-App verweist. 

### <a name="you-cant-add-a-subdomain"></a>Sie können keine Unterdomäne hinzufügen. 

#### <a name="symptom"></a>Symptom

Sie können einer Web-App keinen neuen Hostnamen hinzufügen, um eine Unterdomäne zuzuweisen.

#### <a name="solution"></a>Lösung

- Vergewissern Sie sich gemeinsam mit dem Abonnementadministrator, dass Sie Berechtigungen zum Hinzufügen von Hostnamen zur Web-App haben.
- Wenn Sie weitere Unterdomänen benötigen, empfiehlt es sich, dass Sie für das Domänenhosting zu Azure DNS wechseln. Wenn Sie Azure DNS verwenden, können Sie Ihrer Web-App 500 Hostnamen hinzufügen. Weitere Informationen finden Sie unter [Hinzufügen einer Unterdomäne](https://blogs.msdn.microsoft.com/waws/2014/10/01/mapping-a-custom-subdomain-to-an-azure-website/).











 


















