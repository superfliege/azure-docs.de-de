---
title: 'Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal'
description: In diesem Tutorial erfahren Sie, wie Sie Azure Firewall über das Azure-Portal bereitstellen und konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 11/15/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 1d7c880a56c79d516c3904c3f532eb7006f0b68c
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705836"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Tutorial: Bereitstellen und Konfigurieren von Azure Firewall über das Azure-Portal

Die Steuerung des ausgehenden Netzwerkzugriffs ist ein wichtiger Teil eines umfassenden Netzwerksicherheitsplans. So kann es beispielsweise empfehlenswert sein, den Zugriff auf Websites oder auf ausgehende IP-Adressen und Ports einzuschränken.

Eine Möglichkeit zur Steuerung des ausgehenden Netzwerkzugriffs aus einem Subnetz ist Azure Firewall. Mit Azure Firewall können Sie Folgendes konfigurieren:

* Anwendungsregeln, die vollqualifizierte Domänennamen (Fully Qualified Domain Names, FQDNs) definieren, auf die von einem Subnetz aus zugegriffen werden kann.
* Netzwerkregeln, die die Quelladresse, das Protokoll, den Zielport und die Zieladresse definieren.

Die konfigurierten Firewallregeln werden auf den Netzwerkdatenverkehr angewendet, wenn Sie Ihren Netzwerkdatenverkehr an die Firewall als Subnetz-Standardgateway weiterleiten.

In diesem Tutorial erstellen Sie der Einfachheit halber ein einzelnes vereinfachtes VNET mit drei Subnetzen. Für Produktionsbereitstellungen empfiehlt sich die Verwendung eines [Hub- und Spoke-Modells](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke), bei dem sich die Firewall in einem eigenen VNet befindet und die Workloadserver sich in mittels Peering verknüpften VNets in der gleichen Region mit mindestens einem Subnetz befinden.

- **AzureFirewallSubnet:** Das Subnetz mit der Firewall.
- **Workload-SN:** Das Subnetz mit dem Workloadserver. Der Netzwerkdatenverkehr dieses Subnetzes durchläuft die Firewall.
- **Jump-SN:** Das Subnetz mit dem Sprungserver. Der Sprungserver besitzt eine öffentliche IP-Adresse, mit der Sie eine Remotedesktopverbindung herstellen können. Von dort aus können Sie dann über einen weiteren Remotedesktop eine Verbindung mit dem Workloadserver herstellen.

![Netzwerkinfrastruktur des Tutorials](media/tutorial-firewall-rules-portal/Tutorial_network.png)

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten einer Netzwerkumgebung zu Testzwecken
> * Bereitstellen einer Firewall
> * Erstellen einer Standardroute
> * Konfigurieren einer Anwendung, um den Zugriff auf „github.com“ zuzulassen
> * Konfigurieren einer Netzwerkregel, um den Zugriff auf externe DNS-Server zuzulassen
> * Testen der Firewall

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="set-up-the-network"></a>Einrichten des Netzwerks

Erstellen Sie zunächst eine Ressourcengruppe für die Ressourcen, die zum Bereitstellen der Firewall benötigt werden. Erstellen Sie dann ein VNet, Subnetze und Testserver.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Die Ressourcengruppe enthält alle Ressourcen für das Tutorial.

1. Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) beim Azure-Portal an.
2. Klicken Sie auf der Startseite des Azure-Portals auf **Ressourcengruppen** > **Hinzufügen**.
3. Geben Sie unter **Ressourcengruppenname** die Zeichenfolge **Test-FW-RG** ein.
4. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
5. Wählen Sie unter **Ressourcengruppenstandort** einen Standort aus. Alle weiteren Ressourcen, die Sie erstellen, müssen sich am gleichen Standort befinden.
6. Klicken Sie auf **Create**.

### <a name="create-a-vnet"></a>Erstellen eines VNET

Dieses VNET soll drei Subnetze enthalten.

1. Klicken Sie auf der Startseite des Azure-Portals auf **Alle Dienste**.
2. Klicken Sie unter **Netzwerk** auf **Virtuelle Netzwerke**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie unter **Name** die Zeichenfolge **Test-FW-VN** ein.
5. Geben Sie unter **Adressraum** die Zeichenfolge **10.0.0.0/16** ein.
6. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
7. Wählen Sie unter **Ressourcengruppe** die Option **Vorhandene verwenden** > **Test-FW-RG** aus.
8. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
9. Geben Sie unter **Subnetz** als **Name** die Zeichenfolge **AzureFirewallSubnet** ein. Die Firewall befindet sich diesem Subnetz, und der Subnetzname **muss** „AzureFirewallSubnet“ lauten.
10. Geben Sie unter **Adressbereich** die Zeichenfolge **10.0.1.0/24** ein.
11. Lassen Sie die restlichen Standardeinstellungen unverändert, und klicken Sie auf **Erstellen**.

