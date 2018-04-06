---
title: Einschränken des Netzwerkzugriffs auf PaaS-Ressourcen – Azure-Portal | Microsoft Docs
description: Erfahren Sie, wie Sie den Netzwerkzugriff auf Azure-Ressourcen wie Azure Storage und die Azure SQL-Datenbank mit virtuellen Netzwerkdienstendpunkten mithilfe des Azure-Portals einschränken können.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 9a64a5c1f63dc05cba6fdfa310b694e34bdba7d1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-portal"></a>Einschränken des Netzwerkzugriffs auf PaaS-Ressourcen mit virtuellen Netzwerkdienstendpunkten mithilfe des Azure-Portals

Virtuelle Netzwerkdienstendpunkte ermöglichen es Ihnen, den Netzwerkzugriff auf einige Azure-Dienstressourcen auf ein Subnetz eines virtuellen Netzwerks einzuschränken. Sie können auch den Internetzugriff auf die Ressourcen entfernen. Dienstendpunkte ermöglichen eine direkte Verbindung zwischen Ihrem virtuellen Netzwerk und unterstützten Azure-Diensten, sodass Sie mithilfe des privaten Adressraums Ihres virtuellen Netzwerks auf die Azure-Dienste zugreifen können. Datenverkehr, der über Dienstendpunkte für Azure-Ressourcen bestimmt ist, verbleibt immer im Microsoft Azure-Backbonenetzwerk. In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines virtuellen Netzwerks mit einem Subnetz
> * Hinzufügen eines Subnetzes und Aktivieren eines Dienstendpunkts
> * Erstellen einer Azure-Ressource und Zulassen des Netzwerkzugriffs darauf ausschließlich aus einem Subnetz
> * Bereitstellen eines virtuellen Computers für jedes Subnetz
> * Bestätigen des Zugriffs auf eine Ressource aus einem Subnetz
> * Bestätigen, dass der Zugriff auf eine Ressource aus einem Subnetz und dem Internet verweigert wird

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich unter http://portal.azure.com beim Azure-Portal an.

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Netzwerk** und anschließend **Virtuelles Netzwerk** aus.
3. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, und wählen Sie dann **Erstellen** aus:

    |Einstellung|Wert|
    |----|----|
    |NAME| myVirtualNetwork |
    |Adressraum| 10.0.0.0/16|
    |Abonnement| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe | Klicken Sie auf **Neu erstellen**, und geben Sie *myResourceGroup* ein.|
    |Speicherort| Wählen Sie **USA, Osten** aus. |
    |Subnetzname| Public|
    |Subnetzadressbereich| 10.0.0.0/24|
    |Dienstendpunkte| Deaktiviert|

    ![Eingeben grundlegender Informationen zu Ihrem virtuellen Netzwerk](./media/tutorial-restrict-network-access-to-resources/create-virtual-network.png)


## <a name="enable-a-service-endpoint"></a>Aktivieren eines Dienstendpunkts

1. Geben Sie oben im Portal im Feld **Ressourcen, Dienste und Dokumente durchsuchen** *myVirtualNetwork* ein. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Hinzufügen eines Subnetzes zum virtuellen Netzwerk Klicken Sie unter **EINSTELLUNGEN** auf **Subnetze** und anschließend auf **+ Subnetz**, wie in der folgenden Abbildung gezeigt:

    ![Hinzufügen des Subnetzes](./media/tutorial-restrict-network-access-to-resources/add-subnet.png) 

3. Wählen Sie unter **Subnetz hinzufügen** die folgenden Informationen aus, oder geben Sie sie ein, und klicken Sie auf **OK**:

    |Einstellung|Wert|
    |----|----|
    |NAME| Private |
    |Adressbereich| 10.0.1.0/24|
    |Dienstendpunkte| Wählen Sie unter **Dienste** **Microsoft.Storage** aus.|

## <a name="restrict-network-access-to-and-from-a-subnet"></a>Einschränken des Netzwerkzugriffs auf ein Subnetz und aus diesem

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie die Option **Netzwerk** und dann **Netzwerksicherheitsgruppe** aus.
Geben Sie unter **Netzwerksicherheitsgruppe erstellen** die folgenden Informationen ein, oder wählen Sie sie aus, und wählen Sie dann **Erstellen** aus:

    |Einstellung|Wert|
    |----|----|
    |NAME| myNsgPrivate |
    |Abonnement| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe | Wählen Sie **Vorhandene verwenden** und dann *myResourceGroup* aus.|
    |Speicherort| Wählen Sie **USA, Osten** aus. |

