## <a name="os-config"></a>Hinzufügen von IP-Adressen zu einem VM-Betriebssystem

Verbinden und Adressen der Anmeldung mit einem virtuellen Computer, die Sie mit mehreren privaten IP-Adresse erstellt haben. Sie müssen manuell hinzufügen alle die private IP-Adressen (einschließlich der primären), die Sie mit dem virtuellen Computer hinzugefügt. Die folgenden Schritte für Ihr Betriebssystem des virtuellen Computers:

### <a name="windows"></a>Windows

1. Geben Sie an einer Eingabeaufforderung *Ipconfig/all*.  Nur die *primären* private IP-Adresse (über DHCP).
2. Typ *ncpa.cpl* in der Befehlszeile zum Öffnen der **Netzwerkverbindungen** Fenster.
3. Öffnen Sie die Eigenschaften für den entsprechenden Adapter: **LAN-Verbindung**.
4. Doppelklicken Sie auf Internetprotokoll Version 4 (IPv4).
5. Wählen Sie **verwenden Sie die folgenden IP-Adresse** , und geben Sie die folgenden Werte:

    * **IP-Adresse**: Geben Sie die *primären* private IP-Adresse
    * **Die Netzwerksubnetz-Maske**: Satz basierend auf dem Subnetz. Wenn das Subnetz/24 wird beispielsweise ist Subnetz klicken Sie dann die Subnetzmaske 255.255.255.0.
    * **Standardgateway**: die erste IP-Adresse im Subnetz. Wenn Ihr Subnetz 10.0.0.0/24 ist, ist die Gateway IP-Adresse 10.0.0.1.
    * Klicken Sie auf **die folgenden DNS-Serveradressen verwenden** , und geben Sie die folgenden Werte:
        * **Bevorzugter DNS-Server**: Wenn Sie einen eigenen DNS-Server nicht verwenden, geben Sie 168.63.129.16.  Wenn Sie einen eigenen DNS-Server verwenden, geben Sie die IP-Adresse für Ihren Server ein.
    * Klicken Sie auf die **erweitert** Schaltfläche, und fügen Sie zusätzliche IP-Adressen hinzu. Fügen Sie jede der sekundären privaten IP-Adressen in Schritt 8 an die NIC mit dem gleichen Subnetz, die für die primäre IP-Adresse angegeben.
        >[!WARNING] 
        >Wenn Sie die oben genannten Schritte nicht ordnungsgemäß befolgen, verlieren Sie möglicherweise Konnektivität mit Ihren virtuellen Computer. Stellen Sie sicher, dass die Informationen für Schritt 5 eingegebene pausieren exakt ist.

    * Klicken Sie auf **OK** um die TCP/IP-Einstellungen zu schließen und dann **OK** erneut aus, um die adaptereinstellungen zu schließen. Die RDP-Verbindung wird erneut hergestellt.

6. Geben Sie an einer Eingabeaufforderung *Ipconfig/all*. Alle IP-Adressen, die Sie hinzugefügt werden und DHCP ist deaktiviert.


### <a name="validation-windows"></a>Überprüfung (Windows)

Um sicherzustellen, dass Sie über die sekundäre IP-Adresse mit dem Internet herstellen können Konfiguration haben Sie über die öffentliche IP-Adresse zugewiesen, sobald Sie die oben beschriebenen Schritte anhand hinzugefügt haben, verwenden den folgenden Befehl:

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
>Für sekundäre IP-Konfigurationen können Sie nur mit dem Internet ping, wenn die Konfiguration eine öffentliche IP-Adresse zugeordnet wurde. Für die primäre IP-Konfigurationen ist eine öffentliche IP-Adresse nicht erforderlich, um mit dem Internet zu pingen.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. Öffnen Sie ein terminal-Fenster.
2. Stellen Sie sicher, dass Sie die Root-Benutzer sind. Wenn Sie nicht sind, geben Sie den folgenden Befehl ein:

    ```bash
    sudo -i
    ```

3. Aktualisieren Sie die Konfigurationsdatei der Netzwerkschnittstelle (vorausgesetzt "eth0").

    * Behalten Sie das vorhandene Zeilenelement für Dhcp. Die primäre IP-Adresse bleibt konfigurierten wie zuvor.
    * Fügen Sie eine Konfiguration für eine weitere statische IP-Adresse mithilfe der folgenden Befehle ein:

        ```bash
        cd /etc/network/interfaces.d/
        ls
        ```

    Daraufhin sollte eine cfg-Datei.
