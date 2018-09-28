---
title: 'Tutorial: Erstellen eines Azure DNS-Alias-Ressourceneintrags zur Unterstützung von Apex-Domänennamen mit Traffic Manager'
description: In diesem Tutorial erfahren Sie, wie Sie einen Azure DNS-Alias-Ressourceneintrag für die Verwendung des Apex-Domänennamens mit Traffic Manager konfigurieren.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 4a5d8968861f6f2938e605d2f7106529ca401df4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967437"
---
# <a name="tutorial-configure-an-alias-record-to-support-apex-domain-names-with-traffic-manager"></a>Tutorial: Konfigurieren eines Alias-Ressourceneintrags zur Unterstützung von Apex-Domänennamen mit Traffic Manager 

Sie können einen Alias-Ressourceneintrag für den Domänennamen-Apex (z.B. „contoso.com“) erstellen, um auf ein Traffic Manager-Profil zu verweisen. Statt hierfür einen Umleitungsdienst zu verwenden, können Sie Azure DNS so konfigurieren, dass direkt aus der Zone auf ein Traffic Manager-Profil verwiesen wird. 


In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines Host-VM und einer Netzwerkinfrastruktur
> * Erstellen eines Traffic Manager-Profils
> * Erstellen eines Alias-Ressourceneintrags
> * Testen des Alias-Ressourceneintrags


Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
Sie müssen über einen Domänennamen verfügen, den Sie zum Testen in Azure DNS hosten können. Sie müssen die vollständige Kontrolle über diese Domäne haben, einschließlich der Fähigkeit, die NS-Einträge (Namenserver) für die Domäne festzulegen.

Anweisungen zum Hosten der Domäne in Azure DNS finden Sie unter [Tutorial: Hosten Ihrer Domäne in Azure DNS](dns-delegate-domain-azure-dns.md).

Die für dieses Tutorial verwendete Beispieldomäne lautet „contoso.com“, Sie sollten jedoch einen eigenen Domänennamen verwenden.

## <a name="create-the-network-infrastructure"></a>Erstellen der Netzwerkinfrastruktur
Erstellen Sie zuerst ein VNet und ein Subnetz, um die Webserver darin zu platzieren.
1. Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.
2. Klicken Sie links oben im Portal auf **Ressource erstellen**, geben Sie im Suchfeld *Ressourcengruppe* ein, und erstellen Sie eine Ressourcengruppe mit dem Namen **RG-DNS-Alias-TM**.
3. Klicken Sie auf **Ressource erstellen**, auf **Netzwerk** und dann auf **Virtuelles Netzwerk**.
4. Erstellen Sie ein virtuelles Netzwerk mit dem Namen **VNet-Servers**, fügen Sie es der Ressourcengruppe **RG-DNS-Alias-TM** hinzu, und benennen Sie das Subnetz mit **SN-Web**.

## <a name="create-two-web-server-virtual-machines"></a>Erstellen von zwei virtuellen Webservercomputern
1. Klicken Sie auf **Ressource erstellen** und auf **Windows Server 2016-VM**.
2. Geben Sie als Namen **Web-01** ein, und fügen Sie der Ressourcengruppe **RG-DNS-Alias-TM** den virtuellen Computer hinzu. Geben Sie einen Benutzernamen und ein Kennwort ein, und klicken Sie auf **OK**.
3. Wählen Sie für **Größe** eine SKU mit 8 GB RAM.
4. Wählen Sie für **Einstellungen** das virtuelle Netzwerk **VNet-Servers** und das Subnetz **SN-Web** aus.
5. Klicken Sie auf **Öffentliche IP-Adresse**, klicken Sie unter **Zuweisung** auf **Statisch**, und klicken Sie dann auf **OK**.
6. Wählen Sie für „Öffentliche Eingangsports“ **HTTP**, **HTTPS** und **RDP (3389)** aus, und klicken Sie dann auf **OK**.
7. Klicken Sie auf der Zusammenfassungsseite auf **Erstellen**.

   Der Vorgang kann einige Minuten dauern.
6. Wiederholen Sie dieses Verfahren, um einen weiteren virtuellen Computer mit dem Namen **Web-02** zu erstellen.

### <a name="add-a-dns-label"></a>Hinzufügen einer DNS-Bezeichnung
Für die Verwendung der öffentlichen IP-Adressen mit Traffic Manager ist eine DNS-Bezeichnung erforderlich.
1. Klicken Sie in der Ressourcengruppe **RG-DNS-Alias-TM** auf die öffentliche IP-Adresse **Web-01-ip**.
2. Klicken Sie unter **Einstellungen** auf **Konfiguration**.
3. Geben Sie im Textfeld „DNS-Namensbezeichnung“ **web01pip** ein.
4. Klicken Sie auf **Speichern**.

Wiederholen Sie dieses Verfahren für die öffentliche IP-Adresse **Web-02-ip**, und verwenden Sie **web02pip** als DNS-Namensbezeichnung.