4. Nachdem die Sicherheitsgruppe erstellt wurde geben Sie im Feld **Ressourcen, Dienste und Dokumente durchsuchen** oben im Portal *myNsgPrivate* ein. Wenn **myNsgPrivate** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
5. Wählen Sie unter **EINSTELLUNGEN** **Ausgangssicherheitsregeln** aus.
6. Wählen Sie **+ Hinzufügen**.
7. Erstellen Sie eine Regel, die den ausgehenden Zugriff auf die öffentlichen IP-Adressen erlaubt, die dem Azure Storage-Dienst zugewiesen sind: Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, und wählen Sie dann **OK** aus:

    |Einstellung|Wert|
    |----|----|
    |Quelle| Wählen Sie **VirtualNetwork** aus. |
    |Quellportbereiche| * |
    |Ziel | Wählen Sie **Diensttag** aus.|
    |Zieldiensttag | Wählen Sie **Storage** aus.|
    |Zielportbereiche| * |
    |Protokoll|Beliebig|
    |anzuzeigen.|ZULASSEN|
    |Priorität|100|
    |NAME|Allow-Storage-All|
8. Erstellen Sie eine Regel, die eine Standardsicherheitsregel außer Kraft setzt, die den ausgehenden Zugriff auf alle öffentlichen IP-Adressen zulässt. Führen Sie die Schritte 6 und 7 erneut aus, und verwenden Sie dabei die folgenden Werte:

    |Einstellung|Wert|
    |----|----|
    |Quelle| Wählen Sie **VirtualNetwork** aus. |
    |Quellportbereiche| * |
    |Ziel | Wählen Sie **Diensttag** aus.|
    |Zieldiensttag| Wählen Sie **Internet** aus.|
    |Zielportbereiche| * |
    |Protokoll|Beliebig|
    |anzuzeigen.|Verweigern|
    |Priorität|110|
    |NAME|Deny-Internet-All|

9. Wählen Sie unter **EINSTELLUNGEN** **Eingangssicherheitsregeln** aus.
10. Wählen Sie **+ Hinzufügen**.
11. Erstellen Sie eine Regel, die eingehenden RDP-Datenverkehr (Remote Desktop Protocol) in das Subnetz von überall aus erlaubt. Die Regel setzt eine Standardsicherheitsregel außer Kraft, die jeglichen eingehenden Verkehr aus dem Internet abweist. Remotedesktopverbindungen in das Subnetz sind zulässig, sodass die Konnektivität in einem späteren Schritt getestet werden kann. Führen Sie die Schritte 6 und 7 erneut aus, und verwenden Sie dabei die folgenden Werte:

    |Einstellung|Wert|
    |----|----|
    |Quelle| Beliebig |
    |Quellportbereiche| * |
    |Ziel | Wählen Sie **Diensttag** aus.|
    |Zieldiensttag| Wählen Sie **VirtualNetwork** aus.|
    |Zielportbereiche| 3389 |
    |Protokoll|Beliebig|
    |anzuzeigen.|ZULASSEN|
    |Priorität|120|
    |NAME|Allow-RDP-All|

12. Wählen Sie unter **EINSTELLUNGEN** die Option **Subnetze** aus.
13. Wählen Sie **+ Zuordnen** aus.
14. Wählen Sie unter **Subnetz zuordnen**, dann **Virtuelles Netzwerk** und dann unter **Virtuelles Netzwerk auswählen** **myVirtualNetwork** aus.
15. Wählen Sie unter **Subnetz auswählen** **Private** und dann **OK** aus.

## <a name="restrict-network-access-to-a-resource"></a>Einschränken des Netzwerkzugriffs auf eine Ressource

Die Schritte, die erforderlich sind, um den Netzwerkzugriff auf Ressourcen einzuschränken, die durch Azure-Dienste erstellt und für Dienstendpunkte aktiviert wurden, sind je nach Dienst unterschiedlich. Informationen zu den Schritten für einzelne Dienste finden Sie in der Dokumentation des jeweiligen Diensts. Der Rest dieses Artikels enthält als Beispiel Schritte zum Einschränken des Netzwerkzugriffs für ein Azure Storage-Konto.

