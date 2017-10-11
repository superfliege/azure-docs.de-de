<!--author=alkohli last changed:02/22/16-->

#### <a name="to-attach-the-sas-cables"></a>Die SAS-Kabel Anfügen
1. Identifizieren Sie das primäre und das ebod-Gehäuse. Die beiden Gehäuse können durch einen Blick auf ihre jeweilige Rückwand identifiziert werden. Siehe die folgende Abbildung als Anleitung an. 
   
    ![Backplane des primären und ebod-Gehäuses](./media/storsimple-sas-cable-8600/HCSBackplaneofprimaryandEBODenclosure.png)
   
    **Ansicht der Rückseite des primären und ebod-Gehäuse**
   
   | Label | Beschreibung |
   |:--- |:--- |
   | 1 |Primäres Gehäuse |
   | 2 |Ebod-Gehäuse |
2. Suchen Sie die Seriennummern auf dem primären und dem ebod-Gehäuse. Der Aufkleber mit der Seriennummer wird auf den Rückwärtspfeil sich bei jedem Gehäuse angebracht. Die Seriennummern müssen auf beiden Gehäusen identisch sein. [Wenden Sie sich an Microsoft Support](../articles/storsimple/storsimple-contact-microsoft-support.md) sofort, wenn die Seriennummern nicht übereinstimmen. Siehe folgende Abbildung, die Seriennummern an.
   
    ![Rückansicht des Gehäuses mit Seriennummer](./media/storsimple-sas-cable-8600/HCSRearviewofenclosureindicatinglocationofserialnumbersticker.png)
   
    **Speicherort der Aufkleber mit Seriennummer**
   
   | Label | Beschreibung |
   |:--- |:--- |
   | 1 |Lasche des Gehäuses |
3. Verwenden Sie die mitgelieferten SAS-Kabel, um das ebod-Gehäuse mit dem primären Gehäuse wie folgt herstellen:
   
   1. Identifizieren Sie die vier SAS-Anschlüsse am primären Gehäuse und ebod-Gehäuse. Die SAS-Anschlüsse EBOD beschriftet sind, auf dem primären Gehäuse und ein Port, auf dem ebod-Gehäuse entsprechen, wie in der SAS-Verkabelung Abbildung unten gezeigt.
   2. Verwenden Sie die mitgelieferten SAS-Kabel, um das ebod-Standardport in Port A. verbinden
   3. Das ebod--Anschluss am Controller 0 muss mit der ein Anschluss am ebod-Controller 0 verbunden sein. Das ebod--Anschluss am Controller 1 muss mit der ein Anschluss am ebod-Controller 1 verbunden sein. Siehe folgende Abbildung Anleitungen. 
      
      ![SAS-Verkabelung des Geräts](./media/storsimple-sas-cable-8600/HCSSAScablingforyourdevice.png)
      
      **SAS-Verkabelung**
      
      | Label | Beschreibung |
      |:--- |:--- |
      | A |Primäres Gehäuse |
      | B |Ebod-Gehäuse |
      | 1 |Controller 0 |
      | 2 |Controller 1 |
      | 3 |Ebod-Controller 0 |
      | 4 |Ebod-Controller 1 |
      | 5, 6 |SAS-Anschlüsse am primären Gehäuse (beschriftet mit EBOD) |
      | 7, 8 |SAS-Anschlüsse am ebod-Gehäuse (Port A) |

