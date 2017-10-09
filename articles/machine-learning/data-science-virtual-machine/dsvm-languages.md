---
title: "Sprachen für die Data Science-VM in Azure | Microsoft-Dokumentation"
description: "Sprachen für die Data Science-VM in Azure"
keywords: "Data Science-Tools, virtuelle Computer für Data Science, Tools für Data Science, Linux Data Science"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: gokuma;bradsev
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 2f2125e739b738847e03ce429d65801969611685
ms.contentlocale: de-de
ms.lasthandoff: 09/25/2017

---

# <a name="languages-supported-on-the-data-science-virtual-machine"></a>Unterstützte Sprachen der Data Science-VM 

Die Data Science-VM (DSVM) verfügt über mehrere vorgefertigte Sprachen und Entwicklungstools für die Erstellung von KI-Anwendungen. In Anschluss werden einige der wichtigsten aufgeführt. 

## <a name="python"></a>Python

|    |           |
| ------------- | ------------- |
| Unterstützte Sprachversionen | 2.7 und 3.5 |
| Unterstützte DSVM-Editionen      | Linux, Windows     |
| Konfiguration/Installation auf der DSVM  | Es werden zwei globale `conda`-Umgebungen erstellt. <br /> *  Die `root`-Umgebung unter `/anaconda/` ist Python 2.7. <br/> *  Die `py35`-Umgebung unter `/anaconda/envs/py35` ist Python 3.5.       |
| Links zu Beispielen      | Jupyter-Beispielnotebooks für Python sind enthalten.     |
| Verwandte Tools auf der DSVM      | PySpark, R, Julia      |
### <a name="how-to-use--run-it"></a>Verwendung/Ausführung    

**Windows:**

* Ausführen an der Eingabeaufforderung

Öffnen Sie eine Eingabeaufforderung, und gehen Sie abhängig von der gewünschten Python-Version wie folgt vor: 

```
# To run Python 2.7
activate 
python --version

# To run Python 3.5
activate py35
python --version

```
* Verwenden in einer IDE

Verwenden Sie Python Tools für Visual Studio (PTVS) aus Visual Studio Community Edition. In PTVS wird nur die Python 2.7-Umgebung automatisch eingerichtet. 
> [!NOTE]
> Wenn Sie PTVS auf Python 3.5 verweisen möchten, müssen Sie in PTVS eine benutzerdefinierte Umgebung erstellen. Navigieren Sie zum Erstellen dieses Umgebungspfads in Visual Studio Community Edition zu **Tools** -> **Python-Tools** -> **Python-Umgebungen**, und klicken Sie auf **+ Benutzerdefiniert**. Legen Sie den Speicherort auf `c:\anaconda\envs\py35` fest, und klicken Sie anschließend auf _Automatisch erkennen_. 

* Verwenden in Jupyter

Öffnen Sie Jupyter, und klicken Sie auf die Schaltfläche `New`, um ein neues Notebook zu erstellen. Daraufhin können Sie für den Kerneltyp entweder _Python [Conda Root]_ (Python 2.7-Umgebung) oder _Python [Conda env:py35]_ (Python 3.5-Umgebung) auswählen. 

* Installieren von Python-Paketen

Die Python-Standardumgebungen auf der DSVM sind globale, für alle Benutzer lesbare Umgebungen. Globale Pakete können allerdings nur von Administratoren geschrieben/installiert werden. Wenn Sie ein Paket in der globalen Umgebung installieren möchten, müssen Sie den Befehl `activate` als Administrator ausführen, um die root- oder die py35-Umgebung zu aktivieren. Anschließend können Sie Pakete mithilfe eines Paket-Managers wie `conda` oder `pip` installieren oder aktualisieren. 


**Linux:**

* Ausführen im Terminal

Öffnen Sie das Terminal, und gehen Sie abhängig von der gewünschten Python-Version wie folgt vor: 

```
# To run Python 2.7
source activate 
python --version

# To run Python 3.5
source activate py35
python --version

```
* Verwenden in einer IDE

