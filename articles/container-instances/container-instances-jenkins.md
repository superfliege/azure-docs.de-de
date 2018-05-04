---
title: Verwenden von Azure Container Instances als Jenkins-Build-Agent
description: Hier erfahren Sie, wie Sie Azure Container Instances als Jenkins-Build-Agent verwenden.
services: container-instances
author: neilpeterson
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/20/2018
ms.author: nepeters
ms.openlocfilehash: dbe418db8fb3d73739a30b60703f15b3dc47b291
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="use-azure-container-instances-as-a-jenkins-build-agent"></a>Verwenden von Azure Container Instances als Jenkins-Build-Agent

Mit Azure Container Instances steht Ihnen eine bedarfsgesteuerte, burstfähige und isolierte Umgebung zum Ausführen von containerbasierten Workloads zur Verfügung. Aufgrund dieser Attribute ist Azure Container Instances eine hervorragende Plattform für die Ausführung von Jenkins-Buildaufträgen in großem Umfang. In diesem Dokument werden ausführlich die Bereitstellung und Verwendung eines Jenkins-Servers erläutert, der mit ACI als Buildziel vorkonfiguriert wurde.

Weitere Informationen zu Azure Container Instances finden Sie unter [Azure Container Instances][about-aci].

## <a name="deploy-jenkins-server"></a>Bereitstellen eines Jenkins-Servers

Klicken Sie im Azure-Portal auf **Ressource erstellen**, und suchen Sie nach **Jenkins**. Wählen Sie das Jenkins-Angebot mit dem Herausgeber **Microsoft** aus, und klicken Sie auf **Erstellen**.

Geben Sie im Formular für grundlegende Angaben die folgenden Informationen ein, und klicken Sie anschließend auf **OK**.

- **Name**: Der Name für die Jenkins-Bereitstellung
- **Benutzername**: Dieser Benutzername wird als Administratorbenutzer für den virtuellen Jenkins-Computer verwendet.
- Für **Authentifizierungstyp** wird „Öffentlicher SSH-Schlüssel“ empfohlen. Wenn Sie diese Option ausgewählt haben, fügen Sie einen öffentlichen SSH-Schlüssel ein, der zum Anmelden beim virtuellen Jenkins-Computer verwendet werden soll.
- **Abonnement**: Wählen Sie ein Azure-Abonnement aus.
- **Ressourcengruppe**: Erstellen Sie eine neue Ressourcengruppe, oder wählen Sie eine vorhandene aus.
- **Standort**: Wählen Sie einen Standort für den Jenkins-Server aus.

![Grundlegende Einstellungen für die Jenkins-Bereitstellung im Portal](./media/container-instances-jenkins/jenkins-portal-01.png)

Geben Sie im Formular „Zusätzliche Einstellungen“ die folgenden Informationen ein:

- **Größe**: Wählen Sie die geeignete Option für die Größenanpassung für den virtuellen Jenkins-Computer aus.
- **VM-Datenträgertyp**: Geben Sie für den Jenkins-Server entweder „HDD“ (Festplattenlaufwerk) oder „SSD“ (Solid State Drive) an.
- **Virtuelles Netzwerk**: (Optional) Wählen Sie „Virtuelles Netzwerk“ aus, um die Standardeinstellungen zu ändern.
- **Subnetze**: Wählen Sie „Subnetze“ aus, überprüfen Sie die Informationen, und klicken Sie auf **OK**.
- **Öffentliche IP-Adresse**: Wenn Sie die Option für die öffentliche IP-Adresse auswählen, können Sie einen benutzerdefinierten Namen festlegen sowie die SKU und eine Zuweisungsmethode konfigurieren.
- **Domänennamenbezeichnung**: Geben Sie einen Wert an, um eine vollqualifizierte URL zum virtuellen Jenkins-Computer zu erstellen.
- **Jenkins-Releasetyp**: Wählen Sie den gewünschten Releasetyp aus den Optionen aus: „LTS“, „Weekly build“ (Wöchentlicher Build) oder „Azure Verified“ (Von Azure verifiziert).

