---
title: Beheben von Fehlern mit Azure NetApp Files-Ressourcenanbietern | Microsoft-Dokumentation
description: Beschreibt Ursachen, Lösungen und Problemumgehungen für häufige Fehler bei Azure NetApp Files-Ressourcenanbietern.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: b-juche
ms.openlocfilehash: f417d83a67f2f3afa33a83a56a72d0d82c64ab0d
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850003"
---
# <a name="troubleshoot-azure-netapp-files-resource-provider-errors"></a>Beheben von Fehlern mit Azure NetApp Files-Ressourcenanbietern 

In diesem Artikel werden häufige Fehler von Azure NetApp Files-Ressourcenanbietern, deren Ursachen, Lösungen und Problemumgehungen beschrieben (sofern vorhanden).

## <a name="common-azure-netapp-files-resource-provider-errors"></a>Die häufigsten Fehler mit Azure NetApp Files-Ressourcenanbietern

***„BareMetalTenantId“*** kann nicht geändert werden.  

Dieser Fehler tritt auf, wenn Sie versuchen, ein Volume zu aktualisieren oder zu patchen und die Eigenschaft `BaremetalTenantId` einen geänderten Wert hat.

* Ursache:   
Sie versuchen, ein Volume zu aktualisieren, und die Eigenschaft `BaremetalTenantId` hat einen anderen Wert als der in Azure gespeicherte Wert.
* Lösung:   
`BaremetalTenantId` nicht in den Patch und die Update (Put)-Anforderung aufnehmen. Alternativ können Sie auch sicherstellen, dass `BaremetalTenantId` in der Anforderung identisch ist.

***Die ServiceLevel kann nicht geändert werden.***  

Dieser Fehler tritt auf, wenn Sie versuchen, einen Kapazitätspool mit einem anderen Servicelevel zu aktualisieren oder zu patchen, wenn der Kapazitätspool bereits Volumes enthält.

* Ursache:   
Sie versuchen, einen Kapazitätspool-Servicelevel zu aktualisieren, wenn der Pool Volumes enthält.
* Lösung:   
Löschen Sie alle Datenträger aus dem Kapazitätspool und ändern Sie dann den Servicelevel.
* Problemumgehung:   
Legen Sie einen weiteren Kapazitätspool an und legen Sie die Volumes dann im neuen Kapazitätspool wieder an.

***PoolId kann nicht geändert werden***  

Dieser Fehler tritt auf, wenn Sie versuchen, einen Kapazitätspool aktualisieren oder zu patchen und die Eigenschaft `PoolId` einen geänderten Wert hat.

* Ursache:   
Sie versuchen, eine `PoolId`-Eigenschaft für den Kapazitätspool zu aktualisieren. Die Eigenschaft `PoolId` ist eine schreibgeschützte Eigenschaft und kann nicht geändert werden.
* Lösung:   
`PoolId` nicht in den Patch und die Update (Put)-Anforderung aufnehmen.  Alternativ können Sie auch sicherstellen, dass `PoolId` in der Anforderung identisch ist.

***Erstellungstoken können nicht geändert werden.***

Dieser Fehler tritt bei dem Versuch auf, den Dateipfad (`CreationToken`) zu ändern, nachdem das Volume erstellt wurde. Der Dateipfad (`CreationToken`) muss beim Erstellen des Volumes festgelegt werden und kann später nicht mehr geändert werden.

* Ursache:   
Sie versuchen, den Dateipfad (`CreationToken`) nach dem Erstellen des Volumes zu ändern. Dies ist kein unterstützter Vorgang. 
* Lösung:   
Wenn Sie den Dateipfad nicht ändern müssen, dann können Sie den Parameter aus der Anforderung entfernen, um die Fehlermeldung zu schließen.
* Problemumgehung:   
Wenn Sie den Dateipfad (`CreationToken`) ändern müssen, können Sie ein neues Volume mit einem neuen Dateipfad erstellen und die Daten dann auf das neue Volume migrieren.

***Erstellungstoken müssen mindestens 16 Zeichen umfassen.***

Dieser Fehler tritt auf, wenn der Dateipfad (`CreationToken`) nicht der erforderlichen Länge entspricht. Der Dateipfad muss mindestens ein Zeichen lang sein.

* Ursache:   
Der Dateipfad ist leer.  Wenn Sie ein Volume über die API erstellen, ist ein Erstellungstoken erforderlich. Wenn Sie das Azure-Portal verwenden, wird der Dateipfad automatisch generiert.
* Lösung:   
Geben Sie mindestens ein Zeichen als Dateipfad an (`CreationToken`).

***Der Domänenname kann nicht geändert werden.***

Dieser Fehler tritt auf, wenn Sie versuchen, den Domänennamen in Active Directory zu ändern.

* Ursache:   
Sie versuchen, die Eigenschaft „Domänenname“ zu aktualisieren.
* Lösung:    
None (Keine): Sie können den Domänennamen nicht ändern.
* Problemumgehung:   
Löschen Sie alle Volumes mithilfe der Active Directory-Konfiguration. Löschen Sie dann die Active Directory-Konfiguration und erstellen Sie die Volumes neu.

***Fehler bei doppeltem Wert für das Objekt „ExportPolicy.Rules[RuleIndex]“***

Dieser Fehler tritt auf, wenn die Exportrichtlinie nicht mit einem eindeutigen Index definiert ist. Wenn Sie Exportrichtlinien definieren, müssen alle Exportrichtlinienregeln einen eindeutigen Index zwischen 1 und 5 aufweisen.

