---
title: Sicheres Migrieren von Benutzern zwischen Produktlizenzen mithilfe von gruppenbasierter Lizenzierung in Azure Active Directory | Microsoft-Dokumentation
description: Beschreibt die empfohlene Vorgehensweise beim Migrieren von Benutzern zwischen verschiedenen Produktlizenzen (z.B. Office 365 E1 und E3) mithilfe von gruppenbasierter Lizenzierung.
services: active-directory
keywords: "Azure AD-Lizenzierung"
documentationcenter: 
author: piotrci
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/07/2018
ms.author: piotrci
ms.openlocfilehash: 97654673b395fd5b8cb41afdcdeaa21aba44f61d
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-safely-migrate-users-between-product-licenses-using-group-based-licensing"></a>Sicheres Migrieren von Benutzern zwischen Produktlizenzen mithilfe von gruppenbasierter Lizenzierung in Azure Active Directory

Dieser Artikel beschreibt die empfohlene Methode zum Verschieben von Benutzern zwischen Produktlizenzen bei Verwendung von gruppenbasierter Lizenzierung. Das Ziel dieses Ansatzes besteht darin, sicherzustellen, dass die Migration ohne Dienst- und Datenverlust abläuft – für einen nahtlosen Benutzerwechsel zwischen den Produkten. Zwei Varianten des Migrationsprozesses werden behandelt:

-   Einfache Variante: Zwischen Produktlizenzen, die keine widersprüchlichen Servicepläne enthalten, z.B.: *Office 365 Enterprise E3* und *Office 365 Enterprise E5*.

-   Komplexere Variante: Zwischen Produktlizenzen, die widersprüchliche Servicepläne enthalten, z.B.: *Office 365 Enterprise E1* und *Office 365 Enterprise E3*. Weitere Informationen zu Konflikten finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal#conflicting-service-plans) und [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-product-and-service-plan-reference#service-plans-that-cannot-be-assigned-at-the-same-time).

Dieser Artikel enthält PowerShell-Beispielcode, der zum Ausführen der Migrations- und Überprüfungsschritte verwendet werden kann. Er ist besonders für umfangreiche Vorgänge nützlich, in denen eine manuelle Ausführung der Schritte nicht möglich ist.

## <a name="before-you-begin"></a>Voraussetzungen
Vor dem Ausführen des Migrationsprozesses muss überprüft werden, ob die folgenden Annahmen für alle zu migrierenden Benutzer wahr („true“) sind. Falls nicht, tritt bei der Migration möglicherweise für einige der Benutzer ein Fehler auf, wodurch diese ggf. nicht mehr auf Dienste oder Daten zugreifen können:

-   Den Benutzern wurde die *Quelllizenz* mithilfe der gruppenbasierten Lizenzierung zugewiesen. Die Lizenzen für das Produkt, von dem die Benutzer verschoben werden sollen, werden aus einer einzigen Quellgruppe geerbt und nicht direkt zugewiesen. Hinweis: Wenn Lizenzen auch direkt zugewiesen werden, verhindern sie möglicherweise die Anwendung der *Ziellizenz*. Weitere Informationen zur direkten und gruppenbasierten Lizenzzuweisung finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-advanced#direct-licenses-coexist-with-group-licenses). Vielleicht möchten Sie auch ein PowerShell-Skript wie [dieses](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-ps-examples#check-if-user-license-is-assigned-directly-or-inherited-from-a-group) verwenden, um zu prüfen, ob Benutzer über direkte Lizenzen verfügen.

