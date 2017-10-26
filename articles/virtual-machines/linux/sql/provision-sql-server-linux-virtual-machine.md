---
title: Erstellen eines virtuellen Linux-Computers mit SQL Server 2017 in Azure | Microsoft-Dokumentation
description: "In diesem Tutorial erfahren Sie, wie Sie über das Azure-Portal einen virtuellen Linux-Computer mit SQL Server 2017 erstellen."
services: virtual-machines-linux
author: rothja
ms.author: jroth
manager: jhubbard
ms.date: 10/25/2017
ms.topic: hero-article
tags: azure-service-management
ms.devlang: na
ms.service: virtual-machines-sql
ms.workload: iaas-sql-server
ms.technology: database-engine
ms.openlocfilehash: 54cb1e11e82c7a9be82252a0b93277353c5005e2
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2017
---
# <a name="provision-a-linux-sql-server-virtual-machine-in-the-azure-portal"></a>Bereitstellen eines virtuellen SQL Server-Computers über das Azure-Portal

> [!div class="op_single_selector"]
> * [Linux](provision-sql-server-linux-virtual-machine.md)
> * [Windows](../../windows/sql/virtual-machines-windows-portal-sql-server-provision.md)

In diesem Schnellstarttutorial erstellen Sie über das Azure-Portal einen virtuellen Linux-Computer mit SQL Server 2017.

In diesem Lernprogramm lernen Sie Folgendes:

