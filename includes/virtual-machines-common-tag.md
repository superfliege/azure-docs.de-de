


## <a name="tagging-a-virtual-machine-through-templates"></a>Kennzeichnen eine virtuelle Maschine über Vorlagen
Zunächst sehen wir uns über Vorlagen tagging. [Diese Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) fügt vor und Tags für die folgenden Ressourcen: Compute (virtueller Computer), Speicher (Speicherkonto) und Netzwerk (öffentliche IP-Adresse, Virtuellenetzwerk und Netzwerkschnittstelle). Diese Vorlage für eine Windows-VM ist jedoch für virtuelle Linux-Computer angepasst werden kann.

Klicken Sie auf die **Bereitstellung in Azure** Schaltfläche aus der [Link Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Dies wird navigieren Sie zu der [Azure-Portal](https://portal.azure.com/) , in dem Sie diese Vorlage bereitstellen können.

![Einfache Bereitstellung mit RFID-Transponder](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Diese Vorlage enthält die folgenden Tags: *Abteilung*, *Anwendung*, und *erstellt von*. Sie können zum Hinzufügen/bearbeiten diese Tags direkt in der Vorlage Wunsch anderen Tagnamen.

![Azure-Tags in einer Vorlage](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Wie Sie sehen können, werden die Tags als Schlüssel/Wert-Paare, getrennt durch einen Doppelpunkt (:)) definiert. Tags müssen im folgenden Format definiert werden:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Speichern Sie die Vorlagendatei, nachdem Sie abgeschlossen haben, indem Sie ihn mit den Tags Ihrer Wahl bearbeiten.

Im nächsten Schritt in der **Parameter bearbeiten** Abschnitt, Sie können ggf. die Werte für die Tags.

![Bearbeiten von Tags im Azure-portal](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Klicken Sie auf **erstellen** beim Bereitstellen dieser Vorlage durch Ihre Tagwerte.

## <a name="tagging-through-the-portal"></a>Über das Portal-Kennzeichnung
Nach dem erstellen Ihre Ressourcen mit Tags, können Sie anzeigen, hinzufügen und Löschen von RFID-Transponder im Portal.

Wählen Sie das Symbol "Tags", um die Tags anzuzeigen:

![Symbol "Tags" im Azure-portal](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Fügen Sie ein neues Tag über das Portal durch Ihren eigenen Schlüssel/Wert-Paar definieren, und speichern Sie sie.

![Neues Tag im Azure-Portal hinzufügen](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Ihr neue Tag sollte jetzt in der Liste der Tags für die Ressource angezeigt werden.

![Neues Tag gespeichert, die im Azure-portal](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