-   Sie haben genügend verfügbare Lizenzen für das Zielprodukt. Falls nicht, kann einigen Benutzern möglicherweise keine *Ziellizenz* zugewiesen werden. Die Anzahl der verfügbaren Lizenzen können Sie [hier](https://portal.azure.com/#blade/Microsoft_AAD_IAM/LicensesMenuBlade/Products) überprüfen.

-   Den Benutzern sind keine anderen Produktlizenzen zugewiesen, die mit der *Ziellizenz* in Konflikt stehen oder das Entfernen der *Quelllizenz* verhindern könnten (z.B. Zusatzprodukte wie Workplace Analytics oder Project Online, die von anderen Produkten abhängig sind).

-   Sie sind darüber informiert, wie Gruppen in Ihrer Umgebung verwaltet werden. Wenn Sie beispielsweise lokale Gruppen verwalten und mit AAD Connect in Azure AD synchronisieren, müssen Sie Benutzer über Ihr lokales System hinzufügen bzw. entfernen. Dabei wird es einige Zeit dauern, bis die Änderungen mit AAD synchronisiert sind und von der gruppenbasierten Lizenzierung übernommen werden. Bei Verwendung von dynamischen Gruppenmitgliedschaften in Azure AD werden Sie Benutzer hinzufügen bzw. entfernen, indem Sie stattdessen deren Attribute ändern. Der Migrationsprozess als solches bleibt weiterhin gleich und unterscheidet sich nur darin, wie das Hinzufügen und Entfernen von Benutzern zu bzw. aus Gruppen erfolgt.

## <a name="migrating-users-between-products-without-conflicting-service-plans"></a>Migrieren von Benutzern zwischen Produkten ohne widersprüchliche Servicepläne
Ziel ist es, mithilfe von gruppenbasierter Lizenzierung die Benutzerlizenzen von einer *Quelllizenz* (in diesem Beispiel: *Office 365 Enterprise E3*) in eine *Ziellizenz* (in diesem Beispiel: *Office 365 Enterprise E5*) zu ändern. Die beiden Produkte enthalten keine widersprüchlichen Servicepläne, sodass sie vollständig und ohne Konflikte gleichzeitig zugewiesen werden können. Während der Migration sollten Benutzer zu keinem Zeitpunkt die Zugriffsmöglichkeit für Dienste oder Daten verlieren. Darüber hinaus wird die Migration in kleinen „Batches“ ausgeführt. So lässt sich das Ergebnis für jeden Batch überprüfen und die Anzahl möglicher Probleme einschränken, die während des Prozesses auftreten können. Im Ganzen verläuft der Prozess dann wie folgt:
1.  Benutzer sind Mitglieder einer Quellgruppe und erben die *Quelllizenz* aus dieser Gruppe.
2.  Erstellen Sie eine Zielgruppe mit der *Ziellizenz* jedoch ohne Mitglieder.
3.  Fügen Sie zur Zielgruppe einen Batch von Benutzern hinzu. Diese Änderung wird von der gruppenbasierten Lizenzierung (GBL) übernommen, und die *Ziellizenz* wird zugewiesen. Beachten Sie, dass dies abhängig von der Größe des Batches und anderen Aktivitäten im Mandanten etwas dauern kann.
4.  Überprüfen Sie, ob der Batch von Benutzern durch die GBL vollständig verarbeitet und jedem Benutzer tatsächlich die *Ziellizenz* zugewiesen wurde. Stellen Sie sicher, dass bei den Benutzern keine Fehler aufgetreten sind, z.B. durch Konflikte mit anderen Produkten oder eine zu geringe Anzahl von Lizenzen. Weitere Informationen zu Fehlern finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal).
5.  Nun sind den Benutzern sowohl die Quell- also auch die *Ziellizenzen* zugewiesen.
6.  Entfernen Sie den gleichen Benutzerbatch aus der Quellgruppe. Die GBL reagiert auf die Änderung, und die *Quelllizenzen* werden für die Benutzer entfernt.
7.  Wiederholen Sie den Prozess für nachfolgende Benutzerbatches.

### <a name="migrating-a-single-user-using-azure-portal"></a>Migrieren eines einzelnen Benutzers über das Azure-Portal
Dies ist eine einfache Schrittanleitung für die Migration eines einzelnen Benutzers.

- **Schritt 1:** Der Benutzer hat die *Quelllizenz* aus einer Gruppe geerbt und verfügt über keine direkten Zuweisungen für die Lizenz.
![Benutzer mit aus Gruppe geerbter Quelllizenz](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseInherited.png)

- **Schritt 2:** Der Benutzer wurde zur Zielgruppe hinzugefügt, und die GBL hat die Änderung verarbeitet. Der Benutzer verfügt nun sowohl über die *Quelllizenz* als auch über die *Ziellizenz*, die er aus beiden Gruppen geerbt hat.
![Benutzer mit aus Gruppen geerbten Quell- und Ziellizenzen](media/active-directory-licensing-group-product-migration/UserWithBothSourceAndTargetLicense.png)

- **Schritt 3:** Der Benutzer wurde aus der Quellgruppe entfernt, und die GBL hat die Änderung verarbeitet. Der Benutzer verfügt jetzt nur noch über die *Ziellizenz*.
![Benutzer mit aus Gruppe geerbter Ziellizenz](media/active-directory-licensing-group-product-migration/UserWithTargetLicenseAssigned.png)

