---
title: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal
description: In diesem Tutorial erfahren Sie, wie Sie Azure Firewall über das Azure-Portal bereitstellen und konfigurieren.
services: firewall
author: vhorne
manager: jpconnock
ms.service: firewall
ms.topic: tutorial
ms.date: 7/11/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 05959143431a2cc11d79a4012f45eb565c1c91f2
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2018
ms.locfileid: "45575996"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal

Für die Steuerung des ausgehenden Zugriffs stehen in Azure Firewall zwei Arten von Regeln zur Verfügung:

- **Anwendungsregeln**

   Ermöglichen das Konfigurieren vollqualifizierter Domänennamen (Fully Qualified Domain Names, FQDNs), auf die von einem Subnetz aus zugegriffen werden kann. So können Sie beispielsweise den Zugriff auf *github.com* aus Ihrem Subnetz zulassen.
- **Netzwerkregeln**

   Ermöglichen das Konfigurieren von Regeln mit Quelladresse, Protokoll, Zielport und Zieladresse. So können Sie beispielsweise eine Regel erstellen, um Datenverkehr über Port 53 (DNS) für die IP-Adresse Ihres DNS-Servers aus Ihrem Subnetz zuzulassen.

Die konfigurierten Firewallregeln werden auf den Netzwerkdatenverkehr angewendet, wenn Sie Ihren Netzwerkdatenverkehr an die Firewall als Subnetz-Standardgateway weiterleiten.

Anwendungs- und Netzwerkregeln werden in *Regelsammlungen* gespeichert. Eine Regelsammlung ist eine Liste von Regeln mit gleicher Aktion und Priorität.  Eine Netzwerkregelsammlung ist eine Liste von Netzwerkregeln; eine Anwendungsregelsammlung ist eine Liste von Anwendungsregeln.

Netzwerkregelsammlungen werden immer vor Anwendungsregelsammlungen verarbeitet. Alle Regeln sind beendigend. Wird also in einer Netzwerkregelsammlung eine Übereinstimmung gefunden, werden die nachfolgenden Anwendungsregelsammlungen für die Sitzung nicht mehr verarbeitet.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten einer Netzwerkumgebung zu Testzwecken
> * Bereitstellen einer Firewall
> * Erstellen einer Standardroute
> * Konfigurieren von Anwendungsregeln
> * Konfigurieren von Netzwerkregeln
> * Testen der Firewall



Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

In den Beispielen der Azure Firewall-Artikel wird davon ausgegangen, dass Sie bereits die Public Preview von Azure Firewall aktiviert haben. Weitere Informationen finden Sie unter [Aktivieren der Public Preview von Azure Firewall](public-preview.md).

In diesem Tutorial erstellen Sie ein einzelnes VNet mit drei Subnetzen:
- **FW-SN:** Das Subnetz mit der Firewall.
- **Workload-SN:** Das Subnetz mit dem Workloadserver. Der Netzwerkdatenverkehr dieses Subnetzes durchläuft die Firewall.
- **Jump-SN:** Das Subnetz mit dem Sprungserver. Der Sprungserver besitzt eine öffentliche IP-Adresse, mit der Sie eine Remotedesktopverbindung herstellen können. Von dort aus können Sie dann über einen weiteren Remotedesktop eine Verbindung mit dem Workloadserver herstellen.

![Netzwerkinfrastruktur des Tutorials](media/tutorial-firewall-rules-portal/Tutorial_network.png)

In diesem Tutorial wird eine vereinfachte Netzwerkkonfiguration verwendet, um die Bereitstellung zu vereinfachen. Für Produktionsbereitstellungen empfiehlt sich die Verwendung eines [Hub- und Spoke-Modells](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke), bei dem sich die Firewall in einem eigenen VNet befindet und die Workloadserver sich in mittels Peering verknüpften VNets in der gleichen Region mit mindestens einem Subnetz befinden.



## <a name="set-up-the-network-environment"></a>Einrichten der Netzwerkumgebung
Erstellen Sie zunächst eine Ressourcengruppe für die Ressourcen, die zum Bereitstellen der Firewall benötigt werden. Erstellen Sie dann ein VNet, Subnetze und Testserver.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
1. Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) beim Azure-Portal an.
1. Klicken Sie auf der Startseite des Azure-Portals auf **Ressourcengruppen** und anschließend auf **Hinzufügen**.
2. Geben Sie unter **Ressourcengruppenname** die Zeichenfolge **Test-FW-RG** ein.
3. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
4. Wählen Sie unter **Ressourcengruppenstandort** einen Standort aus. Alle weiteren Ressourcen, die Sie erstellen, müssen sich am gleichen Standort befinden.
5. Klicken Sie auf **Create**.


