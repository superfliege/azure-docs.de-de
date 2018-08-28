---
title: Verwaltete Azure SQL-Datenbank-Instanz – VNET-Konfiguration | Microsoft-Dokumentation
description: In diesem Thema werden die Konfigurationsoptionen für ein virtuelles Netzwerk (VNET) mit einer verwalteten Azure SQL-Datenbank-Instanz beschrieben.
services: sql-database
author: srdan-bozovic-msft
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: f634167f24c221e702696174ea86a212c535695b
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246529"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz

In einem [virtuellen Azure-Netzwerk (VNET)](../virtual-network/virtual-networks-overview.md) muss eine verwaltete Azure SQL-Datenbank-Instanz (Vorschauversion) bereitgestellt werden. Diese Bereitstellung ermöglicht die folgenden Szenarien: 
- Direktes Herstellen einer Verbindung mit einer verwalteten Instanz von einem lokalen Netzwerk 
- Herstellen einer Verbindung zwischen einer verwalteten Instanz und einem Verbindungsserver oder einem anderen lokalen Datenspeicher 
- Herstellen einer Verbindung zwischen einer verwalteten Instanz und Azure-Ressourcen  

## <a name="plan"></a>Plan

Planen Sie die Bereitstellung einer verwalteten Instanz im virtuellen Netzwerk, indem Sie folgende Fragen beantworten: 
- Planen Sie, einzelne oder mehrere verwaltete Instanzen bereitzustellen? 

  Die Anzahl der verwalteten Instanzen bestimmt die Mindestgröße des Subnetzes, die Ihren verwalteten Instanzen zugeordnet wird. Weitere Informationen finden Sie unter [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances). 
