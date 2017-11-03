---
title: "Verwalten von Key Vault in Azure Stack über das Portal | Microsoft Docs"
description: Hier erfahren Sie, wie Sie Key Vault in Azure Stack mithilfe des Portals verwalten.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: sngun
ms.openlocfilehash: d263cbcc81be37eaedfdb771436fd13ef25362f8
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/20/2017
---
# <a name="manage-key-vault-in-azure-stack-by-using-the-portal"></a>Verwalten von Key Vault in Azure Stack über das Portal

Sie können Key Vault in Azure Stack über das Azure Stack-Portal verwalten. In diesem Artikel erfahren Sie, wie Sie Key Vault in Azure Stack erstellen und verwalten. 

## <a name="prerequisites"></a>Voraussetzungen  

Sie müssen ein Angebot abonnieren, das den Azure Key Vault-Dienst umfasst.
 
## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors 

1. Melden Sie sich am [Benutzerportal](https://portal.local.azurestack.external) an.  

2. Wählen Sie im Dashboard **Neu** > **Sicherheit und Identität** > **Schlüsseltresor** aus.  

    ![Key Vault-Bildschirm](media/azure-stack-kv-manage-portal/image1.png)  

3. Weisen Sie Ihrem Tresor im Bereich **Schlüsseltresor erstellen** unter **Name** einen Namen zu. Tresornamen dürfen nur alphanumerische Zeichen und das Sonderzeichen Bindestrich (-) enthalten. Sie sollten nicht mit einer Zahl beginnen.  

4. Wählen Sie aus der Liste der verfügbaren Abonnements ein **Abonnement** aus. Die Dropdownliste enthält alle Abonnements, für die der Key Vault-Dienst zur Verfügung steht.  

5. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine neue.  

6. Wählen Sie den **Tarif** aus.  
    >[!NOTE]
    > Schlüsseltresore im Azure Stack Development Kit unterstützen nur die SKU **Standard**.

7. Wählen Sie eine der vorhandenen **Zugriffsrichtlinien** aus, oder erstellen Sie eine neue Richtlinie. Mithilfe einer Zugriffsrichtlinie können Sie einen Benutzer, eine Anwendung oder eine Sicherheitsgruppe dazu berechtigen, Vorgänge mit diesem Tresor auszuführen.  

8. Wählen Sie optional eine **Erweiterte Zugriffsrichtlinie** aus, um etwa den Zugriff auf Virtual Machines (für die Bereitstellung), auf Resource Manager (für die Vorlagenbereitstellung) und auf Azure Disk Encryption (für die Volumeverschlüsselung) zu ermöglichen. 
  
9.  Nachdem Sie die Einstellungen konfiguriert haben, wählen Sie **OK** aus, und wählen Sie dann **Erstellen** aus. Daraufhin wird die Schlüsseltresorbereitstellung gestartet. 

## <a name="manage-keys-and-secrets"></a>Verwalten von Schlüsseln und Geheimnissen

Gehen Sie nach dem Erstellen eines Tresors wie folgt vor, um Schlüssel und Geheimnisse innerhalb des Tresors zu erstellen und zu verwalten.

### <a name="create-a-key"></a>Erstellen eines Schlüssels

1. Melden Sie sich am [Benutzerportal](https://portal.local.azurestack.external) an.  

2. Wählen Sie im Dashboard **Alle Ressourcen** aus, wählen Sie den zuvor erstellten Schlüsseltresor aus, und klicken Sie dann auf die Kachel **Schlüssel**.  

3. Wählen Sie im Bereich **Schlüssel** die Option **Hinzufügen** aus. 

4. Wählen Sie im Bereich **Schlüssel erstellen** in der Liste mit den **Optionen** die gewünschte Schlüsselerstellungsmethode aus. Sie können einen neuen Schlüssel **generieren**, einen vorhandenen Schlüssel **hochladen** oder **Sicherung wiederherstellen** verwenden, um eine Sicherung eines Schlüssels auszuwählen.  

5. Geben Sie einen **Namen** für Ihren Schlüssel ein. Der Schlüsselname darf nur alphanumerische Zeichen und Bindestriche (-) enthalten.  

6. Konfigurieren Sie optional Werte für **Aktivierungsdatum festlegen** und **Ablaufdatum festlegen** für Ihren Schlüssel.  

7. Wählen Sie **Erstellen** aus, um die Bereitstellung zu starten.  

Nach erfolgreicher Erstellung des Schlüssels können Sie ihn unter **Schlüssel** auswählen und seine Eigenschaften anzeigen oder ändern. Der Abschnitt „Eigenschaften“ enthält den **Schlüsselbezeichner**. Hierbei handelt es sich um einen URI (Uniform Resource Identifier), über den externe Anwendungen auf diesen Schlüssel zugreifen können. Konfigurieren Sie Einstellungen unter **Zulässige Vorgänge**, um Vorgänge für diesen Schlüssel einzuschränken.

![URI (Schlüssel)](media/azure-stack-kv-manage-portal/image4.png)  

### <a name="create-a-secret"></a>Erstellen eines Geheimnisses 

1. Melden Sie sich am [Benutzerportal](https://portal.local.azurestack.external) an.  
2. Wählen Sie im Dashboard **Alle Ressourcen** aus, wählen Sie den zuvor erstellten Schlüsseltresor aus, und klicken Sie dann auf die Kachel **Geheimnisse**.  

3. Wählen Sie unter **Geheimnisse** die Option **Hinzufügen** aus.  

4. Wählen Sie unter **Geheimnis erstellen** in der Liste mit den **Uploadoptionen** die gewünschte Option für die Geheimniserstellung aus. Sie können ein Geheimnis **manuell** erstellen, indem Sie einen Wert für das Geheimnis eingeben, oder ein **Zertifikat** von Ihrem lokalen Computer hochladen.  

5. Geben Sie einen **Namen** für das Geheimnis ein. Der Geheimnisname darf nur alphanumerische Zeichen und Bindestriche (-) enthalten.  

6. Geben Sie optional den **Inhaltstyp** an, und konfigurieren Sie Werte für **Aktivierungsdatum festlegen** und **Ablaufdatum festlegen** für das Geheimnis.  

7. Wählen Sie **Erstellen** aus, um die Bereitstellung zu starten.  

Nach erfolgreicher Erstellung des Geheimnisses können Sie es unter **Geheimnisse** auswählen und seine Eigenschaften anzeigen oder ändern. Der Abschnitt „Eigenschaften“ enthält die **Geheimnis-ID**. Hierbei handelt es sich um einen URI, über den externe Anwendungen auf dieses Geheimnis zugreifen können. 

![URI (Geheimnis)](media/azure-stack-kv-manage-portal/image5.png) 


## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines virtuellen Computers durch Abrufen des in Key Vault gespeicherten Kennworts](azure-stack-kv-deploy-vm-with-secret.md) 
* [Create a virtual machine and include certificate retrieved from a key vault](azure-stack-kv-push-secret-into-vm.md) (Erstellen eines virtuellen Computers und Einbeziehen eines Zertifikats aus einem Schlüsseltresor)     