### <a name="create-a-storage-account"></a>Speicherkonto erstellen

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie die Option **Speicher** und anschließend **Speicherkonto – Blob, Datei, Tabelle, Warteschlange** aus.
3. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, übernehmen Sie die verbleibenden Standardeinstellungen, und wählen Sie dann **Erstellen** aus:

    |Einstellung|Wert|
    |----|----|
    |NAME| Geben Sie einen Namen ein, der an allen Azure-Standorten eindeutig, zwischen 3 und 24 Zeichen lang ist und nur aus Ziffern und Kleinbuchstaben besteht.|
    |Kontoart|StorageV2 (allgemein, Version 2)|
    |Replikation| Lokal redundanter Speicher (LRS)|
    |Abonnement| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe | Wählen Sie **Vorhandene verwenden** und dann *myResourceGroup* aus.|
    |Speicherort| Wählen Sie **USA, Osten** aus. |

### <a name="create-a-file-share-in-the-storage-account"></a>Erstellen einer Dateifreigabe im Speicherkonto

1. Geben Sie nach dem Erstellen des Speicherkontos oben im Portal im Feld **Ressourcen, Dienste und Dokumente durchsuchen** den Namen des Speicherkontos ein. Wenn der Name Ihres Speicherkontos in den Suchergebnissen angezeigt wird, wählen Sie ihn aus.
2. Klicken Sie auf **Dateien**, wie in der folgenden Abbildung gezeigt:

    ![Speicherkonto](./media/tutorial-restrict-network-access-to-resources/storage-account.png) 
3. Wählen Sie unter **Dateidienst** **+ Dateifreigabe** aus.
4. Geben Sie unter **Name** *my-file-share* ein, und wählen Sie dann **OK** aus.
5. Schließen Sie das Feld **Dateidienst**.

### <a name="enable-network-access-from-a-subnet"></a>Aktivieren des Netzwerkzugriffs aus einem Subnetz

Standardmäßig akzeptieren Speicherkonten Netzwerkverbindungen von Clients in allen Netzwerken. Um den Zugriff nur aus einem bestimmten Netzwerk zuzulassen und Netzwerkzugriff aus allen anderen Netzwerken zu verweigern, führen Sie die folgenden Schritte aus:

1. Wählen Sie unter **EINSTELLUNGEN** für das Speicherkonto **Firewalls und virtuelle Netzwerke** aus.
2. Wählen Sie unter **Virtuelle Netzwerke** **Ausgewählte Netzwerke** aus.
3. Wählen Sie **Vorhandenes virtuelles Netzwerk hinzufügen** aus.
4. Wählen Sie unter **Netzwerke hinzufügen** die folgenden Werte und dann **Hinzufügen** aus:

    |Einstellung|Wert|
    |----|----|
    |Abonnement| Wählen Sie Ihr Abonnement aus.|
    |Virtuelle Netzwerke|Wählen Sie unter **Virtuelle Netzwerke** **myVirtualNetwork** aus.|
    |Subnetze| Wählen Sie unter **Subnetze** **Private** aus.|

    ![Firewalls und virtuelle Netzwerke](./media/tutorial-restrict-network-access-to-resources/storage-firewalls-and-virtual-networks.png) 

5. Wählen Sie **Speichern**aus.
6. Schließen Sie das Feld **Firewalls und virtuelle Netzwerke**.
7. Wählen Sie unter **EINSTELLUNGEN** für das Speicherkonto **Zugriffsschlüssel** aus, wie in der folgenden Abbildung dargestellt:

      ![Firewalls und virtuelle Netzwerke](./media/tutorial-restrict-network-access-to-resources/storage-access-key.png)

8. Notieren Sie den Wert des **Schlüssels**, da Sie ihn beim Zuordnen der Dateifreigabe zu einem Laufwerksbuchstaben in einer VM in einem späteren Schritt manuell eingeben müssen.

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

Zum Testen des Netzwerkzugriffs auf ein Speicherkonto stellen Sie einen virtuellen Computer für jedes Subnetz bereit.

### <a name="create-the-first-virtual-machine"></a>Erstellen des ersten virtuellen Computers