### <a name="automating-migration-using-powershell"></a>Automatisieren der Migration mithilfe von PowerShell
> [!NOTE]
> Dieser Beispielcode verwendet PowerShell-Funktionen, die im [letzten Abschnitt](#powershell-automation-of-migration-and-verification-steps) dieses Dokuments enthalten sind.

Dieser Codeausschnitt veranschaulicht, wie der Migrationsprozess bei größerem Umfang automatisiert werden kann.
```
#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com','MigrationUser2@tailspinonline.com'

###############NON-CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:ENTERPRISEPACK'      #<- this is the O365 E3 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPREMIUM'   #<- this is the O365 E5 product

if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will assign the target license $targetSkuId to all users"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceandTargetLicensePresent} 'STEP 2: Checking if all users still have the source license and now also have the target license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId
```

Beispielausgabe (Migrieren von 2 Benutzern):
```
Verifying initial assumptions:
Enough TailspinOnline:ENTERPRISEPREMIUM licenses available (13) for users: 2.
migrationuser@tailspinonline.com                OK
migrationuser2@tailspinonline.com               OK
Checks passed for all 2 users.

STEP 1: Adding users to the target group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe. This will assign the target license TailspinOnline:ENTERPRISEPREMIUM to all users
Adding user MigrationUser@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe
Adding user MigrationUser2@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe

STEP 2: Checking if all users still have the source license and now also have the target license from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 2: Checking if all users still have the source license and now also have the target license from target group. Check iteration: 2

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.

STEP 3: Removing users from the source group b82c04f0-ce30-4ff1-bac7-735d92d83036. This will remove the source license TailspinOnline:ENTERPRISEPACK.
Removing user MigrationUser@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036
Removing user MigrationUser2@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 2

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.
```

## <a name="migrating-users-between-products-with-conflicting-service-plans"></a>Migrieren von Benutzern zwischen Produkten mit widersprüchlichen Serviceplänen
Ziel ist es, mithilfe von gruppenbasierter Lizenzierung die Benutzerlizenzen von einer *Quelllizenz* (in diesem Beispiel: *Office 365 Enterprise E1*) in eine *Ziellizenz* (in diesem Beispiel: *Office 365 Enterprise E3*) zu ändern. Die beiden Produkte enthalten widersprüchliche Servicepläne (weitere Informationen zu Konflikten finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal#conflicting-service-plans)), für die eine Problemumgehung gefunden werden muss, um eine nahtlose Benutzermigration zu ermöglichen. Während der Migration sollten Benutzer zu keinem Zeitpunkt die Zugriffsmöglichkeit für Dienste oder Daten verlieren. Darüber hinaus wird die Migration in kleinen „Batches“ ausgeführt. So lässt sich das Ergebnis für jeden Batch überprüfen und die Anzahl möglicher Probleme einschränken, die während des Prozesses auftreten können. Im Ganzen verläuft der Prozess dann wie folgt:
1.  Benutzer sind Mitglieder einer Quellgruppe und erben die *Quelllizenz* aus dieser Gruppe.
2.  Erstellen Sie eine Zielgruppe mit der *Ziellizenz* jedoch ohne Mitglieder.
3.  Fügen Sie zur Zielgruppe einen Batch von Benutzern hinzu. Diese Änderung wird von der gruppenbasierten Lizenzierung (GBL) übernommen, die dann versucht, die *Ziellizenz* zuzuweisen. Die Zuweisung ist aufgrund von Konflikten zwischen Diensten in den beiden Produkten nicht erfolgreich, sodass die GBL einen Fehler für jeden Benutzer aufgezeichnet.
Beachten Sie, dass dies abhängig von der Größe des Batches und anderen Aktivitäten im Mandanten etwas dauern kann.
4.  Überprüfen Sie, ob der Batch von Benutzern durch die GBL vollständig verarbeitet und für jeden Benutzer der zum Konflikt führende Fehler aufgezeichnet wurde. Stellen Sie sicher, dass für keinen Benutzer ein unerwarteter Fehler aufgetreten ist. Weitere Informationen zu Fehlern finden Sie [hier](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-licensing-group-problem-resolution-azure-portal).
5.  An diesem Punkt ist den Benutzern weiterhin die *Quelllizenz* zugewiesen und es liegt ein Fehler aufgrund eines Konflikts für die *Ziellizenz* vor – die *Ziellizenz* ist den Benutzern also noch nicht zugewiesen.
6.  Entfernen Sie den gleichen Benutzerbatch aus der Quellgruppe. Die GBL reagiert auf die Änderung, und die *Quelllizenz* wird für die einzelnen Benutzer entfernt. Gleichzeitig wird der Konfliktfehler ebenfalls entfernt (vorausgesetzt, dass keine anderen Produktlizenz zu dem Fehler geführt hat) und die *Ziellizenzen* werden zugewiesen. So wird sichergestellt, dass während des Übergangs keine Dienste oder Daten verloren gehen.
7.  Wiederholen Sie den Prozess für nachfolgende Benutzerbatches.

### <a name="migrating-a-single-user-using-azure-portal"></a>Migrieren eines einzelnen Benutzers über das Azure-Portal
Dies ist eine einfache Schrittanleitung für die Migration eines einzelnen Benutzers.

- **Schritt 1:** Der Benutzer hat die *Quelllizenz* aus einer Gruppe geerbt und verfügt über keine direkten Zuweisungen für die Lizenz.
![Benutzer mit aus Gruppe geerbter Quelllizenz](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseInheritedConflictScenario.png)

- **Schritt 2:** Der Benutzer wurde zur Zielgruppe hinzugefügt, und GBL hat die Änderung verarbeitet. Dem Benutzer ist noch immer die *Quelllizenz* zugewiesen, und für die *Ziellizenz* liegt ein Fehler aufgrund eines Konflikts vor.
![Benutzer mit aus Gruppe geerbter Quelllizenz und Fehler bei der Ziellizenz](media/active-directory-licensing-group-product-migration/UserWithSourceLicenseAndTargetLicenseInConflict.png)

- **Schritt 3:** Der Benutzer wurde aus der Quellgruppe entfernt, und die GBL hat die Änderung verarbeitet. Die *Ziellizenz* wurde dem Benutzer jetzt zugewiesen.![Benutzer mit aus Gruppe geerbter Ziellizenz](media/active-directory-licensing-group-product-migration/UserWithTargetLicenseAssignedConflictScenario.png)

### <a name="automating-migration-using-powershell"></a>Automatisieren der Migration mithilfe von PowerShell
> [!NOTE]
> Dieser Beispielcode verwendet PowerShell-Funktionen, die im [letzten Abschnitt](#powershell-automation-of-migration-and-verification-steps) dieses Dokuments enthalten sind.

Dieser Codeausschnitt veranschaulicht, wie der Migrationsprozess bei größerem Umfang automatisiert werden kann.
```
#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com', 'MigrationUser2@tailspinonline.com'

###############CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:STANDARDPACK'             #<- this is the O365 E1 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPACK'           #<- this is the O365 E3 product

#Assumptions before migration:
#1. Users are already in the source group and they have the source license assigned from that group
#2. Users do not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
#This is important - if not true, removing users from the source group in Step 3 is not going to result in the target license getting applied correctly
#3. There are enough available licenses for the target license to assign to the users we are migrating.
if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will put target license $targetSkuId in conflict state with the source license $sourceSkuId"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceLicensePresentAndTargetLicenseInConflictState} 'STEP 2: Checking if all users still have the source license and are in conflict state for license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId and remove the conflict on target license $targetSkuId which will become assigned."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId
```

Beispielausgabe (Migrieren von 2 Benutzern):
```
Verifying initial assumptions:
Enough TailspinOnline:ENTERPRISEPACK licenses available (61) for users: 2.
migrationuser@tailspinonline.com                OK
migrationuser2@tailspinonline.com               OK
Checks passed for all 2 users.
STEP 1: Adding users to the target group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe. This will put target license TailspinOnline:ENTERPRISEPACK in conflict state with the source license TailspinOnline:STANDARDPACK
Adding user MigrationUser@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe
Adding user MigrationUser2@tailspinonline.com to group bcf279d1-5ad5-46a5-b469-4b8a552aa2fe

STEP 2: Checking if all users still have the source license and are in conflict state for license from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 2: Checking if all users still have the source license and are in conflict state for license from target group. Check iteration: 3

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.

STEP 3: Removing users from the source group b82c04f0-ce30-4ff1-bac7-735d92d83036. This will remove the source license TailspinOnline:STANDARDPACK and remove the conflict on target license TailspinOnline:ENTERPRISEPACK which will become assigned.
Removing user MigrationUser@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036
Removing user MigrationUser2@tailspinonline.com from group b82c04f0-ce30-4ff1-bac7-735d92d83036

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 1

MigrationUser@tailspinonline.com                Expected state: NO
MigrationUser2@tailspinonline.com               Expected state: NO

Total users checked: 2. In expected state: 0. Not yet: 2
Not all users are in expected state. Waiting 60 seconds to try again.

STEP 4: Checking if all users have source license removed and target license assigned from target group. Check iteration: 3

MigrationUser@tailspinonline.com                Expected state: YES
MigrationUser2@tailspinonline.com               Expected state: YES

Total users checked: 2. In expected state: 2. Not yet: 0
Check passed for all users. Exiting check loop.
```

## <a name="powershell-automation-of-migration-and-verification-steps"></a>Automatisieren der Migration und Überprüfungsschritte mit PowerShell
Dieser Abschnitt enthält den PowerShell-Code zum Ausführen der weiter oben im Artikel verwendeten Skripts.

>[!WARNING]
>Dieser Code dient als Beispiel zu Demonstrationszwecken. Wenn Sie ihn in Ihrer Umgebung verwenden möchten, sollten Sie den Code zunächst in kleinerem Umfang oder in einem separaten Testmandanten testen. Passen Sie den Code hierzu ggf. an die spezifischen Anforderungen Ihrer Umgebung an.

Damit Sie den Code ausführen können, verwenden Sie die [Bibliotheken für Azure AD PowerShell v1.0](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-msonlinev1?view=azureadps-1.0] for instructions). Führen Sie zunächst das Cmdlet *connect-msolservice* aus, um sich beim Mandanten anzumelden, bevor Sie das Skript ausführen.
```
#BEGIN: Helper functions used in the script

#Retrieves user object based on ObjectId or UserPrincipalName
function GetUserObject
{
    [OutputType([Microsoft.Online.Administration.User])]
    Param([object]$userId)

    $userIdType = $userId.GetType()

    if($userIdType -eq [Guid])
    {
        return Get-MsolUser -ObjectId $userId
    }
    elseif($userIdType -eq [string])
    {
        return Get-MsolUser -UserPrincipalName $userId
    }
    else
    {
        throw "User Id type must be a Guid or a string (UserPrincipalName). The user id type was: $($userIdType.Name)"
    }
}

#Gets a Guid objectId for a user, even if a UserPrincipal string was passed in. Guid ids are needed for group membership manipulation, where UPNs cannot be used
function GetGuidUserId
{
    [OutputType([Guid])]
    Param([object]$userId)

    [Guid]$guidId = [Guid]::Empty
    if($userId.GetType() -eq [String])
    {
        $user = GetUserObject $userId
        return $user.ObjectId
    }
    elseif($userId.GetType() -eq [Guid])
    {
        return $userId
    }
    else
    {
        throw "UserId type must be a Guid or a string (UserPrincipalName). The user id type was: $($userId.GetType().Name)"
    }
}

#Adds a collection of users to a group. Note: this fails if a user is already a member of the group
function AddUsersToGroup
{
    Param([object[]]$userIds, [Guid]$groupId)

    foreach($userId in $userIds)
    {
        $fetchId = GetGuidUserId $userId
        Write-Host "Adding user $userId to group $groupId"
        Add-MsolGroupMember -GroupObjectId $groupId -GroupMemberType User -GroupMemberObjectId $fetchId
    }
}

#Removes a collection of users from a group. Note: this fails if a user is not a member of the group
function RemoveUsersFromGroup
{
    Param([object[]]$userIds, [Guid]$groupId)

    foreach($userId in $userIds)
    {
        $fetchId = GetGuidUserId $userId
        Write-Host "Removing user $userId from group $groupId"
        Remove-MsolGroupMember -GroupObjectId $groupId -GroupMemberType User -GroupMemberObjectId $fetchId
    }
}

#Returns the license object corresponding to the skuId. Returns NULL if not found
function GetUserLicense
{
    [OutputType([Microsoft.Online.Administration.UserLicense])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    #we look for the specific license SKU in all licenses assigned to the user
    foreach($license in $user.Licenses)
    {
        if ($license.AccountSkuId -ieq $skuId)
        {
            return $license
        }
    }
    return $null
}

#Checks if the specific SKU license is assigned to the user, regardless of how it may be assigned (directly or via GBL)
function IsLicenseAssignedToUser
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [string]$skuId)

    $license = GetUserLicense $user $skuId

    return ($license -ne $null)
}

function GetObjectIdsAssigningLicense
{
    [OutputType([Guid[]])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    $license = GetUserLicense $user $skuId

    if ($license -ne $null)
    {
        return [Guid[]]$license.GroupsAssigningLicense
    }
    return [Array]::CreateInstance([Guid],0)
}

#Returns TRUE if the user is inheriting the license from the specific group.
#Note: this returns true only if the license is successfully assigned from the group. If the license is in error state, this return false
function UserHasLicenseAssignedFromThisGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId, [Guid]$groupId)

    [Guid[]]$objectsAssigningLicense = GetObjectIdsAssigningLicense $user $skuId

    #GroupsAssigningLicense contains a collection of IDs of objects assigning the license
    #This could be a group object or a user object (contrary to what the name suggests)
    foreach ($assignmentSource in $objectsAssigningLicense)
    {
        #If the collection contains at least one ID not matching the user ID this means that the license is inherited from a group.
        #Note: the license may also be assigned directly in addition to being inherited
        if ($assignmentSource -ieq $groupId)
        {
            return $true
        }
    }
    return $false
}

#Returns error objects for a specific license
function GetErrorsForLicense
{
    [OutputType([Microsoft.Online.Administration.IndirectLicenseError[]])]
    Param([Microsoft.Online.Administration.User]$user, [string]$skuId)

    [Microsoft.Online.Administration.IndirectLicenseError[]] $errorObjects = $user.IndirectLicenseErrors | Where {"$($_.AccountSku.AccountName):$($_.AccountSku.SkuPartNumber)" -ieq $skuId}

    if($errorObjects -eq $null)
    {
        #there are no errors at all
        return [Array]::CreateInstance([Microsoft.Online.Administration.IndirectLicenseError],0)
    }

    return $errorObjects
}
#Returns an error label associated with a specific license inherited from a specific group. return $null if there is no error
function GetErrorForLicenseFromGroup
{
    [OutputType([string])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$groupId, [string]$skuId)


    #There are some errors. Check if any of them is associated with the group
    foreach($licenseError in GetErrorsForLicense $user $skuId)
    {
        if($licenseError.ReferencedObjectId -eq $groupId)
        {
            return $licenseError.Error
        }
    }
    return $null
}

#Checks if the license is in an expected state for a given group.
#If expectedError is set to a value, this looks if the license is in that specific error state from the group
#If expectedError is NULL, this checks if the license is successfully assigned from the group
function IsExpectedLicenseStateForGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$groupId, [string]$skuId, [string]$expectedError)

    #we expect the license to be fully assigned from the group and not in error state
    if([string]::IsNullOrEmpty($expectedError))
    {
        #check if the assigned license is inherted from the expected group, without an error on it
        return (UserHasLicenseAssignedFromThisGroup $user $skuId $groupId)
    }
    #we expect the license to be in the specific error state on the specific group
    else
    {
        $error = GetErrorForLicenseFromGroup $user $groupId $skuId
        return ($error -ieq $expectedError)
    }
}

#Detects if the licenses are in the specific state where the source license is still assigned, but the target license is in conflict state
#Note: if the source license is not present, this throws an exception to abort the script, because that state may signify something went wrong with the migration steps and user lost access to services
function VerifySourceLicensePresentAndTargetLicenseInConflictState
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check user still has source license - if not, abort because something is seriously wrong
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $sourceSkuId from source group $sourceGroupId, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is in conflict, as expected
    $conflictError = 'MutuallyExclusiveViolation'
    return (IsExpectedLicenseStateForGroup $user $targetGroupId $targetSkuId $conflictError)
}

#Detects if the licenses are in the specific state where the source license is no longer present, but the target license is correctly assigned
#Note: if the source license is gone, but target license is not present, this throws an exception to abort the script, because something went wrong and the user may have lost access to services
function VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check if user has the source license completely removed, which is a prerequisite to the target license eventually kicking in
    if(IsLicenseAssignedToUser $user $sourceSkuId)
    {
        return $false
    }

    #check user has the target license at all - if not, abort because something is seriously wrong
    if(-Not (IsLicenseAssignedToUser $user $targetSkuId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $targetSkuId assigned, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is assigned from the expected target group, and not in error state anymore
    return (IsExpectedLicenseStateForGroup $user $targetGroupId $targetSkuId $null)
}

#Detects if the licenses are in the specific state where the source license is still assigned and the target license is assigned as well
#Note: if the source license is not present, this throws an exception to abort the script, because that state may signify something went wrong with the migration steps and user lost access to services
function VerifySourceandTargetLicensePresent
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #check user still has source license - if not, abort because something is seriously wrong
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        throw "User $($user.UserPrincipalName) ($($user.ObjectId)) does not have the expected license $sourceSkuId from source group $sourceGroupId, which may result in loss of access and data. This is unexpected and should be investigated. Aborting execution."
    }
    #check if the target license is also assigned from the target group
    return (UserHasLicenseAssignedFromThisGroup $user $targetSkuId $targetGroupId)
}


#Verifies basic assumptions that should be true for a user before we execute the migration process.
#Returns TRUE if all assumptions are true. Prints details
function VerifyAssumptionsForUser
{
    [OutputType([bool])]
    Param([Microsoft.Online.Administration.User]$user,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    $userName = $user.UserPrincipalName
    #1. User has the source license assigned from the source group.
    if(-Not (UserHasLicenseAssignedFromThisGroup $user $sourceSkuId $sourceGroupId))
    {
        Write-Host "$userName does not have source license $sourceSkuId assigned from source group $sourceGroupId."
        return $false
    }

    #2. User does not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
    [Guid[]]$otherObjectsAssigningLicense = GetObjectIdsAssigningLicense $user $sourceSkuId | Where {$_ -ne $sourceGroupId}
    foreach($otherObject in $otherObjectsAssigningLicense)
    {
        if($otherObject -eq $user.ObjectId)
        {
            Write-Host "$userName has source license assigned directly to the user."
        }
        else
        {
            Write-Host "$username has source license assigned from an additional unexpected group $otherObject."
        }
    }
    if($otherObjectsAssigningLicense -and $otherObjectsAssigningLicense.Count -gt 0)
    {
        return $false
    }

    #3. User does not have the target license assigned
    if(IsLicenseAssignedToUser $user $targetSkuId)
    {
        Write-Host "$userName already has target license assigned."
        return $false
    }

    #4. User does not have the target license in error state from some groups
    [Microsoft.Online.Administration.IndirectLicenseError[]]$licenseErrors = GetErrorsForLicense $user $targetSkuId
    foreach($licenseError in $licenseErrors)
    {
        Write-Host "$userName has target license in error state $($licenseError.Error) from unexpected group $($licenseError.ReferencedObjectId)."
    }
    if($licenseErrors -and $licenseErrors.Count -gt 0)
    {
        return $false
    }

    Write-Host "$userName`t`tOK"
    return $true
}

#Checks if all users to be migrated are in correct state
function VerifyAssumptions
{
    [OutputType([bool])]
    Param([object[]]$userIds, [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    Write-Host "Verifying initial assumptions:"

    #Check if there are enough target licenses for all users
    $skuState = Get-MsolAccountSku | Where {$_.AccountSkuId -ieq $targetSkuId}

    if($skuState -eq $null)
    {
        Write-Host "Target license SKU $targetSkuId does not exist in the tenant at all."
        return $false
    }

    $availableLicenses = $skuState.ActiveUnits - $skuState.ConsumedUnits

    if($userIds.Count -gt $availableLicenses)
    {
        Write-Host "Not enough licenses for all users. User count: $($userIds.Count), licenses available: $availableLicenses"
        return $false
    }
    else
    {
        Write-Host "Enough $targetSkuId licenses available ($availableLicenses) for users: $($userIds.Count)."
    }

    #Check if each user to be migrated is in expected state
    $usersOK = 0
    $usersNotOK = 0
    foreach($userId in $userIds)
    {
        $user = GetUserObject $userId
        if(VerifyAssumptionsForUser $user $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId)
        {
            ++$usersOK
        }
        else
        {
            ++$usersNotOK
        }
    }
    if($usersNotOK -gt 0)
    {
        Write-Host "Checks passed for $usersOK users, but failed for $usersNotOK users."
        return $false
    }
    Write-Host "Checks passed for all $usersOK users."
    return $true
}

#Helper function: it executes one of the verification functions (passed in as a delegate using $checkFunction) for each user, keeps track of how many users passed/failed verification
#and repeats the loop until all users have passed the check. The loop may never terminate if some users never reach the expected state, which should be investigated.
#Note: if the verification function fails with an exception (e.g. because it detected unexpected user state) this loop will terminate and investigation into user state is needed
function ExecuteVerificationLoop
{
    Param([System.Management.Automation.ScriptBlock]$checkFunction, [string]$consoleMessage, [object[]]$userIds,  [Guid]$sourceGroupId, [string]$sourceSkuId, [Guid]$targetGroupId, [string]$targetSkuId)

    #how long to wait until the loop is retried
    $sleepIntervalSeconds = 60
    $retryIteration = 1

    While($true)
    {
        Write-Host "`n$consoleMessage. Check iteration: $retryIteration`n"

        $usersInExpectedState = 0
        $usersNotYet = 0

        foreach ($userId in $userIds)
        {
            $user = GetUserObject $userId
            if($checkFunction.InvokeReturnAsIs($user, $sourceGroupId, $sourceSkuId, $targetGroupId, $targetSkuId))
            {
                Write-Host "$userId`t`tExpected state: YES"
                ++$usersInExpectedState
            }
            else
            {
                Write-Host "$userId`t`tExpected state: NO"
                ++$usersNotYet
            }
        }

        Write-Host "`nTotal users checked: $($userIds.Count). In expected state: $usersInExpectedState. Not yet: $usersNotYet"

        if($usersNotYet -eq 0)
        {
            Write-Host "Check passed for all users. Exiting check loop."
            return
        }

        ++$retryIteration
        Write-Host "Not all users are in expected state. Waiting $sleepIntervalSeconds seconds to try again."
        Start-Sleep -Seconds $sleepIntervalSeconds
    }
}
#END: Helper functions used in the script

