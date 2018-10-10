---
title: Includedatei
description: Includedatei
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/27/2018
ms.openlocfilehash: e86f2cc9fd2da585ad13b088442ebef9b55f186a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2018
ms.locfileid: "47010732"
---
Erstellen Sie im selben Verzeichnis wie **aml_config/config.json** ein Skript mit dem Namen **pi.py**.

Kopieren Sie den folgenden Code in dieses Skript:
    
   ```python
   import random, math
   from azureml.core import Run
    
   run = Run.get_submitted_run()
    
   pi_counter = 0
   n_iter = 100000
   run.log("Number of iterations",n_iter)
    
   for i in range(1,n_iter):
       x = random.random()
       y = random.random()
       if x*x + y*y < 1.0:
           pi_counter += 1
       pi_value = 4.0*pi_counter / i
       if i%10000==0:
           error = math.pi-pi_value
           print(i, pi_value, error)
           run.log_row("Pi estimate",iteration=i,pi_value=pi_value)
           run.log_row("Error",iteration=i,error=error)
   ```

Beachten Sie die `log_row`-Aufrufe am Ende.  Nachdem Sie dieses Skript ausgeführt haben, werden diese Werte in Ihrem Arbeitsbereich angezeigt.