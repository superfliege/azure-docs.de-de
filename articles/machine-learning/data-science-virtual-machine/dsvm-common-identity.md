---
title: Einrichten einer gemeinsamen Identität für den virtuellen Computer für Data Science – Azure | Microsoft-Dokumentation
description: Richten Sie eine gemeinsame Identität in den DSVM-Umgebungen des Unternehmensteams ein.
keywords: Deep Learning, KI, Data Science-Tools, Data Science-VM, räumliche Analysen, Data Science-Teamprozess
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 70c6b8cd147cefaa3128bc1e6a414a6fa61dba6d
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830893"
---
# <a name="setup-common-identity-on-the-data-science-vm"></a>Einrichten einer gemeinsamen Identität für die Data Science-VM

Standardmäßig werden auf der Azure-VM, die die Data Science-VM (DSVM) enthält, lokale Benutzerkonten bei der Bereitstellung der VM erstellt, und die Benutzer authentifizieren sich bei der VM mit diesen Anmeldeinformationen. Wenn Sie mehrere VMs haben, auf die Sie zugreifen müssen, kann bei diesem Ansatz die Verwaltung der Anmeldeinformationen schnell mühsam werden. Durch gemeinsame Benutzerkonten und deren Verwaltung mit einem standardbasierten Identitätsanbieter können Sie einen einzelnen Satz von Anmeldeinformationen verwenden, um auf mehrere Ressourcen in Azure, einschließlich mehrerer DSVMs, zuzugreifen. 

Active Directory (AD) ist ein beliebter Identitätsanbieter und wird als Dienst in Azure und lokal unterstützt. Sie können AD oder Azure AD zum Authentifizieren von Benutzern einer eigenständigen Data Science-VM (DSVM) oder in einem Cluster mit DSVMs in einer Azure-VM-Skalierungsgruppe nutzen. Dazu werden die DSVM-Instanzen zu einer AD-Domäne hinzugefügt. Wenn Sie bereits über eine Active Directory-Instanz zum Verwalten von Identitäten verfügen, können Sie sie als gemeinsamen Identitätsanbieter verwenden. Für den Fall, dass Sie nicht über eine AD-Instanz verfügen, können Sie eine verwaltete AD-Instanz in Azure über einen Dienst namens [Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/)(Azure AD DS) ausführen. 