* Ursache:   
Die definierte Exportrichtlinie erfüllt nicht die Anforderungen an die Exportrichtlinienregeln. Sie müssen mindestens über eine und dürfen höchstens über fünf Exportrichtlinienregeln verfügen.
* Lösung:   
Stellen Sie sicher, dass der Index nicht bereits verwendet wird und im Bereich von 1 bis 5 liegt.
* Problemumgehung:   
Verwenden Sie einen anderen Index für die Regel, die Sie festzulegen versuchen.

***Fehler {action} {resourceTypeName} {resourceTypeName}***

Dieser Fehler wird angezeigt, wenn andere Fehlerbehandlungen den Fehler während der Ausführung einer Aktion auf einer Ressource nicht behandelt haben.   Er enthält das Wort „Fehler“. `{action}` kann entweder (`getting`, `creating`, `updating` oder `deleting`) sein.  `{resourceTypeName}` ist `resourceTypeName` (z. B.: `netAppAccount`, `capacityPool`, `volume` usw.).

* Ursache:   
Dieser Fehler ist ein Ausnahmefehler, bei dem die Ursache nicht bekannt ist.
* Lösung:   
Wenden Sie sich an das Azure Support Center, um den genauen Grund in den Protokollen zu melden.
* Problemumgehung:   
None (Keine):

***Der Dateipfadname darf nur Buchstaben, Zahlen und Bindestriche („-“) enthalten.***

Dieser Fehler tritt auf, wenn der Dateipfad nicht unterstützte Zeichen enthält, z. B. einen Punkt („.“), ein Komma („,“), einen Unterstrich („_“) oder ein Dollarzeichen („$“).

* Ursache:   
Dieser Dateipfad enthält nicht unterstützte Zeichen, z. B. einen Punkt („.“), ein Komma („,“), einen Unterstrich („_“) oder ein Dollarzeichen („$“).
* Lösung:   
Entfernen Sie Zeichen, die keine alphabetischen Buchstaben, Zahlen oder Bindestriche („-“) sind, aus dem von Ihnen angegebenen Dateipfad.
* Problemumgehung:   
Sie können einen Unterstrich durch einen Bindestrich ersetzen oder Großbuchstaben anstelle von Leerzeichen verwenden, um den Anfang neuer Wörter anzuzeigen.  Verwenden Sie z. B. „NewVolume“ anstelle von „new volume“.

***FileSystemId kann nicht geändert werden.***

Dieser Fehler tritt auf, wenn Sie versuchen, `FileSystemId` zu ändern.  Das Ändern von `FileSystemdId` ist kein unterstützter Vorgang. 

* Ursache:   
Die ID des Dateisystems wird beim Erstellen des Volumes festgelegt. `FileSystemId` kann nachträglich nicht mehr geändert werden.
* Lösung:   
`FileSystemId` nicht in einen Patch und die Update (Put)-Anforderung aufnehmen.  Alternativ können Sie auch sicherstellen, dass `FileSystemId` in der Anforderung identisch ist.

***ActiveDirectory mit der ID: '{string}' existiert nicht.***

Der `{string}`-Teil ist der Wert, den Sie in der `ActiveDirectoryId`-Eigenschaft für die Active Directory-Verbindung eingegeben haben.

* Ursache:   
Wenn Sie ein Konto mit der Active Directory-Konfiguration erstellt haben, haben Sie einen Wert für `ActiveDirectoryId` eingegeben, der leer sein soll.
* Lösung:   
`ActiveDirectoryId` Erstellen-(Put)-Anforderung aufnehmen.

***Ungültige API-Version.***

Die API-Version ist entweder nicht übermittelt oder enthält einen ungültigen Wert.

* Ursache:   
Der Wert im Query-Parameter `api-version` enthält einen ungültigen Wert.
* Lösung:   
Verwenden Sie den richtigen Wert für die API-Version.  Der Ressourcenanbieter unterstützt viele API-Versionen. Der Wert hat das Format yyyy-mm-dd.

***Ein ungültiger Wert '{value}' wurde für {1} empfangen.***

Diese Meldung zeigt einen Fehler in den Feldern für `RuleIndex`, `AllowedClients`, `UnixReadOnly`, `UnixReadWrite`, `Nfsv3` und `Nfsv4` an.

* Ursache:   
Bei mindestens einem der folgenden Felder ist bei der Eingabeüberprüfungsanforderung ein Fehler aufgetreten: `RuleIndex`, `AllowedClients`, `UnixReadOnly`, `UnixReadWrite`, `Nfsv`3 und `Nfsv4`.
* Lösung:   
Stellen Sie sicher, dass Sie alle erforderlichen und konfliktfreien Parameter über die Befehlszeile festlegen. Sie können z. B. nicht gleichzeitig die Parameter `UnixReadOnly` und `UnixReadWrite` festlegen.
* Problemumgehung:   
Weitere Informationen finden Sie oben unter „Lösung“.

***IP-Bereich{0} bis {1} für vlan {2} wird bereits verwendet***

Dieser Fehler tritt auf, weil die internen Datensätze der verwendeten IP-Bereiche einen Konflikt mit der neu zugewiesenen IP-Adresse haben.

* Ursache:   
Die für die Datenträgererstellung zugewiesene IP-Adresse ist bereits registriert.
Der Grund dafür könnte eine zuvor fehlgeschlagene Datenträgererstellung sein.
* Lösung:   
Wenden Sie sich an das Azure-Supportcenter.

***Fehlender Wert für '{property}'.***

Dieser Fehler zeigt an, dass in der Anforderung eine erforderliche Eigenschaft fehlt. Die Zeichenfolge {property} enthält den Namen der fehlenden Eigenschaft.