Verwenden Sie PyCharm aus Visual Studio Community Edition. 

* Verwenden in Jupyter

Öffnen Sie Jupyter, und klicken Sie auf die Schaltfläche `New`, um ein neues Notebook zu erstellen. Daraufhin können Sie für den Kerneltyp entweder _Python [Conda Root]_ (Python 2.7-Umgebung) oder _Python [Conda env:py35]_ (Python 3.5-Umgebung) auswählen. 

* Installieren von Python-Paketen

Die Python-Standardumgebungen auf der DSVM sind globale, für alle Benutzer lesbare Umgebungen. Globale Pakete können allerdings nur von Administratoren geschrieben/installiert werden. Wenn Sie ein Paket in der globalen Umgebung installieren möchten, müssen Sie den Befehl `source activate` als Administrator oder als Benutzer mit sudo-Berechtigung ausführen, um die root- oder die py35-Umgebung zu aktivieren. Anschließend können Sie Pakete mithilfe eines Paket-Managers wie `conda` oder `pip` installieren oder aktualisieren. 

## <a name="r"></a>R

|    |           |
| ------------- | ------------- |
| Unterstützte Sprachversionen | Microsoft R Open 3.x (vollständig mit CRAN-R kompatibel)<br /> Microsoft R Server 9.x Developer Edition (eine skalierbare R-Plattform für Unternehmen)|
| Unterstützte DSVM-Editionen      | Linux, Windows     |
| Konfiguration/Installation auf der DSVM  | Windows: `C:\Program Files\Microsoft\R Server\R_SERVER` <br />Linux: ` /usr/lib64/microsoft-r/3.3/lib64/R`    |
| Links zu Beispielen      | Jupyter-Beispielnotebooks für R sind enthalten.     |
| Verwandte Tools auf der DSVM      | SparkR, Python, Julia      |
### <a name="how-to-use--run-it"></a>Verwendung/Ausführung    

**Windows:**

* Ausführen an der Eingabeaufforderung

Öffnen Sie die Eingabeaufforderung, und geben Sie `R` ein.

* Verwenden in einer IDE

Verwenden Sie RTools für Visual Studio (RTVS) aus Visual Studio Community Edition oder RStudio. Diese stehen im Startmenü oder als Symbol auf dem Desktop zur Verfügung. 

* Verwenden in Jupyter

Öffnen Sie Jupyter, und klicken Sie auf die Schaltfläche `New`, um ein neues Notebook zu erstellen. Nun können Sie den Kerneltyp _R_ auswählen, um den Jupyter R-Kernel (IRKernel) zu verwenden. 

* Installieren von R-Paketen

R wird auf der DSVM in einer globalen, für alle Benutzer lesbaren Umgebung installiert. Globale Pakete können allerdings nur von Administratoren geschrieben/installiert werden. Führen Sie R mit einer der oben genannten Methoden aus, um das Paket in der globalen Umgebung zu installieren. Anschließend können Sie Pakete durch Ausführen des R-Paket-Managers `install.packages()` installieren oder aktualisieren. 

**Linux:**

* Ausführen im Terminal

Öffnen Sie das Terminal, und führen Sie `R` aus.  

* Verwenden in einer IDE

Verwenden Sie RStudio (auf der Linux-DSVM installiert).  

* Verwenden in Jupyter

Öffnen Sie Jupyter, und klicken Sie auf die Schaltfläche `New`, um ein neues Notebook zu erstellen. Nun können Sie den Kerneltyp _R_ auswählen, um den Jupyter R-Kernel (IRKernel) zu verwenden. 

* Installieren von R-Paketen

R wird auf der DSVM in einer globalen, für alle Benutzer lesbaren Umgebung installiert. Globale Pakete können allerdings nur von Administratoren geschrieben/installiert werden. Führen Sie R mit einer der oben genannten Methoden aus, um das Paket in der globalen Umgebung zu installieren. Anschließend können Sie Pakete durch Ausführen des R-Paket-Managers `install.packages()` installieren oder aktualisieren. 


## <a name="julia"></a>Julia

