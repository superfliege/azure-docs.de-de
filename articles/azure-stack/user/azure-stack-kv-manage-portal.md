---
title: Verwalten von Key Vault in Azure Stack mithilfe von PowerShell | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie Key Vault in Azure Stack mithilfe von PowerShell verwalten.
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
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 41cbe1526368dd88fe98f92937c6ef2b65f24682
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="manage-key-vault-in-azure-stack-using-the-portal"></a>Verwalten von Key Vault in Azure Stack über das Portal

Sie können Key Vault in Azure Stack über das Azure Stack-Portal verwalten. In diesem Artikel erfahren Sie, wie Sie Key Vault in Azure Stack erstellen und verwalten. 

## <a name="prerequisites"></a>Voraussetzungen  

* Sie müssen ein Angebot abonnieren, das den Key Vault-Dienst umfasst.  
 
## <a name="create-a-key-vault"></a>Erstellen eines Schlüsseltresors 

1. Melden Sie sich beim Benutzerportal (https://portal.local.azurestack.external) an.  

2. Klicken Sie auf dem Dashboard auf **Neu > Sicherheit und Identität > Key Vault**.  

    ![Key Vault-Bildschirm](media/azure-stack-kv-manage-portal/image1.png)  

3. Weisen Sie Ihrem Tresor auf dem Blatt **Schlüsseltresor erstellen** unter **Name** einen Namen zu. Der Tresorname darf nur alphanumerische Zeichen und Bindestriche (-) enthalten, und er darf nicht mit einer Zahl beginnen.  

4. Wählen Sie aus der Liste der verfügbaren Abonnements ein **Abonnement** aus. Die Dropdownliste enthält alle Abonnements, für die der Key Vault-Dienst zur Verfügung steht.  

5. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine neue.  

6. Wählen Sie den **Tarif** aus.  
    >[!NOTE]
    > Schlüsseltresore im Azure Stack Development Kit unterstützen nur die SKU **Standard**.

7. Wählen Sie eine vorhandene **Zugriffsrichtlinie** aus, oder erstellen Sie eine neue. Mithilfe einer Zugriffsrichtlinie können Sie einen Benutzer, eine Anwendung oder eine Sicherheitsgruppe dazu berechtigen, Vorgänge mit diesem Tresor auszuführen.  

8. Wählen Sie optional eine **erweiterte Zugriffsrichtlinie** aus, um etwa den Zugriff auf Virtual Machines (für die Bereitstellung), auf Resource Manager (für die Vorlagenbereitstellung) und auf Azure Disk Encryption (für die Volumeverschlüsselung) zu ermöglichen. 
  
9.  Klicken Sie nach dem Konfigurieren der Einstellungen auf **OK** und anschließend auf **Erstellen**. Daraufhin wird die Schlüsseltresorbereitstellung gestartet. 

## <a name="manage-keys-and-secrets"></a>Verwalten von Schlüsseln und Geheimnissen

Gehen Sie nach dem Erstellen eines Tresors wie folgt vor, um Schlüssel und Geheimnisse innerhalb des Tresors zu erstellen und zu verwalten.

## <a name="create-a-key"></a>Erstellen eines Schlüssels

1. Melden Sie sich beim Benutzerportal (https://portal.local.azurestack.external) an.  

2. Klicken Sie auf dem Dashboard auf **Alle Ressourcen**, wählen Sie den zuvor erstellten Schlüsseltresor aus, und klicken Sie auf die Kachel **Schlüssel**.  

3. Klicken Sie auf dem Blatt **Schlüssel** auf **Hinzufügen**. 

4. Wählen Sie auf dem Blatt **Schlüssel erstellen** in der Liste mit den**** Optionen die gewünschte Schlüsselerstellungsmethode aus. Sie können einen neuen Schlüssel**** generieren, einen vorhandenen Schlüssel**** hochladen oder einen gesicherten Schlüssel**** wiederherstellen.  

5. Geben Sie einen**** Namen für Ihren Schlüssel ein. Der Schlüsselname darf nur alphanumerische Zeichen und Bindestriche (-) enthalten.  

6. Konfigurieren Sie optional Werte für **Aktivierungsdatum festlegen** und **Ablaufdatum festlegen** für Ihren Schlüssel.  

7. Klicken Sie auf **Erstellen**, um die Bereitstellung zu starten.  

Nach erfolgreicher Erstellung des Schlüssels können Sie ihn auf dem Blatt **Schlüssel** auswählen und seine Eigenschaften anzeigen oder ändern. Der Abschnitt mit den Eigenschaften enthält den **Schlüsselbezeichner**. Hierbei handelt es sich um einen URI, über den externe Anwendungen auf diesen Schlüssel zugreifen können. Konfigurieren Sie Einstellungen unter **Zulässige Vorgänge**, um Vorgänge für diesen Schlüssel einzuschränken.

![URI (Schlüssel)](media/azure-stack-kv-manage-portal/image4.png)  

## <a name="create-a-secret"></a>Erstellen eines Geheimnisses 

1. Melden Sie sich beim Benutzerportal (https://portal.local.azurestack.external) an.  
2. Klicken Sie auf dem Dashboard auf **Alle Ressourcen**, wählen Sie den zuvor erstellten Schlüsseltresor aus, und klicken Sie auf die Kachel **Geheimnisse**.  

3. Klicken Sie auf dem Blatt **Geheimnisse** auf **Hinzufügen**.  

4. Wählen Sie auf dem Blatt **Geheimnis erstellen** in der Liste mit**** Uploadoptionen die gewünschte Option für die Geheimniserstellung aus. Sie können ein Geheimnis**** manuell erstellen, indem Sie einen Wert für das Geheimnis eingeben, oder aber ein **Zertifikat** von Ihrem lokalen Computer hochladen.  

5. Geben Sie einen Namen**** für das Geheimnis ein. Der Geheimnisname darf nur alphanumerische Zeichen und Bindestriche (-) enthalten.  

6. Geben Sie optional den **Inhaltstyp** an, und konfigurieren Sie Werte für **Aktivierungsdatum festlegen** und **Ablaufdatum festlegen** für das Geheimnis.  

7. Klicken Sie auf „Erstellen“, um die Bereitstellung zu starten.  

Nach erfolgreicher Erstellung des Geheimnisses können Sie es auf dem Blatt **Geheimnisse** auswählen und seine Eigenschaften anzeigen oder ändern. Der Abschnitt mit den Eigenschaften enthält die **Geheimnis-ID**. Hierbei handelt es sich um einen URI, über den externe Anwendungen auf dieses Geheimnis zugreifen können. 

![URI (Geheimnis)](media/azure-stack-kv-manage-portal/image5.png) 


## <a name="next-steps"></a>Nächste Schritte
* [Create a virtual machine by retrieving the password stored in a Key Vault](azure-stack-kv-deploy-vm-with-secret.md) (Erstellen eines virtuellen Computers durch Abrufen des gespeicherten Kennworts aus einem Schlüsseltresor)  
* [Create a virtual machine and include certificate retrieved from a key vault](azure-stack-kv-push-secret-into-vm.md) (Erstellen eines virtuellen Computers und Einbeziehen eines Zertifikats aus einem Schlüsseltresor)     



