
1. Melden Sie sich bei Ihrem Azure-Abonnement über die Schritte unter [mit Azure verbinden aus dem Azure-CLI-1.0](../articles/xplat-cli-connect.md).

2. Stellen Sie sicher, dass Sie im klassischen Modus-Bereitstellung sind, wie folgt:

    ```azurecli
    azure config mode asm
    ```

3. Ermitteln Sie die Linux-Image, das wie folgt aus den verfügbaren Images geladen werden sollen:

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    Verwenden Sie ein Windows-Eingabeaufforderungsfenster **suchen** statt Grep.
   
4. Verwendung `azure vm create` So erstellen Sie einen virtuellen Computer mit Linux-Image aus der vorherigen Liste. Dieser Schritt wird ein Cloud-Dienst und Speicher-Konto erstellt. Sie können auch diese VM verbinden, an einen vorhandenen Clouddienst mit einem `-c` Option. Erstellen Sie einen SSH-Endpunkt zum Anmelden bei der virtuellen Linux-Computer mit der `-e` Option. Das folgende Beispiel erstellt einen virtuellen Computer mit dem Namen `myVM` mithilfe der `Ubuntu-14_04_4-LTS` -image der `West US` Speicherort, und fügt einen Benutzernamen `ops`:
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    Die Ausgabe lautet wie im folgenden Beispiel:

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > Sie müssen für einen virtuellen Linux-Computer angeben der `-e` option `vm create`. Es ist nicht möglich, um SSH zu aktivieren, nachdem die virtuelle Maschine erstellt wurde. Weitere Informationen zu SSH, finden Sie unter [Verwenden von SSH mit Linux unter Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

5. Sie können die Attribute des virtuellen Computers überprüfen, indem Sie mit der `azure vm show` Befehl. Das folgende Beispiel listet Informationen für den virtuellen Computer mit dem Namen `myVM`:

    ```azurecli   
    azure vm show myVM
    ```

6. Starten Sie Ihren virtuellen Computer mit der `azure vm start` Befehl wie folgt:

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie ausführliche Informationen zu all diese Azure-CLI 1.0 Befehle für virtuellen Computer, die [API für die klassische Bereitstellung mit der Azure-CLI-1.0](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

