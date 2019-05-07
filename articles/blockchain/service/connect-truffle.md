---
title: Herstellen einer Verbindung mithilfe von Truffle
description: Herstellen einer Verbindung mit einem Azure Blockchain-Netzwerk mithilfe von Truffle
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: jackyhsu
manager: femila
ms.openlocfilehash: 037f37d6a8e1c41579403dbf7c9dd265efbb5d10
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2019
ms.locfileid: "65030028"
---
# <a name="quickstart-use-truffle-to-connect-to-a-an-azure-blockchain-service-network"></a>Schnellstart: Verwenden von Truffle zum Herstellen einer Verbindung mit einem Azure Blockchain-Netzwerk

Truffle ist eine Entwicklungsumgebung für Blockchains, die Sie verwenden können, um eine Verbindung mit einem Azure Blockchain-Knoten herzustellen.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* [Erstellen eines Azure Blockchain-Mitglieds](create-member.md)
* Für Truffle ist die Installation mehrerer Tools erforderlich, dazu gehören [Node.js](https://nodejs.org), [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) und [Truffle](https://github.com/trufflesuite/truffle).

    Für ein schnelles Setup unter Windows 10 installieren Sie [Ubuntu unter Windows](https://www.microsoft.com/p/ubuntu/9nblggh4msv6) für die Bash Unix-Terminalshell, und installieren Sie dann [Truffle](https://github.com/trufflesuite/truffle). Die Distribution „Ubuntu unter Windows“ enthält Node.js und Git.

## <a name="create-truffle-project"></a>Erstellen eines Truffle-Projekts

1. Öffnen Sie die Bash-Terminalshell.
1. Wählen Sie das Verzeichnis aus, in dem Sie das Truffle-Projektverzeichnis erstellen möchten. Beispiel:

    ``` bash
    cd /mnt/c
    ```

1. Erstellen Sie ein Verzeichnis für das Projekt, und ändern Sie den Pfad in das neue Verzeichnis. Beispiel:

    ``` bash
    mkdir truffledemo
    cd truffledemo
    ```

1. Installieren Sie die Ethereum-JavaScript-API (web3) im Projektordner. Derzeit ist die web3-Version 1.0.0-beta.37 erforderlich.

    ``` bash
    npm install web3@1.0.0-beta.37
    ```

    Möglicherweise werden während der Installation npm-Warnungen ausgelöst.

1. Initialisieren Sie das Truffle-Projekt.

    ``` bash
    truffle init
    ```

1. Starten Sie die interaktive Entwicklungskonsole von Truffle.

    ``` bash
    truffle develop
    ```

    Truffle erstellt eine lokale Entwicklungsblockchain und stellt eine interaktive Konsole bereit.

## <a name="connect-to-transaction-node"></a>Herstellen einer Verbindung mit dem Transaktionsknoten

Hier wird Web3 verwendet, um eine Verbindung mit dem Transaktionsknoten herzustellen. Sie können die Web3-Verbindungszeichenfolge aus dem Azure-Portal abrufen.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
1. Navigieren Sie zu Ihrem Azure Blockchain-Mitglied. Klicken Sie auf **Transaktionsknoten** und dann auf den Link „default transaction node“ (Standardtransaktionsknoten).

    ![Standardtransaktionsknoten auswählen](./media/connect-truffle/transaction-nodes.png)

1. Klicken Sie auf **Beispielcode > Web3**.
1. Kopieren Sie den JavaScript-Code aus **HTTPS (Zugriffsschlüssel 1)**. Den Code benötigen Sie für die interaktive Entwicklungskonsole von Truffle.

    ![Web3-Code](./media/connect-truffle/web3-code.png)

1. Fügen Sie den JavaScript-Code aus dem vorherigen Schritt in die interaktive Entwicklungskonsole von Truffle ein. Mit dem Code wird ein web3-Objekt erstellt, das mit Ihrem Azure Blockchain-Transaktionsknoten verbunden ist.

    Beispielausgabe:

    ```bash
    truffle(develop)> var Web3 = require("Web3");
    truffle(develop)> var provider = new Web3.providers.HttpProvider("https://myblockchainmember.blockchain.azure.com:3200/hy5FMu5TaPR0Zg8GxiPwned");
    truffle(develop)> var web3 = new Web3(provider);
    truffle(develop)>
     ```

    Sie können Methoden für das **web3-Objekt** aufrufen, um mit Ihrem Transaktionsknoten zu interagieren.

1. Rufen Sie die Methode **getBlockNumber** auf, damit die aktuelle Blocknummer zurückgegeben wird.

    ```bash
    web3.eth.getBlockNumber();
    ```

    Beispielausgabe:

    ```bash
    truffle(develop)> web3.eth.getBlockNumber();
    18567
    ```
1. Beenden Sie die Truffle-Entwicklungskonsole.

    ```bash
    .exit
    ```

## <a name="next-steps"></a>Nächste Schritte

In diesem Schnellstart haben Sie ein Truffle-Projekt erstellt, um eine Verbindung zum Azure Blockchain-Standardtransaktionsknoten herzustellen.

Im nächsten Tutorial wird Truffle verwendet, um eine Transaktion an Ihr Konsortiumblockchainnetzwerk zu senden.

> [!div class="nextstepaction"]
> [Senden einer Transaktion](send-transaction.md)