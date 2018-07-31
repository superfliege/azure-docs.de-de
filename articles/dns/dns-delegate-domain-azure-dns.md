---
title: 'Tutorial: Hosten Ihrer Domäne und Unterdomäne in Azure DNS'
description: In diesem Tutorial erfahren Sie, wie Sie Azure DNS zum Hosten Ihrer DNS-Zonen konfigurieren.
services: dns
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: tutorial
ms.date: 6/13/2018
ms.author: victorh
ms.openlocfilehash: 44f5bf9a28d56e85bae1d50136c50868ec96eb4e
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205440"
---
# <a name="tutorial-host-your-domain-in-azure-dns"></a>Tutorial: Hosten Ihrer Domäne in Azure DNS

Sie können Azure DNS verwenden, um Ihre DNS-Domäne zu hosten und die DNS-Einträge zu verwalten. Durch das Hosten Ihrer Domänen in Azure können Sie Ihre DNS-Einträge mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste verwalten. 

Nehmen wir an, Sie erwerben die Domäne „contoso.net“ von einer Domänennamen-Registrierungsstelle und erstellen dann eine Zone mit dem Namen „contoso.net“ in Azure DNS. Als Besitzer der Domäne bietet Ihre Registrierungsstelle Ihnen die Möglichkeit, die Namenservereinträge (NS-Einträge) für Ihre Domäne zu konfigurieren. Die Registrierungsstelle speichert die NS-Einträge in der übergeordneten Zone „.net“. Internetbenutzer auf der ganzen Welt, die DNS-Einträge in „contoso.net“ auflösen möchten, werden dann an Ihre Domäne in der Azure DNS-Zone weitergeleitet.


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer DNS-Zone
> * Abrufen einer Liste der Namenserver
> * Delegieren der Domäne
> * Sicherstellen, dass die Delegierung funktioniert


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

1. Melden Sie sich beim Azure-Portal an.
1. Klicken Sie oben links auf **Ressource erstellen** > **Netzwerk** > **DNS-Zone**, um die Seite **DNS-Zone erstellen** zu öffnen.

   ![DNS-Zone](./media/dns-delegate-domain-azure-dns/openzone650.png)

1. Geben Sie auf der Seite **DNS-Zone erstellen** die folgenden Werte ein, und wählen Sie dann **Erstellen**:

   | **Einstellung** | **Wert** | **Details** |
   |---|---|---|
   |**Name**|[Ihr Domänenname] |Der von Ihnen erworbene Domänenname. In diesem Tutorial wird als Beispiel „contoso.net“ verwendet.|
   |**Abonnement**|[Ihr Abonnement]|Wählen Sie ein Abonnement aus, in dem Sie die Zone erstellen möchten.|
   |**Ressourcengruppe**|**Neu erstellen:** contosoRG|Erstellen Sie eine Ressourcengruppe. Der Name der Ressourcengruppe muss innerhalb des von Ihnen ausgewählten Abonnements eindeutig sein. |
   |**Location**|USA (Ost)||

> [!NOTE]
> Der Standort der Ressourcengruppe hat keine Auswirkung auf die DNS-Zone. Der Standort der DNS-Zone ist immer „global“ und wird nicht angezeigt.

## <a name="retrieve-name-servers"></a>Abrufen von Namenservern

Bevor Sie Ihre DNS-Zone an Azure DNS delegieren können, müssen Sie die Namenserver für Ihre Zone ermitteln. Azure DNS weist bei jeder Zonenerstellung Namenserver aus einem Pool zu.

1. Wählen Sie nach der Erstellung der DNS-Zone im Bereich **Favoriten** des Azure-Portals die Option **Alle Ressourcen**. Wählen Sie auf der Seite **Alle Ressourcen** Ihre DNS-Zone aus. Falls das von Ihnen ausgewählte Abonnement bereits mehrere Ressourcen enthält, können Sie in das Feld **Nach Name filtern** Ihren Domänennamen eingeben um komfortabel auf das Anwendungsgateway zuzugreifen. 

1. Rufen Sie die Namenserver über die Seite „DNS-Zone“ ab. In diesem Beispiel wurden der Zone „contoso.net“ die Namenserver *ns1-01.azure-dns.com*, *ns2-01.azure-dns.net*, *ns3-01.azure-dns.org* und *ns4-01.azure-dns.info* zugewiesen:

   ![Liste der Namenserver](./media/dns-delegate-domain-azure-dns/viewzonens500.png)

Azure DNS erstellt in Ihrer Zone automatisch autoritative NS-Einträge für die zugewiesenen Namenserver.


## <a name="delegate-the-domain"></a>Delegieren der Domäne

Nachdem Sie die DNS-Zone erstellt haben und über die Namenserver verfügen, müssen Sie die übergeordnete Domäne mit den Azure DNS-Namenservern aktualisieren. Jede Registrierungsstelle hat ihre eigenen DNS-Verwaltungstools, um die Namenservereinträge für eine Domäne zu ändern. Bearbeiten Sie auf der DNS-Verwaltungsseite der Registrierungsstelle die NS-Einträge, und ersetzen Sie die NS-Einträge durch die Azure DNS-Namenserver.

Wenn Sie eine Domäne an Azure DNS delegieren, müssen Sie die von Azure DNS bereitgestellten Namenserver verwenden. Wir empfehlen, unabhängig vom Namen Ihrer Domäne alle vier Namenserver zu verwenden. Für die Domänendelegierung ist es nicht erforderlich, dass ein Namensserver dieselbe Domäne der obersten Ebene wie Ihre Domäne verwendet.

Delegierungen, für die Namenserver in Ihrer eigenen Zone verwendet werden (manchmal als *Vanity-Namenserver* bezeichnet), werden derzeit in Azure DNS nicht unterstützt.

## <a name="verify-that-the-delegation-is-working"></a>Sicherstellen, dass die Delegierung funktioniert

Nach Abschluss der Delegierung können Sie überprüfen, ob sie funktioniert, indem Sie mithilfe eines Tools wie *nslookup* den SOA-Eintrag (Start of Authority, Autoritätsursprung) für die Zone abrufen. Der SOA-Eintrag wird beim Erstellen der Zone automatisch erstellt. Nach Abschluss der Delegierung müssen Sie unter Umständen mindestens zehn Minuten warten, bis Sie überprüfen können, ob sie funktioniert. Es kann einige Zeit dauern, bis Änderungen über das DNS-System weitergegeben wurden.

Sie müssen die Azure DNS-Namenserver nicht angeben. Wenn die Delegierung ordnungsgemäß eingerichtet ist, werden die Namenserver beim normalen DNS-Auflösungsvorgang automatisch gefunden.

Geben Sie an einer Eingabeaufforderung etwa den folgenden nslookup-Befehl ein:

```
nslookup -type=SOA contoso.net
```

Hier sehen Sie eine Beispielantwort für den vorherigen Befehl:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.net
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Ressourcengruppe **contosoRG** behalten, wenn Sie das nächste Tutorial absolvieren möchten. Löschen Sie andernfalls die Ressourcengruppe **contosoRG**, um die in diesem Tutorial erstellten Ressourcen zu löschen. Klicken Sie dazu auf die Ressourcengruppe **contosoRG** und dann auf **Ressourcengruppe löschen**. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie eine DNS-Zone für Ihre Domäne erstellt und sie an Azure DNS delegiert. Weitere Informationen zu Azure DNS und Web-Apps finden Sie im Tutorial für Web-Apps.

> [!div class="nextstepaction"]
> [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](./dns-web-sites-custom-domain.md)
