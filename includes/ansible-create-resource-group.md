---
author: tomarcher
ms.service: ansible
ms.topic: include
ms.date: 08/09/2018
ms.author: tarcher
ms.openlocfilehash: fa1f7fe0b4b70aae4f9165197d5d1463df1f2e3b
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/09/2018
ms.locfileid: "40025935"
---
1. Erstellen Sie in Cloud Shell eine Datei namens `rg.yml`.

    ```bash
    vi rg.yml
    ```

1. Drücken Sie die **I-TASTE**, um den Einfügemodus zu starten.

1. Fügen Sie den folgenden Code in den Editor ein:

   ```yaml
   ---
   - hosts: localhost
     connection: local
     tasks:
       - name: Create resource group
         azure_rm_resourcegroup:
           name: ansible-rg
           location: eastus
         register: rg
       - debug:
           var: rg
   ```

1. Drücken Sie die **ESC**-TASTE, um den Einfügemodus zu beenden.

1. Speichern Sie die Datei, und geben Sie den folgenden Befehl ein, um den vi-Editor zu schließen:

    ```bash
    :wq
    ```

1. Führen Sie das Playbook `rg.yml` aus:

   ```bash
   ansible-playbook rg.yml
   ```

Die Ergebnisse der Ausführung des ansible-Befehls sollten ungefähr wie folgt aussehen:

```output
PLAY [localhost] *********************************************************************************

TASK [Gathering Facts] ***************************************************************************
ok: [localhost]

TASK [Create resource group] *********************************************************************
changed: [localhost]

TASK [debug] *************************************************************************************
ok: [localhost] => {
    "rg": {
        "changed": true,
        "contains_resources": false,
        "failed": false,
        "state": {
            "id": "/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/ansible-rg",
            "location": "eastus",
            "name": "ansible-rg",
            "provisioning_state": "Succeeded",
            "tags": null
        }
    }
}

PLAY RECAP ***************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0
```