- Müssen Sie Ihre verwaltete Instanz in einem bestehenden virtuellen Netzwerk bereitstellen, oder erstellen Sie ein neues Netzwerk? 

   Wenn Sie die Verwendung eines vorhandenen virtuellen Netzwerks beabsichtigen, müssen Sie diese Netzwerkkonfiguration ändern, um Ihre verwaltete Instanz einzubeziehen. Weitere Informationen finden Sie unter [Ändern eines vorhandenen virtuellen Netzwerks für die verwaltete Instanz](#modify-an-existing-virtual-network-for-managed-instances). 

   Wenn Sie vorhaben, ein neues virtuelles Netzwerk zu erstellen, lesen Sie [Erstellen eines neuen virtuellen Netzwerks für die verwaltete Instanz](#create-a-new-virtual-network-for-managed-instances).

## <a name="requirements"></a>Anforderungen

Für die Erstellung einer verwalteten Azure SQL-Datenbank-Instanz benötigen Sie ein dediziertes Subnetz innerhalb des virtuellen Netzwerks, das den folgenden Anforderungen entspricht:
- **Leer**: Das Subnetz darf keinem anderen Clouddienst zugeordnet sein und kein Gatewaysubnetz sein. Sie können keine verwalteten Instanzen im Subnetz erstellen, die andere Ressourcen als verwaltete Instanzen enthält, oder zu einem späteren Zeitpunkt Ressourcen innerhalb des Subnetzes hinzufügen.
- **Keine NSG**: Dem Subnetz darf keine Netzwerksicherheitsgruppe zugeordnet sein.
- **Spezielle Routingtabelle**: Das Subnetz muss eine benutzerdefinierte Routentabelle (User Route Table, UDR) mit dem nächsten Hop 0.0.0.0/0 als einzige ihr zugewiesene Route aufweisen. Weitere Informationen finden Sie unter [Erstellen und Zuweisen der erforderlichen Routingtabelle](#create-the-required-route-table-and-associate-it).
3. **Optionales benutzerdefiniertes DNS**: Wenn ein benutzerdefiniertes DNS für das VNET angegeben ist, muss die IP-Adresse des rekursiven Azure-Konfliktlösers (z.B. 168.63.129.16) zur Liste hinzugefügt werden. Weitere Informationen finden Sie unter [Konfigurieren des benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md).
4. **Kein Dienstendpunkt**: Dem Subnetz darf kein Dienstendpunkt (Speicher oder SQL) zugeordnet sein. Stellen Sie sicher, dass die Option „Dienstendpunkte“ bei der Erstellung des VNET auf „Deaktiviert“ festgelegt ist.
5. **Ausreichende IP-Adressen**: Das Subnetz muss mindestens 16 IP-Adressen aufweisen. Weitere Informationen finden Sie unter [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances).

> [!IMPORTANT]
> Wenn das Zielsubnetz nicht sämtliche der zuvor genannten Voraussetzungen erfüllt, können Sie keine neue verwaltete Instanz bereitstellen. Das Ziel-VNET und das Subnetz müssen diese Anforderungen an verwaltete Instanzen (vor und nach der Bereitstellung) erfüllen, da es anderenfalls dazu führen kann, dass die Instanz in einen fehlerhaften Zustand wechselt und nicht mehr verfügbar ist. Um diesen Zustand zu verlassen, müssen Sie eine neue Instanz in einem VNET mit den entsprechenden Netzwerkrichtlinien erstellen, Daten auf Instanzebene neu erstellen und Ihre Datenbanken wiederherstellen. Dies führt zu beträchtlichen Ausfallzeiten bei Ihren Anwendungen.

##  <a name="determine-the-size-of-subnet-for-managed-instances"></a>Ermitteln der Größe des Subnetzes für verwaltete Instanzen

Bei der Erstellung einer verwalteten Instanz ordnet Azure abhängig von der Größe des Tarifs, den Sie bei der Bereitstellung ausgewählt haben, eine Reihe von virtuellen Computern zu. Da diese virtuellen Computer Ihrem Subnetz zugeordnet sind, benötigen sie IP-Adressen. Um bei regulären Vorgängen und bei der Wartung von Diensten Hochverfügbarkeit sicherzustellen, kann Azure zusätzliche virtuelle Computer zuordnen. Daher ist die Anzahl der erforderlichen IP-Adressen in einem Subnetz größer als die Anzahl der verwalteten Instanzen in diesem Subnetz. 

Prinzipiell benötigt eine verwaltete Instanz zwischen 16 und 256 IP-Adressen in einem Subnetz. Daher können Sie bei der Definition Ihrer Subnetz-IP-Adressbereiche die Subnetzmasken /28 bis /24 verwenden. 

Wenn Sie mehrere verwaltete Instanzen innerhalb des Subnetzes bereitstellen möchten und die Subnetzgröße optimieren müssen, führen Sie mithilfe der folgenden Parameter eine Berechnung durch: 

- Azure verwendet fünf IP-Adressen im Subnetz für den eigenen Bedarf. 
- Jede allgemeine Instanz benötigt zwei Adressen. 
- Jede unternehmenskritisch Instanz benötigt vier Adressen.

**Beispiel**: Sie planen, mit drei universelle und zwei unternehmenskritischen verwalteten Instanzen zu arbeiten. Dies bedeutet, dass Sie 5 + 3 * 2 + 2 * 4 = 19 IP-Adressen benötigen. Da IP-Adressbereiche in Zweierpotenzen definiert sind, benötigen Sie den IP-Adressbereich von 32 (2^5) IP-Adressen. Aus diesem Grund müssen Sie das Subnetz mit der Subnetzmaske /27 reservieren. 

## <a name="create-a-new-virtual-network-for-managed-instance-using-azure-resource-manager-deployment"></a>Erstellen eines neuen virtuellen Netzwerks für die verwaltete Instanz unter Verwendung der Azure Resource Manager-Bereitstellung

Die einfachste Möglichkeit zum Erstellen und Konfigurieren eines virtuellen Netzwerks ist die Verwendung der Azure Resource Manager-Bereitstellungsvorlage.

1. Melden Sie sich beim Azure-Portal an.

2. Verwenden Sie die Schaltfläche **In Azure bereitstellen**, um das virtuelle Netzwerk in Azure-Cloud bereitzustellen:

  <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

  Diese Schaltfläche öffnet ein Formular zum Konfigurieren der Netzwerkumgebung, in der Sie die verwaltete Instanz bereitstellen können.

  > [!Note]
  > Diese Azure Resource Manager-Vorlage stellt das virtuelle Netzwerk mit zwei Subnetzen bereit. Ein Subnetz namens **ManagedInstances** ist für verwaltete Instanzen reserviert und verfügt über eine vorkonfigurierte Routingtabelle, während das andere Subnetz namens **Standard** für andere Ressourcen verwendet wird, die auf die verwaltete Instanz zugreifen sollen (z. B. Azure Virtual Machines). Sie können das Subnetz **Standard** entfernen, wenn Sie es nicht benötigen.

3. Konfigurieren Sie die Netzwerkumgebung. Im folgenden Formular können Sie Parameter für Ihre Netzwerkumgebung konfigurieren:

![Azure-Netzwerk konfigurieren](./media/sql-database-managed-instance-get-started/create-mi-network-arm.png)

Sie können die Namen des VNETs und der Subnetze ändern und die Ihren Netzwerkressourcen zugeordneten IP-Adressbereiche anpassen. Sobald Sie auf die Schaltfläche „Kaufen“ klicken, erstellt und konfiguriert dieses Formular Ihre Umgebung. Wenn Sie keine zwei Subnetze benötigen, können Sie das Subnetz „Standard“ löschen. 

## <a name="create-a-new-virtual-network-for-managed-instances-using-portal"></a>Erstellen eines neuen virtuellen Netzwerks für verwaltete Instanzen unter Verwendung des Portals

Für die Erstellung einer verwalteten Instanz muss zunächst ein virtuelles Netzwerk in Azure erstellt werden. Hierfür können Sie das Azure-Portal, [PowerShell](../virtual-network/quick-create-powershell.md) oder die [Azure CLI](../virtual-network/quick-create-cli.md) verwenden. Im folgenden Abschnitt werden die Schritte zur Verwendung des Azure-Portals erläutert. Die hier erläuterten Details gelten für jede dieser Methoden.

1. Klicken Sie im Azure-Portal links oben auf **Ressource erstellen**.
2. Klicken Sie auf die Option **Virtuelles Netzwerk**, vergewissern Sie sich, dass der **Resource Manager** als Bereitstellungsmodus ausgewählt ist, und klicken Sie dann auf **Erstellen**.

   ![Erstellen eines virtuellen Netzwerks](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Füllen Sie das Formular für das virtuelle Netzwerk mit den angeforderten Informationen aus, wie im folgenden Screenshot gezeigt wird:

   ![Formular für die Erstellung des virtuellen Netzwerks](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Klicken Sie auf **Create**.

   Der Adressraum und das Subnetz werden in CIDR-Notation angegeben. 

   > [!IMPORTANT]
   > Mit den Standardwerten wird ein Subnetz erstellt, das den gesamten VNET-Adressraum akzeptiert. Wenn Sie diese Option wählen, können Sie keine anderen Ressourcen im virtuellen Netzwerk als verwaltete Instanzen erstellen. 

   Die empfohlene Vorgehensweise würde folgendermaßen lauten: 
   - Berechnen der Subnetzgröße gemäß dem Abschnitt [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances)  
   - Bewerten der Anforderungen für das restliche VNET 
   - Entsprechendes Ausfüllen der VNET- und Subnetzadressbereiche 

   Stellen Sie sicher, dass die Option „Dienstendpunkte“ auf **Deaktiviert** festgelegt ist. 

   ![Formular für die Erstellung des virtuellen Netzwerks](./media/sql-database-managed-instance-tutorial/service-endpoint-disabled.png)

### <a name="create-the-required-route-table-and-associate-it"></a>Erstellen und Zuweisen der erforderlichen Routentabelle

1. Melden Sie sich auf dem Azure-Portal an.  
2. Klicken Sie auf **Routingtabelle** und dann auf der Seite „Routingtabelle“ auf **Erstellen**.

   ![Formular für die Erstellung einer Routingtabelle](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

3. Erstellen Sie eine Route für den nächsten Hop 0.0.0.0/0 wie in den folgenden Screenshots gezeigt:

   ![Hinzufügen einer Routingtabelle](./media/sql-database-managed-instance-tutorial/route-table-add.png)

   ![Route](./media/sql-database-managed-instance-tutorial/route.png)

4. Ordnen Sie diese Route dem Subnetz für die verwaltete Instanz zu, wie in den folgenden Screenshots gezeigt wird:

    ![Subnetz](./media/sql-database-managed-instance-tutorial/subnet.png)

    ![Festlegen der Routingtabelle](./media/sql-database-managed-instance-tutorial/set-route-table.png)

    ![Festlegen der Routingtabelle – Speichern](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)


Sobald Ihr VNET erstellt wurde, können Sie die verwaltete Instanz erstellen.  

## <a name="modify-an-existing-virtual-network-for-managed-instances"></a>Ändern eines bestehenden virtuellen Netzwerks für verwaltete Instanzen 

In den Fragen und Antworten in diesem Abschnitt wird erläutert, wie eine verwaltete Instanz zu einem vorhandenen virtuellen Netzwerk hinzugefügt wird. 

**Verwenden Sie das klassische oder das Resource Manager-Bereitstellungsmodell für das bestehende virtuelle Netzwerk?** 

Sie können nur eine verwaltete Instanz in virtuellen Resource Manager-Netzwerken erstellen. 

**Möchten Sie ein neues Subnetz für verwaltete SQL-Instanzen erstellen oder ein vorhandenes verwenden?**

Wenn Sie ein neues Subnetz erstellen möchten, gehen Sie wie folgt vor: 

- Berechnen Sie die Subnetzgröße entsprechend der Richtlinien im Abschnitt [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances).
- Führen Sie die Schritte unter [Hinzufügen, Ändern oder Löschen von Subnetzen virtueller Netzwerke](../virtual-network/virtual-network-manage-subnet.md) durch. 
- Erstellen Sie eine Routingtabelle, die einen einzelnen Eintrag mit dem nächsten Hop **0.0.0.0/0** enthält, und verknüpfen Sie sie mit dem Subnetz für die verwaltete Instanz.  

Falls Sie eine verwaltete Instanz in einem vorhandenen Subnetz erstellen möchten, gehen Sie wie folgt vor: 
- Überprüfen Sie, ob das Subnetz leer ist. Es darf keine verwaltete Instanz in einem Subnetz erstellt werden, die andere Ressourcen wie etwa das Gatewaysubnetz enthält. 
- Berechnen Sie die Subnetzgröße entsprechend den Richtlinien im Abschnitt [Ermitteln der Größe des Subnetzes für verwaltete Instanzen](#determine-the-size-of-subnet-for-managed-instances), und vergewissern Sie sich, ob die Größe ordnungsgemäß angepasst wurde. 
- Vergewissern Sie sich, dass die Dienstendpunkte im Subnetz nicht aktiviert sind.
- Stellen Sie sicher, dass dem Subnetz keine Netzwerksicherheitsgruppen zugeordnet sind. 

**Haben Sie einen benutzerdefinierten DNS-Server konfiguriert?** 

Falls ja, lesen Sie den Artikel [Konfigurieren eines benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md). 

- Erstellen Sie die erforderliche Routingtabelle, und weisen Sie sie zu. Informationen hierzu finden Sie unter [Erstellen und Zuweisen der erforderlichen Routingtabelle](#create-the-required-route-table-and-associate-it).

## <a name="next-steps"></a>Nächste Schritte

- Eine Übersicht finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Ein Tutorial zum Erstellen eines VNET, Erstellen einer verwalteten Instanz und Wiederherstellen einer Datenbank anhand einer Datenbanksicherung finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-create-tutorial-portal.md).
- Bei Problemen im Zusammenhang mit dem DNS lesen Sie [Konfigurieren eines benutzerdefinierten DNS](sql-database-managed-instance-custom-dns.md).