* Ursache:   
Bei mindestens einer Eigenschaften ist bei der Eingabeüberprüfungsanforderung ein Fehler aufgetreten.
* Lösung:   
Achten Sie darauf, dass Sie alle erforderlichen und konfliktfreien Eigenschaften in der Anforderung einstellen, insbesondere die Eigenschaft aus der Fehlermeldung.

***MountTargets kann nicht geändert werden.***

Dieser Fehler tritt auf, wenn ein Benutzer versucht, die Volume-Eigenschaft „MountTargets“ zu aktualisieren oder zu patchen.

* Ursache:   
Sie versuchen, die Eigenschaft `MountTargets` des Volumes zu aktualisieren. Die Änderung dieser Eigenschaft wird nicht unterstützt.
* Lösung:   
`MountTargets` nicht in einen Patch und die Update (Put)-Anforderung aufnehmen.  Alternativ können Sie auch sicherstellen, dass `MountTargets` in der Anforderung identisch ist.

***Name wird bereits verwendet.***

Dieser Fehler zeigt an, dass der Name der Ressource bereits verwendet wird.

* Ursache:   
Sie versuchen, eine Ressource mit einem Namen zu erstellen, der bereits für eine vorhandene Ressource verwendet wird.
* Lösung:   
Verwenden Sie beim Erstellen einer Ressource einen eindeutigen Namen.

***Der Dateipfad wird bereits verwendet.***

Dieser Fehler zeigt an, dass der Dateipfad für das Volume bereits verwendet wird.

* Ursache:   
Sie versuchen, ein Volume mit einem Dateipfad zu erstellen, der mit einer vorhandenen Volume identisch ist.
* Lösung:   
Verwenden Sie beim Erstellen des Volumes einen eindeutigen Dateipfad.

***Name ist zu lang.***

Dieser Fehler zeigt an, dass der Ressourcenname nicht der erforderlichen Höchstlänge entspricht.

* Ursache:   
Der Ressourcenname ist zu lang.
* Lösung:   
Geben Sie einen kürzeren Namen für die Ressource ein.

***Dateipfad ist zu lang.***

Dieser Fehler zeigt an, dass der Dateipfad für das Volume nicht der erforderlichen Höchstlänge entspricht.

* Ursache:   
Der Datei des Volumes ist zu lang.
* Lösung:   
Verwenden Sie einen kürzeren Dateipfad.

***Name ist zu kurz.***

Dieser Fehler zeigt an, dass der Ressourcenname nicht der erforderlichen Mindestlänge entspricht.

* Ursache:   
Der Ressourcenname ist zu kurz.
* Lösung:   
Geben Sie einen längeren Namen für die Ressource ein.

***Dateipfad ist zu kurz.***

Dieser Fehler zeigt an, dass der Dateipfad nicht der erforderlichen Mindestlänge entspricht.

* Ursache:   
Der Datei des Volumes ist zu kurz.
* Lösung:   
Erweitern Sie die Länge des Dateifpads des Volumes.

***Azure NetApp Files-API nicht erreichbar.***

Die Azure API basiert zur Verwaltung von Volumes auf der Azure NetApp Files-API. Dieser Fehler zeigt ein Problem mit der API-Verbindung an.

* Ursache:   
Die zugrunde liegende API antwortet nicht, was zu einem internen Fehler führt. Dieser Fehler ist wahrscheinlich nur temporär.
* Lösung:   
Das Problem ist wahrscheinlich nur temporär. Die Anforderung sollte nach einiger Zeit erfolgreich durchgeführt werden.
* Problemumgehung:   
None (Keine): Die zugrunde liegende API ist für die Volumenverwaltung unerlässlich.

***Keine Vorgangsergebnis-ID gefunden für „{0}“.***

Dieser Fehler zeigt an, dass ein interner Fehler den Abschluss des Vorgangs verhindert.

* Ursache:   
Ein interner Fehler ist aufgetreten und hat verhindert, dass der Vorgang abgeschlossen wurde.
* Lösung:   
Dieser Fehler ist wahrscheinlich nur temporär. Warten Sie einige Minuten, und versuchen Sie erneut. Wenn das Problem weiterhin besteht, erstellen Sie ein Ticket, damit der technische Support das Problem untersuchen kann.
* Problemumgehung:   
Warten Sie einige Minuten und überprüfen Sie, ob das Problem weiterhin besteht.

***Die Kombination der Protokolltypen CIFS und NFS ist nicht zulässig***

Dieser Fehler tritt auf, wenn Sie ein Volume erstellen und in den Volumeeigenschaften sowohl die Protokolltypen CIFS (SMB) als auch NFS vorhanden sind.

* Ursache:   
Die Protokolltypen CIFS (SMB) und NFS werden beide in den Volumeeigenschaften verwendet.
* Lösung:   
Entfernen Sie einen der Protokolltypen.
* Problemumgehung:   
Belassen Sie die Protokolltypeigenschaft leer oder NULL.

***Anzahl der Elemente: {Value} für das Objekt: ExportPolicy.Rules [RuleIndex] liegt außerhalb des Min-Max-Bereichs.***

Dieser Fehler tritt auf, wenn die Regeln der Exportrichtlinien nicht den Anforderungen an den minimalen oder maximalen Bereich entsprechen. Wenn Sie die Exportrichtlinie definieren, muss sie mindestens über eine Exportrichtlinienregel und darf maximal über fünf Exportrichtlinienregeln verfügen.

