Sie können überprüfen, dass die Verbindung mit erfolgreich die [az Netzwerk VPN-Verbindung anzeigen](/cli/azure/network/vpn-connection#show) Befehl. Im Beispiel "– Name" bezieht sich auf den Namen der Verbindung, die Sie testen möchten. Wenn die Verbindung wird gerade eingerichtet wird, zeigt zugehöriger Verbindungsstatus "Verbinden". Nachdem die Verbindung hergestellt wird, ändert den Status in "Verbunden".

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