1. Klicken Sie im Azure-Portal links oben auf **+ Ressource erstellen**.
2. Wählen Sie **Compute** und dann **Windows Server 2016 Datacenter**.
3. Geben Sie die folgenden Informationen ein, oder wählen Sie sie aus, und wählen Sie dann **OK** aus:

    |Einstellung|Wert|
    |----|----|
    |NAME| myVmPublic|
    |Benutzername|Geben Sie den gewünschten Benutzernamen ein.|
    |Password| Geben Sie das gewünschte Kennwort ein. Das Kennwort muss mindestens zwölf Zeichen lang sein und die [definierten Anforderungen an die Komplexität](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm) erfüllen.|
    |Abonnement| Wählen Sie Ihr Abonnement aus.|
    |Ressourcengruppe| Wählen Sie **Vorhandene verwenden** und dann **myResourceGroup** aus.|
    |Speicherort| Wählen Sie **USA, Osten** aus.|

    ![Eingeben von grundlegenden Informationen zu einem virtuellen Computer](./media/tutorial-restrict-network-access-to-resources/virtual-machine-basics.png)
4. Wählen Sie eine Größe für den virtuellen Computer aus, und wählen Sie dann **Auswählen** aus.
5. Wählen Sie unter **Einstellungen** **Netzwerk** und dann **myVirtualNetwork** aus. Wählen Sie dann **Subnetz** und **Public** aus, wie in der folgenden Abbildung zu sehen:

    ![Auswählen eines virtuellen Netzwerks](./media/tutorial-restrict-network-access-to-resources/virtual-machine-settings.png)
6. Wählen Sie auf der Seite **Zusammenfassung** die Option **Erstellen** aus, um die Bereitstellung des virtuellen Computers zu starten. Die Bereitstellung der VM nimmt einige Minuten in Anspruch, Sie können aber während der Erstellung der VM bereits mit dem nächsten Schritt fortfahren.

### <a name="create-the-second-virtual-machine"></a>Erstellen des zweiten virtuellen Computers

Führen Sie die Schritte 1–6 erneut aus, benennen Sie aber in Schritt 3 den virtuellen Computer *myVmPrivate*, und wählen Sie in Schritt 5 das Subnetz **Private** aus.

Die Bereitstellung des virtuellen Computers dauert einige Minuten. Fahren Sie erst mit dem nächsten Schritt fort, nachdem die Erstellung abgeschlossen wurde und seine Einstellungen sich im Portal geöffnet haben.

## <a name="confirm-access-to-storage-account"></a>Bestätigen des Zugriffs auf das Speicherkonto

1. Sobald die Erstellung des virtuellen Computers *myVmPrivate* abgeschlossen ist, öffnet Azure seine Einstellungen. Stellen Sie eine Verbindung mit der VM her, indem Sie die Schaltfläche **Verbinden** auswählen, wie in der folgenden Abbildung gezeigt:

    ![Herstellen einer Verbindung mit einem virtuellen Computer](./media/tutorial-restrict-network-access-to-resources/connect-to-virtual-machine.png)