### <a name="create-a-vnet"></a>Erstellen eines VNET
1. Klicken Sie auf der Startseite des Azure-Portals auf **Alle Dienste**.
2. Klicken Sie unter **Netzwerk** auf **Virtuelle Netzwerke**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie unter **Name** die Zeichenfolge **Test-FW-VN** ein.
5. Geben Sie unter **Adressraum** die Zeichenfolge **10.0.0.0/16** ein.
7. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
8. Klicken Sie unter **Ressourcengruppe** auf **Vorhandene verwenden**, und wählen Sie anschließend **Test-FW-RG** aus.
9. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
10. Geben Sie unter **Subnetz** als **Name** die Zeichenfolge **AzureFirewallSubnet** ein.

    Die Firewall befindet sich diesem Subnetz, und der Subnetzname **muss** „AzureFirewallSubnet“ lauten.
11. Geben Sie unter **Adressbereich** die Zeichenfolge **10.0.1.0/24** ein.
12. Lassen Sie die restlichen Standardeinstellungen unverändert, und klicken Sie auf **Erstellen**.

> [!NOTE]
> Die Mindestgröße des Subnetzes AzureFirewallSubnet beträgt /25.

### <a name="create-additional-subnets"></a>Erstellen zusätzlicher Subnetze

Erstellen Sie als Nächstes Subnetze für den Sprungserver sowie ein Subnetz für die Workloadserver.

1. Klicken Sie auf der Startseite des Azure-Portals auf **Ressourcengruppen** und anschließend auf **Test-FW-RG**.
2. Klicken Sie auf das virtuelle Netzwerk **Test-FW-VN**.
3. Klicken Sie auf **Subnetze** und anschließend auf **+Subnetz**.
4. Geben Sie unter **Name** die Zeichenfolge **Workload-SN** ein.
5. Geben Sie unter **Adressbereich** die Zeichenfolge **10.0.2.0/24** ein.
6. Klicken Sie auf **OK**.

Erstellen Sie ein weiteres Subnetz namens **Jump-SN** mit dem Adressbereich **10.0.3.0/24**.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie nun die virtuellen Sprung- und Workloadcomputer, und platzieren Sie sie in den entsprechenden Subnetzen.

1. Klicken Sie auf der Startseite des Azure-Portals auf **Alle Dienste**.
2. Klicken Sie unter **Compute** auf **Virtuelle Computer**.
3. Klicken Sie auf **Hinzufügen** > **Windows Server** > **Windows Server 2016 Datacenter** > **Erstellen**.

**Grundlagen**

1. Geben Sie unter **Name** die Zeichenfolge **Srv-Jump** ein.
5. Geben Sie einen Benutzernamen und ein Kennwort ein.
6. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
7. Klicken Sie unter **Ressourcengruppe** auf **Vorhandene verwenden**, und wählen Sie **Test-FW-RG** aus.
8. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
9. Klicken Sie auf **OK**.

**Größe**

1. Wählen Sie eine angemessene Größe für einen virtuellen Testcomputer unter Windows Server aus. Verwenden Sie beispielsweise **B2ms** (8 GB RAM, 16 GB Speicher).
2. Klicken Sie auf **Auswählen**.

**Einstellungen**

1. Wählen Sie unter **Netzwerk** für **Virtuelles Netzwerk** die Option **Test-FW-VN** aus.
2. Wählen Sie unter **Subnetz** die Option **Jump-SN** aus.
3. Wählen Sie unter **Öffentliche Eingangsports hinzufügen** die Option **RDP (3389)** aus. 

    Es empfiehlt sich zwar, den Zugriff auf Ihre öffentliche IP-Adresse zu beschränken, der Port 3389 muss jedoch geöffnet werden, um eine Remotedesktopverbindung mit dem Sprungserver herstellen zu können. 
2. Lassen Sie die restlichen Standardeinstellungen unverändert, und klicken Sie auf **OK**.

**Zusammenfassung**

Überprüfen Sie die Zusammenfassung, und klicken Sie anschließend auf **Erstellen**. Dies nimmt einige Minuten in Anspruch.

Wiederholen Sie diesen Prozess, um einen weiteren virtuellen Computer namens **Srv-Work** zu erstellen.

Konfigurieren Sie die **Einstellungen** für den virtuellen Computer „Srv-Work“ mit den Angaben aus der folgenden Tabelle. Die restliche Konfiguration ist mit der Konfiguration des virtuellen Computers „Srv-Jump“ identisch.


