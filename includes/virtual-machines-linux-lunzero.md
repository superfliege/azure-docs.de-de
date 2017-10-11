Beim Hinzufügen von Datenträgern in einer Linux-VM können Fehler auftreten, wenn ein Datenträger mit LUN 0 nicht vorhanden ist. Wenn Sie einen Datenträger, indem Sie manuell hinzufügen der `azure vm disk attach-new` -Befehl an, und legen Sie eine LUN (`--lun`) anstatt ermöglicht der Azure-Plattform die entsprechenden LUN zu bestimmen, darauf achten, dass ein Datenträger bereits vorhanden ist / sind unter LUN 0 vorhanden. 

Das folgende Beispiel zeigt einen Ausschnitt der Ausgabe `lsscsi`:

```bash
[5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc 
[5:0:0:1]    disk    Msft     Virtual Disk     1.0   /dev/sdd 
```

Zwei Datenträger vorhanden, auf die LUN 0 "und" LUN 1 (die erste Spalte in der `lsscsi` Ausgabe Details `[host:channel:target:lun]`). Beide Festplatten sollte Accessbile von innerhalb des virtuellen Computers. Wenn Sie manuell den ersten Datenträger an LUN 1 hinzugefügt werden und den zweiten Datenträger an LUN 2 angegeben hatten, möglicherweise nicht die Datenträger ordnungsgemäß innerhalb Ihres virtuellen Computers angezeigt werden.

> [!NOTE]
> Azure `host` Wert ist 5 in diesen Beispielen, aber dies kann variieren, je nach den Typ des Speichers, die Sie auswählen.
> 
> 

Dieses Verhalten Datenträger ist nicht an ein Azure-Problem, doch die Möglichkeit, in der die Linux-Kernel die SCSI-Spezifikationen folgt. Bei der Linux-Kernel den SCSI-Bus auf angeschlossene Geräte überprüft, muss ein Gerät unter LUN 0 in der Reihenfolge für das System den Scanvorgang für zusätzliche Geräte fortzusetzen gefunden werden. Als solche:

* Überprüfen Sie die Ausgabe des `lsscsi` nach dem Hinzufügen eines Datenträgers, um sicherzustellen, dass Sie einen Datenträger mit LUN 0 haben.
* Wenn die Festplatte nicht ordnungsgemäß innerhalb Ihres virtuellen Computers angezeigt wird, stellen Sie sicher, dass ein Datenträger mit LUN 0 vorhanden ist.