Die Dokumentation für [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/) bietet detaillierte [Anweisungen](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution#synchronized-identity) für verwaltete Active Directory-Instanzen, einschließlich der Verbindung von Azure AD mit Ihrem lokalen Verzeichnis, sofern vorhanden. 

Der restliche Artikel beschreibt die Schritte zum Einrichten eines vollständig verwalteten AD-Domänendiensts in Azure mithilfe von Azure AD DS und zum Verknüpfen Ihrer DSVMs mit der verwalteten AD-Domäne, um Benutzern über ein gemeinsames Benutzerkonto und gemeinsame Anmeldeinformationen Zugriff auf einen Pool von DSVMs (und andere Azure-Ressourcen) zu ermöglichen. 

##  <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Einrichten einer vollständig verwalteten Active Directory-Domäne in Azure

Azure AD DS vereinfacht die Verwaltung Ihrer Identitäten, indem Ihnen ein vollständig verwalteter Dienst in Azure bereitgestellt wird. In dieser Active Directory-Domäne werden Benutzer und Gruppen verwaltet.  Die Schritte zum Einrichten einer von Azure gehosteten AD-Domäne und der Benutzerkonten in Ihrem Verzeichnis sind:

1. Hinzufügen von Benutzern zu Active Directory im Portal 

    a. Melden Sie sich beim [Azure Active Directory Admin Center](https://aad.portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
    
    b. Wählen Sie **Azure Active Directory** und dann **Benutzer und Gruppen** aus.
    
    c. Wählen Sie unter **Benutzer und Gruppen** die Option **Alle Benutzer** und dann **Neuer Benutzer** aus.
   
   ![Auswählen des Befehls „Hinzufügen“](./media/add-user.png)
    
    d. Geben Sie Details zum Benutzer ein, beispielsweise **Name** und **Benutzername**. Der Domänennamensteil des Benutzernamens muss entweder der anfängliche Standarddomänenname [Domänenname].onmicrosoft.com oder ein überprüfter, nicht im Verbund konfigurierter [benutzerdefinierter Domänenname](../../active-directory/add-custom-domain.md) wie z.B. contoso.com sein.
    
    e. Kopieren oder notieren Sie sich das generierte Benutzerkennwort, damit sie es nach Abschluss des Vorgangs dem Benutzer mitteilen können.
    
    f. Optional können Sie die Informationen unter **Profil**, **Gruppen** oder **Verzeichnisrolle** für den Benutzer ausfüllen. 
    
    g. Wählen Sie unter **Benutzer** die Option **Erstellen** aus.
    
    h. Sorgen Sie für eine sichere Übermittlung des generierten Kennworts an den neuen Benutzer, damit sich dieser anmelden kann.

2.  Erstellen von Azure AD Domain Services

    Folgen Sie zum Erstellen einer Azure AD DS-Instanz den Anweisungen im Artikel [Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) (Aufgabe 1 bis 5). Es ist wichtig, die vorhandenen Benutzerkennwörter in Active Directory zu aktualisierten, damit das Kennwort in Azure AD DS synchronisiert wird. Es ist auch wichtig, DNS zu Azure AD DS hinzuzufügen, wie in Aufgabe 4 des oben genannten Artikels angegeben. 

3.  Erstellen Sie ein separates DSVM-Subnetz im virtuellen Netzwerk, das in Aufgabe 2 des vorherigen Schritts erstellt wird.
4.  Erstellen Sie eine oder mehrere Data Science-VM-Instanzen im DSVM-Subnetz. 
5.  Fügen Sie mit diesen [Anweisungen](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) die DSVM zu AD hinzu. 
6.  Stellen Sie als Nächstes eine Azure Files-Freigabe zum Hosten Ihres Basis- oder Notebook-Verzeichnisses bereit, damit Ihr Arbeitsbereich auf jedem Computer eingebunden werden kann. (Wenn Sie strenge Berechtigungen auf Dateiebene benötigen, muss NFS auf einer oder mehreren VMs ausgeführt werden.)

    a. [Erstellen Sie eine Azure-Dateifreigabe](../../storage/files/storage-how-to-create-file-share.md).
    
    b. Stellen Sie sie auf der Linux-DSVM bereit. Wenn Sie auf die Schaltfläche „Verbinden“ für die Azure Files-Instanz in Ihrem Speicherkonto im Azure-Portal klicken, wird der Befehl angezeigt, der in der Bash-Shell auf der Linux-DSVM ausgeführt werden soll. Der Befehl sieht wie folgt aus:
```
sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
```
7.  Sie haben Azure Files z.B. in „/data/workspace“ eingebunden. Erstellen Sie jetzt Verzeichnisse für jeden Benutzer in der Freigabe. „/data/workspace/user1“, „/data/workspace/user2“ usw. Erstellen Sie ein ```notebooks```-Verzeichnis im Arbeitsbereich der einzelnen Benutzer. 
8. Erstellen Sie symbolische Verknüpfungen für ```notebooks``` in ```$HOME/userx/notebooks/remote```.   

Jetzt sind die Benutzer in Ihrer in Azure gehosteten Active Directory-Instanz und können sich mithilfe der AD-Anmeldeinformationen bei allen DSVMs (SSH, JupyterHub) anmelden, die zu Azure AD DS gehören. Da sich der Benutzerarbeitsbereich in einer Azure Files-Freigabe befindet, hat der Benutzer über jede DSVM mithilfe von JupyterHub Zugriff auf seine Notebooks und auf seine sonstige Arbeit. 

Für die automatische Skalierung können Sie die VM-Skalierungsgruppe verwenden, um einen Pool von VMs zu erstellen, die alle auf diese Weise zur Domäne hinzugefügt werden und bei denen der freigegebene Datenträger eingebunden ist. Benutzer können sich an jedem verfügbaren Computer in der VM-Skalierungsgruppe anmelden und haben Zugriff auf den freigegebenen Datenträger, auf dem ihre Notebooks gespeichert sind. 

## <a name="next-steps"></a>Nächste Schritte

* [Sicheres Speichern von Anmeldeinformationen für den Zugriff auf Cloudressourcen](dsvm-secure-access-keys.md)