|Einstellung  |Wert  |
|---------|---------|
|Subnetz|Workload-SN|
|Öffentliche IP-Adresse|Keine|
|Öffentliche Eingangsports hinzufügen|Keine öffentlichen Eingangsports|


## <a name="deploy-the-firewall"></a>Bereitstellen der Firewall

1. Klicken Sie auf der Startseite des Portals auf **Ressource erstellen**.
2. Klicken Sie auf **Netzwerk** und nach **Empfohlen** auf **Alle anzeigen**.
3. Klicken Sie auf **Firewall** und anschließend auf **Erstellen**. 
4. Konfigurieren Sie die Firewall auf der Seite **Firewall erstellen** anhand der folgenden Tabelle:
   
   |Einstellung  |Wert  |
   |---------|---------|
   |NAME     |Test-FW01|
   |Abonnement     |\<Ihr Abonnement\>|
   |Ressourcengruppe     |**Vorhandene verwenden**: Test-FW-RG |
   |Standort     |Wählen Sie den gleichen Standort aus wie zuvor.|
   |Virtuelles Netzwerk auswählen     |**Vorhandenes verwenden**: Test-FW-VN|
   |Öffentliche IP-Adresse     |**Neu erstellen**. Die öffentliche IP-Adresse muss vom Standard-SKU-Typ sein.|

2. Klicken Sie auf **Überprüfen + erstellen**.
3. Überprüfen Sie die Zusammenfassung, und klicken Sie dann auf **Erstellen**, um die Firewall zu erstellen.

   Die Bereitstellung dauert einige Minuten.
4. Navigieren Sie nach Abschluss der Bereitstellung zur Ressourcengruppe **Test-FW-RG**, und klicken Sie auf die Firewall **Test-FW01**.
6. Notieren Sie sich die private IP-Adresse. Diese wird später für die Erstellung der Standardroute benötigt.


## <a name="create-a-default-route"></a>Erstellen einer Standardroute

Konfigurieren Sie die ausgehende Standardroute für das Subnetz **Workload-SN** so, dass sie die Firewall durchläuft.

1. Klicken Sie auf der Startseite des Azure-Portals auf **Alle Dienste**.
2. Klicken Sie unter **Netzwerk** auf **Routingtabellen**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie unter **Name** die Zeichenfolge **Firewall-route** ein.
5. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
6. Klicken Sie unter **Ressourcengruppe** auf **Vorhandene verwenden**, und wählen Sie **Test-FW-RG** aus.
7. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
8. Klicken Sie auf **Create**.
9. Klicken Sie auf **Aktualisieren** und anschließend auf die Routingtabelle **Firewall-route**.
10. Klicken Sie auf **Subnetze** und anschließend auf **Zuordnen**.
11. Klicken Sie auf **Virtuelles Netzwerk**, und wählen Sie **Test-FW-VN** aus.
12. Klicken Sie unter **Subnetz** auf **Workload-SN**.
13. Klicken Sie auf **OK**.
14. Klicken Sie auf **Routen** und dann auf **Hinzufügen**.
15. Geben Sie unter **Routenname** die Zeichenfolge **FW-DG** ein.
16. Geben Sie unter **Adresspräfix** die Zeichenfolge **0.0.0.0/0** ein.
17. Wählen Sie unter **Typ des nächsten Hops** die Option **Virtuelles Gerät** aus.

    Azure Firewall ist eigentlich ein verwalteter Dienst, in dieser Situation kann aber „Virtuelles Gerät“ verwendet werden.
1. Geben Sie unter **Adresse des nächsten Hops** die private IP-Adresse für die Firewall ein, die Sie sich zuvor notiert haben.
2. Klicken Sie auf **OK**.


## <a name="configure-application-rules"></a>Konfigurieren von Anwendungsregeln


1. Öffnen Sie **Test-FW-RG**, und klicken Sie auf die Firewall **Test-FW01**.
1. Klicken Sie auf der Seite **Test-FW01** unter **Einstellungen** auf **Regeln**.
2. Klicken Sie auf **Anwendungsregelsammlung hinzufügen**.
3. Geben Sie unter **Name** die Zeichenfolge **App-Coll01** ein.
1. Geben Sie für **Priorität** den Wert **200** ein.
2. Wählen Sie für **Aktion** die Option **Zulassen** aus.

6. Geben Sie unter **Regeln** für **Name** die Zeichenfolge **AllowGH** ein.
7. Geben Sie unter **Quelladressen** die Zeichenfolge **10.0.2.0/24** ein.
8. Geben Sie unter **Protokoll:Port** die Zeichenfolge **http, https** ein. 
9. Geben Sie unter **Ziel-FQDNs** die Zeichenfolge **github.com** ein.
10. Klicken Sie auf **Hinzufügen**.