> [!NOTE]
> Die Mindestgröße des Subnetzes AzureFirewallSubnet beträgt /25.

### <a name="create-additional-subnets"></a>Erstellen zusätzlicher Subnetze

Erstellen Sie als Nächstes Subnetze für den Sprungserver sowie ein Subnetz für die Workloadserver.

1. Klicken Sie auf der Startseite des Azure-Portals auf **Ressourcengruppen** > **Test-FW-RG**.
2. Klicken Sie auf das virtuelle Netzwerk **Test-FW-VN**.
3. Klicken Sie auf **Subnetze** > **+Subnetz**.
4. Geben Sie unter **Name** die Zeichenfolge **Workload-SN** ein.
5. Geben Sie unter **Adressbereich** die Zeichenfolge **10.0.2.0/24** ein.
6. Klicken Sie auf **OK**.

Erstellen Sie ein weiteres Subnetz namens **Jump-SN** mit dem Adressbereich **10.0.3.0/24**.

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Erstellen Sie nun die virtuellen Sprung- und Workloadcomputer, und platzieren Sie sie in den entsprechenden Subnetzen.

1. Klicken Sie im Azure-Portal auf **Ressource erstellen**.
2. Klicken Sie auf **Compute**, und wählen Sie dann in der Liste der ausgewählten Elemente die Option **Windows Server 2016 Datacenter**.
3. Geben Sie die folgenden Werte für den virtuellen Computer ein:

    - *Test-FW-RG*: Ressourcengruppe
    - *Srv-Jump*: Name des virtuellen Computers
    - *azureuser*: Name des Administratorbenutzers
    - *Azure123456!* als Kennwort

4. Klicken Sie unter **Regeln für eingehende Ports** für **Öffentliche Eingangsports** auf **Ausgewählte Ports zulassen**.
5. Wählen Sie unter **Eingangsports auswählen** die Option **RDP (3389)** aus.

6. Übernehmen Sie die anderen Standardwerte, und klicken Sie auf **Weiter: Datenträger**.
7. Übernehmen Sie die Datenträger-Standardwerte, und klicken Sie auf **Weiter: Netzwerk**.
8. Stellen Sie sicher, dass als virtuelles Netzwerk **Test-FW-VN** und als Subnetz **Jump-SN** ausgewählt ist.
9. Klicken Sie unter **Öffentliche IP** auf **Neu erstellen**.
10. Geben Sie als Namen für die öffentliche IP-Adresse **Srv-Jump-PIP** ein, und klicken Sie auf **OK**.
11. Übernehmen Sie die anderen Standardwerte, und klicken Sie auf **Weiter: Verwaltung**.
12. Klicken Sie auf **Aus**, um die Startdiagnose zu deaktivieren. Übernehmen Sie die anderen Standardwerte, und klicken Sie auf **Überprüfen + erstellen**.
13. Überprüfen Sie die Einstellungen auf der Seite „Zusammenfassung“, und klicken Sie dann auf **Erstellen**.

Wiederholen Sie diesen Prozess, um einen weiteren virtuellen Computer namens **Srv-Work** zu erstellen.

Konfigurieren Sie den virtuellen Computer „Srv-Work“ mit den Angaben aus der folgenden Tabelle. Die restliche Konfiguration ist mit der Konfiguration des virtuellen Computers „Srv-Jump“ identisch.

|Einstellung  |Wert  |
|---------|---------|
|Subnetz|Workload-SN|
|Öffentliche IP-Adresse|Keine|
|Öffentliche Eingangsports|Keine|

## <a name="deploy-the-firewall"></a>Bereitstellen der Firewall

Stellen Sie die Firewall im VNET bereit.