* [Erstellen eines virtuellen Linux-SQL-Computers aus dem Katalog](#create)
* [Herstellen einer Verbindung mit dem neuen virtuellen Computer per SSH](#connect)
* [Ändern des Systemadministratorkennworts](#password)
* [Konfigurieren für Remoteverbindungen](#remote)

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen, bevor Sie beginnen.

## <a id="create"></a> Erstellen eines virtuellen Linux-Computers mit SQL Server

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.

1. Klicken Sie im linken Bereich auf **Neu**.

1. Klicken Sie im Bereich **Neu** auf **Compute**.

1. Klicken Sie neben der Überschrift **Vorgestellt** auf **Alle anzeigen**.

   ![Alle VM-Images anzeigen](./media/provision-sql-server-linux-virtual-machine/azure-compute-blade.png)

1. Geben Sie im Suchfeld die Zeichenfolge **SQL Server 2017** ein, und starten Sie die Suche durch Drücken der**EINGABETASTE**.

1. Klicken Sie auf das**Filtersymbol**, begrenzen Sie die Suche auf **Linux-basierte** Images von **Microsoft**, und klicken Sie anschließend auf **Fertig**.

    ![Suchfilter für VM-Images mit SQL Server 2017](./media/provision-sql-server-linux-virtual-machine/searchfilter.png)

1. Wählen Sie in den Suchergebnissen ein Linux-Image mit SQL Server 2017 aus. In diesem Tutorial verwenden wir **Free SQL Server License: SQL Server 2017 Developer on Red Hat Enterprise Linux 7.4** (Kostenlose SQL Server-Lizenz: SQL Server 2017 Developer unter Red Hat Enterprise Linux 7.4).

   > [!TIP]
   > Mit der Developer Edition können Sie für Entwicklungs- und Testaufgaben die Features der Enterprise Edition ohne SQL Server-Lizenzkosten nutzen. Sie zahlen nur für die Ausführung des virtuellen Linux-Computers.

1. Klicken Sie auf **Erstellen**.

1. Geben Sie im Fenster **Grundlagen** die Details für Ihren virtuellen Linux-Computer an. 

    ![Fenster „Grundlagen“](./media/provision-sql-server-linux-virtual-machine/basics.png)

    > [!Note]
    > Für die Authentifizierung können Sie einen öffentlichen SSH-Schlüssel oder ein Kennwort verwenden. SSH ist sicherer. Wie Sie einen SSH-Schlüssel generieren, erfahren Sie unter [Erstellen und Verwenden eines SSH-Schlüsselpaars (öffentlich und privat) für virtuelle Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-mac-create-ssh-keys).

1. Klicken Sie auf **OK**.

1. Wählen Sie im Fenster **Größe** eine Computergröße aus. Weitere Größen können durch Klicken auf **Alle anzeigen** angezeigt werden. Weitere Informationen zu VM-Größen finden Sie unter [Größen für virtuelle Linux-Computer in Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes).

    ![Auswählen einer VM-Größe](./media/provision-sql-server-linux-virtual-machine/vmsizes.png)

   > [!TIP]
   > Für Entwicklungsaufgaben und Funktionstests empfehlen wir mindestens die Größe **DS2**. Für Leistungstests sollten Sie mindestens **DS13** verwenden.

1. Klicken Sie auf **Auswählen**.

1. Im Fenster **Einstellungen** können Sie die Einstellungen anpassen oder die Standardeinstellungen übernehmen.

1. Klicken Sie auf **OK**.

1. Klicken Sie auf der Seite **Zusammenfassung** auf **Kaufen**, um den virtuellen Computer zu erstellen.

## <a id="connect"></a> Herstellen einer Verbindung mit dem virtuellen Linux-Computer

Wenn Sie bereits über eine BASH-Shell verfügen, verwenden Sie den Befehl **ssh**, um eine Verbindung mit dem virtuellen Azure-Computer herzustellen. Ersetzen Sie im folgenden Befehl den VM-Benutzernamen und die IP-Adresse, um eine Verbindung mit Ihrem virtuellen Linux-Computer herzustellen.

```bash
ssh azureadmin@40.55.55.555
```

Die IP-Adresse Ihres virtuellen Computers finden Sie im Azure-Portal.

![IP-Adresse im Azure-Portal](./media/provision-sql-server-linux-virtual-machine/vmproperties.png)

Wenn Sie mit Windows arbeiten und über keine BASH-Shell verfügen, können Sie einen SSH-Client (beispielsweise PuTTY) installieren.

1. [Laden Sie PuTTY herunter, und installieren Sie es.](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

1. Führen Sie PuTTY aus.

1. Geben Sie im Konfigurationsbildschirm von PuTTY die öffentliche IP-Adresse Ihres virtuellen Computers ein.

1. Klicken Sie auf „Öffnen“, und geben Sie Ihren Benutzernamen und Ihr Kennwort ein.

Weitere Informationen zum Herstellen einer Verbindung mit virtuellen Linux-Computern finden Sie unter [Erstellen einer Linux-VM mit dem Azure-Portal](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-portal#ssh-to-the-vm).

## <a id="password"></a> Ändern des Systemadministratorkennworts

Der neue virtuelle Computer installiert SQL Server mit einem zufälligen Systemadministratorkennwort. Dieses Kennwort muss zurückgesetzt werden, um unter Verwendung der Systemadministratoranmeldung eine Verbindung mit SQL Server herstellen zu können.

1. Öffnen Sie nach dem Herstellen einer Verbindung mit Ihrem virtuellen Linux-Computer ein neues Befehlsterminal.

1. Ändern Sie das Systemadministratorkennwort mithilfe der folgenden Befehle:

   ```bash
   sudo systemctl stop mssql-server
   sudo /opt/mssql/bin/mssql-conf set-sa-password
   ```

   Geben Sie ein neues Systemadministratorkennwort und eine Kennwortbestätigung ein, wenn Sie dazu aufgefordert werden.

1. Starten Sie den SQL Server-Dienst neu.

   ```bash
   sudo systemctl start mssql-server
   ```

## <a name="add-the-tools-to-your-path-optional"></a>Hinzufügen der Tools zu Ihrem Pfad (optional)

Einige [SQL Server-Pakete](sql-server-linux-virtual-machines-overview.md#packages) werden standardmäßig installiert. Hierzu zählt auch das Paket mit den SQL Server-Befehlszeilentools. Das Toolpaket enthält die Tools **sqlcmd** und **bcp**. Der Einfachheit halber können Sie optional den Toolpfad (`/opt/mssql-tools/bin/`) Ihrer **PATH**-Umgebungsvariablen hinzufügen.

1. Führen Sie die folgenden Befehle aus, um **PATH** sowohl für Anmeldesitzungen als auch für interaktive Sitzungen/Sitzungen ohne Anmeldung zu ändern:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a id="remote"></a> Konfigurieren für Remoteverbindungen

Wenn Sie mit SQL Server auf dem virtuellen Azure-Computer eine Remoteverbindung herstellen möchten, müssen Sie in der Netzwerksicherheitsgruppe eine Eingangsregel konfigurieren. Die Regel lässt Datenverkehr an dem Port zu, an dem SQL Server lauscht (standardmäßig 1433). In den folgenden Schritten wird die Vorgehensweise mit dem Azure-Portal gezeigt: 

1. Wählen Sie im Portal **Virtuelle Computer**, und wählen Sie dann die SQL Server-VM aus.

1. Wählen Sie in der Eigenschaftenliste die Option **Netzwerk** aus.

1. Klicken Sie im Fenster **Netzwerk** unter **Regeln für eingehende Ports** auf die Schaltfläche **Hinzufügen**.

   ![Regeln für eingehende Ports](./media/provision-sql-server-linux-virtual-machine/networking.png)

1. Wählen Sie in der Liste **Dienst** die Option **MS SQL** aus.

    ![MS SQL-Sicherheitsgruppenregel](./media/provision-sql-server-linux-virtual-machine/sqlnsgrule.png)

1. Klicken Sie auf **OK** , um die Regel für den virtuellen Computer zu speichern.

### <a name="open-the-firewall-on-rhel"></a>Öffnen der Firewall in RHEL

In diesem Tutorial wurde die Erstellung eines virtuellen RHEL-Computers (Red Hat Enterprise Linux) erläutert. Wenn Sie mit virtuellen RHEL-Computern eine Remoteverbindung herstellen möchten, müssen Sie in der Linux-Firewall den Port 1433 öffnen.

1. [Stellen Sie eine Verbindung mit Ihrem virtuellen RHEL-Computer her.](#connect)

1. Führen Sie in der BASH-Shell die folgenden Befehle aus:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd –-reload
   ```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über einen virtuellen Azure-Computer mit SQL Server 2017 verfügen, können Sie mit **sqlcmd** eine lokale Verbindung herstellen und Transact-SQL-Abfragen ausführen.

Falls Sie den virtuellen Azure-Computer für SQL Server-Remoteverbindungen konfiguriert haben, können Sie auch eine Remoteverbindung herstellen. Ein Beispiel dafür, wie Sie unter Windows eine Remoteverbindung mit SQL Server unter Linux herstellen, finden Sie unter [Use SSMS on Windows to connect to SQL Server on Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-ssms) (Verwenden von SSMS unter Windows, um eine Verbindung mit SQL Server unter Linux herzustellen). Informationen zum Herstellen einer Verbindung mit Visual Studio Code finden Sie unter [Use Visual Studio Code to create and run Transact-SQL scripts for SQL Server](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode) (Erstellen und Ausführen von Transact-SQL-Skripts für SQL Server mithilfe von Visual Studio Code).

Allgemeine Informationen zu SQL Server unter Linux finden Sie in der [Übersicht über SQL Server 2017 unter Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview). Ausführlichere Informationen zur Verwendung virtueller Linux-Computer mit SQL Server 2017 finden Sie in der [Übersicht über virtuelle SQL Server 2017-Computer in Azure](sql-server-linux-virtual-machines-overview.md).
