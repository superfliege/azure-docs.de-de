Die folgenden Beschränkungen gelten für Azure Backup.

| Limit-Bezeichner | Standardlimit |
| --- | --- |
| Anzahl der Server/Computer, die für jeden Tresor registriert werden kann |für Windows Server/Client/SCDPM 50 <br/> 200 für IaaS-VMs |
| Größe der Datenquelle für Daten, die in Azure-tresorspeicher gespeichert |Max. 54400 GB<sup>1</sup> |
| Anzahl der backup-Tresoren, die in jeder Azure-Abonnement erstellt werden können |25 (sicherungstresore) <br/> 25 Recovery Services-Tresor pro region |
| Anzahl der Häufigkeit, mit der Sicherung pro Tag eingeplant werden kann |3 pro Tag für Windows Server und Client <br/> 2 pro Tag für SCDPM <br/> Einmal pro Tag für IaaS-VMs |
| Datenträger einem virtuellen Computer in Azure für die Sicherung zugeordnet |16 |

* <sup>1</sup>der 54400-GB-Grenze gilt nicht für IaaS-VM-Sicherungen.