1. Klicken Sie auf der Startseite des Portals auf **Ressource erstellen**.
2. Klicken Sie auf **Netzwerk** und nach **Empfohlen** auf **Alle anzeigen**.
3. Klicken Sie auf **Firewall** > **Erstellen**. 
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
10. Klicken Sie auf **Subnetze** > **Zuordnen**.
11. Klicken Sie auf **Virtuelles Netzwerk** > **Test-FW-VN**.
12. Klicken Sie unter **Subnetz** auf **Workload-SN**. Stellen Sie sicher, dass Sie nur das Subnetz **Workload-SN** für diese Route auswählen. Andernfalls funktioniert die Firewall nicht korrekt.

13. Klicken Sie auf **OK**.
14. Klicken Sie auf **Routen** > **Hinzufügen**.
15. Geben Sie unter **Routenname** die Zeichenfolge **FW-DG** ein.
16. Geben Sie unter **Adresspräfix** die Zeichenfolge **0.0.0.0/0** ein.
17. Wählen Sie unter **Typ des nächsten Hops** die Option **Virtuelles Gerät** aus.

    Azure Firewall ist eigentlich ein verwalteter Dienst, in dieser Situation kann aber „Virtuelles Gerät“ verwendet werden.
18. Geben Sie unter **Adresse des nächsten Hops** die private IP-Adresse für die Firewall ein, die Sie sich zuvor notiert haben.
19. Klicken Sie auf **OK**.

## <a name="configure-an-application-rule"></a>Konfigurieren einer Anwendungsregel

Hierbei handelt es sich um die Anwendungsregel, die ausgehenden Zugriff auf „github.com“ ermöglicht.

1. Öffnen Sie **Test-FW-RG**, und klicken Sie auf die Firewall **Test-FW01**.
2. Klicken Sie auf der Seite **Test-FW01** unter **Einstellungen** auf **Regeln**.
3. Klicken Sie auf die Registerkarte **Anwendungsregelsammlung**.
4. Klicken Sie auf **Anwendungsregelsammlung hinzufügen**.
5. Geben Sie unter **Name** die Zeichenfolge **App-Coll01** ein.
6. Geben Sie für **Priorität** den Wert **200** ein.
7. Wählen Sie für **Aktion** die Option **Zulassen** aus.
8. Geben Sie unter **Regeln** > **Ziel-FQDNs** für **Name** die Zeichenfolge **AllowGH** ein.
9. Geben Sie unter **Quelladressen** die Zeichenfolge **10.0.2.0/24** ein.
10. Geben Sie unter **Protokoll:Port** die Zeichenfolge **http, https** ein.
11. Geben Sie unter **Ziel-FQDNs** die Zeichenfolge **github.com** ein.
12. Klicken Sie auf **Hinzufügen**.

Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind. Diese FQDNs sind plattformspezifisch und können nicht für andere Zwecke verwendet werden. Weitere Informationen finden Sie unter [Infrastruktur-FQDNs](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Konfigurieren einer Netzwerkregel

Hierbei handelt es sich um die Netzwerkregel, die ausgehenden Zugriff auf zwei IP-Adressen am Port 53 (DNS) zulässt.

1. Klicken Sie auf die Registerkarte **Netzwerkregelsammlung**.
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

Testen Sie nun die Firewall, um sicherzustellen, dass sie wie erwartet funktioniert.

1. Überprüfen Sie im Azure-Portal die Netzwerkeinstellungen für den virtuellen Computer **Srv-Work**, und notieren Sie sich die private IP-Adresse.
2. Stellen Sie eine Remotedesktopverbindung mit dem virtuellen Computer **Srv-Jump** her, und öffnen Sie dort eine Remotedesktopverbindung mit der privaten IP-Adresse von **Srv-Work**.

5. Navigieren Sie in Internet Explorer zu http://github.com.
6. Klicken Sie in den Sicherheitswarnungen auf **OK** > **Schließen**.

   Daraufhin sollte die GitHub-Startseite angezeigt werden.

7. Navigieren Sie zu http://www.msn.com.

   Sie sollten durch die Firewall blockiert werden.

Damit haben Sie sich vergewissert, dass die Firewallregeln funktionieren:

- Sie können zum einzigen zulässigen FQDN navigieren, aber nicht zu anderen.
- Sie können DNS-Namen mithilfe des konfigurierten externen DNS-Servers auflösen.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Firewallressourcen für das nächste Tutorial behalten oder die Ressourcengruppe **Test-FW-RG** löschen, wenn Sie sie nicht mehr benötigen. Dadurch werden alle firewallbezogenen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Tutorial: Überwachen von Azure Firewall-Protokollen](./tutorial-diagnostics.md)