* Ursache:   
Die von Ihnen definierte Exportrichtlinie entspricht nicht dem erforderlichen Bereich.
* Lösung:   
Stellen Sie sicher, dass der Index nicht bereits verwendet wird und im Bereich von 1 bis 5 liegt.
* Problemumgehung:   
Es ist nicht zwingend erforderlich, die Exportrichtlinien auf die Volumes anzuwenden. Sie können die Exportrichtlinie vollständig auslassen, wenn Sie keine Exportrichtlinienregeln benötigen.

***Nur ein aktives Verzeichnis erlaubt***

Dieser Fehler tritt auf, wenn Sie versuchen, eine Active Directory-Konfiguration zu erstellen, und für das Abonnement in der Region bereits eine existiert. Der Fehler kann auch auftreten, wenn Sie versuchen, mehr als eine Active Directory-Konfiguration zu erstellen.

* Ursache:   
Sie versuchen, ein aktives Verzeichnis zu erstellen (nicht zu aktualisieren), aber es existiert bereits eines.
* Lösung:   
Wenn die Active Directory-Konfiguration nicht verwendet wird, können Sie zunächst die bestehende Konfiguration löschen und dann den Erstellungsvorgang erneut ausführen.
* Problemumgehung:   
None (Keine): Nur ein Active Directory erlaubt.

***Der Vorgang '{operation}' wird nicht unterstützt.***

Dieser Fehler zeigt an, dass der Vorgang für das aktive Abonnement oder die aktive Ressource nicht verfügbar ist.

* Ursache:   
Der Vorgang ist für das Abonnement oder die Ressource nicht verfügbar.
* Lösung:   
Stellen Sie sicher, dass der Vorgang ordnungsgemäß eingegeben wurde und für die Ressource und das Abonnement, das Sie verwenden, verfügbar ist.

***OwnerId kann nicht geändert werden***

Dieser Fehler tritt auf, wenn Sie versuchen, die Eigenschaft „OwnerId“ des Volumes zu ändern. Das Ändern der OwnerId ist kein unterstützter Vorgang. 

* Ursache:   
Die Eigenschaft `OwnerId` wird beim Erstellen des Volumes festgelegt. Die Eigenschaft kann nachträglich nicht mehr geändert werden.
* Lösung:   
`OwnerId` nicht in einen Patch und die Update (Put)-Anforderung aufnehmen. Alternativ können Sie auch sicherstellen, dass `OwnerId` in der Anforderung identisch ist.

***Übergeordneter Pool nicht gefunden***

Dieser Fehler tritt auf, wenn Sie versuchen, ein Volume anzulegen und der Kapazitätspool, in dem Sie das Volume anlegen, nicht gefunden wird.

* Ursache:   
Der Kapazitätspool, in dem das Volume angelegt wird, wird nicht gefunden.
* Lösung:   
Wahrscheinlich war der Pool zum Zeitpunkt der Datenträgererstellung noch nicht vollständig angelegt oder bereits gelöscht.

***Patchvorgang wird für diesen Ressourcentyp nicht unterstützt.***

Dieser Fehler tritt auf, wenn Sie versuchen, das Einbindungsziel oder die Momentaufnahme zu ändern.

* Ursache:   
Das Einbindungsziel wird bereits beim Erstellen definiert und kann nachträglich nicht mehr geändert werden.
Die Momentaufnahmen enthalten keine Eigenschaften, die geändert werden können.
* Lösung:   
None (Keine): Diese Ressourcen haben keine Eigenschaften, die geändert werden können.

***Poolgröße zu klein für die Gesamtvolumengröße.***

Dieser Fehler tritt auf, wenn Sie die Größe des Kapazitätspools aktualisieren, und die Größe ist kleiner als der Gesamtwert der `usedBytes` aller Datenträger in diesem Kapazitätspool.  Dieser Fehler kann auch auftreten, wenn Sie ein neues Volume erstellen oder die Größe eines bestehenden Volume ändern, und die neue Volumegröße den freien Speicherplatz im Kapazitätspool überschreitet.

* Ursache:   
Sie versuchen, den Kapazitätspool auf eine kleinere Größe als „usedBytes“ in allen Volumes des Kapazitätspools zu aktualisieren.  Oder Sie versuchen, ein Volume anzulegen, das größer ist als der vorhandene Platz im Kapazitätspool.  Oder aber Sie versuchen, die Größe eines Volumes zu ändern, und die neue Größe überschreitet den verfügbaren Platz im Kapazitätspool.
* Lösung:   
Stellen Sie die Größe des Kapazitätspools auf einen größeren Wert ein oder erstellen Sie ein kleineres Volume für ein Volume.
* Problemumgehung:   
Entfernen Sie genügend Volumes, damit die Größe des Kapazitätspools auf diese Größe aktualisiert werden kann.

***Die Eigenschaft: „Speicherort“ für die Momentaufnahme muss mit dem Volume übereinstimmen***

Dieser Fehler tritt auf, wenn Sie eine Momentaufnahme mit einem anderen Speicherort als dem Volume erstellen, das Eigentümer der Momentaufnahme ist.

* Ursache:   
Ungültiger Wert in der Eigenschaft „Speicherort“ für die Momentaufnahme.
* Lösung:   
Legen Sie gültige Zeichenfolge in der Eigenschaft „Speicherort“ fest.

***Der Name „{ResourceType}“ muss mit dem Namen des Ressourcenbezeichners übereinstimmen.***

Dieser Fehler tritt auf, wenn Sie eine Ressource erstellen und in die Eigenschaft „Name“ einen Wert eintragen, der sich von dem in der Eigenschaft „Name“ von `resourceId` unterscheidet.

