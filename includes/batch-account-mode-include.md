---
title: Includedatei
description: Includedatei
services: batch
author: dlepow
ms.service: batch
ms.topic: include
ms.date: 05/04/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 62bb91a2e51c39caf31719f72d68a6edab9205bc
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
ms.locfileid: "33903329"
---
> [!NOTE]
> Beim Erstellen eines Batchkontos können Sie zwischen zwei Modi der *Poolzuordnung* wählen: **Benutzerabonnement** und **Batchdienst**. In den meisten Fällen sollten Sie den standardmäßigen Batchdienstmodus wählen, in dem Pools im Hintergrund von Azure verwalteten Abonnements zugeordnet werden. Im alternativen Benutzerabonnementmodus werden virtuelle Batchcomputer und andere Ressourcen direkt in Ihrem Abonnement erstellt, wenn ein Pool erstellt wird. Der Benutzerabonnementmodus ist erforderlich, wenn Sie mithilfe von [reservierten Azure VM-Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) Batchpools erstellen möchten. Wenn Sie ein Batch-Konto im Benutzerabonnementmodus erstellen möchten, müssen Sie auch Ihr Abonnement in Azure Batch registrieren und das Konto einer Azure Key Vault-Instanz zuordnen.