> [!NOTE]
> Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind. Diese FQDNs sind plattformspezifisch und können nicht für andere Zwecke verwendet werden. Zu den zulässigen Infrastruktur-FQDNs zählen folgende:
>- Computezugriff auf das Speicher-PIR (Platform Image Repository, PIR)
>- Speicherzugriff auf den Status verwalteter Datenträger
>- Windows-Diagnose
>
> Diese integrierte Infrastrukturregelsammlung kann außer Kraft gesetzt werden. Erstellen Sie hierzu eine Anwendungsregelsammlung vom Typ *Alle ablehnen*, die ganz zum Schluss verarbeitet wird. Sie wird immer vor der Infrastrukturregelsammlung verarbeitet. Alles, was sich nicht in der Infrastrukturregelsammlung befindet, wird standardmäßig abgelehnt.

## <a name="configure-network-rules"></a>Konfigurieren von Netzwerkregeln

1. Klicken Sie auf **Netzwerkregelsammlung hinzufügen**.
2. Geben Sie unter **Name** die Zeichenfolge **Net-Coll01** ein.
3. Geben Sie für **Priorität** den Wert **200** ein.
4. Wählen Sie für **Aktion** die Option **Zulassen** aus.

6. Geben Sie unter **Regeln** für **Name** die Zeichenfolge **AllowDNS** ein.
8. Wählen Sie für **Protokoll** die Option **UDP** aus.
9. Geben Sie unter **Quelladressen** die Zeichenfolge **10.0.2.0/24** ein.
10. Geben Sie für die Zieladresse die Zeichenfolge **209.244.0.3,209.244.0.4** ein.
11. Geben Sie unter **Zielports** den Wert **53** ein.
12. Klicken Sie auf **Hinzufügen**.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>Ändern der primären und sekundären DNS-Adresse für die Netzwerkschnittstelle **Srv-Work**

In diesem Tutorial konfigurieren Sie zu Testzwecken die primäre und sekundäre DNS-Serveradresse. Hierbei handelt es sich nicht um eine generelle Azure Firewall-Anforderung. 

1. Öffnen Sie über das Azure-Portal die Ressourcengruppe **Test-FW-RG**.
2. Klicken Sie auf die Netzwerkschnittstelle für den virtuellen Computer **Srv-Work**.
3. Klicken Sie unter **Einstellungen** auf **DNS-Server**.
4. Klicken Sie unter **DNS-Server** auf **Benutzerdefiniert**.
5. Geben Sie **209.244.0.3** in das Textfeld **DNS-Server hinzufügen** und **209.244.0.4** in das nächste Textfeld ein.
6. Klicken Sie auf **Speichern**. 
7. Starten Sie den virtuellen Computer **Srv-Work** neu.


## <a name="test-the-firewall"></a>Testen der Firewall

1. Überprüfen Sie im Azure-Portal die Netzwerkeinstellungen für den virtuellen Computer **Srv-Work**, und notieren Sie sich die private IP-Adresse.
2. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer **Srv-Jump** her, und öffnen Sie dort eine Remotedesktopverbindung mit der privaten IP-Adresse von **Srv-Work**.

5. Navigieren Sie in Internet Explorer zu http://github.com.
6. Klicken Sie auf **OK** und anschließend in den Sicherheitswarnungen auf **Schließen**.

   Daraufhin sollte die GitHub-Startseite angezeigt werden.

7. Navigieren Sie zu http://www.msn.com.

   Sie sollten durch die Firewall blockiert werden.

Damit haben Sie sich vergewissert, dass die Firewallregeln funktionieren:

- Sie können zum einzigen zulässigen FQDN navigieren, aber nicht zu anderen.
- Sie können DNS-Namen mithilfe des konfigurierten externen DNS-Servers auflösen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Firewallressourcen für das nächste Tutorial behalten oder die Ressourcengruppe **Test-FW-RG** löschen, wenn Sie sie nicht mehr benötigen. Dadurch werden alle firewallbezogenen Ressourcen gelöscht.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Einrichten des Netzwerks
> * Erstellen einer Firewall
> * Erstellen einer Standardroute
> * Konfigurieren von Anwendungs- und Netzwerkfirewallregeln
> * Testen der Firewall

Als Nächstes können Sie die Azure Firewall-Protokolle überwachen.

> [!div class="nextstepaction"]
> [Tutorial: Überwachen von Azure Firewall-Protokollen](./tutorial-diagnostics.md)
