---
title: Filtern von eingehendem Datenverkehr per Azure Firewall-DNAT im Azure-Portal
description: In diesem Tutorial erfahren Sie, wie Sie Azure Firewall-DNAT über das Azure-Portal bereitstellen und konfigurieren.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 9/27/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 894389ec07fb8e371a269f895473fe82985de7c3
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47405970"
---
# <a name="tutorial-filter-inbound-traffic-with-azure-firewall-dnat-using-the-azure-portal"></a>Tutorial: Filtern von eingehendem Datenverkehr per Azure Firewall-DNAT im Azure-Portal

Sie können die Ziel-Netzwerkadressübersetzung (Destination Network Address Translation, DNAT) von Azure Firewall so konfigurieren, dass eingehender Datenverkehr für Ihre Subnetze übersetzt und gefiltert wird. Wenn Sie DNAT konfigurieren, ist die Aktion für die NAT-Regelsammlung auf **Ziel-Netzwerkadressübersetzung (DNAT)** festgelegt. Jede Regel in der NAT-Regelsammlung kann dann verwendet werden, um die öffentliche IP-Adresse und den Port Ihrer Firewall in eine private IP-Adresse und den zugehörigen Port zu übersetzen. Mit DNAT-Regeln wird implizit eine entsprechende Netzwerkregel hinzugefügt, um den übersetzten Datenverkehr zuzulassen. Sie können dieses Verhalten außer Kraft setzen, indem Sie explizit eine Netzwerkregelsammlung mit Ablehnungsregeln hinzufügen, die für den übersetzten Datenverkehr geeignet sind. Weitere Informationen zur Logik für die Azure Firewall-Regelverarbeitung finden Sie unter [Logik für die Azure Firewall-Regelverarbeitung](rule-processing.md).

> [!NOTE]
> DNAT funktioniert nicht für Port 80 und 22. Wir arbeiten daran, dieses Problem in der nahen Zukunft zu beheben. Verwenden Sie in der Zwischenzeit einen anderen Port als Zielport in NAT-Regeln. Port 80 oder 22 kann weiterhin als übersetzter Port verwendet werden. Beispielsweise können Sie das öffentliche Element „ip:81“ dem privaten Element „ip:80“ zuordnen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Einrichten einer Netzwerkumgebung zu Testzwecken
> * Bereitstellen einer Firewall
> * Erstellen einer Standardroute
> * Konfigurieren einer DNAT-Regel
> * Testen der Firewall

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