|    |           |
| ------------- | ------------- |
| Unterstützte Sprachversionen | 0.5 |
| Unterstützte DSVM-Editionen      | Linux, Windows     |
| Konfiguration/Installation auf der DSVM  | Windows: Installiert unter `C:\JuliaPro-VERSION`<br /> Linux: Installiert unter `/opt/JuliaPro-VERSION`    |
| Links zu Beispielen      | Jupyter-Beispielnotebooks für Julia sind enthalten.     |
| Verwandte Tools auf der DSVM      | Python, R      |
### <a name="how-to-use--run-it"></a>Verwendung/Ausführung    

**Windows:**

* Ausführen an der Eingabeaufforderung

Öffnen Sie die Eingabeaufforderung, und führen Sie `julia` aus. 
* Verwenden in einer IDE

Verwenden Sie `Juno` – die auf der DSVM installierte und als Desktopverknüpfung verfügbare Julia-IDE.

* Verwenden in Jupyter

Öffnen Sie Jupyter, und klicken Sie auf die Schaltfläche `New`, um ein neues Notebook zu erstellen. Nun können Sie den Kerneltyp `Julia VERSION` auswählen. 

* Installieren von Julia-Paketen

Julia befindet sich standardmäßig in einer globalen, für alle Benutzer lesbaren Umgebung. Globale Pakete können allerdings nur von Administratoren geschrieben/installiert werden. Führen Sie Julia mit einer der oben genannten Methoden aus, um das Paket in der globalen Umgebung zu installieren. Anschließend können Sie mithilfe von Befehlen des Julia-Paket-Managers (beispielsweise `Pkg.add()`) Pakete installieren oder aktualisieren. 


**Linux:**
* Ausführen im Terminal

Öffnen Sie das Terminal, und führen Sie `julia` aus. 
* Verwenden in einer IDE

Verwenden Sie `Juno` – die auf der DSVM installierte und als Verknüpfung im Anwendungsmenü verfügbare Julia-IDE.

* Verwenden in Jupyter

Öffnen Sie Jupyter, und klicken Sie auf die Schaltfläche `New`, um ein neues Notebook zu erstellen. Nun können Sie den Kerneltyp `Julia VERSION` auswählen. 

* Installieren von Julia-Paketen

Julia befindet sich standardmäßig in einer globalen, für alle Benutzer lesbaren Umgebung. Globale Pakete können allerdings nur von Administratoren geschrieben/installiert werden. Führen Sie Julia mit einer der oben genannten Methoden aus, um das Paket in der globalen Umgebung zu installieren. Anschließend können Sie mithilfe von Befehlen des Julia-Paket-Managers (beispielsweise `Pkg.add()`) Pakete installieren oder aktualisieren. 

## <a name="other-languages"></a>Weitere Sprachen

**C#:** Verfügbar unter Windows und erreichbar in Visual Studio Community Edition oder über eine `Developer Command Prompt for Visual Studio` (durch Ausführen des Befehls `csc`). 

**Java:** OpenJDK steht sowohl in der Linux- als auch in der Windows-Edition der DSVM zur Verfügung und ist auf den Pfad festgelegt. Geben Sie zur Verwendung von Java den Befehl `javac` oder `java` an einer Eingabeaufforderung (Windows) bzw. in einer Bash-Shell (Linux) ein. 

**Node.js:** Node.js steht sowohl in der Linux- als auch in der Windows-Edition der DSVM zur Verfügung und ist auf den Pfad festgelegt. Geben Sie zur Verwendung von Node.js den Befehl `node` oder `npm` an einer Eingabeaufforderung (Windows) bzw. in einer Bash-Shell (Linux) ein. Unter Windows werden die Node.js-Tools für Visual Studio als Erweiterung installiert, sodass Ihnen für die Entwicklung Ihrer Node.js-Anwendung eine grafische IDE zur Verfügung steht. 

**F#:** Verfügbar unter Windows und erreichbar in Visual Studio Community Edition oder über eine `Developer Command Prompt for Visual Studio` (durch Ausführen des Befehls `fsc`). 



