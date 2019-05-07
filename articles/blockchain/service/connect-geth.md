---
title: Herstellen einer Verbindung mit Azure Blockchain mithilfe von Geth
description: Herstellen einer Verbindung mit einem Azure Blockchain-Netzwerk mithilfe von Geth
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 0716a9326a54ae31d4f355fe5f4c88488339b390
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029978"
---
# <a name="quickstart-use-geth-to-connect-to-a-transaction-node"></a>Schnellstart: Herstellen einer Verbindung mit einem Transaktionsknoten mithilfe von Geth

Geth ist ein Go Ethereum-Client, mit dem Sie eine Geth-Instanz an einen Azure Blockchain-Transaktionsknoten anfügen können.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie [Geth](https://github.com/ethereum/go-ethereum/wiki/geth).
* [Erstellen Sie ein Azure Blockchain-Mitglied](create-member.md)

## <a name="get-the-geth-connection-string"></a>Abrufen der Geth-Verbindungszeichenfolge

Die Geth-Verbindungszeichenfolge finden Sie im Azure-Portal.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu Ihrem Azure Blockchain-Mitglied. Klicken Sie auf **Transaktionsknoten** und dann auf den Link „default transaction node“ (Standardtransaktionsknoten).

    ![Standardtransaktionsknoten auswählen](./media/connect-geth/transaction-nodes.png)

1. Wählen Sie **Connection strings** (Verbindungszeichenfolgen).
1. Kopieren Sie die Verbindungszeichenfolge aus **HTTPS (Access key 1)** (HTTPS (Zugriffsschlüssel 1)). Sie benötigen den Befehl für den nächsten Abschnitt.

    ![Verbindungszeichenfolge](./media/connect-geth/connection-string.png)

## <a name="connect-to-geth"></a>Herstellen einer Verbindung mit Geth

1. Öffnen Sie eine Eingabeaufforderung oder Shell.
1. Verwenden Sie den Unterbefehl „geth attach“, um eine Anfügung an die ausgeführte Geth-Instanz auf Ihrem Transaktionsknoten durchzuführen. Fügen Sie die Verbindungszeichenfolge als Argument für den Unterbefehl ein. Beispiel:

    ```
    geth attach <connection string>
    ```

1. Sobald eine Verbindung mit der Ethereum-Konsole des Transaktionsknotens hergestellt wurde, können Sie die Web3-JavaScript- DApp-API oder die Administrator-API aufrufen.

    Mit der folgenden API können Sie beispielsweise den Wert von chainId ermitteln:

    ```bash
    admin.nodeInfo.protocols.istanbul.config.chainId
    ```

    In diesem Fall lautet der Wert von chainId 297.

    ![Azure Blockchain-Option](./media/connect-geth/geth-attach.png)

1. Geben Sie `exit` ein, um die Verbindung zur Konsole zu trennen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie den Geth-Client verwendet, um eine Anfügung an eine Geth-Instanz auf einem Azure Blockchain-Transaktionsknoten durchzuführen. Im nächsten Tutorial erfahren Sie, wie Sie eine Transaktion mithilfe von Truffle bereitstellen und senden.

> [!div class="nextstepaction"]
> [Senden einer Transaktion](send-transaction.md)