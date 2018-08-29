---
title: Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein App Service-Zertifikat erwerben und es an Ihre App Service-App binden.
services: app-service
documentationcenter: .net
author: cephalin
manager: cfowler
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: apurvajo;cephalin
ms.openlocfilehash: 21dde5b0199345e9e8faffc6feac4854a3aa7350
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "42142408"
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service

In diesem Tutorial wird gezeigt, wie Sie Ihre Web-App schützen, indem Sie ein SSL-Zertifikat für Ihre Instanz von **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)** erwerben, sicher in [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) speichern und einer benutzerdefinierten Domäne zuordnen.

## <a name="step-1---sign-in-to-azure"></a>Schritt 1: Anmelden bei Azure

Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.

## <a name="step-2---place-an-ssl-certificate-order"></a>Schritt 2: Bestellen eines SSL-Zertifikats

Ein SSL-Zertifikat können Sie bestellen, indem Sie im **Azure-Portal** ein neues [App Service-Zertifikat](https://portal.azure.com/#create/Microsoft.SSL) erstellen.

![Zertifikaterstellung](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Geben Sie einen aussagekräftigen **Namen** für Ihr SSL-Zertifikat sowie den **Domänennamen** ein.

> [!NOTE]
> Dieser Schritt ist einer der wichtigsten Teile des Einkaufsvorgangs. Achten Sie darauf, dass Sie den richtigen Hostnamen (benutzerdefinierte Domäne) eingeben, den Sie mit diesem Zertifikat schützen möchten. Stellen Sie dem Hostnamen **NICHT** WWW voran. 
>

Wählen Sie Ihr **Abonnement**, Ihre **Ressourcengruppe** und Ihre **Zertifikat-SKU** aus.

> [!TIP]
> App Service-Zertifikate können für alle Dienste – ob von Azure oder nicht – verwendet werden und sind nicht auf App Services beschränkt. Hierzu müssen Sie eine lokale PFX-Kopie eines App Service-Zertifikats erstellen, sodass Sie es beliebig verwenden können. Weitere Informationen finden Sie unter [Create a local PFX copy of an App Service Certificate](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/) (Erstellen einer lokalen PFX-Kopie eines App Service-Zertifikats).
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>Schritt 3: Speichern des Zertifikats in Azure Key Vault

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) ist ein Azure-Dienst zum Schutz von kryptografischen Schlüsseln und Geheimnissen, die von Cloudanwendungen und -diensten verwendet werden.
>

Öffnen Sie nach Erwerb des SSL-Zertifikats die Seite [App Service-Zertifikate](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders).

![Bild von Bereitschaft zum Speichern in KV einfügen](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Der Zertifikatsstatus lautet **Ausstehende Ausstellung**, da Sie noch ein paar Schritte ausführen müssen, bevor Sie das Zertifikat verwenden können.

Klicken Sie auf der Seite „Zertifikateigenschaften“ auf **Zertifikatkonfiguration** und anschließend auf **Schritt 1: Speichern**, um dieses Zertifikat in Azure Key Vault zu speichern.

Klicken Sie auf der Seite **Key Vault-Status** auf **Key Vault-Repository**, um einen bereits vorhandenen Schlüsseltresor zum Speichern dieses Zertifikats auszuwählen, ODER klicken Sie auf **Neuen Schlüsseltresor erstellen**, um einen neuen Schlüsseltresor im gleichen Abonnement und in der gleichen Ressourcengruppe zu erstellen.

> [!NOTE]
> Die Kosten für die Speicherung des Zertifikats sind bei Azure Key Vault sehr gering.
> Weitere Informationen finden Sie in der **[Preisübersicht zu Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Nach der Wahl des Key Vault-Repositorys für die Speicherung des Zertifikats sollte der Schritt **Speichern** als erfolgreich angezeigt werden.

![Bild von der erfolgreichen Ausführung des Schritts „Speichern“ einfügen](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Schritt 4: Überprüfen des Domänenbesitzes

Klicken Sie auf der Seite **Zertifikatkonfiguration**, die Sie in Schritt 3 verwendet haben, auf **Schritt 2: Überprüfen**.

Wählen Sie die bevorzugte Überprüfungsmethode für die Domäne aus. 

App Service-Zertifikate unterstützen vier Arten der Domänenüberprüfung: App Service-, Domänen-, Mail- und manuelle Überprüfung. Diese Überprüfungstypen werden im Abschnitt [Erweitert](#advanced) ausführlicher erläutert.

> [!NOTE]
> Die **App Service-Überprüfung** ist die einfachste Option, wenn die Domäne, die Sie überprüfen möchten, bereits einer App Service-App im gleichen Abonnement zugeordnet ist. Sie nutzt die Tatsache aus, dass die App Service-App den Domänenbesitz bereits überprüft hat.
>

Klicken Sie auf die Schaltfläche **Überprüfen**, um diesen Schritt abzuschließen.

![Bild von der Domänenüberprüfung einfügen](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Klicken Sie nach dem Klicken auf **Überprüfen** auf die Schaltfläche **Aktualisieren**. Daraufhin sollte der Schritt **Überprüfen** als erfolgreich angezeigt werden.

![Bild von der erfolgreichen Ausführung des Schritts „Überprüfen“ einfügen](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Schritt 5: Zuweisen des Zertifikats zur App Service-App

> [!NOTE]
> Bevor Sie die Schritte in diesem Abschnitt ausführen, müssen Sie einen benutzerdefinierten Domänennamen mit Ihrer App verknüpft haben. Weitere Informationen finden Sie unter **[Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App](app-service-web-tutorial-custom-domain.md)**.
>

Klicken Sie im **[Azure-Portal](https://portal.azure.com/)** auf der linken Seite auf **App Service**.

Klicken Sie auf den Namen Ihrer App, der Sie dieses Zertifikat zuweisen möchten.

Klicken Sie in den **Einstellungen** auf **SSL-Einstellungen**.

Klicken Sie auf **App Service-Zertifikat importieren**, und wählen Sie das soeben erworbene Zertifikat aus.

![Bild von Zertifikatimport einfügen](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

Klicken Sie im Abschnitt **SSL-Bindungen** auf **Add bindings** (Bindungen hinzufügen), und wählen Sie mithilfe der Dropdownlisten den Domänennamen, der mit SSL geschützt werden soll, sowie das zu verwendende Zertifikat aus. Sie können auch auswählen, ob SSL auf der **[Servernamensanzeige](http://en.wikipedia.org/wiki/Server_Name_Indication)** (Server Name Indication, SNI) oder IP basieren soll.

![Bild von SSL-Bindungen einfügen](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Klicken Sie auf **Bindung hinzufügen** , um die Änderungen zu speichern und SSL zu aktivieren.

> [!NOTE]
> Wenn Sie **IP-basiertes SSL** ausgewählt haben und Ihre benutzerdefinierte Domäne einen A-Eintrag verwendet, sind folgende Zusatzschritte erforderlich. Diese werden im Abschnitt [Erweitert](#Advanced) ausführlicher erläutert.

Ihre App sollte nun über `HTTPS://` (anstelle von `HTTP://`) erreichbar sein. So können Sie überprüfen, ob das Zertifikat richtig konfiguriert wurde.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Schritt 6: Verwaltungsaufgaben

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Erweitert

### <a name="verifying-domain-ownership"></a>Überprüfen des Domänenbesitzes

App Service-Zertifikate unterstützen zwei weitere Arten der Domänenüberprüfung: Domänen- und manuelle Überprüfung.

#### <a name="domain-verification"></a>Domänenüberprüfung

Verwenden Sie diese Option nur für [App Service-Domänen, die Sie von Azure erworben haben](custom-dns-web-site-buydomains-web-app.md). Azure fügt die TXT-Überprüfungseinträge automatisch für Sie hinzu und schließt den Vorgang ab.

#### <a name="manual-verification"></a>Manuelle Überprüfung

> [!IMPORTANT]
> HTML-Webseitenüberprüfung (funktioniert nur mit Standardzertifikat-SKU)
>

1. Erstellen der HTML-Datei **starfield.html**

1. Der Inhalt dieser Datei muss exakt dem Namen des Domänenüberprüfungstokens entsprechen. (Sie können das Token von der Seite mit dem Domänenüberprüfungsstatus kopieren.)

1. Aktualisieren Sie diese Datei im Stammverzeichnis des Webservers, der Ihre Domäne hostet: `/.well-known/pki-validation/starfield.html`

1. Klicken Sie nach Abschluss der Überprüfung auf **Aktualisieren**, um den Zertifikatstatus zu aktualisieren. Es kann einige Minuten dauern, bis die Überprüfung abgeschlossen ist.

> [!TIP]
> Vergewissern Sie sich in einem Terminal mithilfe von `curl -G http://<domain>/.well-known/pki-validation/starfield.html`, dass die Antwort das `<verification-token>` enthält.

#### <a name="dns-txt-record-verification"></a>Überprüfung des DNS-TXT-Eintrags

1. Erstellen Sie mithilfe des DNS-Managers einen TXT-Eintrag in der Unterdomäne `@` mit einem Wert, der dem Domänenüberprüfungstoken entspricht.
1. Klicken Sie nach Abschluss der Überprüfung auf **Aktualisieren**, um den Zertifikatstatus zu aktualisieren.

> [!TIP]
> Sie müssen einen TXT-Eintrag unter `@.<domain>` mit dem Wert `<verification-token>` erstellen.

### <a name="assign-certificate-to-app-service-app"></a>Zuweisen des Zertifikats zur App Service-App

Wenn Sie **IP-basiertes SSL** ausgewählt haben und Ihre benutzerdefinierte Domäne einen A-Datensatz verwendet, müssen Sie die folgenden zusätzlichen Schritte ausführen:

Nach der Konfiguration einer IP-basierten SSL-Bindung wird Ihrer App eine dedizierte IP-Adresse zugewiesen. Diese IP-Adresse befindet sich auf der Seite **Benutzerdefinierte Domäne** unter den Einstellungen Ihrer App (direkt über dem Abschnitt **Hostnamen**). Sie ist als **Externe IP-Adresse** angegeben.

![Bild von IP-SSL einfügen](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Diese IP-Adresse unterscheidet sich von der virtuellen IP-Adresse, die zuvor zum Konfigurieren des A-Eintrags für Ihre Domäne verwendet wurde. Wenn SNI-basiertes SSL konfiguriert bzw. die Verwendung von SSL nicht konfiguriert ist, ist für diesen Eintrag keine Adresse angegeben.

Ändern Sie mit den von der Domänennamen-Registrierungsstelle bereitgestellten Tools den A-Datensatz für den benutzerdefinierten Domänennamen, sodass dieser auf die im vorherigen Schritt genannte IP-Adresse verweist.

## <a name="rekey-and-sync-the-certificate"></a>Erneute Schlüsselerstellung für das Zertifikat und Synchronisierung

Wenn Sie erneut einen Schlüssel für Ihr Zertifikat erstellen müssen, wählen Sie auf der Seite **Zertifikateigenschaften** die Option **Erstellung neuer Schlüssel und Synchronisierung** aus.

Klicken Sie auf die Schaltfläche **Erneute Schlüsselerstellung**, um den Prozess zu initiieren. Dieser Prozess kann 1 bis 10 Minuten in Anspruch nehmen.

![Bild von erneuter Schlüsselerstellung für SSL einfügen](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Bei erneuter Schlüsselerstellung für Ihr Zertifikat wird von der Zertifizierungsstelle ein neues Zertifikat ausgestellt.

## <a name="renew-the-certificate"></a>Erneuern von Zertifikaten

Um die automatische Verlängerung zu aktivieren, klicken Sie auf der Zertifikatverwaltungsseite auf **Einstellungen für die automatische Verlängerung**. Klicken Sie nacheinander auf **Ein** und **Speichern**. Zertifikate können vor Ablauf automatisch um 90 Tage verlängert werden, wenn Sie die automatische Verlängerung aktiviert haben.

![](./media/app-service-web-purchase-ssl-web-site/auto-renew.png)

Um das Zertifikat manuell zu verlängern, klicken Sie stattdessen auf **Manuelle Verlängerung**. Sie können anfordern, dass Ihr Zertifikat vor Ablauf manuell um 60 Tage verlängert wird.

> [!NOTE]
> Das verlängerte Zertifikat ist nicht automatisch an Ihre App gebunden, unabhängig davon, ob es manuell oder automatisch verlängert wurde. Informationen dazu, wie Sie es an Ihre App binden, finden Sie unter [Erneuern von Zertifikaten](./app-service-web-tutorial-custom-ssl.md#renew-certificates). 

<a name="notrenewed"></a>
## <a name="why-is-my-certificate-not-auto-renewed"></a>Warum wird mein Zertifikat nicht automatisch verlängert?

Wenn Ihr SSL-Zertifikat für die automatische Verlängerung konfiguriert ist, aber nicht automatisch verlängert wird, liegt möglicherweise eine ausstehende Domänenüberprüfung vor. Beachten Sie Folgendes: 

- Für GoDaddy (wird zum Generieren von App Service-Zertifikaten verwendet) ist alle zwei Jahre eine Überprüfung der Domäne erforderlich. Der Domänenadministrator erhält alle drei Jahre eine E-Mail mit dem Hinweis, die Domäne zu überprüfen. Wenn E-Mail oder Domäne nicht überprüft werden, kann das App Service-Zertifikat nicht automatisch verlängert werden. 
- Aufgrund einer Änderung in der GoDaddy-Richtlinie erfordern alle App Service-Zertifikate, die vor dem 1. März 2017 ausgestellt wurden, bei der nächsten Verlängerung eine erneute Überprüfung der Domäne (selbst wenn für das Zertifikat die automatische Verlängerung aktiviert ist). Überprüfen Sie Ihre E-Mail, und führen Sie diese einmalige Domänenüberprüfung durch, um die automatische Verlängerung des App Service-Zertifikats fortzusetzen. 

## <a name="more-resources"></a>Weitere Ressourcen

* [Erzwingen von HTTPS](app-service-web-tutorial-custom-ssl.md#enforce-https)
* [Erzwingen von TLS 1.1/1.2](app-service-web-tutorial-custom-ssl.md#enforce-tls-1112)
* [Verwenden eines SSL-Zertifikats in Ihrem Anwendungscode in Azure App Service](app-service-web-ssl-cert-load.md)
* [Häufig gestellte Fragen: App Service-Zertifikate](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/24/faq-app-service-certificates/)