#BEGIN: Execute script

#enable strict mode
Set-StrictMode -Version latest
#stop on first exception thrown
$ErrorActionPreference = "Stop"

#A batch of users that we want to migrate in this iteration. This can be an array of User Principal Names (string) or ObjectIds (Guid)
#Note: this could be loaded from a text file that represents a larger batch of users we want to migrate
[string[]]$usersToMigrate = 'MigrationUser@tailspinonline.com', 'MigrationUser2@tailspinonline.com'

###############CONFLICTING LICENSES SCENARIO################
#The group and license that we are moving from
$sourceGroupId = [Guid]'b82c04f0-ce30-4ff1-bac7-735d92d83036'
$sourceSkuId = 'TailspinOnline:STANDARDPACK'             #<- this is the O365 E1 product
#The group and license that we are moving to
$targetGroupId = [Guid]'bcf279d1-5ad5-46a5-b469-4b8a552aa2fe'
$targetSkuId = 'TailspinOnline:ENTERPRISEPACK'           #<- this is the O365 E3 product

#Assumptions before migration:
#1. Users are already in the source group and they have the source license assigned from that group
#2. Users do not have the same source license assigned from another group at the same time and they do not have the source license assigned directly
#This is important - if not true, removing users from the source group in Step 3 is not going to result in the target license getting applied correctly
#3. There are enough available licenses for the target license to assign to the users we are migrating.
if(-Not (VerifyAssumptions $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId))
{
    throw "Some users did not pass validation checks. See the output for details. Aborting migration process."
}

