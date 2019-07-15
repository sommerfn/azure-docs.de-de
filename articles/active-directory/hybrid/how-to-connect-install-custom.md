---
title: 'Azure AD Connect: Benutzerdefinierte Installation | Microsoft-Dokumentation'
description: In diesem Dokument werden die Optionen für die benutzerdefinierte Installation für Azure AD Connect aufgeführt. Gehen Sie folgendermaßen vor, um Active Directory über Azure AD Connect zu installieren.
services: active-directory
keywords: Was ist Azure AD Connect, Active Directory installieren, erforderliche Komponenten für Azure AD
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 6d42fb79-d9cf-48da-8445-f482c4c536af
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/26/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 969d08888002c97453cb2af18f42371227676597
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109480"
---
# <a name="custom-installation-of-azure-ad-connect"></a>Benutzerdefinierte Installation von Azure AD Connect
Die **benutzerdefinierten Einstellungen** von Azure AD Connect werden verwendet, wenn Sie mehr Optionen für die Installation benötigen. Sie kommen zum Einsatz, wenn Sie über mehrere Gesamtstrukturen verfügen oder optionale Features konfigurieren möchten, die nicht Teil der Expressinstallation sind. Sie werden in allen Fällen verwendet, in denen die Option [**Expressinstallation**](how-to-connect-install-express.md) für Ihre Bereitstellung oder Topologie nicht ausreicht.

