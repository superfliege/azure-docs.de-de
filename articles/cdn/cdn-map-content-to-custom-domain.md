---
title: "Hinzufügen einer benutzerdefinierten Domäne zum CDN-Endpunkt | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zum Zuordnen von CDN-Inhalten zu einer benutzerdefinierten Domäne."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: mazha
ms.openlocfilehash: 98d4900e28f1850050dc4fbe1f97435e52afaf08
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="add-a-custom-domain-to-your-cdn-endpoint"></a>Hinzufügen einer benutzerdefinierten Domäne zum CDN-Endpunkt
Nachdem Sie ein Profil erstellt haben, erstellen Sie in der Regel auch mindestens einen [CDN-Endpunkt](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint) (eine Unterdomäne von `azureedge.net`) zum Übermitteln Ihrer Inhalte über HTTP und HTTPS. Dieser Endpunkt ist standardmäßig in allen Ihren URLs enthalten (z.B. `https://contoso.azureedge.net/photo.png`). Der Einfachheit halber können Sie Ihrem Endpunkt mit Azure CDN eine benutzerdefinierte Domäne (z.B. `www.contoso.com`) zuordnen. Mit dieser Option verwenden Sie eine benutzerdefinierte Domäne statt des Endpunkts, um Ihre Inhalte zu übermitteln. Diese Option ist beispielsweise hilfreich, wenn Ihr eigener Domänennamen Ihren Kunden zu Brandingzwecken angezeigt werden soll.