4. Öffnen Sie die Datei an. Die folgenden Zeilen am Ende der Datei sollte angezeigt werden:

    ```bash
    auto eth0
    iface eth0 inet dhcp
    ```

5. Fügen Sie nach den vorhandenen Zeilen, die in dieser Datei die folgenden Zeilen hinzu:

    ```bash
    iface eth0 inet static
    address <your private IP address here>
    netmask <your subnet mask>
    ```

6. Speichern Sie die Datei mit dem folgenden Befehl ein:

    ```bash
    :wq
    ```

7. Zurücksetzen der Netzwerkschnittstelle mit den folgenden Befehl aus:

    ```bash
    sudo ifdown eth0 && sudo ifup eth0
    ```

    > [!IMPORTANT]
    > Führen Sie Ifdown und Ifup in der gleichen Zeile, wenn eine Remoteverbindung zu verwenden.
    >

8. Stellen Sie sicher, dass die Netzwerkschnittstelle mit dem folgenden Befehl die IP-Adresse hinzugefügt wird:

    ```bash
    ip addr list eth0
    ```

    Daraufhin sollte die IP-Adresse, die Sie als Teil der Liste hinzugefügt.

### <a name="linux-redhat-centos-and-others"></a>Linux (Redhat, CentOS usw.)

1. Öffnen Sie ein terminal-Fenster.
2. Stellen Sie sicher, dass Sie die Root-Benutzer sind. Wenn Sie nicht sind, geben Sie den folgenden Befehl ein:

    ```bash
    sudo -i
    ```

3. Geben Sie Ihr Kennwort ein, und befolgen Sie die Anweisungen, wie Sie dazu aufgefordert werden. Sobald Sie die Root-Benutzer sind, navigieren Sie zu den Skripts Netzwerkordner mit den folgenden Befehl aus:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Liste der verwandten Ifcfg-Dateien, die mit dem folgenden Befehl:

    ```bash
    ls ifcfg-*
    ```

    Daraufhin sollte *Ifcfg eth0* als eine der Dateien.

5. Um eine IP-Adresse hinzuzufügen, erstellen Sie eine Konfigurationsdatei dafür, wie unten dargestellt. Beachten Sie, dass eine Datei für jede IP-Konfiguration erstellt werden muss.

    ```bash
    touch ifcfg-eth0:0
    ```

6. Öffnen der *Ifcfg-Eth0:0* -Datei mit den folgenden Befehl aus:

    ```bash
    vi ifcfg-eth0:0
    ```

7. Hinzufügen von Inhalt in die Datei *Eth0:0* in diesem Fall mit dem folgenden Befehl. Achten Sie darauf, dass Sie Informationen, die auf Basis Ihrer IP-Adresse zu aktualisieren.

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. Speichern Sie die Datei mit dem folgenden Befehl ein:

    ```bash
    :wq
    ```

9. Die Netzwerkdienste neu, und stellen Sie sicher, dass die Änderungen erfolgreich sind, indem Sie die folgenden Befehle ausführen:

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    Sie sollten die IP-Adresse, die Sie hinzugefügt haben, finden Sie unter *Eth0:0*, in der Liste zurückgegeben.

### <a name="validation-linux"></a>Überprüfung (Linux)

Um sicherzustellen, dass Sie von Ihrem sekundäre IP-Konfiguration über die öffentliche IP-Adresse eine Verbindung mit dem Internet können zugeordnet, verwenden Sie den folgenden Befehl:

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
>Für sekundäre IP-Konfigurationen können Sie nur mit dem Internet ping, wenn die Konfiguration eine öffentliche IP-Adresse zugeordnet wurde. Für die primäre IP-Konfigurationen ist eine öffentliche IP-Adresse nicht erforderlich, um mit dem Internet zu pingen.

Für virtuelle Linux-Computer, wenn ausgehende Verbindungen von einem sekundären NIC überprüfen möchten müssen Sie möglicherweise den entsprechende Routen hinzufügen. Es gibt viele Methoden dafür aus. Weitere Informationen finden Sie entsprechende Dokumentation für Ihre Linux-Distributionen. Im folgenden finden eine Methode, um dies zu erreichen:

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Achten Sie darauf, dass Sie ersetzen:
    - **10.0.0.5** mit der privaten IP-Adresse, die über eine öffentliche IP-Adresse zugeordnet ist
    - **10.0.0.1** mit dem Standardgateway
    - **eth2 ändern** auf den Namen der sekundären NIC