2. Nachdem Sie die Schaltfläche **Verbinden** ausgewählt haben, wird eine RDP-Datei (Remotedesktopprotokoll) erstellt und auf Ihren Computer heruntergeladen.  
3. Öffnen Sie die heruntergeladene RDP-Datei. Wenn Sie dazu aufgefordert werden, wählen Sie **Verbinden** aus. Geben Sie den Benutzernamen und das Kennwort ein, die Sie beim Erstellen des virtuellen Computers festgelegt haben. Unter Umständen müssen Sie auf **Weitere Optionen** und anschließend auf **Anderes Konto verwenden** klicken, um die Anmeldeinformationen anzugeben, die Sie beim Erstellen des virtuellen Computers eingegeben haben. 
4. Klicken Sie auf **OK**.
5. Während des Anmeldevorgangs wird unter Umständen eine Zertifikatwarnung angezeigt. Wenn eine Warnung angezeigt wird, klicken Sie auf **Ja** bzw. **Weiter**, um mit dem Herstellen der Verbindung fortzufahren.
6. Ordnen Sie auf dem virtuellen Computer *myVmPrivate* mithilfe von PowerShell die Azure-Dateifreigabe dem Laufwerk Z zu. Ersetzen Sie vor dem Ausführen der nachfolgenden Befehle die Werte `<storage-account-key>` und `<storage-account-name>` durch Werte, die Sie unter [Erstellen eines Speicherkontos](#create-a-storage-account) angegeben und abgerufen haben.

    ```powershell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
    ```
    
    PowerShell gibt eine Ausgabe ähnlich der folgenden Beispielausgabe zurück:

    ```powershell
    Name           Used (GB)     Free (GB) Provider      Root
    ----           ---------     --------- --------      ----
    Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
    ```

    Die Azure-Dateifreigabe wurde dem Laufwerk Z erfolgreich zugeordnet.

7. Bestätigen Sie an einer Eingabeaufforderung, dass der virtuelle Computer über keine ausgehende Verbindung mit einer beliebigen anderen öffentlichen IP-Adresse verfügt:

    ```
    ping bing.com
    ```
    
    Sie erhalten keine Antworten, da die dem Subnetz *Private* zugeordnete Netzwerksicherheitsgruppe keinen ausgehenden Zugriff auf andere öffentliche IP-Adressen als auf die dem Azure Storage-Dienst zugewiesenen Adressen erlaubt.

8. Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmPrivate*.

## <a name="confirm-access-is-denied-to-storage-account"></a>Bestätigen, dass der Zugriff auf das Speicherkonto verweigert wird

1. Geben Sie oben im Portal im Feld *Ressourcen, Dienste und Dokumente durchsuchen* **myVmPublic** ein.
2. Wenn **myVmPublic** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
3. Führen Sie in [Bestätigen des Zugriffs auf das Speicherkonto](#confirm-access-to-storage-account) für die VM *myVmPublic* die Schritte 1–6 aus.

    Der Zugriff wird verweigert, und die Fehlermeldung `New-PSDrive : Access is denied` wird angezeigt. Der Zugriff wird verweigert, da der virtuelle Computer *myVmPublic* im Subnetz *Public* bereitgestellt wird. Für das Subnetz *Public* ist kein Dienstendpunkt für Azure Storage aktiviert, und das Speicherkonto erlaubt nur den Netzwerkzugriff aus dem Subnetz *Private*, nicht aus dem Subnetz *Public*.

4. Schließen Sie die Remotedesktopsitzung für den virtuellen Computer *myVmPublic*.

5. Navigieren Sie auf Ihrem Computer zum Azure-[Portal](https://portal.azure.com).
6. Geben Sie den Namen des Speicherkontos ein, das Sie im Feld **Ressourcen, Dienste und Dokumente durchsuchen** erstellt haben. Wenn der Name Ihres Speicherkontos in den Suchergebnissen angezeigt wird, wählen Sie ihn aus.
7. Wählen Sie **Dateien** aus.
8. Sie erhalten den Fehler, der in der folgenden Abbildung dargestellt ist:

    ![Zugriffsfehler](./media/tutorial-restrict-network-access-to-resources/access-denied-error.png)

    Der Zugriff wird verweigert, da sich Ihr Computer nicht im Subnetz *Private* des Netzwerks *MyVirtualNetwork* befindet.

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Löschen Sie die Ressourcengruppe mit allen ihren Ressourcen, wenn Sie sie nicht mehr benötigen:

1. Geben Sie im oben im Portal im Feld *Suche* die Zeichenfolge **myResourceGroup** ein. Wenn **myResourceGroup** in den Suchergebnissen angezeigt wird, wählen Sie diese Angabe aus.
2. Wählen Sie die Option **Ressourcengruppe löschen**.
3. Geben Sie für **Geben Sie den Ressourcengruppennamen ein:** den Namen *myResourceGroup* ein, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie einen Dienstendpunkt für ein Subnetz eines virtuellen Netzwerks aktiviert. Sie haben erfahren, dass Dienstendpunkte für Ressourcen aktiviert werden können, die mit mehreren Azure-Diensten bereitgestellt werden. Sie haben ein Azure Storage-Konto erstellt und den Netzwerkzugriff auf das Speicherkonto ausschließlich auf Ressourcen im Subnetz eines virtuellen Netzwerks eingeschränkt. Bevor Sie Dienstendpunkte in virtuellen Produktionsnetzwerken erstellen, sollten Sie sich unbedingt gründlich mit [ Dienstendpunkten](virtual-network-service-endpoints-overview.md) vertraut machen.

Wenn Sie mehrere virtuelle Netzwerke in Ihrem Konto verwenden, können Sie zwei virtuelle Netzwerke miteinander verbinden, damit die Ressourcen in jedem virtuellen Netzwerk miteinander kommunizieren können. Im nächsten Tutorial erfahren Sie, wie virtuelle Netzwerke verbunden werden.

> [!div class="nextstepaction"]
> [Herstellen von Verbindungen zwischen virtuellen Netzwerken](./tutorial-connect-virtual-networks-portal.md)