Write-Host "STEP 1: Adding users to the target group $targetGroupId. This will put target license $targetSkuId in conflict state with the source license $sourceSkuId"
AddUsersToGroup $usersToMigrate $targetGroupId

#Verify that the target license shows up in conflict state for each user on the list. This step will run in a loop, forever, until all users are in the expected state.
#Since GBL may take some time to reflect the changes on users, this loop should terminate after some time dependent on the size of the user collection.
#Note: If the loop has not terminated for a long time, stop the script and inspect the users reported as not yet in the expected state and verify that they are not blocked for some other reason.
ExecuteVerificationLoop ${function:VerifySourceLicensePresentAndTargetLicenseInConflictState} 'STEP 2: Checking if all users still have the source license and are in conflict state for license from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#Now it is safe to remove the users from the source group
Write-Host "STEP 3: Removing users from the source group $sourceGroupId. This will remove the source license $sourceSkuId and remove the conflict on target license $targetSkuId which will become assigned."
RemoveUsersFromGroup $usersToMigrate $sourceGroupId

#Verify that target license is now active on each user and the source license has been removed.
ExecuteVerificationLoop ${function:VerifySourceLicenseRemovedAndTargetLicenseAssignedFromGroup} 'STEP 4: Checking if all users have source license removed and target license assigned from target group' $usersToMigrate $sourceGroupId $sourceSkuId $targetGroupId $targetSkuId

#END: Execute script
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Szenarien für die Lizenzverwaltung über Gruppen finden Sie in folgenden Artikeln.

* [Bestimmen und Beheben von Lizenzproblemen für eine Gruppe in Azure Active Directory](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [PowerShell-Beispiele für die gruppenbasierte Lizenzierung in Azure AD](active-directory-licensing-ps-examples.md)
* [Gruppenbasierte Azure Active Directory-Lizenzierung – zusätzliche Szenarien](active-directory-licensing-group-advanced.md)
