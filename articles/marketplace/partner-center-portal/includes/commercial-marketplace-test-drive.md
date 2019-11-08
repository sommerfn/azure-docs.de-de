---
title: include file
description: include file
services: commercial marketplace
documentationcenter: partner-center-commercial-marketplace
author: qianw211
manager: evansma
editor: ''
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/13/2019
ms.author: v-qiwe
ms.custom: include file
ms.openlocfilehash: 831e3330126d82795cece128c28cb96b7199d69a
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825531"
---
Auf der Registerkarte **Testversion** können Sie eine Demonstration (oder „Testversion“) einrichten, sodass Kunden Ihr Angebot testen können, bevor sie sich zum Kauf entschließen. Erfahren Sie mehr im Artikel [Was ist die Testversion?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive). Wenn Sie keine Testversion mehr für Ihr Angebot bereitstellen möchten, kehren Sie zur Seite **Angebotseinrichtung** zurück, und deaktivieren Sie **Testversion aktivieren**.

### <a name="technical-configuration"></a>Technische Konfiguration
Die folgenden Typen von Testversionen sind verfügbar, jeweils mit eigenen Anforderungen an die technische Konfiguration.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Logik-App](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (keine technische Konfiguration erforderlich)

#### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Technical configuration for Azure Resource Manager test drive (Technische Konfiguration für Azure Resource Manager-Testversion)

Eine Bereitstellungsvorlage, die alle Azure-Ressourcen enthält, aus denen Ihre Lösung besteht. In den für dieses Szenario geeigneten Produkten werden nur Azure-Ressourcen verwendet. Erfahren Sie mehr über das Einrichten einer [Azure Resource Manager-Testversion](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

- **Regionen** (erforderlich): Es gibt derzeit 26 von Azure unterstützte Regionen, in denen Ihre Testversion zur Verfügung gestellt werden kann. In der Regel sollten Sie Ihre Testversion in den Regionen zur Verfügung stellen, in denen Sie die größte Anzahl von Kunden erwarten, damit sie für eine optimale Leistung die nächstgelegene Region auswählen können. Sie müssen sicherstellen, dass in Ihrem Abonnement alle Ressourcen in jeder von Ihnen ausgewählten Region bereitgestellt werden können.

- **Instances**: Wählen Sie den Typ („Heiß“ oder „Kalt“) und die Anzahl der verfügbaren Instanzen aus. Diese wird mit der Anzahl der Regionen multipliziert, in denen Ihr Angebot verfügbar ist.

**Hot**: Dieser Typ von Instanz wird bereits bereitgestellt, und auf ihn kann in jeder ausgewählten Region zugegriffen werden. Die Kunden haben sofort Zugriff auf Instanzen vom Typ *Heiß* und müssen nicht auf die Bereitstellung warten. Der Nachteil dabei ist, dass diese Instanzen immer unter Ihrem Azure-Abonnement ausgeführt werden, sodass höhere Betriebszeitkosten anfallen. Es wird dringend empfohlen, mindestens eine Instanz des Typs *Heiß* festzulegen, da die meisten Kunden nicht auf eine vollständige Bereitstellung warten möchten. Wenn keine Instanz vom Typ *Heiß* verfügbar ist, nimmt die Verwendung durch die Kunden ab.

**Kalt**: Dieser Typ von Instanz stellt die Gesamtzahl der Instanzen dar, die pro Region bereitgestellt werden können. Bei Instanzen des Typs „Kalt“ muss die Bereitstellung der gesamten Resource Manager-Vorlage für die Testversion durchgeführt werden, wenn der Kunde die Testversion anfordert. Daher dauert das Laden von Instanzen des Typs *Kalt* weitaus länger als das Laden von Instanzen des Typs *Heiß*. Sie müssen jedoch nur für die Dauer der Testversion bezahlen. Instanzen vom Typ „Kalt“ werden *nicht* wie Instanzen vom Typ *Heiß* stets im Azure-Abonnement ausgeführt.

- **Test drive Azure Resource Manager template** (Vorlage für Azure Resource Manager-Testversion): Laden Sie die ZIP-Datei hoch, die die Azure Resource Manager-Vorlage enthält.  Im Schnellstartartikel [Erstellen und Bereitstellen von Azure Resource Manager-Vorlagen über das Azure-Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal) erhalten Sie weitere Informationen zum Erstellen einer Azure Resource Manager-Vorlage.

- **Dauer der Testversion** (erforderlich): Geben Sie den Zeitraum, in dem die Testversion aktiv bleibt, als Anzahl von Stunden ein. Nach Ablauf dieses Zeitraums wird die Testversion automatisch beendet. Dies Dauer kann nur als ganze Zahl von Stunden festgelegt werden (Beispiel: „2“ ist zulässig, „1,5“ ist unzulässig).

#### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Technical configuration for Dynamics 365 test drive (Technische Konfiguration für Dynamics 365-Testversion)

Microsoft kann mit diesem Typ von Testversion die Dienstbereitstellung hosten und verwalten, um das Einrichten einer Testversion zu vereinfachen. Die Konfiguration für diesen Typ von gehosteter Testversion ist gleich, unabhängig davon ob die Testversion für Business Central, Customer Engagement oder Operations vorgesehen ist.

- **Maximale Anzahl gleichzeitiger Testversionen** (erforderlich): Legen Sie die maximale Anzahl von Kunden fest, die Ihre Testversion gleichzeitig verwenden können. Jeder gleichzeitige Benutzer nutzt eine Dynamics 365-Lizenz, während die Testversion aktiv ist. Daher müssen Sie sicherstellen, dass genügend Lizenzen für die festgelegte maximale Anzahl verfügbar sind. Empfohlener Wert liegt zwischen 3 und 5.

- **Dauer der Testversion** (erforderlich): Geben Sie den Zeitraum ein, in dem die Testversion aktiv bleibt, indem Sie die Anzahl der Stunden festlegen. Nach Ablauf dieser Anzahl von Stunden wird die Sitzung beendet, und sie belegt nicht mehr eine Ihrer Lizenzen. Der empfohlene Wert beträgt 2 bis 24 Stunden, abhängig von der Komplexität Ihres Angebots. Dies Dauer kann nur als ganze Zahl von Stunden festgelegt werden (Beispiel: „2“ ist zulässig, „1,5“ ist unzulässig).  Der Benutzer kann eine neue Sitzung anfordern, wenn die Zeit abgelaufen ist und er erneut auf die Testversion zugreifen möchte.

- **Instanz-URL** (erforderlich): Die URL, über die der Kunde die Testversion startet. Dies ist in der Regel die URL Ihrer Dynamics 365-Instanz, auf der Ihre App mit installierten Beispieldaten ausgeführt wird (z.B. https://testdrive.crm.dynamics.com) ).

- **Web-API-URL der Instanz** (erforderlich): Rufen Sie die Web-API-URL für Ihre Dynamics 365-Instanz ab, indem Sie sich bei Ihrem Microsoft 365-Konto anmelden, zu **Einstellungen** \&gt; **Anpassung** \&gt; **Entwicklerressourcen** \&gt; **Instance Web API (Service Root URL)** (Instanz-Web-API (Stamm-URL des Diensts)) navigieren und die hier angegebene URL kopieren (z.B. https://testdrive.crm.dynamics.com/api/data/v9.0) ).

- **Rollenname** (erforderlich): Geben Sie den Namen der Sicherheitsrolle ein, den Sie in der benutzerdefinierten Dynamics 365-Testversion definiert haben. Dieser wird dem Benutzer während der Dauer der Testversion zugewiesen (z.B. „Testversionsrolle“).

#### <a name="technical-configuration-for-logic-app-test-drive"></a>Technical configuration for Logic app test drive (Technische Konfiguration für Logik-App-Testversion)

Für alle benutzerdefinierten Produkte sollte dieser Typ von Testversionsbereitstellungsvorlage, der vielfältige komplexe Lösungsarchitekturen umfasst, verwendet werden. Um weitere Informationen über das Einrichten von Logik-App-Testversionen zu erhalten, besuchen Sie [Operations](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) und [Customer Engagement](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) auf GitHub (in englischer Sprache).

- **Region** (erforderlich, Dropdownliste für Einfachauswahl): Es gibt derzeit 26 von Azure unterstützte Regionen, in denen Ihre Testversion zur Verfügung gestellt werden kann. Die Ressourcen für Ihre Logik-App werden in der von Ihnen gewählten Region bereitgestellt. Wenn die Logik-App benutzerdefinierte Ressourcen umfasst, die in einer bestimmten Region gespeichert sind, müssen Sie diese Region hier auswählen. Dazu empfiehlt es sich, die Logik-App lokal in Ihrem Azure-Abonnement im Portal vollständig bereitzustellen und zu überprüfen, ob sie ordnungsgemäß ausgeführt wird, bevor Sie diese Auswahl vornehmen.

- **Maximale Anzahl gleichzeitiger Testversionen** (erforderlich): Legen Sie die maximale Anzahl von Kunden fest, die Ihre Testversion gleichzeitig verwenden können. Diese Testversionen sind bereits bereitgestellt, sodass Kunden sofort auf sie zugreifen können, ohne auf eine Bereitstellung warten zu müssen.

- **Dauer der Testversion** (erforderlich): Geben Sie den Zeitraum, in dem die Testversion aktiv bleibt, als Anzahl von Stunden ein. Nach Ablauf dieses Zeitraums wird die Testversion automatisch beendet.

- **Name der Azure-Ressourcengruppe** (erforderlich): Geben Sie den Namen der [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) ein, in dem die Logik-App-Testversion gespeichert wird.

- **Azure logic app name** (Name der Azure-Logik-App) (erforderlich): Geben Sie den Namen der Logik-App ein, von der die Testversion dem Benutzer zugewiesen wird. Diese Logik-App muss in der oben angegebenen Azure-Ressourcengruppe gespeichert werden.

- **Deprovision logic app name** (Logik-App-Name für Aufhebung der Bereitstellung) (erforderlich): Geben Sie den Namen der Logik-App ein, mit der die Bereitstellung der Testversion aufgehoben wird, wenn sie vom Kunden nicht mehr verwendet wird. Diese Logik-App muss in der oben angegebenen Azure-Ressourcengruppe gespeichert werden.

#### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Technical configuration not required for Power BI test drives (Power BI-Testversionen erfordern keine technische Konfiguration)

Für Produkte, mit denen ein interaktives Power BI-Visual veranschaulicht werden soll, kann ein eingebetteter Link zum Freigeben eines benutzerdefinierten Dashboards als Testversion verwendet werden. Deshalb ist keine technische Konfiguration erforderlich. Erfahren Sie mehr über das Einrichten von[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)-Vorlagen-Apps.

### <a name="deployment-subscription-details"></a>Abonnementdetails für Bereitstellung der Testversion

Um die Testversion in Ihrem Auftrag bereitzustellen, erstellen Sie ein eigenes eindeutiges Azure-Abonnement, und stellen Sie es bereit. (Power BI-Testversionen nicht erforderlich).

- **Azure-Abonnement-ID** (erforderlich für Azure Resource Manager und Logik-Apps): Geben Sie die Abonnement-ID ein, um für Ressourcennutzungsberichte und Abrechnungszwecke Zugriff auf die Dienste Ihres Azure-Kontos zu gewähren. Es wird empfohlen, [ein eigenes Azure-Abonnement zu erstellen](https://docs.microsoft.com/azure/billing/billing-create-subscription), das für Testversionen verwendet wird, falls Sie es noch nicht getan haben. Sie finden die Azure-Abonnement-IDs, indem Sie sich beim [Azure-Portal](https://portal.azure.com/) anmelden und im Menü auf der linken Seite zu den **Abonnements** navigieren. Durch Auswahl der Registerkarte wird Ihre Abonnement-ID angezeigt (z.B. a83645ac-1234-5ab6-6789-1h234g764ghty).

- **ID des Azure AD-Mandanten** (erforderlich): Geben Sie Ihre [Mandanten-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) von Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ aus, klicken Sie auf **Eigenschaften**, und suchen Sie dann die aufgeführte Nummer der **Verzeichnis-ID** (z.B. 50c464d3-4930-494c-963c-1e951d15360e). Sie können auch unter [https://www.whatismytenantid.com](https://www.whatismytenantid.com) die Mandanten-ID Ihrer Organisation mithilfe einer Domänennamen-URL nachschlagen.

- **Name des Azure AD-Mandanten** (erforderlich für Dynamics 365): Geben Sie den Namen Ihres Azure Active Directory (AD) ein. Um diesen Namen zu suchen, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an. Oben rechts wird der Mandantenname unter Ihrem Kontonamen angegeben.

- **Azure AD-App-ID** (erforderlich): Geben Sie die [Anwendungs-ID](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) von Azure Active Directory (AD) ein. Um die ID zu ermitteln, melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, wählen Sie im linken Menü die Registerkarte „Active Directory“ aus, klicken Sie auf **App-Registrierungen**, und suchen Sie dann die aufgeführte Nummer der **Anwendungs-ID** (z.B. 50c464d3-4930-494c-963c-1e951d15360e).

- **Geheimer Azure AD-App-Clientschlüssel** (erforderlich): Geben Sie Ihren [geheimen Clientschlüssel](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets) für die Azure AD-Anwendung ein. Um diesen Wert zu finden, melden Sie sich beim [Azur-Portal](https://portal.azure.com/) an. Wählen Sie im linken Menü die Registerkarte **Azure Active Directory** aus, wählen Sie **App-Registrierungen** aus und dann Ihre Testversion-App. Wählen Sie als Nächstes **Zertifikate und Geheimnisse** und dann **Neuer geheimer Clientschlüssel** aus, geben Sie eine Beschreibung ein, wählen Sie **Nie** unter **Läuft ab** aus, und wählen Sie dann **Hinzufügen** aus. Stellen Sie sicher, dass Sie den Wert kopieren. (Navigieren Sie nicht von der Seite weg, bevor Sie dies tun, sonst haben Sie keinen Zugriff auf den Wert.)

Klicken Sie auf **Speichern**, bevor Sie mit dem nächsten Abschnitt fortfahren!

### <a name="test-drive-listings-optional"></a>Test drive listings (Testversionslisten) (optional)

Auf der Registerkarte **Test drive listings** (Testversionslisten) unter **Testversion** werden die Sprachen (und Märkte) angezeigt, in denen Ihre Testversion verfügbar ist. Derzeit ist nur der Standort „Englisch (USA)“ verfügbar. Auf dieser Seite werden zudem der Status der sprachspezifischen Liste und das Datum/die Uhrzeit der Hinzufügung angezeigt. Sie müssen für jede Sprache/jeden Markt die Testversionsdetails (Beschreibung, Benutzerhandbuch, Videos usw.) definieren.

- **Beschreibung:** (erforderlich): Beschreiben Sie Ihre Testversion. Geben Sie an, was vorgeführt wird, nennen Sie die Ziele, mit denen der Benutzer experimentieren soll, sowie die zu erkundenden Funktionen, und geben Sie relevante Informationen an, die dem Benutzer helfen zu entscheiden, ob er Ihr Angebot erwerben soll. In diesem Feld können bis zu 3.000 Zeichen Text eingegeben werden. 

- **Zugriffsinformationen** (für Azure Resource Manager und Logik-Testversionen erforderlich): Erläutern Sie, was ein Kunde wissen muss, um auf die Testversion zuzugreifen und sie zu verwenden. Beschreiben Sie ein Szenario für die Verwendung Ihres Angebots, und teilen Sie genau mit, was der Kunde wissen sollte, um auf die Funktionen in der gesamten Testversion zuzugreifen. In diesem Feld können bis zu 10.000 Zeichen Text eingegeben werden.

- **Benutzerhandbuch** (erforderlich): Eine ausführliche Beschreibung der Verwendung Ihrer Testversion. Das Benutzerhandbuch sollte genau beschreiben, was der Kunde durch die Erkundung der Testversion erlangen soll, und als Referenz für die Fragen des Kunden dienen. Die Datei muss das PDF-Format aufweisen und nach dem Hochladen mit einem Namen (max. 255 Zeichen) versehen werden.

- **Videos: Videos hinzufügen** (optional): Videos können auf YouTube oder Vimeo hochgeladen werden, und hier kann mit einem Link und einer Miniaturansicht (533 x 324 Pixel) auf sie verwiesen werden. So erhalten die Kunden Informationen zu einzelnen Schritten, um ein besseres Verständnis der Testversion zu erlangen. Unter anderem können Sie ihnen zeigen, wie sie die Funktionen ihres Angebots erfolgreich verwenden, und Szenarien erläutern, in denen die Vorteile hervorgehoben werden.
  - **Name** (erforderlich)
  - **URL (nur YouTube oder Vimeo)** (erforderlich)
  - **Thumbnail (533 x 324px)** (Miniaturansicht (533 x 324 px)): Die Bilddatei muss im PNG-Format vorliegen.

Klicken Sie auf **Speichern**, nachdem Sie die Angaben in diesen Feldern vorgenommen haben.