![Zusätzliche Einstellungen für die Jenkins-Bereitstellung im Portal](./media/container-instances-jenkins/jenkins-portal-02.png)

Wählen Sie für die Dienstprinzipalintegration **Auto(MSI)** aus, damit die [verwaltete Azure-Dienstidentität][managed-service-identity] (Azure Managed Service Identity, MSI) automatisch eine Authentifizierungsidentität für die Jenkins-Instanz erstellt. Wählen Sie „Manuell“ aus, um Ihre eigenen Dienstprinzipal-Anmeldeinformationen anzugeben.

Cloud-Agents konfigurieren eine cloudbasierte Plattform für Jenkins-Buildaufträge. Wählen Sie in diesem Dokument „ACI“ aus. Mit dem ACI-Cloud-Agent wird jeder Jenkins-Buildauftrag in einer Azure Container Instances-Instanz ausgeführt.

![Cloudintegrationseinstellungen für die Jenkins-Bereitstellung im Portal](./media/container-instances-jenkins/jenkins-portal-03.png)

Nachdem Sie die Integrationseinstellungen angegeben haben, klicken Sie auf **OK** und anschließend in der Validierungszusammenfassung erneut auf **OK**. Klicken Sie in der Zusammenfassung der Nutzungsbedingungen auf **Erstellen**. Die Bereitstellung des Jenkins-Servers dauert einige Minuten.

## <a name="configure-jenkins"></a>Konfigurieren von Jenkins

Navigieren Sie im Azure-Portal zur Jenkins-Ressourcengruppe, wählen Sie den virtuellen Jenkins-Computer aus, und notieren Sie sich den DNS-Namen.

![Jenkins-Anmeldeanweisungen](./media/container-instances-jenkins/jenkins-portal-fqdn.png)

Navigieren Sie zum DNS-Namen des virtuellen Jenkins-Computers, und kopieren Sie die zurückgegebene SSH-Zeichenfolge.

![Jenkins-Anmeldeanweisungen](./media/container-instances-jenkins/jenkins-portal-04.png)

Öffnen Sie eine Terminalsitzung auf Ihrem Bereitstellungssystem, und fügen Sie die SSH-Zeichenfolge aus dem letzten Schritt ein. Verwenden Sie den beim Bereitstellen des Jenkins-Servers angegebenen Benutzernamen.

Führen Sie nach der Verbindungsherstellung den folgenden Befehl aus, um das anfängliche Administratorkennwort abzurufen.

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Navigieren Sie in einem Browser zu http://localhost:8080, ohne die SSH-Sitzung und den Tunnel zu schließen. Fügen Sie wie in der folgenden Abbildung gezeigt das anfängliche Administratorkennwort in das entsprechende Feld ein. Klicken Sie anschließend auf **Weiter**.

![Entsperren von Jenkins](./media/container-instances-jenkins/jenkins-portal-05.png)

Klicken Sie auf **Install suggested plugins** (Vorgeschlagene Plug-Ins installieren), um alle empfohlenen Jenkins-Plug-Ins zu installieren.

![Installieren von Jenkins-Plug-Ins](./media/container-instances-jenkins/jenkins-portal-06.png)

Erstellen Sie ein neues Administratorbenutzerkonto. Dieses Konto wird zum Anmelden bei und Arbeiten mit der Jenkins-Instanz verwendet.

![Erstellen eines Jenkins-Benutzers](./media/container-instances-jenkins/jenkins-portal-07.png)

Wenn Sie fertig sind, klicken Sie auf **Save and Finish** (Speichern und beenden) und dann auf **Start using Jenkins** (Jenkins verwenden), um die Konfiguration abzuschließen.

Jenkins ist jetzt konfiguriert und kann zum Erstellen und Bereitstellen von Code verwendet werden. In diesem Beispiel wird anhand einer einfachen Java-Anwendung ein Jenkins-Build in Azure Container Instances veranschaulicht.

## <a name="create-build-job"></a>Erstellen eines Build-Auftrags