Wenn Sie nicht bereits über eine benutzerdefinierte Domäne verfügen, müssen Sie zunächst bei einem Domänenanbieter eine erwerben. Nachdem Sie eine benutzerdefinierte Domäne erhalten haben, gehen Sie folgendermaßen vor:
1. [Greifen Sie auf die DNS-Einträge Ihres Domänenanbieters zu](#step-1-access-dns-records-by-using-your-domain-provider)
2. [Erstellen Sie die CNAME-DNS-Einträge](#step-2-create-the-cname-dns-records)
    - Option 1: Direkte Zuordnung Ihrer benutzerdefinierten Domäne zum CDN-Endpunkt
    - Option 2: Zuordnung Ihrer benutzerdefinierten Domäne zum CDN-Endpunkt mithilfe von „cdnverify“ 
3. [Aktivieren der Zuordnung von CNAME-Einträgen in Azure](#step-3-enable-the-cname-record-mapping-in-azure)
4. [Überprüfen, ob die benutzerdefinierte Unterdomäne auf Ihren CDN-Endpunkt verweist](#step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint)
5. [(Abhängiger Schritt) Zuordnen der permanenten benutzerdefinierten Domäne zum CDN-Endpunkt](#step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint)

## <a name="step-1-access-dns-records-by-using-your-domain-provider"></a>Schritt 1: Zugreifen auf DNS-Datensätze mithilfe Ihres Domänenanbieters

Wenn Sie Azure zum Hosten Ihrer [DNS-Domänen](https://docs.microsoft.com/en-us/azure/dns/dns-overview) verwenden, müssen Sie das DNS des Domänenanbieters an eine Azure DNS-Instanz delegieren. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns).

Wenn Sie andernfalls Ihren Domänenanbieter verwenden, um Ihre DNS-Domäne zu behandeln, melden Sie sich bei der Website des Domänenanbieters an. Suchen Sie die Seite für die Verwaltung von DNS-Einträgen mithilfe der Dokumentation des Anbieters, oder suchen Sie nach Bereichen der Website mit der Bezeichnung **Domänennamen**, **DNS**, oder **Namensserververwaltung**. Die Seite für DNS-Einträge finden Sie häufig in Ihren Kontoinformationen und beim Suchen nach einem Link wie **Eigene Domänen**. Einige Anbieter verfügen über unterschiedliche Links, um unterschiedliche Arten von Einträgen hinzuzufügen.

> [!NOTE]
> Bei einigen Anbietern wie GoDaddy werden Änderungen an DNS-Einträgen erst wirksam, wenn Sie einen gesonderten Link **Änderungen speichern** wählen. 


## <a name="step-2-create-the-cname-dns-records"></a>Schritt 2: Erstellen Sie die CNAME-DNS-Einträge

Bevor Sie eine benutzerdefinierte Domäne mit einem Azure CDN-Endpunkt verwenden können, müssen Sie zunächst einen kanonischen Namenseintrag (CNAME) bei Ihrem Domänenanbieter erstellen. Ein CNAME-Eintrag ist ein Eintrag in das Domain Name System (DNS), der eine Quelldomäne durch Angeben eines Aliasdomänennamens für den „kanonischen“ oder „echten“ Domänennamen einer Zieldomäne zuordnet. Für Azure CDN ist die Quelldomäne Ihre benutzerdefinierte Domäne (und die Unterdomäne), und die Zieldomäne entspricht Ihrem CDN-Endpunkt. Azure CDN überprüft den CNAME-DNS-Eintrag, wenn Sie die benutzerdefinierte Domäne dem Endpunkt von Portal oder API hinzufügen. 

Ein CNAME-Eintrag ordnet eine bestimmte Domäne und Unterdomäne zu, z.B. `www.contoso.com` oder `cdn.contoso.com`; es ist nicht möglich, einen CNAME-Eintrag einer Stammdomäne wie `contoso.com` zuzuordnen. Eine Unterdomäne kann nur einem CDN-Endpunkt zugordnet werden, und der von Ihnen erstellte CNAME-Eintrag leitet sämtlichen Datenverkehr für die Unterdomäne an den angegebenen Endpunkt weiter. Wenn Sie dem CDN-Endpunkt beispielsweise die Unterdomäne `www.contoso.com` zuordnen, können Sie diese Unterdomäne keinem anderen Azure-Endpunkt, z.B. einem Speicherkonto- oder Clouddienst-Endpunkt, zuordnen. Innerhalb einer Domäne können Sie jedoch verschiedene Unterdomänen für unterschiedliche Dienstendpunkte verwenden. Außerdem haben Sie die Möglichkeit, demselben CDN-Endpunkt verschiedene Unterdomänen zuzuordnen.

Verwenden Sie eine der folgenden Optionen, um Ihre benutzerdefinierte Domäne einem CDN-Endpunkt zuzuordnen:

- Option 1: Direkte Zuordnung. Wenn kein Produktionsdatenverkehr auf der benutzerdefinierten Domäne ausgeführt wird, können Sie eine benutzerdefinierte Domäne direkt einem CDN-Endpunkt zuordnen. Die Zuordnung der benutzerdefinierten Domäne zum CDN-Endpunkt kann jedoch zu einer kurzzeitigen Downtime für die Domäne führen, während Sie die Domäne im Azure-Portal registrieren. Ihr CNAME-Zuordnungseintrag muss folgendes Format aufweisen: 
 
  | NAME             | TYPE  | VALUE                  |
  |------------------|-------|------------------------|
  | www\.consoto.com | CNAME | consoto\.azureedge.net |


- Option 2: Zuordnung mit der **cdnverify**-Unterdomäne. Wenn Produktionsdatenverkehr, der nicht unterbrochen werden kann, auf der benutzerdefinierten Domäne ausgeführt wird, können Sie eine temporäre CNAME-Zuordnung zu Ihrem CDN-Endpunkt erstellen. Mit dieser Option nutzen Sie die **cdnverify**-Unterdomäne von Azure, um einen vorübergehenden Registrierungsschritt bereitzustellen, damit Benutzer während der DNS-Zuordnung ohne Unterbrechung auf Ihre Domäne zugreifen können.

   1. Erstellen Sie einen neuen CNAME-Eintrag, und geben Sie einen Unterdomänenalias ein, der die Unterdomäne **cdnverify** enthält. Beispielsweise **cdnverify.www** oder **cdnverify.cdn**. 
   2. Geben Sie den Hostnamen, der Ihr CDN-Endpunkt ist, im folgenden Format an: `cdnverify.<EndpointName>.azureedge.net`. Ihr CNAME-Zuordnungseintrag muss folgendes Format aufweisen: 

   | NAME                       | TYPE  | VALUE                            |
   |----------------------------|-------|----------------------------------|
   | cdnverify.www\.consoto.com | CNAME | cdnverify.consoto\.azureedge.net | 


## <a name="step-3-enable-the-cname-record-mapping-in-azure"></a>Schritt 3: Aktivieren der Zuordnung von CNAME-Einträgen in Azure

Nachdem Sie Ihre benutzerdefinierte Domäne in einem der vorherigen Schritte registriert haben, können Sie dann das Feature „benutzerdefinierte Domäne“ in Azure CDN aktivieren. 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und navigieren Sie zu dem CDN-Profil mit dem Endpunkt, den Sie einer benutzerdefinierten Domäne zuordnen möchten.  
2. Wählen Sie auf dem Blatt **CDN-Profil** den CDN-Endpunkt, den Sie der Unterdomäne zuordnen möchten.
3. Klicken Sie in der oberen linken Ecke des Endpunktblatts auf **Benutzerdefinierte Domäne**. 

   ![Schaltfläche „Benutzerdefinierte Domäne“](./media/cdn-map-content-to-custom-domain/cdn-custom-domain-button.png)

4. Geben Sie im Textfeld **Benutzerdefinierter Hostname** Ihre benutzerdefinierte Domäne einschließlich der Unterdomäne ein. Beispiel: `www.contoso.com` oder `cdn.contoso.com`.

   ![Dialogfeld „Hinzufügen eines benutzerdefinierten Domänennamens“](./media/cdn-map-content-to-custom-domain/cdn-add-custom-domain-dialog.png)

5. Klicken Sie auf **Hinzufügen**.

   Azure überprüft, ob der CNAME-Eintrag für den eingegebenen Domänennamen vorhanden ist. Wenn der CNAME-Eintrag korrekt ist, wird Ihre benutzerdefinierte Domäne überprüft. Es kann eine Weile dauern, bis der CNAME-Eintrag an alle Namensserver verteilt ist. Wenn die Domäne nicht sofort erkannt wird, stellen Sie sicher, dass der CNAME-Eintrag richtig ist, warten Sie einige Minuten, und überprüfen Sie die Domäne dann erneut. Bei **Azure CDN von Verizon**-Endpunkten (Standard und Premium) kann es bis zu 90 Minuten dauern, Einstellungen der benutzerdefinierten Domäne an alle CDN-Edgeknoten weiterzugeben.  


## <a name="step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>Schritt 4: Überprüfen Sie, ob die benutzerdefinierte Unterdomäne auf Ihren CDN-Endpunkt verweist

Nachdem Sie die Registrierung der benutzerdefinierten Domäne abgeschlossen haben, stellen Sie sicher, dass die benutzerdefinierte Unterdomäne auf Ihren CDN-Endpunkt verweist.
 
1. Stellen Sie sicher, dass öffentliche Inhalte zum Zwischenspeichern auf dem Endpunkt verfügbar sind. Wenn der CDN-Endpunkt beispielsweise einem Speicherkonto zugeordnet ist, wird der Inhalt vom CDN in öffentlichen Blobcontainern zwischengespeichert. Zum Testen der benutzerdefinierten Domäne stellen Sie sicher, dass der Container den öffentlichen Zugriff zulässt und mindestens ein Blob enthält.

2. Navigieren Sie in Ihrem Browser mithilfe der benutzerdefinierten Domäne zur Adresse des Blobs. Wenn Ihre benutzerdefinierte Domäne z.B. `cdn.contoso.com` ist, sollte die URL für das zwischengespeicherte Blob der folgenden URL ähneln: `http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg`.


## <a name="step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint"></a>Schritt 5 (abhängiger Schritt): Zuordnen der permanenten benutzerdefinierten Domäne zum CDN-Endpunkt

Dieser Schritt ist abhängig von Schritt 2, Option 2 (Zuordnung mit der **cdnverify**-Unterdomäne). Wenn Sie die temporäre **cdnverify**-Unterdomäne verwendet und überprüft haben, ob sie funktioniert, können Sie die permanente benutzerdefinierte Domäne dem CDN-Endpunkt zuordnen.

1. Erstellen Sie auf der Website Ihres Domänenanbieters einen CNAME-DNS-Eintrag, um die permanente benutzerdefinierte Domäne dem CDN-Endpunkt zuzuordnen. Ihr CNAME-Zuordnungseintrag muss folgendes Format aufweisen: 
 
   | NAME             | TYPE  | VALUE                  |
   |------------------|-------|------------------------|
   | www\.consoto.com | CNAME | consoto\.azureedge.net |
2. Löschen Sie den CNAME-Eintrag mit der **cdnverify**-Unterdomäne, die Sie zuvor erstellt haben.

## <a name="see-also"></a>Weitere Informationen
[Aktivieren des CDN (Content Delivery Network) für Azure](cdn-create-new-endpoint.md)  
[Delegieren einer Domäne an Azure DNS](../dns/dns-domain-delegation.md)