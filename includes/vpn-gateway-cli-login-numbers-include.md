1. Melden Sie sich mit Ihrem Azure-Abonnement mit der [az Anmeldung](/cli/azure/#login) Befehl, und befolgen Sie die auf dem Bildschirm Richtungen. Weitere Informationen zum Anmelden finden Sie unter [erste Schritte mit Azure-CLI-2.0](/cli/azure/get-started-with-azure-cli).

  ```azurecli
  az login
  ```
2. Wenn Sie über mehrere Azure-Abonnement verfügen, Liste der Abonnements für das Konto.

  ```azurecli
  az account list --all
  ```
3. Geben Sie das Abonnement, das Sie verwenden möchten.

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```