Bei Verwendung eines Containerimages als Jenkins-Buildziel müssen Sie ein Image angeben, das alle für einen erfolgreichen Buildvorgang erforderlichen Tools enthält. Klicken Sie zum Angeben des Images auf **Manage Jenkins** (Jenkins verwalten)  > **Configure System** (System konfigurieren), und scrollen Sie nach unten zum Abschnitt **Cloud**. Ändern Sie in diesem Beispiel den Wert des Docker-Images in `microsoft/java-on-azure-jenkins-slave`.

Klicken Sie anschließend auf **Save** (Speichern), um zum Jenkins-Dashboard zurückzukehren.

![Jenkins-Cloudkonfiguration](./media/container-instances-jenkins/jenkins-aci-image.png)

Erstellen Sie nun einen Jenkins-Buildauftrag. Klicken Sie auf **New Item** (Neues Element), und geben Sie einen Namen für das Buildprojekt ein (etwa `aci-java-demo`). Wählen Sie **Freestyle project** (Freestyle-Projekt) aus, und klicken Sie auf **OK**.

![Erstellen eines Jenkins-Auftrags](./media/container-instances-jenkins/jenkins-new-job.png)

Vergewissern Sie sich, dass unter **General** (Allgemein) die Option **Restrict where this project can be run** (Ausführungsort dieses Projekts beschränken) aktiviert ist. Geben Sie als Bezeichnungsausdruck `linux` ein. Mit dieser Konfiguration wird sichergestellt, dass dieser Buildauftrag in der ACI-Cloud ausgeführt wird.

![Erstellen eines Jenkins-Auftrags](./media/container-instances-jenkins/jenkins-job-01.png)

Wählen Sie unter „Source Code Management“ (Quellcodeverwaltung) die Option `Git` aus, und geben Sie als Repository-URL `https://github.com/spring-projects/spring-petclinic.git` ein. Dieses GitHub-Repository enthält den Beispielanwendungscode.

![Hinzufügen von Quellcode zum Jenkins-Auftrag](./media/container-instances-jenkins/jenkins-job-02.png)

Klicken Sie unter „Build“ auf **Add build step** (Buildschritt hinzufügen), und wählen Sie `Invoke top-level Maven targets` aus. Geben Sie als Ziel für den Buildschritt `package` ein.

![Hinzufügen eines Jenkins-Buildschritts](./media/container-instances-jenkins/jenkins-job-03.png)

Wenn Sie abschließend **Speichern** aus.

## <a name="run-the-build-job"></a>Ausführen des Buildauftrags

Starten Sie einen Build manuell, um den Buildauftrag zu testen und sich mit Azure Container Instances als Buildplattform vertraut zu machen.

Klicken Sie auf **Build Now** (Jetzt erstellen), um einen Buildauftrag zu starten. Es dauert einige Minuten, bis der Auftrag gestartet wird. Wenn er ausgeführt wird, sollte ein ähnlicher Status wie in der folgenden Abbildung angezeigt werden:

![Hinzufügen eines Jenkins-Buildschritts](./media/container-instances-jenkins/jenkins-job-status.png)

Öffnen Sie während der Auftragsausführung das Azure-Portal, und sehen Sie sich die Jenkins-Ressourcengruppe an. Sie sollten sehen, dass eine Azure Container Instances-Instanz erstellt wurde. Der Jenkins-Auftrag wird in dieser Instanz ausgeführt.

![Jenkins-Builds in ACI](./media/container-instances-jenkins/jenkins-aci.png)

Da Jenkins mehr Aufträge als die konfigurierte Anzahl von Jenkins-Executors (Standard: 2) ausführt, werden mehrere Azure Container Instances-Instanzen erstellt.

![Jenkins-Builds in ACI](./media/container-instances-jenkins/jenkins-aci-multi.png)

Nach Abschluss aller Buildaufträge, werden die Azure Container Instances-Instanzen entfernt.

![Jenkins-Builds in ACI](./media/container-instances-jenkins/jenkins-aci-none.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Jenkins in Azure finden Sie unter [Azure und Jenkins][jenkins-azure].

<!-- LINKS - internal -->
[about-aci]: ./container-instances-overview.md
[jenkins-azure]: ../jenkins/overview.md
[managed-service-identity]: ../active-directory/managed-service-identity/overview.md