In diesem Tutorial erstellen Sie zwei mittels Peering verknüpfte VNETs:
- **VN-Hub**:In diesem VNET befindet sich die Firewall.
- **VN-Spoke**: In diesem VNET befindet sich der Workloadserver.

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
1. Melden Sie sich unter [http://portal.azure.com](http://portal.azure.com) beim Azure-Portal an.
1. Klicken Sie auf der Startseite des Azure-Portals auf **Ressourcengruppen** und anschließend auf **Hinzufügen**.
2. Geben Sie unter **Ressourcengruppenname** die Zeichenfolge **RG-DNAT-Test** ein.
3. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
4. Wählen Sie unter **Ressourcengruppenstandort** einen Standort aus. Alle weiteren Ressourcen, die Sie erstellen, müssen sich am gleichen Standort befinden.
5. Klicken Sie auf **Create**.

## <a name="set-up-the-network-environment"></a>Einrichten der Netzwerkumgebung
Erstellen Sie zuerst die VNETs, und führen Sie anschließend das Peering dafür durch.

### <a name="create-the-hub-vnet"></a>Erstellen des Hub-VNET
1. Klicken Sie auf der Startseite des Azure-Portals auf **Alle Dienste**.
2. Klicken Sie unter **Netzwerk** auf **Virtuelle Netzwerke**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie unter **Name** den Namen **VN-Hub** ein.
5. Geben Sie unter **Adressraum** die Zeichenfolge **10.0.0.0/16** ein.
7. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
8. Klicken Sie unter **Ressourcengruppe** auf **Vorhandene verwenden**, und wählen Sie anschließend **RG-DNAT-Test** aus.
9. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
10. Geben Sie unter **Subnetz** als **Name** die Zeichenfolge **AzureFirewallSubnet** ein.

     Die Firewall befindet sich diesem Subnetz, und der Subnetzname **muss** „AzureFirewallSubnet“ lauten.
     > [!NOTE]
     > Die Mindestgröße des Subnetzes AzureFirewallSubnet beträgt /25.
11. Geben Sie unter **Adressbereich** die Zeichenfolge **10.0.1.0/24** ein.
12. Lassen Sie die restlichen Standardeinstellungen unverändert, und klicken Sie auf **Erstellen**.

### <a name="create-a-spoke-vnet"></a>Erstellen eines Spoke-VNET

1. Klicken Sie auf der Startseite des Azure-Portals auf **Alle Dienste**.
2. Klicken Sie unter **Netzwerk** auf **Virtuelle Netzwerke**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie unter **Name** den Namen **VN-Spoke** ein.
5. Geben Sie unter **Adressraum** die Zeichenfolge **192.168.0.0/16** ein.
7. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
8. Klicken Sie unter **Ressourcengruppe** auf **Vorhandene verwenden**, und wählen Sie anschließend **RG-DNAT-Test** aus.
9. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
10. Geben Sie unter **Subnetz** für **Name** den Namen **SN-Workload** ein.

    Der Server befindet sich in diesem Subnetz.
1. Geben Sie unter **Adressbereich** die Zeichenfolge **192.168.1.0/24** ein.
2. Lassen Sie die restlichen Standardeinstellungen unverändert, und klicken Sie auf **Erstellen**.

### <a name="peer-the-vnets"></a>Verknüpfen der VNETs per Peering

Führen Sie nun das Peering für die beiden VNETs durch.

#### <a name="hub-to-spoke"></a>Hub zu Spoke

1. Klicken Sie auf das virtuelle Netzwerk **VN-Hub**.
2. Klicken Sie unter **Einstellungen** auf **Peerings**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie **Peer-HubSpoke** als Namen ein.
5. Wählen Sie **VN-Spoke** für das virtuelle Netzwerk aus.
7. Klicken Sie auf **OK**.

#### <a name="spoke-to-hub"></a>Spoke zu Hub

1. Klicken Sie auf das virtuelle Netzwerk **VN-Spoke**.
2. Klicken Sie unter **Einstellungen** auf **Peerings**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie **Peer-SpokeHub** als Namen ein.
5. Wählen Sie **VN-Hub** für das virtuelle Netzwerk aus.
6. Klicken Sie auf **Weitergeleiteten Datenverkehr zulassen**.
7. Klicken Sie auf **OK**.

## <a name="create-a-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie einen virtuellen Workloadcomputer, und ordnen Sie ihn im Subnetz **SN-Workload** an.

1. Klicken Sie auf der Startseite des Azure-Portals auf **Alle Dienste**.
2. Klicken Sie unter **Compute** auf **Virtuelle Computer**.
3. Klicken Sie auf **Hinzufügen** > **Windows Server** > **Windows Server 2016 Datacenter** > **Erstellen**.

**Grundlagen**

1. Geben Sie unter **Name** den Namen **Srv-Workload** ein.
5. Geben Sie einen Benutzernamen und ein Kennwort ein.
6. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
7. Klicken Sie unter **Ressourcengruppe** auf **Vorhandene verwenden**, und wählen Sie **RG-DNAT-Test** aus.
8. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
9. Klicken Sie auf **OK**.

**Größe**

1. Wählen Sie eine angemessene Größe für einen virtuellen Testcomputer unter Windows Server aus. Verwenden Sie beispielsweise **B2ms** (8 GB RAM, 16 GB Speicher).
2. Klicken Sie auf **Auswählen**.

**Einstellungen**

1. Wählen Sie unter **Netzwerk** für **Virtuelles Netzwerk** die Option **VN-Spoke**.
2. Wählen Sie unter **Subnetz** die Option **SN-Workload**.
3. Klicken Sie auf **Öffentliche IP-Adresse** und dann auf **Keine**.
4. Wählen Sie für **Öffentliche Eingangsports hinzufügen** die Option **Keine öffentlichen Eingangsports**. 
2. Lassen Sie die restlichen Standardeinstellungen unverändert, und klicken Sie auf **OK**.

**Zusammenfassung**

Überprüfen Sie die Zusammenfassung, und klicken Sie anschließend auf **Erstellen**. Dies nimmt einige Minuten in Anspruch.

Nachdem die Bereitstellung abgeschlossen ist, können Sie sich die private IP-Adresse für den virtuellen Computer notieren. Sie wird später beim Konfigurieren der Firewall benötigt. Klicken Sie auf den Namen des virtuellen Computers und dann unter **Einstellungen** auf **Netzwerk**, um nach der privaten IP-Adresse zu suchen.


## <a name="deploy-the-firewall"></a>Bereitstellen der Firewall

1. Klicken Sie auf der Startseite des Portals auf **Ressource erstellen**.
2. Klicken Sie auf **Netzwerk** und nach **Empfohlen** auf **Alle anzeigen**.
3. Klicken Sie auf **Firewall** und anschließend auf **Erstellen**. 
4. Konfigurieren Sie die Firewall auf der Seite **Firewall erstellen** anhand der folgenden Tabelle:
   
   |Einstellung  |Wert  |
   |---------|---------|
   |NAME     |FW-DNAT-test|
   |Abonnement     |\<Ihr Abonnement\>|
   |Ressourcengruppe     |**Vorhandene verwenden**: RG-DNAT-Test |
   |Standort     |Wählen Sie den gleichen Standort aus wie zuvor.|
   |Virtuelles Netzwerk auswählen     |**Vorhandene verwenden**: VN-Hub|
   |Öffentliche IP-Adresse     |**Neu erstellen**. Die öffentliche IP-Adresse muss vom Standard-SKU-Typ sein.|

2. Klicken Sie auf **Überprüfen + erstellen**.
3. Überprüfen Sie die Zusammenfassung, und klicken Sie dann auf **Erstellen**, um die Firewall zu erstellen.

   Die Bereitstellung dauert einige Minuten.
4. Nachdem die Bereitstellung abgeschlossen ist, können Sie zur Ressourcengruppe **RG-DNAT-Test** navigieren und auf die Firewall **FW-DNAT-test** klicken.
6. Notieren Sie sich die private IP-Adresse. Diese wird später für die Erstellung der Standardroute benötigt.


## <a name="create-a-default-route"></a>Erstellen einer Standardroute

Konfigurieren Sie die ausgehende Standardroute für das Subnetz **SN-Workload** so, dass sie die Firewall durchläuft.

1. Klicken Sie auf der Startseite des Azure-Portals auf **Alle Dienste**.
2. Klicken Sie unter **Netzwerk** auf **Routingtabellen**.
3. Klicken Sie auf **Hinzufügen**.
4. Geben Sie unter **Name** den Namen **RT-FWroute** ein.
5. Wählen Sie unter **Abonnement** Ihr Abonnement aus.
6. Wählen Sie unter **Ressourcengruppe** die Option **Vorhandene verwenden**, und wählen Sie **RG-DNAT-Test** aus.
7. Wählen Sie unter **Standort** den gleichen Standort aus wie zuvor.
8. Klicken Sie auf **Create**.
9. Klicken Sie auf **Aktualisieren** und anschließend auf die Routingtabelle **RT-FWroute**.
10. Klicken Sie auf **Subnetze** und anschließend auf **Zuordnen**.
11. Klicken Sie auf **Virtuelles Netzwerk**, und wählen Sie **VN-Spoke** aus.
12. Klicken Sie unter **Subnetz** auf **SN-Workload**.
13. Klicken Sie auf **OK**.
14. Klicken Sie auf **Routen** und dann auf **Hinzufügen**.
15. Geben Sie unter **Routenname** die Zeichenfolge **FW-DG** ein.
16. Geben Sie unter **Adresspräfix** die Zeichenfolge **0.0.0.0/0** ein.
17. Wählen Sie unter **Typ des nächsten Hops** die Option **Virtuelles Gerät** aus.

    Azure Firewall ist eigentlich ein verwalteter Dienst, in dieser Situation kann aber „Virtuelles Gerät“ verwendet werden.
1. Geben Sie unter **Adresse des nächsten Hops** die private IP-Adresse für die Firewall ein, die Sie sich zuvor notiert haben.
2. Klicken Sie auf **OK**.


## <a name="configure-a-dnat-rule"></a>Konfigurieren einer DNAT-Regel

1. Öffnen Sie **RG-DNAT-Test**, und klicken Sie auf die Firewall **FW-DNAT-test**. 
1. Klicken Sie auf der Seite **FW-DNAT-test** unter **Einstellungen** auf **Regeln**. 
2. Klicken Sie auf **Add DNAT rule collection** (DNAT-Regelsammlung hinzufügen). 
3. Geben Sie unter **Name** den Namen **RC-DNAT-01** ein. 
1. Geben Sie für **Priorität** den Wert **200** ein. 
6. Geben Sie unter **Regeln** für **Name** die Zeichenfolge **RL-01** ein. 
7. Geben Sie unter **Quelladressen** das Zeichen „*“ ein. 
8. Geben Sie unter **Zieladressen** die öffentliche IP-Adresse der Firewall ein. 
9. Geben Sie unter **Zielports** den Wert **3389** ein. 
10. Geben Sie für **Übersetzte Adresse** die private IP-Adresse für den virtuellen Computer „Srv-Workload“ ein. 
11. Geben Sie für **Übersetzter Port** den Wert **3389** ein. 
12. Klicken Sie auf **Hinzufügen**. 

## <a name="test-the-firewall"></a>Testen der Firewall

1. Verbinden Sie einen Remotedesktop mit der öffentlichen IP-Adresse der Firewall. Sie sollten mit dem virtuellen Computer **Srv-Workload** verbunden werden.
3. Schließen Sie den Remotedesktop.
4. Ändern Sie die Aktion für die Netzwerkregelsammlung **RC-Net-01** in **Verweigern**.
5. Versuchen Sie erneut, eine Verbindung mit der öffentlichen IP-Adresse der Firewall herzustellen. Dieser Vorgang sollte aufgrund der Regel **Verweigern** jetzt nicht erfolgreich sein.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Sie können die Firewallressourcen für das nächste Tutorial behalten oder die Ressourcengruppe **RG-DNAT-Test** löschen, wenn Sie sie nicht mehr benötigen. Dadurch werden alle firewallbezogenen Ressourcen gelöscht.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Einrichten einer Netzwerkumgebung zu Testzwecken
> * Bereitstellen einer Firewall
> * Erstellen einer Standardroute
> * Konfigurieren einer DNAT-Regel
> * Testen der Firewall

Als Nächstes können Sie die Azure Firewall-Protokolle überwachen.

> [!div class="nextstepaction"]
> [Tutorial: Überwachen von Azure Firewall-Protokollen](./tutorial-diagnostics.md)