* Ursache:   
Ungültiger Wert in der Eigenschaft „Name“ beim Erstellen einer Ressource.
* Lösung:   
Lassen Sie die Eigenschaft „Name“ leer oder ermöglichen Sie, dass sie den gleichen Wert wie die Eigenschaft „Name“ (zwischen dem letzten Backslash „/“ und dem Fragezeichen „?“) in `resourceId` verwendet.

***Der Protokolltyp „{value}“ ist nicht bekannt***

Dieser Fehler tritt auf, wenn Sie ein Volume mit einem unbekannten Protokolltyp erstellen.  Gültige Werte sind „NFSv3“ und „CIFS“.

* Ursache:   
Sie versuchen, einen ungültigen Wert in der Volume-Eigenschaft `protocolType` festzulegen.
* Lösung:   
Legen Sie eine gültige Zeichenfolge in `protocolType` fest.
* Problemumgehung:   
Legen Sie `protocolType` als NULL fest.

***Protokolltypen können nicht geändert werden***

Dieser Fehler tritt auf, wenn Sie versuchen, `ProtocolType` für ein Volume zu aktualisieren oder zu patchen.  Das Ändern von ProtocolType ist kein unterstützter Vorgang.

* Ursache:   
Die Eigenschaft `ProtocolType` wird beim Erstellen des Volumes festgelegt.  Sie kann nicht aktualisiert werden.
* Lösung:   
None (Keine):
* Problemumgehung:   
Erstellen Sie ein weiteres Volume mit neuen Protokolltypen.

***Das Erstellen der Ressource vom Typ „{resourceType}“ würde das Kontingent der „{quota}“ Ressourcen vom Typ „{resourceType}“ pro „{parentResourceType}“ überschreiten. Die aktuelle Ressourcenanzahl liegt bei „{currentCount}“. Bitte löschen Sie einige Ressourcen dieses Typs, bevor Sie eine neue erstellen.***

Dieser Fehler tritt auf, wenn Sie versuchen, eine Ressource anzulegen (`NetAppAccount`, `CapacityPool`, `Volume` oder`Snapshot`), aber Ihr Kontingent seine Grenzen erreicht hat.

* Ursache:   
Sie versuchen, eine Ressource anzulegen, aber das Kontingent ist erreicht (Beispiel: `NetAppAccounts` pro Abonnement oder `CapacityPools` pro `NetAppAccount`).
* Lösung:   
Erhöhen Sie das Kontingent.
* Problemumgehung:   
Löschen Sie ungenutzte Ressourcen des gleichen Typs und legen Sie sie erneut an.

***Es wurde ein Wert für die schreibgeschützte Eigenschaft „{propertyName}“ empfangen.***

Dieser Fehler tritt auf, wenn Sie einen Wert für eine Eigenschaft definieren, der nicht geändert werden kann. So können Sie z. B. die Volume-ID nicht ändern.

* Ursache:   
Sie versuchen, einen Parameter (z. B. die Volume-ID) zu ändern, der nicht geändert werden kann.
* Lösung:   
Ändern Sie den Wert für die Eigenschaft nicht.

***Die angeforderte Ressource wurde nicht gefunden.***

Dieser Fehler tritt auf, wenn Sie versuchen, auf eine nicht vorhandene Ressource zu verweisen, z. B. ein Volume oder eine Momentaufnahme. Die Ressource wurde möglicherweise gelöscht oder weist einen falsch geschriebenen Ressourcennamen auf.

* Ursache:   
Sie versuchen, auf eine nicht vorhandene Ressource (z. B. ein Volume oder eine Momentaufnahme) zu verweisen, die bereits gelöscht wurde oder einen falsch geschriebenen Ressourcennamen aufweist.
* Lösung:   
Überprüfen Sie die Anforderung auf Rechtschreibfehler, um sicherzustellen, dass sie ordnungsgemäß referenziert wird.
* Problemumgehung:   
Weitere Informationen finden Sie oben im Abschnitt „Lösung“.

***Servicelevel „{volumeServiceLevel}“ ist höher als der übergeordnete „{poolServiceLevel}“***

Dieser Fehler tritt auf, wenn Sie ein Volume anlegen oder aktualisieren und Sie den Servicelevel auf ein höheres Niveau als den Kapazitätspool eingestellt haben, der es enthält.

* Ursache:   
Sie versuchen, ein Volume mit einem höher eingestuften Servicelevel als dem des übergeordneten Kapazitätspool zu erstellen oder zu aktualisieren.
* Lösung:   
Setzen Sie den Servicelevel auf den gleichen oder einen niedrigeren Rang als den des übergeordneten Kapazitätspools.
* Problemumgehung:   
Legen Sie das Volume in einem anderen Kapazitätspool mit einem passenden Servicelevel an. Alternativ können Sie alle Datenträger aus dem Kapazitätspool löschen und den Servicelevel für den Kapazitätspool auf einen höheren Rang setzen.

***Der Name des SMB-Servers darf nicht länger als 10 Zeichen sein.***

Dieser Fehler tritt auf, wenn Sie eine Active Directory-Konfiguration für ein Konto erstellen oder aktualisieren.

* Ursache:   
Die Länge des SMB-Servernamens beträgt mehr als 10 Zeichen.
* Lösung:   
Verwenden Sie einen kürzeren Servernamen. Die maximale Länge beträgt 10 Zeichen.
* Problemumgehung:   
None (Keine):  Weitere Informationen finden Sie oben unter „Lösung“. 

***SubnetId kann nicht geändert werden.***

Dieser Fehler tritt bei dem Versuch auf, `subnetId` zu ändern, nachdem das Volume erstellt wurde.  `SubnetId` muss beim Erstellen des Volumes festgelegt werden und kann später nicht mehr geändert werden.

