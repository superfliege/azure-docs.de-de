---
title: 'Tutorial: Erstellen eines Azure DNS-Aliaseintrags zum Verweisen auf eine öffentliche Azure-IP-Adresse.'
description: In diesem Tutorial wird gezeigt, wie Sie einen Azure DNS-Aliaseintrag zum Verweisen auf eine öffentliche Azure-IP-Adresse konfigurieren.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 842015d853790e3ac78214cca6a70becb7f9fadf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991228"
---
# <a name="tutorial-configure-an-alias-record-to-refer-to-an-azure-public-ip-address"></a>Tutorial: Konfigurieren eines Aliaseintrags zum Verweisen auf eine öffentliche Azure-IP-Adresse 

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen einer Netzwerkinfrastruktur
> * Erstellen eines virtuellen Webservercomputers
> * Erstellen eines Aliaseintrags
> * Testen des Aliaseintrags


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen über einen Domänennamen verfügen, den Sie zum Testen in Azure DNS hosten können. Sie müssen die vollständige Kontrolle über diese Domäne haben, einschließlich der Fähigkeit, die NS-Einträge (Namenserver) für die Domäne festzulegen.

Anweisungen zum Hosten der Domäne in Azure DNS finden Sie unter [Tutorial: Hosten Ihrer Domäne in Azure DNS](dns-delegate-domain-azure-dns.md).

Die für dieses Tutorial verwendete Beispieldomäne lautet „contoso.com“, Sie sollten jedoch einen eigenen Domänennamen verwenden.

## <a name="create-the-network-infrastructure"></a>Erstellen der Netzwerkinfrastruktur
Erstellen Sie zuerst ein VNET und ein Subnetz, um die Webserver darin zu platzieren.
1. Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.
2. Klicken Sie links oben im Portal auf **Ressource erstellen**, geben Sie im Suchfeld *Ressourcengruppe* ein, und erstellen Sie eine Ressourcengruppe mit dem Namen **RG-DNS-Alias-pip**.
3. Klicken Sie auf **Ressource erstellen**, auf **Netzwerk** und dann auf **Virtuelles Netzwerk**.
4. Erstellen Sie ein virtuelles Netzwerk mit dem Namen **VNet-Server**, fügen Sie es der Ressourcengruppe **RG-DNS-Alias-pip** hinzu, und nennen Sie das Subnetz **SN-Web**.

## <a name="create-a-web-server-virtual-machine"></a>Erstellen eines virtuellen Webservercomputers
1. Klicken Sie auf **Ressource erstellen** und auf **Windows Server 2016-VM**.
2. Geben Sie als Namen **Web-01** ein, und fügen Sie der Ressourcengruppe **RG-DNS-Alias-TM** den virtuellen Computer hinzu. Geben Sie einen Benutzernamen und ein Kennwort ein, und klicken Sie auf **OK**.
3. Wählen Sie für **Größe** eine SKU mit 8 GB RAM.
4. Wählen Sie für **Einstellungen** das virtuelle Netzwerk **VNet-Servers** und das Subnetz **SN-Web** aus. Wählen Sie für „Öffentliche Eingangsports“ die Optionen **HTTP**, **HTTPS** und **RDP (3389)** aus, und klicken Sie dann auf **OK**.
5. Klicken Sie auf der Zusammenfassungsseite auf **Erstellen**.

   Der Vorgang kann einige Minuten dauern.

### <a name="install-iis"></a>Installieren von IIS

Installieren Sie IIS auf **Web-01**.

1. Stellen Sie eine Verbindung mit **Web-01** her, und melden Sie sich an.
2. Klicken Sie im Server-Manager-Dashboard auf **Rollen und Features hinzufügen**.
3. Klicken Sie dreimal auf **Weiter**, und wählen Sie auf der Seite **Serverrollen** die Option **Webserver (IIS)** aus.
4. Klicken Sie auf **Features hinzufügen** und auf **Weiter**.
5. Klicken Sie viermal auf **Weiter**, und klicken Sie dann auf **Installieren**.

   Dies nimmt einige Minuten in Anspruch.
6. Klicken Sie nach Abschluss der Installation auf **Schließen**.
7. Öffnen Sie einen Webbrowser, und navigieren Sie zu **localhost**, um sicherzustellen, dass die IIS-Standardwebseite angezeigt wird.

## <a name="create-an-alias-record"></a>Erstellen eines Aliaseintrags

Erstellen Sie einen Aliaseintrag, der auf die öffentliche IP-Adresse verweist.

1. Klicken Sie auf die Azure DNS-Zone, um sie zu öffnen.
2. Klicken Sie auf **Datensatzgruppe**.
3. Geben Sie in das Textfeld **Name** den Namen **web01** ein.
4. Übernehmen Sie für **Typ** die Option **A-Eintrag**.
5. Aktivieren Sie das Kontrollkästchen **Alias Record Set** (Aliaseintragssatz).
6. Klicken Sie auf **Azure-Dienst auswählen**, und wählen Sie dann die öffentliche IP-Adresse **Web-01-ip** aus.

## <a name="test-the-alias-record"></a>Testen des Aliaseintrags

1. Klicken Sie in der Ressourcengruppe **RG-DNS-Alias-pip** auf den virtuellen Computer **Web-01**. Notieren Sie sich die öffentliche IP-Adresse.
1. Navigieren Sie in einem Webbrowser zum vollqualifizierten Domänennamen für den virtuellen Computer „Web01-01“. Beispiel: **web01.contoso.com**. Die IIS-Standardwebseite sollte angezeigt werden.
2. Schließen Sie den Webbrowser.
3. Beenden Sie den virtuellen Computer **Web-01**, und starten Sie ihn neu.
4. Notieren Sie sich beim Neustart die neue öffentliche IP-Adresse für den virtuellen Computer.
5. Öffnen Sie eine neue Webbrowserinstanz, und navigieren Sie erneut zum vollqualifizierten Domänennamen für den virtuellen Computer „Web01-01“. Beispiel: **web01.contoso.com**.
6. Dieser Vorgang sollte immer noch erfolgreich sein, da Sie einen Aliaseintrag und nicht einen A-Eintrag zum Verweisen auf die öffentliche IP-Adressressource verwendet haben.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe **RG-DNS-Alias-pip** nicht mehr benötigt wird, können Sie sie löschen, um alle für dieses Tutorial erstellten Ressourcen zu löschen.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Aliaseintrag zum Verweisen auf eine öffentliche Azure-IP-Adresse erstellt. Weitere Informationen zu Azure DNS und Web-Apps finden Sie im Tutorial für Web-Apps.

> [!div class="nextstepaction"]
> [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](./dns-web-sites-custom-domain.md)