### <a name="install-iis"></a>Installieren von IIS

Installieren Sie IIS auf **Web-01** und **Web-02**.

1. Stellen Sie eine Verbindung mit **Web-01** her, und melden Sie sich an.
2. Klicken Sie im Server-Manager-Dashboard auf **Rollen und Features hinzufügen**.
3. Klicken Sie dreimal auf **Weiter**, und wählen Sie auf der Seite **Serverrollen** die Option **Webserver (IIS)** aus.
4. Klicken Sie auf **Features hinzufügen** und auf **Weiter**.
5. Klicken Sie viermal auf **Weiter**, und klicken Sie dann auf **Installieren**.

   Dies nimmt einige Minuten in Anspruch.
6. Klicken Sie nach Abschluss der Installation auf **Schließen**.
7. Öffnen Sie einen Webbrowser, und navigieren Sie zu **localhost**, um sicherzustellen, dass die IIS-Standardwebseite angezeigt wird.

Wiederholen Sie diesen Vorgang, um IIS auf **Web-02** zu installieren.


## <a name="create-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils

Due 

1. Öffnen Sie die Ressourcengruppe **RG-DNS-Alias-TM**, und klicken Sie auf die öffentliche IP-Adresse **Web-01-ip**. Notieren Sie sich die IP-Adresse für die spätere Verwendung. Wiederholen Sie den Vorgang für die öffentliche IP-Adresse **Web-02-ip**.
1. Klicken Sie auf **Ressource erstellen**, auf **Netzwerk** und dann auf **Traffic Manager-Profil**.
2. Geben Sie als Namen **TM-alias-test** ein, und fügen Sie der Ressourcengruppe **RG-DNS-Alias-TM** das Profil hinzu.
3. Klicken Sie auf **Create**.
4. Wenn die Bereitstellung abgeschlossen ist, klicken Sie auf **Zu Ressource wechseln**.
5. Klicken Sie auf der Seite „Traffic Manager-Profil“ unter **Einstellungen** auf **Endpunkte**.
6. Klicken Sie auf **Hinzufügen**.
7. Wählen Sie für **Typ** die Option **Externer Endpunkt** aus, und geben Sie für **Name** den Namen **EP-Web01** ein.
8. Geben Sie im Textfeld **Vollqualifizierter Domänenname (FQDN) oder IP-Adresse** die IP-Adresse für **Web-01-ip** ein, die Sie zuvor notiert haben.
9. Wählen Sie den gleichen **Speicherort** wie für die anderen Ressourcen aus, und klicken Sie auf **OK**.

Wiederholen Sie dieses Verfahren, um den Endpunkt **Web-02** unter Verwendung der IP-Adresse hinzuzufügen, die Sie zuvor für **Web-02-ip** notiert haben.

## <a name="create-an-alias-record"></a>Erstellen eines Alias-Ressourceneintrags

Erstellen Sie einen Alias-Ressourceneintrag, der auf das Traffic Manager-Profil verweist.

1. Klicken Sie auf die Azure DNS-Zone, um sie zu öffnen.
2. Klicken Sie auf **Datensatzgruppe**.
3. Lassen Sie das Textfeld **Name** leer, um den Domänennamen-Apex (z.B. „contoso.com“) darzustellen.
4. Übernehmen Sie **A**-Eintrag als **Typ**.
5. Klicken Sie auf das Kontrollkästchen **Alias-Ressourceneintrag**.
6. Klicken Sie auf **Azure-Dienst auswählen**, und wählen Sie das Traffic Manager-Profil **TM-alias-test** aus.

## <a name="test-the-alias-record"></a>Testen des Alias-Ressourceneintrags

1. Navigieren Sie in einem Webbrowser zu dem Domänennamen-Apex (z.B. „contoso.com“). Die IIS-Standardwebseite sollte angezeigt werden. Schließen Sie den Webbrowser.
2. Fahren Sie den virtuellen Computer **Web-01** herunter, und warten Sie einige Minuten, bis er vollständig heruntergefahren wurde.
3. Öffnen Sie einen neuen Webbrowser, und navigieren Sie erneut zum Domänennamen-Apex.
4. Es sollte erneut die IIS-Standardseite angezeigt werden, da Traffic Manager die Änderung berücksichtigt und den Datenverkehr zu **Web-02** umgeleitet hat.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn die Ressourcengruppe **RG-DNS-Alias-TM** nicht mehr benötigt wird, können Sie sie löschen, um alle für dieses Tutorial erstellten Ressourcen zu löschen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Alias-Ressourceneintrag erstellt, um mit dem Apex-Domänennamen auf ein Traffic Manager-Profil zu verweisen. Weitere Informationen zu Azure DNS und Web-Apps finden Sie im Tutorial für Web-Apps.

> [!div class="nextstepaction"]
> [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne](./dns-web-sites-custom-domain.md)