* Ursache:   
Sie versuchen, `subnetId` nach dem Erstellen des Volumes zu ändern. Dies ist kein unterstützter Vorgang. 
* Lösung:   
Wenn Sie `subnetId` nicht ändern müssen, dann können Sie den Parameter aus der Anforderung entfernen, um die Fehlermeldung zu schließen.
* Problemumgehung:   
Wenn Sie `subnetId` ändern müssen, können Sie ein neues Volume mit einem neuen `subnetId` erstellen und die Daten dann auf das neue Volume migrieren.

***SubnetId hat kein gültiges Format.***

Dieser Fehler tritt auf, wenn Sie versuchen, ein neues Volume zu erstellen, aber die `subnetId` ist nicht keine `resourceId` für ein Subnetz.

* Ursache:   
Dieser Fehler tritt auf, wenn Sie versuchen, ein neues Volume zu erstellen, aber die `subnetId` ist nicht keine `resourceId` für ein Subnetz. 
* Lösung:   
Überprüfen Sie den Wert für `subnetId`, um sicherzustellen, dass er eine `resourceId` für das verwendete Subnetz enthält.
* Problemumgehung:   
None (Keine): Weitere Informationen finden Sie oben unter „Lösung“. 

***Das Subnetz muss über eine Delegatierung von Microsoft.NetApp/volumes verfügen.***

Dieser Fehler tritt auf, wenn Sie ein Volume erstellen und das ausgewählte Subnetz nicht an `Microsoft.NetApp/volumes` delegiert wird.

* Ursache:   
Sie haben versucht, ein Volume anzulegen und ein Subnetz ausgewählt, das nicht an `Microsoft.NetApp/volumes` delegiert ist.
* Lösung:   
Wählen Sie ein anderes Subnetz aus, das an `Microsoft.NetApp/volumes` delegiert wird.
* Problemumgehung:   
Fügen Sie dem Subnetz eine korrekte Delegatierung hinzu.

***Der angegebene Ressourcentyp ist unbekannt/nicht anwendbar.***

Dieser Fehler tritt auf, wenn eine Namensprüfung entweder für einen nicht anwendbaren Ressourcentyp oder für einen unbekannten Ressourcentyp angefordert wurde.

* Ursache:   
Die Namensprüfung wurde für einen unbekannten oder nicht unterstützten Ressourcentyp angefordert.
* Lösung:   
Überprüfen Sie, ob die Ressource, für die Sie die Anforderung stellen, unterstützt wird und keine Rechtschreibfehler enthält.
* Problemumgehung:   
Weitere Informationen finden Sie oben unter „Lösung“.

***Unbekannter Azure NetApp Files-Fehler.***

Die Azure API basiert zur Verwaltung von Volumes auf der Azure NetApp Files-API. Der Fehler zeigt ein Problem bei der Kommunikation mit der API an.

* Ursache:   
Die zugrunde liegende API sendet einen unbekannten Fehler. Dieser Fehler ist wahrscheinlich nur temporär.
* Lösung:   
Das Problem ist wahrscheinlich nur temporär, und die Anforderung sollte nach einiger Zeit erfolgreich durchgeführt werden. Wenn das Problem weiterhin besteht, erstellen Sie ein Supportticket, um das Problem untersuchen zu lassen.
* Problemumgehung:   
None (Keine): Die zugrunde liegende API ist für die Volumenverwaltung unerlässlich.

***Empfangener Wert für eine unbekannte Eigenschaft „{propertyName}“.***

Dieser Fehler tritt auf, wenn nicht vorhandene Eigenschaften für eine Ressource wie Volume, Momentaufnahme oder Einbindungsziel bereitgestellt werden.

* Ursache:   
Die Anforderung verfügt über eine Reihe von Eigenschaften, die für die einzelnen Ressourcen verwendet werden können. Sie können keine nicht vorhandenen Eigenschaften in die Anforderung aufnehmen.
* Lösung:   
Stellen Sie sicher, dass alle Eigenschaftsnamen richtig geschrieben und die Eigenschaften für das Abonnement und die Ressource verfügbar sind.
* Problemumgehung:   
Verringern Sie die Anzahl der in der Anforderung definierten Eigenschaften, um die Eigenschaft zu beseitigen, die den Fehler verursacht.

***Aktualisierungsvorgang wird für diesen Ressourcentyp nicht unterstützt.***

Es können nur Volumes aktualisiert werden. Dieser Fehler tritt auf, wenn Sie versuchen, einen nicht unterstützten Aktualisierungsvorgang durchzuführen, z. B. das Aktualisieren einer Momentaufnahme.

* Ursache:   
Die Ressource, die Sie zu aktualisieren versuchen, unterstützt den Aktualisierungsvorgang nicht. Lediglich die Eigenschaften von Volumes können geändert werden.
* Lösung:   
None (Keine): Die Ressource, die Sie zu aktualisieren versuchen, unterstützt den Aktualisierungsvorgang nicht. Somit kann sie nicht geändert werden.
* Problemumgehung:   
Erstellen Sie für ein Volume eine neue Ressource mit vorhandenem Update, und migrieren Sie die Daten.

***Ein Volume kann nicht in einem Pool angelegt werden, der nicht über den Status „erfolgreich“ verfügt.***

Dieser Fehler tritt auf, wenn Sie versuchen, ein Volume in einem Pool anzulegen, der sich nicht im erfolgreichen Zustand befindet. Wahrscheinlich ist der Erstellungsvorgang für den Kapazitätspool aus irgendeinem Grund fehlgeschlagen.