Vor dem Installieren von Azure AD Connect müssen Sie [Azure AD Connect](https://go.microsoft.com/fwlink/?LinkId=615771) herunterladen und die im folgenden Artikel beschriebenen Schritte zur Vorbereitung ausführen: [Azure AD Connect: Hardware und Voraussetzungen](how-to-connect-install-prerequisites.md). Stellen Sie außerdem sicher, dass die erforderlichen Konten verfügbar sind. Dies ist unter [AzureAD Connect-Konten und -Berechtigungen](reference-connect-accounts-permissions.md) beschrieben.

Wenn die benutzerdefinierten Einstellungen nicht zu Ihrer Topologie passen, z.B. in Bezug auf die DirSync-Aktualisierung, helfen Ihnen die Szenarios in der verwandten Dokumentation weiter.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Installation von Azure AD Connect mit benutzerdefinierten Einstellungen
### <a name="express-settings"></a>Expresseinstellungen
Klicken Sie auf dieser Seite auf **Anpassen** , um eine Installation mit benutzerdefinierten Einstellungen zu starten.

### <a name="install-required-components"></a>Installieren der erforderlichen Komponenten
Bei der Installation der Synchronisierungsdienste können Sie den optionalen Konfigurationsabschnitt deaktiviert lassen, sodass Azure AD Connect alles automatisch einrichtet. Azure AD Connect richtet eine SQL Server 2012 Express LocalDB-Instanz ein, erstellt die entsprechenden Gruppen und weist Berechtigungen zu. Wenn Sie die Standardeinstellungen ändern möchten, können Sie sich in der folgenden Tabelle über die verfügbaren optionalen Konfigurationsoptionen informieren.

![Erforderliche Komponenten](./media/how-to-connect-install-custom/requiredcomponents2.png)

| Optionale Konfiguration | BESCHREIBUNG |
| --- | --- |
| Verwenden eines vorhandenen SQL Servers |Ermöglicht Ihnen die Angabe des SQL-Servernamens und des Instanznamens. Wählen Sie diese Option aus, wenn Sie bereits über einen Datenbankserver verfügen, den Sie verwenden möchten. Geben Sie unter **Instanzname** den Instanznamen, ein Komma und die Portnummer ein, falls das Browsen für SQL Server nicht aktiviert ist.  Geben Sie dann den Namen der Azure AD Connect-Datenbank an.  Ihre SQL-Berechtigungen bestimmen, ob eine neue Datenbank erstellt wird oder Ihr SQL-Administrator die Datenbank im Voraus erstellen muss.  Ob Sie über SQL SA-Berechtigungen verfügen, erfahren Sie unter [Installieren mithilfe einer vorhandenen Datenbank](how-to-connect-install-existing-database.md).  Ob Sie delegierte Berechtigungen (DBO) erhalten haben, erfahren Sie unter [Installieren von Azure AD Connect mit delegierten SQL-Administratorrechten](how-to-connect-install-sql-delegation.md). |
| Verwenden eines vorhandenen Dienstkontos |Für Azure AD Connect wird standardmäßig ein virtuelles Dienstkonto für die Synchronisierungsdienste genutzt. Wenn Sie einen Remote-SQL Server oder einen Proxy mit Authentifizierungsanforderung verwenden, benötigen Sie ein **verwaltetes Dienstkonto** oder ein Dienstkonto in der Domäne und das Kennwort. Geben Sie in diesen Fällen das zu verwendende Konto ein. Stellen Sie sicher, dass der die Installation ausführende Benutzer ein SA in SQL ist, damit das Dienstkonto erstellt werden kann.  Weitere Informationen finden Sie unter [Azure AD Connect: Konten und Berechtigungen](reference-connect-accounts-permissions.md#adsync-service-account). </br></br>Mit dem neuesten Build kann der SQL-Administrator nun eine Out-of-Band-Datenbankbereitstellung ausführen, sodass die Datenbank anschließend vom Azure AD Connect-Administrator mit Datenbankbesitzerrechten installiert werden kann.  Weitere Informationen finden Sie unter [Install Azure AD Connect using SQL delegated administrator permissions](how-to-connect-install-sql-delegation.md) (Installieren von Azure AD Connect mit Berechtigungen eines delegierten SQL-Administrators).|
| Angeben benutzerdefinierter Synchronisierungsgruppen |Azure AD Connect erstellt beim Installieren der Synchronisierungsdienste standardmäßig vier lokale Gruppen auf dem Server. Diese Gruppen sind: Administratorengruppe, Operatorengruppe, Durchsuchen-Gruppe und die Gruppe „Kennwort zurücksetzen“. Hier können Sie Ihre eigenen Gruppen angeben. Die Gruppen müssen sich lokal auf dem Server befinden und dürfen nicht in der Domäne sein. |

### <a name="user-sign-in"></a>Benutzeranmeldung
Nach der Installation der erforderlichen Komponenten werden Sie aufgefordert, die Methode für das einmalige Anmelden Ihrer Benutzer auszuwählen. Die folgende Tabelle enthält eine kurze Beschreibung der verfügbaren Optionen. Eine vollständige Beschreibung der Anmeldemethoden finden Sie unter [Benutzeranmeldung](plan-connect-user-signin.md).

![Benutzeranmeldung](./media/how-to-connect-install-custom/usersignin4.png)

| Option zum einmaligen Anmelden | BESCHREIBUNG |
| --- | --- |
| Kennworthashsynchronisierung |Benutzer können sich bei Microsoft Cloud Services wie Office 365 mit dem Kennwort anmelden, das sie auch in ihrem lokalen Netzwerk verwenden. Die Benutzerkennwörter werden über einen Kennworthash mit Azure AD synchronisiert, und die Authentifizierung erfolgt in der Cloud. Weitere Informationen finden Sie unter [Kennworthashsynchronisierung](how-to-connect-password-hash-synchronization.md). |
|Passthrough-Authentifizierung|Benutzer können sich bei Microsoft Cloud Services wie Office 365 mit dem Kennwort anmelden, das sie auch in ihrem lokalen Netzwerk verwenden.  Das Benutzerkennwort wird zur Überprüfung an den lokalen Active Directory-Domänencontroller übergeben.
| Verbund mit AD FS |Benutzer können sich bei Microsoft Cloud Services wie Office 365 mit dem Kennwort anmelden, das sie auch in ihrem lokalen Netzwerk verwenden.  Die Benutzer werden für die Anmeldung jeweils an ihre lokale AD FS-Instanz umgeleitet, und die Authentifizierung erfolgt lokal. |
| Verbund mit PingFederate|Benutzer können sich bei Microsoft Cloud Services wie Office 365 mit dem Kennwort anmelden, das sie auch in ihrem lokalen Netzwerk verwenden.  Die Benutzer werden für die Anmeldung jeweils zu ihrer lokalen PingFederate-Instanz umgeleitet, und die Authentifizierung erfolgt lokal. |
| Nicht konfigurieren |Kein Feature für die Benutzeranmeldung wird installiert oder konfiguriert. Wählen Sie diese Option aus, wenn Sie bereits über einen Verbundserver eines Drittanbieters verfügen oder eine andere vorhandene Lösung eingesetzt wird. |
|Einmaliges Anmelden aktivieren|Diese Option ist sowohl mit Kennworthashsynchronisierung als auch mit Passthrough-Authentifizierung verfügbar und ermöglicht das einmalige Anmelden für Desktopbenutzer im Unternehmensnetzwerk. Weitere Informationen finden Sie unter [Einmaliges Anmelden](how-to-connect-sso.md). </br>Beachten Sie, dass diese Option für AD FS-Kunden nicht verfügbar ist, da AD FS bereits das einmalige Anmelden dieser Art ermöglicht</br>

### <a name="connect-to-azure-ad"></a>Stellen Sie eine Verbindung mit Azure AD her.
Geben Sie im Bildschirm "Mit Azure AD verbinden" ein Konto und ein Kennwort für den globalen Administrator ein. Wenn Sie auf der vorherigen Seite **Verbund mit AD FS** ausgewählt haben, melden Sie sich nicht mit einem Konto in einer Domäne an, die Sie für den Verbund aktivieren möchten. Wir empfehlen Ihnen die Verwendung eines Kontos in der standardmäßigen Domäne **onmicrosoft.com**, die in Ihrem Azure AD-Mandanten enthalten ist.

Dieses Konto dient ausschließlich der Erstellung eines Dienstkontos in Azure AD und wird nach Abschluss des Assistenten nicht mehr verwendet.  
![Benutzeranmeldung](./media/how-to-connect-install-custom/connectaad.png)

Wenn MFA für Ihr globales Administratorkonto aktiviert ist, müssen Sie das Kennwort im Anmelde-Popupfenster erneut eingeben und die MFA-Abfrage ausfüllen. Bei der Abfrage kann es sich etwa um die Eingabe eines Überprüfungscodes oder um einen Anruf handeln.  
![Benutzeranmeldung mit MFA](./media/how-to-connect-install-custom/connectaadmfa.png)

Für das globale Administratorkonto kann auch [Privileged Identity Management](../privileged-identity-management/pim-getting-started.md) aktiviert sein.

Falls ein Fehler auftritt und Sie Probleme mit der Konnektivität haben, können Sie unter [Problembehebung bei Konnektivitätsproblemen](tshoot-connect-connectivity.md) nach einer Lösung suchen.

## <a name="pages-under-the-sync-section"></a>Seiten im Abschnitt „Synchronisierung“

### <a name="connect-your-directories"></a>Verzeichnisse verbinden
Zum Herstellen einer Verbindung mit Ihren Active Directory Domain Services benötigt Azure AD Connect den Namen der Gesamtstruktur und die Anmeldeinformationen für ein Konto, das über ausreichende Berechtigungen verfügt.

![Verzeichnis verbinden](./media/how-to-connect-install-custom/connectdir01.png)

Nachdem Sie den Namen der Gesamtstruktur eingegeben und auf **Verzeichnis hinzufügen** geklickt haben, wird ein Popupdialogfeld angezeigt, in dem Ihnen folgende Optionen angeboten werden:

| Option | BESCHREIBUNG |
| --- | --- |
| Erstellen eines neuen Kontos | Wählen Sie diese Option aus, wenn der Azure AD Connect-Assistent das AD DS-Konto erstellen soll, das von Azure AD Connect beim Synchronisieren der Verzeichnisse für die Verbindung mit der AD-Gesamtstruktur benötigt wird. Wenn Sie diese Option ausgewählt haben, geben Sie den Benutzernamen und das Kennwort für ein Administratorkonto des Unternehmens ein. Das angegebene Administratorkonto des Unternehmens wird vom Azure AD-Assistenten verwendet, um das erforderliche AD DS-Konto zu erstellen. Sie können den Domänenteil entweder im NetBIOS- oder FQDN-Format eingeben, also „FABRIKAM\administrator“ oder „fabrikam.com\administrator“. |
| Vorhandenes Konto verwenden | Wählen Sie diese Option aus, wenn Sie ein vorhandenes AD DS-Konto angeben möchten, das von Azure AD Connect beim Synchronisieren der Verzeichnisse für die Verbindung mit der AD-Gesamtstruktur verwendet werden soll. Sie können den Domänenteil entweder im NetBIOS- oder FQDN-Format eingeben, also „FABRIKAM\syncuser“ oder „fabrikam.com\syncuser“. Dieses Konto kann ein normales Benutzerkonto sein, da nur die standardmäßigen Leseberechtigungen benötigt werden. Abhängig von Ihrem Szenario benötigen Sie jedoch möglicherweise weitere Berechtigungen. Weitere Informationen finden Sie unter [Azure AD Connect: Konten und Berechtigungen](reference-connect-accounts-permissions.md##create-the-ad-ds-connector-account). |

![Verzeichnis verbinden](./media/how-to-connect-install-custom/connectdir02.png)

### <a name="azure-ad-sign-in-configuration"></a>Konfiguration der Azure AD-Anmeldung
Auf dieser Seite können Sie die UPN-Domänen anzeigen, die in der lokalen AD DS-Instanz vorhanden sind und in Azure AD überprüft wurden. Darüber hinaus können Sie auf dieser Seite das Attribut für „userPrincipalName“ konfigurieren.

![Nicht überprüfte Domänen](./media/how-to-connect-install-custom/aadsigninconfig2.png)  
Überprüfen Sie alle Domänen, die als **Nicht hinzugefügt** und **Nicht überprüft** markiert sind. Stellen Sie sicher, dass die verwendeten Domänen in Azure AD überprüft wurden. Klicken Sie auf das Symbol zum Aktualisieren, wenn Sie Ihre Domänen überprüft haben. Weitere Informationen finden Sie unter [Hinzufügen und Überprüfen der Domäne](../active-directory-domains-add-azure-portal.md).

**userPrincipalName** : Das userPrincipalName-Attribut wird von Benutzern verwendet, wenn sie sich bei Azure AD und Office 365 anmelden. Die verwendeten Domänen, auch als UPN-Suffix bezeichnet, sollte in Azure AD überprüft werden, bevor die Benutzer synchronisiert werden. Microsoft empfiehlt, das Standardattribut „userPrincipalName“ beizubehalten. Wenn dieses Attribut nicht routingfähig ist und nicht überprüft werden kann, können Sie ein anderes Attribut auswählen. So können Sie beispielsweise „email“ als Attribut mit der Anmelde-ID verwenden. Wenn Sie ein anderes Attribut als „userPrincipalName“ verwenden, wird dieses als **alternative ID**bezeichnet. Der Attributwert der alternativen ID muss dem RFC822-Standard entsprechen. Eine alternative ID kann mit Kennworthashsynchronisierung, Pass-Through-Authentifizierung und Verbund verwendet werden. Das Attribut darf in Active Directory nicht als mehrwertiges Attribut definiert werden, auch wenn es nur einen einzelnen Wert hat.

>[!NOTE]
> Beim Aktivieren der Passthrough-Authentifizierung müssen Sie mindestens über eine verifizierte Domäne verfügen, um im Assistenten fortfahren zu können.

> [!WARNING]
> Eine alternative ID ist nicht mit allen Office 365-Workloads kompatibel. Weitere Informationen finden Sie unter [Konfigurieren der alternativen Anmelde-ID](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id).
>
>

### <a name="domain-and-ou-filtering"></a>Filterung von Domänen und Organisationseinheiten
Standardmäßig werden alle Domänen und Organisationseinheiten synchronisiert. Wenn Sie Domänen oder Organisationseinheiten nicht in Azure AD synchronisieren möchten, können Sie diese Domänen und Organisationseinheiten deaktivieren.  
![Domänen und Organisationseinheiten filtern](./media/how-to-connect-install-custom/domainoufiltering.png)  
Diese Seite des Assistenten dient zum Konfigurieren der domänenbasierten und OE-basierten Filterung. Wenn Sie Änderungen vornehmen möchten, ist ratsam, vorher die Informationen in den Abschnitten zur [domänenbasierten Filterung](how-to-connect-sync-configure-filtering.md#domain-based-filtering) und [OE-basierten Filterung](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) zu lesen. Einige OEs sind für die Funktionalität sehr wichtig und sollten nicht deaktiviert werden.

Bei Verwendung der OE-basierten Filterung mit einer Azure AD Connect-Version vor 1.1.524.0 werden neue Organisationseinheiten, die später hinzugefügt werden, standardmäßig synchronisiert. Falls neue OEs nicht synchronisiert werden sollen, können Sie dies konfigurieren, nachdem der Assistent die [OE-basierte Filterung](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) abgeschlossen hat. Bei Azure AD Connect, Version 1.1.524.0 oder höher, können Sie angeben, ob neue OEs synchronisiert werden sollen oder nicht.

Wenn Sie die Verwendung der [gruppenbasierten Filterung](#sync-filtering-based-on-groups) planen, sollten Sie sicherstellen, dass die OE mit der Gruppe eingebunden ist und dass dafür nicht die OE-Filterung verwendet wird. Die OE-Filterung wird vor der gruppenbasierten Filterung ausgewertet.

Möglicherweise sind bestimmte Domänen aufgrund von Beschränkungen der Firewall nicht erreichbar. Diese Domänen sind standardmäßig nicht ausgewählt und mit einer Warnung versehen.  
![Nicht erreichbare Domänen](./media/how-to-connect-install-custom/unreachable.png)  
Falls diese Warnung angezeigt wird, sollten Sie sich vergewissern, dass die entsprechenden Domänen tatsächlich nicht erreichbar sind und die Warnung zu erwarten ist.

### <a name="uniquely-identifying-your-users"></a>Eindeutige Identifizierung der Benutzer

#### <a name="select-how-users-should-be-identified-in-your-on-premises-directories"></a>Auswählen, wie Benutzer in Ihren lokalen Verzeichnissen identifiziert werden sollen
Mit dem Feature zum Abgleich über Gesamtstrukturen können Sie definieren, wie Benutzer Ihrer AD DS-Gesamtstrukturen in Azure AD repräsentiert werden. Ein Benutzer kann entweder nur einmal in allen Gesamtstrukturen vorhanden sein oder über eine Kombination aus aktivierten und deaktivierten Konten verfügen. In bestimmten Gesamtstrukturen wird der Benutzer unter Umständen auch als Kontakt dargestellt.

![Eindeutig](./media/how-to-connect-install-custom/unique2.png)

| Einstellung | BESCHREIBUNG |
| --- | --- |
| [Benutzer sind nur einmal in allen Gesamtstrukturen vorhanden.](plan-connect-topologies.md#multiple-forests-single-azure-ad-tenant) |Alle Benutzer werden in Azure AD jeweils als einzelne Objekte erstellt. Die Objekte werden nicht im Metaverse verknüpft. |
| [E-Mail-Attribut](plan-connect-topologies.md#multiple-forests-single-azure-ad-tenant) |Diese Option verknüpft Benutzer und Kontakte, wenn dieses Attribut in verschiedenen Gesamtstrukturen denselben Wert aufweist. Verwenden Sie diese Option, wenn Ihre Kontakte mit GALSync erstellt wurden. Bei Verwendung dieser Option werden Benutzerobjekte, deren E-Mail-Attribut nicht aufgefüllt ist, nicht mit Azure AD synchronisiert. |
| [ObjectSID und msExchangeMasterAccountSID/ msRTCSIP-OriginatorSid](plan-connect-topologies.md#multiple-forests-single-azure-ad-tenant) |Mit dieser Option wird ein aktivierter Benutzer in einer Kontogesamtstruktur mit einem deaktivierten Benutzer in einer Ressourcengesamtstruktur verknüpft. In Exchange wird diese Konfiguration als verknüpftes Postfach bezeichnet. Diese Option kann auch verwendet werden, wenn Sie nur Lync verwenden und Exchange in der Ressourcengesamtstruktur nicht vorhanden ist. |
| sAMAccountName und MailNickName |Mit dieser Option werden Attribute mit der Stelle verknüpft, an der sich erwartungsgemäß die Anmelde-ID für den Benutzer befindet. |
| Ein bestimmtes Attribut |Mit dieser Option können Sie Ihr eigenes Attribut auswählen. Bei Verwendung dieser Option werden Benutzerobjekte, deren (ausgewähltes) Attribut nicht aufgefüllt ist, nicht mit Azure AD synchronisiert. **Einschränkung:** Wählen Sie unbedingt ein Attribut aus, das bereits im Metaverse vorhanden ist. Wenn Sie ein benutzerdefiniertes (nicht im Metaverse vorhandenes) Attribut auswählen, kann der Assistent nicht abgeschlossen werden. |

#### <a name="select-how-users-should-be-identified-with-azure-ad---source-anchor"></a>Auswählen, wie Benutzer bei Azure AD identifiziert werden sollen – Quellanker
Das sourceAnchor-Attribut ist während der Lebensdauer eines Benutzerobjekts unveränderlich. Das Attribut ist der Primärschlüssel, der den lokalen Benutzer mit dem Benutzer in Azure AD verknüpft.

| Einstellung | BESCHREIBUNG |
| --- | --- |
| Ich möchte den Quellanker durch Azure verwalten lassen | Wählen Sie diese Option aus, wenn Azure AD das Attribut für Sie auswählen soll. Wenn Sie diese Option auswählen, wendet der Azure AD Connect-Assistent die Auswahllogik für das sourceAnchor-Attribut an, die im Abschnitt [Azure AD Connect: Designkonzepte – Verwenden von msDS-ConsistencyGuid als sourceAnchor](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor) beschrieben wird. Nach Abschluss der benutzerdefinierten Installation informiert der Assistent Sie darüber, welches Attribut als Quellankerattribut ausgewählt wurde. |
| Ein bestimmtes Attribut | Wählen Sie diese Option aus, wenn Sie ein vorhandenes AD-Attribut als sourceAnchor-Attribut angeben möchten. |

Da das Attribut nicht geändert werden kann, müssen Sie sorgfältig planen, welches Attribut Sie verwenden möchten. Hier empfiehlt sich "objectGUID". Dieses Attribut wird nicht geändert, es sei denn, das Benutzerkonto wird zwischen Gesamtstrukturen/Domänen verschoben. Vermeiden Sie die Verwendung von Attributen, die sich ändern, wenn eine Person heiratet oder den Aufgabenbereich wechselt. Sie können keine Attribute mit einem @-signZeichen verwenden, sodass E-Mail-Adressen und Benutzerprinzipalnamen ungeeignet sind. Bei dem Attribut wird die Groß-/Kleinschreibung beachtet. Beim Verschieben von Objekten zwischen Gesamtstrukturen muss daher die Groß-/Kleinschreibung beibehalten werden. Binäre Attribute werden base64-codiert, andere Attributtypen bleiben dagegen unverschlüsselt. In Verbundszenarien und bei einigen Azure AD-Schnittstellen wird dieses Attribut auch als immutableID-Attribut bezeichnet. Weitere Informationen zum Quellanker finden Sie unter [Entwurfskonzepte](plan-connect-design-concepts.md#sourceanchor).

### <a name="sync-filtering-based-on-groups"></a>Synchronisierungsfilterung anhand von Gruppen
Mit der Filterung nach Gruppen haben Sie die Möglichkeit, nur eine kleine Teilmenge von Objekten für einen Piloten zu synchronisieren. Erstellen Sie dazu in Ihrem lokalen Active Directory eine Gruppe für diesen Zweck. Fügen Sie anschließend Benutzer und Gruppen hinzu, die als direkte Mitglieder mit Azure AD synchronisiert werden sollen. Später können Sie Benutzer hinzufügen und entfernen, um die Liste mit den Objekten zu verwalten, die in Azure AD vorhanden sein sollen. Alle Objekte, die Sie synchronisieren möchten, müssen direkte Mitglieder der Gruppe sein. Benutzer, Gruppen, Kontakte und Computer/Geräte müssen jeweils direkte Mitglieder sein. Geschachtelte Gruppenmitgliedschaften werden nicht aufgelöst. Wenn Sie eine Gruppe als Mitglied hinzufügen, wird nur die Gruppe hinzugefügt, nicht aber ihre Mitglieder.

![Synchronisierungsfilterung](./media/how-to-connect-install-custom/filter2.png)

> [!WARNING]
> Dieses Feature dient nur zur Unterstützung von Pilotbereitstellungen. Verwenden Sie es nicht in einer Produktionsbereitstellung.
>
>

In einer Produktionsbereitstellung wird es schwer, eine einzelne Gruppe mit allen zu synchronisierenden Objekten zu verwalten. Verwenden Sie stattdessen eine der unter [Konfigurieren der Filterung](how-to-connect-sync-configure-filtering.md) beschriebenen Methoden.

### <a name="optional-features"></a>Optionale Features
Über diesen Bildschirm können Sie die optionalen Features für Ihre spezifischen Szenarios auswählen.

>[!WARNING]
>Für Azure AD Connect Version **1.0.8641.0** und früher wird der Azure Access Control Service für das Kennwortrückschreiben verwendet.  Dieser Dienst wird am **7. November 2018** außer Betrieb genommen.  Wenn Sie eine dieser Versionen von Azure AD Connect nutzen und das Kennwortrückschreiben aktiviert haben, können Benutzer unter Umständen ihre Kennwörter nicht mehr ändern oder zurücksetzen, nachdem der Dienst außer Betrieb genommen wurde. Das Kennwortrückschreiben mit diesen Versionen von Azure AD Connect wird nicht unterstützt.
>
>Weitere Informationen zu Azure Access Control Service finden Sie unter [Gewusst wie: Migrieren aus dem Azure Access Control Service](../develop/active-directory-acs-migration.md).
>
>[Klicken Sie hier](https://www.microsoft.com/en-us/download/details.aspx?id=47594), um die aktuelle Version von Azure AD Connect herunterzuladen.

![Optionale Features](./media/how-to-connect-install-custom/optional2.png)

> [!WARNING]
> Wenn derzeit DirSync oder Azure AD Sync aktiv ist, aktivieren Sie keine der Features zum Rückschreiben in Azure AD Connect.



| Optionale Features | BESCHREIBUNG |
| --- | --- |
| Exchange-Hybridbereitstellung |Das Exchange-Hybridbereitstellungsfeature ermöglicht die Koexistenz lokaler und Office 365-basierter Exchange-Postfächer. Azure AD Connect synchronisiert eine bestimmte Gruppe von [Attributen](reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback) aus Azure AD mit Ihrem lokalen Verzeichnis. |
| Öffentliche Exchange-E-Mail-Ordner | Mit dem Feature „Öffentliche Exchange-E-Mail-Ordner“ können Sie Objekte für E-Mail-aktivierte öffentliche Ordner von Ihrer lokalen Active Directory-Instanz nach Azure AD synchronisieren. |
| Azure AD-App- und Attributfilterung |Mithilfe der App- und Attributfilterung von Azure AD kann die Gruppe synchronisierter Attribute individuell konfiguriert werden. Durch diese Option wird der Assistent um zwei weitere Konfigurationsseiten erweitert. Weitere Informationen finden Sie unter [Azure AD-App- und Attributfilterung](#azure-ad-app-and-attribute-filtering). |
| Kennworthashsynchronisierung |Diese Option ist verfügbar, wenn Sie als Anmeldelösung die Verbundoption ausgewählt haben. Die Kennworthashsynchronisierung kann dann als Sicherungsoption verwendet werden. Weitere Informationen finden Sie unter [Implement password hash synchronization with Azure AD Connect sync](how-to-connect-password-hash-synchronization.md) (Implementieren der Kennworthashsynchronisierung mit Azure AD Connect-Synchronisierung). </br></br>Wenn Sie die Passthrough-Authentifizierung ausgewählt haben, kann diese Option auch standardmäßig aktiviert werden, um sicherzustellen, dass Legacyclients unterstützt werden und eine Sicherungsoption vorhanden ist. Weitere Informationen finden Sie unter [Implement password hash synchronization with Azure AD Connect sync](how-to-connect-password-hash-synchronization.md) (Implementieren der Kennworthashsynchronisierung mit Azure AD Connect-Synchronisierung).|
| Kennwortrückschreiben |Durch Aktivieren des Kennwortrückschreibens werden Kennwortänderungen aus Azure AD in Ihr lokales Verzeichnis zurückgeschrieben. Weitere Informationen finden Sie unter [Erste Schritte mit der Kennwortverwaltung](../authentication/quickstart-sspr.md). |
| Gruppenrückschreiben |Bei Verwendung des Features **Office 365-Gruppen** können diese Gruppen in Ihrem lokalen Active Directory dargestellt werden. Diese Option ist nur verfügbar, wenn Exchange in Ihrem lokalen Active Directory vorhanden ist. Weitere Informationen finden Sie unter [Gruppenrückschreiben](how-to-connect-preview.md#group-writeback). |
| Geräterückschreiben |Ermöglicht für bedingte Szenarios das Rückschreiben von Geräteobjekten in Azure AD auf Ihr lokales Active Directory. Weitere Informationen finden Sie unter [Aktivieren des Geräterückschreibens in Azure AD Connect](how-to-connect-device-writeback.md). |
| Verzeichniserweiterungen-Attributsynchronisierung |Durch Aktivieren der Attributsynchronisierung für Verzeichniserweiterungen werden die angegebenen Attribute mit Azure AD synchronisiert. Weitere Informationen finden Sie unter [Verzeichniserweiterungen](how-to-connect-sync-feature-directory-extensions.md). |

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD-App- und Attributfilterung
Wenn Sie einschränken möchten, welche Attribute mit Azure AD synchronisiert werden, wählen Sie zunächst die verwendeten Dienste aus. Falls Sie auf dieser Seite Konfigurationsänderungen vornehmen, muss durch erneutes Ausführen des Installations-Assistenten explizit ein neuer Dienst ausgewählt werden.

![Optionale Features – Apps](./media/how-to-connect-install-custom/azureadapps2.png)

Auf der Grundlage der im vorherigen Schritt ausgewählten Dienste werden auf dieser Seite alle synchronisierten Attribute angezeigt. Diese Liste ist eine Kombination aller Objekttypen, die synchronisiert werden. Falls bestimmte Attribute nicht synchronisiert werden sollen, können Sie deren Auswahl aufheben.

![Optionale Features – Attribute](./media/how-to-connect-install-custom/azureadattributes2.png)

> [!WARNING]
> Das Entfernen von Attributen kann sich negativ auf die Funktionalität auswirken. Bewährte Methoden und Empfehlungen finden Sie unter [Synchronisierte Attribute](reference-connect-sync-attributes-synchronized.md#attributes-to-synchronize).
>
>

### <a name="directory-extension-attribute-sync"></a>Verzeichniserweiterungen-Attributsynchronisierung
Das Schema in Azure AD kann durch von Ihrer Organisation hinzugefügte benutzerdefinierte Attribute oder durch andere Attribute in Active Directory erweitert werden. Wählen Sie auf der Seite **Optionale Features** die Option **Verzeichniserweiterungen-Attributsynchronisierung** aus, um dieses Feature zu verwenden. Auf dieser Seite können weitere zu synchronisierende Attribute ausgewählt werden.

>[!NOTE]
>Für das Feld „Verfügbare Attribute“ wird die Groß-/Kleinschreibung berücksichtigt.

![Verzeichniserweiterungen](./media/how-to-connect-install-custom/extension2.png)

Weitere Informationen finden Sie unter [Verzeichniserweiterungen](how-to-connect-sync-feature-directory-extensions.md).

### <a name="enabling-single-sign-on-sso"></a>Aktivieren des einmaligen Anmeldens (Single Sign-On, SSO)
Das Konfigurieren des einmaligen Anmeldens zur Verwendung mit der Kennwortsynchronisierung oder Passthrough-Authentifizierung ist ein einfacher Prozess, den Sie nur einmal für jede Gesamtstruktur durchführen müssen, die mit Azure AD synchronisiert wird. Die Konfiguration umfasst zwei Schritte:

1.  Erstellen des erforderlichen Computerkontos in Ihrer lokalen Active Directory-Instanz
2.  Konfigurieren der Intranetzone der Clientcomputer zur Unterstützung des einmaligen Anmeldens

#### <a name="create-the-computer-account-in-active-directory"></a>Erstellen des Computerkontos in Active Directory
Für jede Gesamtstruktur, die in Azure AD Connect hinzugefügt wurde, müssen Sie Domänenadministrator-Anmeldeinformationen angeben, damit das Computerkonto in jeder Gesamtstruktur erstellt werden kann. Die Anmeldeinformationen werden nur zum Erstellen des Kontos verwendet und nicht für andere Vorgänge gespeichert oder genutzt. Fügen Sie die Anmeldeinformationen einfach wie folgt auf der Seite **Einmaliges Anmelden aktivieren** des Azure AD Connect-Assistenten hinzu:

![Einmaliges Anmelden aktivieren](./media/how-to-connect-install-custom/enablesso.png)

>[!NOTE]
>Sie können eine bestimmte Gesamtstruktur auch überspringen, wenn das einmalige Anmelden für die Gesamtstruktur nicht verwendet werden soll.

#### <a name="configure-the-intranet-zone-for-client-machines"></a>Konfigurieren der Intranetzone für Clientcomputer
Damit der Client automatisch in der Intranetzone angemeldet wird, müssen Sie dafür sorgen, dass die URL Teil der Intranetzone ist. So wird sichergestellt, dass der in die Domäne eingebundene Computer automatisch ein Kerberos-Ticket an Azure AD sendet, wenn eine Verbindung mit dem Unternehmensnetzwerk besteht.
Auf einem Computer mit den Gruppenrichtlinien-Verwaltungstools:

1.  Öffnen Sie die Gruppenrichtlinien-Verwaltungstools.
2.  Bearbeiten Sie die Gruppenrichtlinie, die auf alle Benutzer angewendet wird. Beispiel: Standardrichtlinie der Domäne.
3.  Navigieren Sie zu **Benutzerkonfiguration\Administrative Vorlagen\Windows-Komponenten\Internet Explorer\Internetsystemsteuerung\Sicherheitsseite**, und wählen Sie wie in der folgenden Abbildung dargestellt die Option **Liste der Site zu Zonenzuweisungen**.
4.  Aktivieren Sie die Richtlinie, und geben Sie Folgendes in das Dialogfeld ein:

        Value: `https://autologon.microsoftazuread-sso.com`  
        Data: 1  


5.  Es sollte in etwa wie folgt aussehen:  
![Intranetzonen](./media/how-to-connect-install-custom/sitezone.png)

6.  Klicken Sie zweimal auf **OK**.

## <a name="configuring-federation-with-ad-fs"></a>Konfigurieren des Verbunds mit AD FS
Das Konfigurieren von AD FS mit Azure AD Connect ist ganz einfach und mit wenigen Mausklicks erledigt. Für die Konfiguration wird Folgendes benötigt:

* Ein Server unter Windows Server 2012 R2 (oder höher) für den Verbundserver mit aktivierter Remoteverwaltung
* Ein Server unter Windows Server 2012 R2 (oder höher) für den Webanwendungsproxy-Server mit aktivierter Remoteverwaltung
* Ein SSL-Zertifikat für den Verbunddienstnamen, den Sie verwenden möchten (z.B. „sts.contoso.com“)

>[!NOTE]
>Sie können das SSL-Zertifikat für Ihre AD FS-Farm mit Azure AD Connect aktualisieren, auch wenn Sie es nicht für die Verwaltung Ihrer Verbundvertrauensstellung verwenden.

### <a name="ad-fs-configuration-pre-requisites"></a>Voraussetzungen für die AD FS-Konfiguration
Vergewissern Sie sich, dass WinRM auf den Remoteservern aktiviert ist, damit Sie Ihre AD FS-Farm mithilfe von Azure AD Connect konfigurieren können. Vergewissern Sie sich, dass Sie die anderen Aufgaben unter [Voraussetzungen für die Verbundinstallation und -konfiguration](how-to-connect-install-prerequisites.md#prerequisites-for-federation-installation-and-configuration) abgeschlossen haben. Machen Sie sich außerdem unter [Tabelle 3: Azure AD Connect und Verbund-/WAP-Server](reference-connect-ports.md#table-3---azure-ad-connect-and-ad-fs-federation-serverswap) mit den Portanforderungen vertraut.

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Erstellen einer neuen AD FS-Farm oder Verwenden einer vorhandenen AD FS-Farm
Sie können eine vorhandene AD FS-Farm verwenden oder eine neue AD FS-Farm erstellen. Wenn Sie eine neue Farm erstellen, müssen Sie ein SSL-Zertifikat bereitstellen. Bei Verwendung eines kennwortgeschützten SSL-Zertifikats werden Sie zur Eingabe des Kennworts aufgefordert.

![AD FS-Farm](./media/how-to-connect-install-custom/adfs1.png)

Wenn Sie sich für die Verwendung einer bereits vorhandenen AD FS-Farm entscheiden, gelangen Sie direkt zur Konfiguration der Vertrauensstellung zwischen AD FS und Azure AD.

>[!NOTE]
>Mit Azure AD Connect kann nur eine einzelne AD FS-Farm verwaltet werden. Wenn für die ausgewählte AD FS-Farm bereits eine Verbundvertrauensstellung mit Azure AD konfiguriert ist, wird diese von Azure AD Connect neu erstellt.

### <a name="specify-the-ad-fs-servers"></a>Angeben der AD FS-Server
Geben Sie die Server ein, auf denen Sie AD FS installieren möchten. Sie können je nach Ihren Kapazitätsplanungsanforderungen einen oder mehrere Server hinzufügen. Verknüpfen Sie vor dieser Konfiguration alle AD FS-Server mit Active Directory. (Für die WAP-Server ist dieser Schritt nicht erforderlich.) Microsoft empfiehlt, einen einzelnen AD FS-Server für Test- und Pilotbereitstellungen zu installieren. Nach der Erstkonfiguration können Sie dann Azure AD Connect erneut ausführen und weitere Server hinzufügen und bereitstellen, um Ihren Skalierungsanforderungen gerecht zu werden.

> [!NOTE]
> Vergewissern Sie sich vor dieser Konfiguration, dass alle Ihre Server mit einer AD-Domäne verknüpft sind.
>
>

![AD FS-Server](./media/how-to-connect-install-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Angeben von Webanwendungsproxy-Servern
Geben Sie die Server ein, die Sie als Webanwendungsproxy-Server verwenden möchten. Der Webanwendungsproxy-Server wird in Ihrer DMZ bereitgestellt (Extranetzugriff) und unterstützt Authentifizierungsanforderungen aus dem Extranet. Sie können je nach Ihren Kapazitätsplanungsanforderungen einen oder mehrere Server hinzufügen. Microsoft empfiehlt, einen einzelnen Webanwendungsproxy-Server für Test- und Pilotbereitstellungen zu installieren. Nach der Erstkonfiguration können Sie dann Azure AD Connect erneut ausführen und weitere Server hinzufügen und bereitstellen, um Ihren Skalierungsanforderungen gerecht zu werden. Es empfiehlt sich, eine entsprechende Anzahl von Proxyservern bereitzustellen, um die Authentifizierung über das Intranet zu ermöglichen.

> [!NOTE]
> <li> Wenn es sich bei dem verwendeten Konto nicht um ein lokales Administratorkonto auf den WAP-Servern handelt, werden Sie zur Eingabe der Administratoranmeldeinformationen aufgefordert.</li>
> <li> Vergewissern Sie sich vor diesem Schritt, dass zwischen dem Azure AD Connect-Server und dem Webanwendungsproxy-Server eine HTTP/HTTPS-Verbindung besteht.</li>
> <li> Vergewissern Sie sich zudem, dass zwischen dem Webanwendungsserver und dem AD FS-Server eine HTTP/HTTPS-Verbindung besteht, um die Durchleitung von Authentifizierungsanforderungen zu ermöglichen.</li>
>

![Web App](./media/how-to-connect-install-custom/adfs3.png)

Sie werden zur Eingabe von Anmeldeinformationen aufgefordert, damit der Webanwendungsserver eine sichere Verbindung mit dem AD FS-Server herstellen kann. Dabei muss es sich um die Anmeldeinformationen für einen lokalen Administrator für den AD FS-Server sein.

![Proxy](./media/how-to-connect-install-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Angeben eines Dienstkontos für den AD FS-Dienst
Der AD FS-Dienst erfordert ein Domänendienstkonto zur Authentifizierung von Benutzern und zur Suche nach Benutzerinformationen in Active Directory. Zwei Dienstkontotypen werden unterstützt:

* **Gruppenverwaltetes Dienstkonto** : Wurde in den Active Directory-Domänendiensten mit Windows Server 2012 eingeführt. Dieser Kontotyp stellt für Dienste wie AD FS ein einzelnes Konto bereit, das keine regelmäßige Aktualisierung des Kontokennworts erfordert. Verwenden Sie diese Option, wenn Sie in der Domäne, der die AD FS-Server angehören, bereits über Windows Server 2012-Domänencontroller verfügen.
* **Domänenbenutzerkonto** : Bei diesem Kontotyp müssen Sie ein Kennwort angeben und das Kennwort regelmäßig aktualisieren, wenn sich das Kennwort ändert oder abläuft. Verwenden Sie diese Option nur, wenn Sie in der Domäne, der die AD FS-Server angehören, über keine Windows Server 2012-Domänencontroller verfügen.

Wenn Sie das gruppenverwaltete Dienstkonto ausgewählt haben und dieses Feature in Active Directory noch nie verwendet wurde, werden Sie zur Eingabe von Enterprise-Administratoranmeldeinformationen aufgefordert. Diese werden zum Initiieren des Schlüsselspeichers und zum Aktivieren des Features in Active Directory verwendet.

> [!NOTE]
> Azure AD Connect überprüft, ob der AD FS-Dienst bereits als Dienstprinzipalname in der Domäne registriert ist.  Die gleichzeitige Registrierung von doppelten Dienstprinzipalnamen ist in AD DS nicht zulässig.  Wenn ein doppelter Dienstprinzipalname gefunden wird, können Sie den Vorgang erst fortsetzen, wenn der Dienstprinzipalname entfernt wurde.

![AD FS-Dienstkonto](./media/how-to-connect-install-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Auswählen der zu verbindenden Azure AD-Domäne
Diese Konfiguration wird verwendet, um die Verbundbeziehung zwischen AD FS und Azure AD einzurichten. Damit wird AD FS zur Ausstellung von Sicherheitstoken an Azure AD konfiguriert, und Azure AD wird so konfiguriert, dass es den Token dieser spezifischen Instanz von AD FS vertraut. Auf dieser Seite kann bei der Erstinstallation nur eine einzelne Domäne konfiguriert werden. Weitere Domänen können Sie später durch erneutes Ausführen von Azure AD Connect konfigurieren.

![Azure AD-Domäne](./media/how-to-connect-install-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Überprüfen der für den Verbund ausgewählten Azure AD-Domäne
Wenn Sie die Domäne in einem Verbund verwenden möchten, stellt Azure AD Connect die erforderlichen Informationen zur Überprüfung einer nicht überprüften Domäne bereit. Informationen zur Verwendung dieser Informationen finden Sie unter [Hinzufügen und Überprüfen der Domäne](../active-directory-domains-add-azure-portal.md).

![Azure AD-Domäne](./media/how-to-connect-install-custom/verifyfeddomain.png)

> [!NOTE]
> AD Connect versucht in der Konfigurationsphase, die Domäne zu überprüfen. Wenn Sie die Konfiguration ohne Angabe der erforderlichen DNS-Einträge fortsetzen, kann die Konfiguration nicht abgeschlossen werden.
>
>

## <a name="configuring-federation-with-pingfederate"></a>Konfigurieren des Verbunds mit PingFederate
Das Konfigurieren von PingFederate mit Azure AD Connect ist ganz einfach und mit wenigen Mausklicks erledigt. Allerdings ist Folgendes erforderlich:
- PingFederate 8.4 oder höher.  Weitere Informationen finden Sie unter [PingFederate Integration with Azure Active Directory and Office 365](https://docs.pingidentity.com/bundle/O365IG20_sm_integrationGuide/page/O365IG_c_integrationGuide.html) (PingFederate-Integration in Azure Active Directory und Office 365)
- Ein SSL-Zertifikat für den Verbunddienstnamen, den Sie verwenden möchten (z.B. „sts.contoso.com“)

### <a name="verify-the-domain"></a>Überprüfen der Domäne
Nachdem Sie den Verbund mit PingFederate ausgewählt wurde, werden Sie dazu aufgefordert, die Domäne zu überprüfen, mit der ein Verbund hergestellt werden soll.  Wählen Sie im Dropdownfeld die Domäne aus.

![Domäne überprüfen](./media/how-to-connect-install-custom/ping1.png)

### <a name="export-the-pingfederate-settings"></a>Exportieren der PingFederate-Einstellungen


PingFederate muss als Verbundserver für jede Azure-Verbunddomäne konfiguriert werden.  Klicken Sie auf die Schaltfläche „Einstellungen exportieren“, und teilen Sie diese Informationen Ihrem PingFederate-Administrator mit.  Der Verbundserveradministrator aktualisiert die Konfiguration und gibt dann die PingFederate-Server-URL und die Portnummer an, damit Azure AD Connect die Metadateneinstellungen überprüfen kann.  

![Domäne überprüfen](./media/how-to-connect-install-custom/ping2.png)

Wenden Sie sich an den PingFederate-Administrator, um alle Überprüfungsprobleme zu beheben.  Im Folgenden finden ein Beispiel für einen PingFederate-Server, der nicht über eine gültige Vertrauensstellung mit Azure verfügt:

![Vertrauen](./media/how-to-connect-install-custom/ping5.png)




### <a name="verify-federation-connectivity"></a>Überprüfen der Verbundkonnektivität
Azure AD Connect überprüft die Authentifizierungsendpunkte, die aus den PingFederate-Metadaten im vorherigen Schritt abgerufen wurden.  Azure AD Connect versucht zunächst die Endpunkte mithilfe Ihrer lokalen DNS-Server zu beheben.  Als Nächstes wird versucht, die Endpunkte mit einem externen DNS-Anbieter zu beheben.  Wenden Sie sich an den PingFederate-Administrator, um alle Überprüfungsprobleme zu beheben.  

![Überprüfen der Konnektivität](./media/how-to-connect-install-custom/ping3.png)

### <a name="verify-federation-login"></a>Überprüfen der Verbundanmeldung
Zu guter Letzt können Sie den neu konfigurierten Verbundanmeldevorgang überprüfen, indem Sie sich bei der Verbunddomäne anmelden. Wenn dies erfolgreich ist, wird der Verbund mit PingFederate erfolgreich konfiguriert.
![Überprüfen der Anmeldung](./media/how-to-connect-install-custom/ping4.png)

## <a name="configure-and-verify-pages"></a>Konfigurieren und Überprüfen von Seiten
Die Konfiguration wird auf dieser Seite vorgenommen.

> [!NOTE]
> Wenn Sie einen Verbund konfiguriert haben, sollten Sie sich vor dem Fortsetzen der Installation vergewissern, dass Sie die [Namensauflösung für Verbundserver](how-to-connect-install-prerequisites.md#name-resolution-for-federation-servers) konfiguriert haben.
>
>


![Bereit für Konfiguration](./media/how-to-connect-install-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Stagingmodus
Mit dem Stagingmodus können Sie parallel einen neuen Synchronisierungsserver einrichten. Nur ein Synchronisierungsserver, der einen Export zu einem Verzeichnis in der Cloud durchführt, wird unterstützt. Wenn Sie jedoch eine Verschiebung von einem anderen Server durchführen möchten, z. B. einem Server, auf dem DirSync ausgeführt wird, kann Azure AD Connect im Stagingmodus aktiviert werden. Bei aktiviertem Stagingmodus werden Daten vom Synchronisierungsmodul wie gewohnt importiert und synchronisiert, es findet aber kein Export an Azure AD oder AD statt. Kennwortsynchronisierung und Kennwortrückschreiben sind im Stagingmodus deaktiviert.

![Stagingmodus](./media/how-to-connect-install-custom/stagingmode.png)

Im Stagingmodus können Sie die erforderlichen Änderungen am Synchronisierungsmodul vornehmen und überprüfen, was exportiert werden soll. Wenn die Konfiguration Ihren Vorstellungen entspricht, führen Sie erneut den Installations-Assistenten aus, und deaktivieren Sie den Stagingmodus. Daraufhin werden Daten von diesem Server an Azure AD exportiert. Stellen Sie sicher, das der andere Server währenddessen deaktiviert ist, sodass nur ein Server aktiv exportieren kann.

Weitere Informationen finden Sie unter [Stagingmodus](how-to-connect-sync-staging-server.md).

### <a name="verify-your-federation-configuration"></a>Überprüfen der Verbundkonfiguration
Wenn Sie auf die Schaltfläche „Überprüfen“ klicken, überprüft Azure AD Connect die DNS-Einstellungen.

**Intranet-Konnektivitätsprüfungen**

* Verbund-FQDN auflösen: Azure AD Connect überprüft, ob der Verbund-FQDN von DNS aufgelöst werden kann, um die Konnektivität zu gewährleisten. Falls der FQDN nicht aufgelöst werden kann, ist die Überprüfung nicht erfolgreich. Vergewissern Sie sich, dass der DNS-Eintrag für den Verbunddienst-FQDN vorhanden ist, damit die Überprüfung erfolgreich abgeschlossen werden kann.
* DNS-A-Eintrag: Azure AD Connect überprüft, ob für Ihren Verbunddienst ein A-Eintrag vorhanden ist. Sollte kein A-Eintrag vorhanden sein, ist die Überprüfung nicht erfolgreich. Erstellen Sie einen A-Eintrag (und keinen CNAME-Eintrag) für Ihren Verbund-FQDN, damit die Überprüfung erfolgreich abgeschlossen werden kann.

**Extranet-Konnektivitätsprüfungen**

* Verbund-FQDN auflösen: Azure AD Connect überprüft, ob der Verbund-FQDN von DNS aufgelöst werden kann, um die Konnektivität zu gewährleisten.

![Abgeschlossen](./media/how-to-connect-install-custom/completed.png)

![Überprüfen](./media/how-to-connect-install-custom/adfs7.png)

Um zu überprüfen, ob die End-to-End-Authentifizierung erfolgreich war, sollten Sie manuell einen oder mehrere der folgenden Tests ausführen:

* Sobald die Synchronisierung abgeschlossen ist, überprüfen Sie mithilfe der zusätzlichen Aufgabe „Verbundanmeldung überprüfen“ in Azure AD Connect die Authentifizierung für ein lokales Benutzerkonto Ihrer Wahl.
* Vergewissern Sie sich, dass Sie sich über den Browser eines in die Domäne eingebundenen Computers im Intranet anmelden können: Stellen Sie eine Verbindung mit https://myapps.microsoft.com her, und überprüfen Sie die Anmeldung mit dem angemeldeten Konto. Das integrierte AD DS-Administratorkonto wird nicht synchronisiert und kann nicht für die Überprüfung verwendet werden.
* Vergewissern Sie sich, dass Sie sich auf einem Gerät über das Extranet anmelden können. Stellen Sie auf einem privaten Computer oder auf einem mobilen Gerät eine Verbindung mit https://myapps.microsoft.com her, und geben Sie Ihre Anmeldeinformationen an.
* Überprüfen Sie die Anmeldung per Rich Client. Stellen Sie eine Verbindung mit https://testconnectivity.microsoft.com her, klicken Sie auf die Registerkarte **Office 365**, und wählen Sie den **Office 365-Test für einmaliges Anmelden** aus.

## <a name="troubleshooting"></a>Problembehandlung
Der folgende Abschnitt enthält Details zur Problembehandlung sowie Informationen, die hilfreich sind, wenn bei der Installation von Azure AD Connect Probleme auftreten.

### <a name="the-adsync-database-already-contains-data-and-cannot-be-overwritten"></a>“The ADSync database already contains data and cannot be overwritten” (Die ADSync-Datenbank enthält bereits Daten und kann nicht überschrieben werden)
Wenn Sie die benutzerdefinierte Installation von Azure AD Connect durchführen und die Option **Use an existing SQL server** (Vorhandenen SQL-Server verwenden) auf der Seite **Install required components** (Erforderliche Komponenten installieren) verwenden, wird unter Umständen der folgende Fehler angezeigt: **The ADSync database already contains data and cannot be overwritten. Please remove the existing database and try again.** (Die ADSync-Datenbank enthält bereits Daten und kann nicht überschrieben werden. Entfernen Sie die vorhandene Datenbank, und versuchen Sie es noch mal.)

![Error](./media/how-to-connect-install-custom/error1.png)

Das liegt daran, dass bereits eine Datenbank mit dem Namen **ADSync** auf der SQL-Instanz des SQL-Servers, den Sie oben im Textfeld angegeben haben, vorhanden ist.

Dies tritt in der Regel nach der Deinstallation von Azure AD Connect auf.  Die Datenbank wird bei der Deinstallation nicht vom SQL Server gelöscht.

Um dieses Problem zu beheben, stellen Sie zunächst sicher, dass die **ADSync**-Datenbank, die von Azure AD Connect vor der Deinstallation verwendet wurde, nicht mehr verwendet wird.

Als Nächstes empfiehlt es sich, dass Sie die Datenbank vor dem Löschen sichern.

Zu guter Letzt müssen Sie die Datenbank löschen.  Verwenden Sie hierfür **Microsoft SQL Server Management Studio**, und stellen Sie eine Verbindung mit der SQL-Instanz her. Suchen Sie die **ADSync**-Datenbank, klicken Sie mit der rechten Maustaste darauf, und wählen Sie im Kontextmenü die Option **Löschen**.  Klicken Sie anschließend zum Löschen auf die Schaltfläche **OK**.

![Error](./media/how-to-connect-install-custom/error2.png)

Nach dem Löschen der **ADSync**-Datenbank klicken Sie auf die Schaltfläche **Installieren**, um die Installation erneut zu versuchen.

## <a name="next-steps"></a>Nächste Schritte
Melden Sie sich nach Abschluss der Installation von Windows ab und erneut wieder an, ehe Sie den Synchronisierungsdienst-Manager oder Synchronisierungsregel-Editor verwenden.

Nachdem Sie Azure AD Connect installiert haben, können Sie [die Installation überprüfen und Lizenzen zuweisen](how-to-connect-post-installation.md).

Hier finden Sie weitere Informationen zu diesen Features, die bei der Installation aktiviert wurden: [Azure AD Connect-Synchronisierung: Verhindern von versehentlichen Löschvorgängen](how-to-connect-sync-feature-prevent-accidental-deletes.md) und [Überwachen der Azure AD Connect-Synchronisierung mit Azure AD Connect Health](how-to-connect-health-sync.md).

Weitere Informationen zu folgenden allgemeinen Themen: [Scheduler und Auslösen der Synchronisierung](how-to-connect-sync-feature-scheduler.md).

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](whatis-hybrid-identity.md).
