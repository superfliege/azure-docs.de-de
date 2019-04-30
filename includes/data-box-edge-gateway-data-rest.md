---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: d8570c0be6b64d4e289575ce3f3f1721c4a65074
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684473"
---
Für ruhende Daten gilt Folgendes:

- Für ruhende Daten wird die BitLocker-XTS AES-256-Verschlüsselung zum Schutz der lokalen Daten verwendet.
- Für die Daten, die auf Freigaben gespeichert sind, ist der Zugriff auf die Freigaben eingeschränkt.

    - Für SMB-Clients, die auf die Freigabedaten zugreifen, sind Benutzeranmeldeinformationen erforderlich, die der Freigabe zugeordnet sind. Diese Anmeldeinformationen werden bei der Freigabeerstellung definiert.
    - Für NFS-Clients, die auf die Freigaben zugreifen, müssen bei der Freigabeerstellung die IP-Adressen der Clients hinzugefügt werden.