* Ursache:   
Der Kapazitätspool, der das neue Volume enthält, befindet sich in einem fehlerhaften Zustand.
* Lösung:   
Überprüfen Sie, ob der Kapazitätspool erfolgreich angelegt wurde und sich nicht in einem fehlerhaften Zustand befindet.
* Problemumgehung:   
Legen Sie einen neuen Kapazitätspool an und legen Sie das Volumen im neuen Pool an.

***Das Volume wird gerade erstellt und kann derzeit nicht gelöscht werden.***

Dieser Fehler tritt auf, wenn Sie versuchen, ein Volume zu löschen, das sich noch in der Erstellung befindet.

* Ursache:   
Ein Volume wird noch erstellt, wenn Sie versuchen, das Volume zu löschen.
* Lösung:   
Warten Sie, bis die Datenträgererstellung abgeschlossen ist, und versuchen Sie dann erneut, den Löschvorgang durchzuführen.
* Problemumgehung:   
Weitere Informationen finden Sie oben unter „Lösung“.

***Das Volume wird gelöscht und kann derzeit nicht gelöscht werden.***

Dieser Fehler tritt auf, wenn Sie versuchen, ein Volume zu löschen, während es bereits gelöscht wird.

* Ursache:   
Ein Volume wird bereits gelöscht, während Sie versuchen, das Volume zu löschen.
* Lösung:   
Warten Sie, bis der aktuelle Löschvorgang abgeschlossen ist.
* Problemumgehung:   
Weitere Informationen finden Sie oben unter „Lösung“.

***Das Volume wird gerade aktualisiert und kann derzeit nicht gelöscht werden.***

Dieser Fehler tritt auf, wenn Sie versuchen, ein Volume zu löschen, das gerade aktualisiert wird.

* Ursache:   
Ein Volume wird aktualisieren, während Sie versuchen, das Volume zu löschen.
* Lösung:   
Warten Sie, bis die Aktualisierung ist, und versuchen Sie dann erneut, den Löschvorgang durchzuführen.
* Problemumgehung:   
Weitere Informationen finden Sie oben unter „Lösung“.

***Volume wurde nicht gefunden oder wurde nicht erfolgreich erstellt.***

Dieser Fehler tritt auf, wenn die Erstellung des Volumes fehlgeschlagen ist und Sie versuchen, das Volume zu ändern oder eine Momentaufnahme für das Volume zu erstellen.

* Ursache:   
Das Volume existiert nicht, oder die Erstellung ist fehlgeschlagen.
* Lösung:   
Überprüfen Sie, ob Sie das richtige Volume ändern und ob die Erstellung des Volume erfolgreich war. Oder überprüfen Sie, ob das Volume, für das Sie eine Momentaufnahme erstellen, existiert.
* Problemumgehung:   
None (Keine):  Weitere Informationen finden Sie oben unter „Lösung“. 

***Das angegebene Erstellungs-Token ist bereits vorhanden***

Dieser Fehler tritt auf, wenn Sie versuchen, ein Volume zu erstellen, und Sie ein Erstellungs-Token (Exportpfad) angeben, für das bereits ein Volume existiert.

* Ursache:   
Das Erstellungs-Token (Exportpfad), das Sie bei der Datenträgererstellung angegeben haben, ist bereits einem anderen Datenträger zugeordnet. 
* Lösung:   
Wählen Sie ein anderes Erstellungs-Token.  Alternativ können Sie auch das andere Volume löschen.

***Das angegebene Erstellungs-Token ist reserviert***

Dieser Fehler tritt auf, wenn Sie versuchen, ein Volume zu erstellen, und als Dateipfad „default“ oder „none“ angeben (Erstellungs-Token).

* Ursache:    
Sie versuchen, ein Volume zu erstellen, und haben als Dateipfad „default“ oder „none“ angegeben (Erstellungs-Token).
* Lösung:   
Wählen Sie einen anderen Dateipfad (Erstellungs-Token).
 
***Active Directory-Anmeldeinformationen werden verwendet***

Dieser Fehler tritt auf, wenn Sie versuchen, die Active Directory-Konfiguration aus einem Konto zu löschen, in dem noch mindestens ein SMB-Volume vorhanden ist.  Das SMB-Volume wurde unter Verwendung der Active Directory-Konfiguration erstellt, die Sie zu löschen versuchen.

* Ursache:   
Sie versuchen, die Active Directory-Konfiguration aus einem Konto zu löschen, aber es existiert noch mindestens ein SMB-Volume, das ursprünglich mit der Active Directory-Konfiguration erstellt wurde. 
* Lösung:   
Löschen Sie zunächst alle SMB-Volumes, die mit der Active Directory-Konfiguration erstellt wurden.  Versuchen Sie dann erneut, die Konfiguration zu löschen.

***Die Zuordnung der Organisationseinheit kann nicht geändert werden, wenn die Anmeldeinformationen verwendet werden***

Dieser Fehler tritt auf, wenn Sie versuchen, die Organisationseinheit einer Active Directory-Konfiguration zu ändern, aber mindestens ein SMB-Volume noch vorhanden ist.  Das SMB-Volume wurde unter Verwendung der Active Directory-Konfiguration erstellt, die Sie zu löschen versuchen.

* Ursache:   
Sie versuchen, die Organisationseinheit einer Active Directory-Konfiguration zu ändern.  Es existiert jedoch noch mindestens ein SMB-Volume, das ursprünglich mithilfe der Active Directory-Konfiguration erstellt wurde.
* Lösung:   
 Löschen Sie zunächst alle SMB-Volumes, die mit der Active Directory-Konfiguration erstellt wurden.  Versuchen Sie dann erneut, die Konfiguration zu löschen. 

