<!--author=alkohli last changed: 9/16/15-->


#### <a name="to-cable-your-device-for-power"></a>Um das Gerät zur Stromversorgung zu Verkabeln.
> [!NOTE]
> Beide Gehäuse des StorSimple-Geräts enthalten redundante PCMs an. In jedem Gehäuse müssen die PCMs installiert und verbunden mit verschiedenen Stromquellen, um hohe Verfügbarkeit sicherzustellen.
> 
> 

1. Stellen Sie sicher, dass die Netzschalter an allen PCMs in die Position aus befinden.
2. Auf dem primären Gehäuse die Netzkabel an beiden pcms an. Die Netzkabel sind rot im Diagramm Verkabelung Power unten gekennzeichnet.
3. Stellen Sie sicher, dass auf dem primären Gehäuse die beiden PCMs separate Stromquellen verwendet.
4. Schließen Sie die Netzkabel an die Distribution Units, wie in der stromverkabelung Diagramm gezeigt.
5. Wiederholen Sie die Schritte 2 bis 4 für das ebod-Gehäuse.
6. Schalten Sie das ebod-Gehäuse aus, indem Sie den Netzschalter an jedem PCM in die Position ein umlegen.
7. Stellen Sie sicher, dass das ebod-Gehäuse eingeschaltet ist, von, ob die grünen LEDs auf der Rückseite des ebod-Controllers eingeschaltet sind.
8. Aktivieren Sie auf dem primären Gehäuse, indem jedes pcms in die Position ein.
9. Stellen Sie sicher, dass das System eingeschaltet ist, die gerätecontrollers die LEDs auf.
10. Stellen Sie sicher, dass die Verbindung zwischen dem ebod-Controller und dem Gerätecontroller aktiv ist, überprüfen Sie, dass die vier LEDs neben dem SAS-Anschluss am ebod-Controller grün leuchten.
    
    > [!IMPORTANT]
    > Um hohe Verfügbarkeit für Ihr System sicherzustellen, wird empfohlen, dass Sie unbedingt das in der folgenden Abbildung gezeigte stromverkabelungsschema einhalten.
    > 
    > 
    
    ![Verkabelung des 4U-Geräts für Stromversorgung](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Stromverkabelung**
    
    | Label | Beschreibung |
    |:--- |:--- |
    | 1 |Primäres Gehäuse |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |Controller 0 |
    | 5 |Controller 1 |
    | 6 |Ebod-Controller 0 |
    | 7 |Ebod-Controller 1 |
    | 8 |Ebod-Gehäuse |
    | 9 |PDUs |