***Active Directory-Aktualisierung wird bereits ausgeführt***

Dieser Fehler tritt auf, wenn Sie versuchen, eine Active Directory-Konfiguration zu bearbeiten, für die bereits ein Bearbeitungsvorgang ausgeführt wird.

* Ursache:   
Sie versuchen, eine Active Directory-Konfiguration zu bearbeiten, aber ein weiterer Bearbeitungsvorgang ist bereits im Gange.
* Lösung:   
Warten Sie, bis der gerade laufende Bearbeitungsvorgang abgeschlossen ist.

***Löschen Sie zuerst alle Volumes mit den ausgewählten Anmeldeinformationen***

Dieser Fehler tritt auf, wenn Sie versuchen, eine Active Directory-Konfiguration zu löschen, aber mindestens ein SMB-Volume noch vorhanden ist.  Das SMB-Volume wurde unter Verwendung der Active Directory-Konfiguration erstellt, die Sie zu löschen versuchen.

* Ursache:   
Sie versuchen, die Active Directory-Konfiguration zu löschen, aber es existiert noch mindestens ein SMB-Volume, das ursprünglich mit der Active Directory-Konfiguration erstellt wurde.
* Lösung:   
Löschen Sie zunächst alle SMB-Volumes, die mit der Active Directory-Konfiguration erstellt wurden.  Versuchen Sie dann erneut, die Konfiguration zu löschen. 

***Keine Active Directory-Anmeldeinformationen in der Region gefunden***

Dieser Fehler tritt auf, wenn Sie versuchen, ein SMB-Volume zu erstellen, aber dem Konto für die Region keine Active Directory-Konfiguration hinzugefügt wurde.

* Ursache:   
Sie versuchen, ein SMB-Volume zu erstellen, aber dem Konto wurde keine Active Directory-Konfiguration hinzugefügt. 
* Lösung:   
Fügen Sie dem Konto eine Active Directory-Konfiguration hinzu, bevor Sie ein SMB-Volume erstellen.

***DNS-Server konnte nicht abgefragt werden. Überprüfen Sie, ob die Netzwerkkonfiguration korrekt ist und ob DNS-Server verfügbar sind.***

Dieser Fehler tritt auf, wenn Sie versuchen, ein SMB-Volume zu erstellen, aber ein DNS-Server (der in Ihrer Active Directory-Konfiguration angegeben ist) nicht erreichbar ist. 

* Ursache:   
Sie versuchen, ein SMB-Volume zu erstellen, aber ein DNS-Server (der in Ihrer Active Directory-Konfiguration angegeben ist) ist nicht erreichbar.
* Lösung:   
Überprüfen Sie Ihre Active Directory-Konfiguration und stellen Sie sicher, dass die IP-Adressen der DNS-Server korrekt und erreichbar sind.
Wenn es keine Probleme mit den IP-Adressen des DNS-Servers gibt, stellen Sie sicher, dass keine Firewalls den Zugriff blockieren.

***Zu viele gleichzeitige Aufträge***

Dieser Fehler tritt auf, wenn Sie versuchen, eine Momentaufnahme zu erstellen, wenn drei weitere Vorgänge zur Erstellung von Momentaufnahmen für das Abonnement bereits ausgeführt werden.

* Ursache:   
Sie versuchen, eine Momentaufnahme zu erstellen, wenn drei weitere Vorgänge zur Erstellung von Momentaufnahmen für das Abonnement bereits ausgeführt werden. 
* Lösung:   
Die Erstellung von Momentaufnahmeaufträgen dauert höchstens ein paar Sekunden.  Warten Sie ein paar Sekunden und versuchen Sie erneut, die Erstellung der Momentaufnahme durchzuführen.

***Es können keine zusätzlichen Aufträge geschaffen werden. Bitte warten Sie, bis die laufenden Aufträge abgeschlossen sind und versuchen Sie es erneut.***

Dieser Fehler kann auftreten, wenn Sie versuchen, ein Volume unter bestimmten Umständen zu erstellen oder zu löschen.

* Ursache:   
Sie versuchen, unter bestimmten Umständen ein Volume zu erstellen oder zu löschen.
* Lösung:   
Warten Sie etwa eine Minute und versuchen Sie den Vorgang erneut.

***Das Volumen ist in einem Übergangszustand***

Dieser Fehler kann auftreten, wenn Sie versuchen, ein Volume zu löschen, das sich gerade in einem Übergangszustand befindet (d. h. sich gerade im Erstellungs-, Aktualisierungs- oder Löschzustand befindet).

* Ursache:   
Sie versuchen, ein Volume zu löschen, das sich derzeit in einem Übergangszustand befindet.
* Lösung:   
Warten Sie, bis der gerade laufende (zustandsüberwachende) Vorgang abgeschlossen ist, und führen Sie den Vorgang erneut durch.

***Das neue Volume konnte nicht von der Momentaufnahme des Quellvolumens getrennt werden***

 Dieser Fehler kann auftreten, wenn Sie versuchen, ein Volume aus einer Momentaufnahme zu erstellen.  

* Ursache:   
Sie versuchen, ein Volume aus einer Momentaufnahme zu erstellen, und das Volume endet in einem Fehlerzustand.
* Lösung:   
Löschen Sie das Volume und versuchen Sie dann erneut, das Volume aus der Momentaufnahme zu erstellen.

 
## <a name="next-steps"></a>Nächste Schritte

* [Entwickeln für Azure NetApp Files mit REST-API](azure-netapp-files-develop-with-rest-api.md)
