---
title: Integrieren einer Stamm- oder Apex-Domäne in eine Front Door-Instanz mithilfe des Azure-Portals
description: Informationen zum Integrieren einer Stamm- oder Apex-Domäne in eine Front Door-Instanz mithilfe des Azure-Portals
services: front-door
author: sharad4u
ms.service: front-door
ms.topic: article
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: 464c38b0ece274d2ea7df89ab9fd7c0a60dc4b58
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332292"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Integrieren einer Stamm- oder Apex-Domäne in Ihre Front Door-Instanz
Azure Front Door verwendet CNAME-Einträge zum Überprüfen des Domänenbesitzes beim Integrieren von benutzerdefinierten Domänen. Außerdem gibt Front Door nicht die Frontend-IP-Adresse frei, die Ihrem Front Door-Profil zugeordnet ist. Daher können Sie Ihre Apex-Domäne keiner IP-Adresse zuordnen, wenn Sie die Absicht haben, die Front Door-Instanz zu integrieren.

Das DNS-Protokoll verhindert die Zuweisung von CNAME-Einträgen am Zonen-Apex. Wenn die Domäne beispielsweise `contoso.com` heißt, können Sie CNAME-Einträge für `somelabel.contoso.com`, aber keinen CNAME-Eintrag für `contoso.com` selbst erstellen. Diese Einschränkung stellt Anwendungsbesitzer, die über Anwendungen mit Lastenausgleich über Azure Front Door verfügen, vor ein Problem. Da für die Verwendung eines Front Door-Profils die Erstellung eines CNAME-Eintrags erforderlich ist, kann vom Zonen-Apex aus nicht auf das Front Door-Profil verwiesen werden.

Dieses Problem wird in Azure DNS mithilfe von Aliaseinträgen gelöst. Im Gegensatz zu CNAME-Einträgen werden Aliaseinträge am Zonen-Apex erstellt, und Anwendungsbesitzer können damit ihren Zonen-Apex-Eintrag an ein Front Door-Profil mit öffentlichen Endpunkten verweisen. Anwendungsbesitzer verweisen auf das gleiche Front Door-Profil, das auch für andere Domänen innerhalb ihrer DNS-Zone verwendet wird. Beispielsweise können `contoso.com` und `www.contoso.com` beide auf dasselbe Front Door-Profil verweisen. 

Zum Zuweisen Ihrer Apex- oder Stammdomäne an Ihr Front Door-Profil muss in erster Linie der CNAME-Eintrag vereinfacht oder DNS nachverfolgt werden. Es handelt sich dabei um einen Mechanismus, bei dem der DNS-Anbieter auf rekursive Weise den CNAME-Eintrag auflöst, bis eine IP-Adresse erreicht wird. Diese Funktion wird von Azure DNS für Front Door-Endpunkte unterstützt. 

> [!NOTE]
> Es gibt außerdem weitere DNS-Anbieter, die das Vereinfachen von CNAME oder das Nachverfolgen von DNS unterstützen. Allerdings wird den Azure Front Door-Kunden die Verwendung von Azure DNS für das Hosten von Domänen empfohlen.

Sie können das Azure-Portal verwenden, um eine Apex-Domäne in Ihre Front Door-Instanz zu integrieren und HTTPS für diese Instanz zu aktivieren, indem Sie sie einem Zertifikat für die SSL-Terminierung zuweisen. Apex-Domänen werden auch als Stammdomänen oder Naked Domains bezeichnet.

In diesem Artikel werden folgende Vorgehensweisen behandelt:

> [!div class="checklist"]
> * Erstellen eines Aliaseintrags, der auf das Front Door-Profil verweist
> * Hinzufügen einer Stammdomäne zur einer Front Door-Instanz
> * Einrichten von HTTPS auf der Stammdomäne

> [!NOTE]
> In diesem Tutorial wird vorausgesetzt, dass Sie bereits ein Front Door-Profil erstellt haben. Weitere Informationen zu den Grundlegen erhalten Sie unter [Quickstart: Create a Front Door (Schnellstart: Erstellen einer Front Door-Instanz)](./quickstart-create-front-door.md) oder unter [Erstellen einer Front Door-Ressource mit Umleitung von HTTP zu HTTPS](./front-door-how-to-redirect-https.md).

## <a name="create-an-alias-record-for-zone-apex"></a>Erstellen eines Aliaseintrags für einen Zonen-Apex

1. Öffnen Sie die Konfiguration **Azure DNS**, damit die Domäne integriert werden kann.
2. Erstellen oder bearbeiten Sie den Eintrag für den Zonen-Apex.
3. Wählen Sie den Eintrag **Typ** als Eintrag _A_ aus, und klicken Sie anschließend für **Alias record set** (Aliasdatensatz) auf _Ja_. Als **Aliastyp** sollte _Azure-Ressource_ festgelegt sein.
4. Wählen Sie das Azure-Abonnement aus, auf dem Ihr Front Door-Profil gehostet wird, und klicken Sie anschließend in der Dropdownliste **Azure-Ressource** auf die entsprechende Front Door-Ressource.
5. Klicken Sie auf **OK**, um die Änderungen zu übermitteln.

    ![Erstellen eines Aliaseintrags für einen Zonen-Apex](./media/front-door-apex-domain/front-door-apex-alias-record.png)

6. Wenn Sie den oben beschriebenen Schritt ausführen, werden ein Eintrag für den Zonen-Apex, der auf Ihre Front Door-Ressource zeigt, sowie ein CNAME-Eintrag erstellt, der „afdverify“ (z. B. `afdverify.contosonews.com`) `afdverify.<name>.azurefd.net` zuordnet. Dies wird zum Integrieren der Domäne in Ihr Front Door-Profil verwendet.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Integrieren der benutzerdefinierten Domäne in Ihre Front Door-Instanz

1. Klicken Sie in Front Door auf der Registerkarte „Designer“ im Abschnitt „Front-End-Hosts“ auf das Symbol „+“, um eine neue benutzerdefinierte Domäne hinzuzufügen.
2. Geben Sie in das Feld „Benutzerdefinierter Hostname“ den Namen der Stamm- oder Apex-Domäne ein, z. B. `contosonews.com`.
3. Nachdem die CNAME-Zuordnung von der Domäne zu Ihrer Front Door-Instanz überprüft wurde, klicken Sie auf **Hinzufügen**, um die benutzerdefinierte Domäne hinzuzufügen.
4. Klicken Sie auf **Speichern**, um die Änderungen zu übermitteln.

![Menü „Benutzerdefinierte Domänen“](./media/front-door-apex-domain/front-door-onboard-apex-domain.png)

## <a name="enable-https-on-your-custom-domain"></a>Aktivieren von HTTPS für eine benutzerdefinierte Domäne

1. Klicken Sie auf die benutzerdefinierte Domäne, die hinzugefügt wurde, und ändern Sie den Status unter dem Abschnitt **HTTPS für benutzerdefinierte Domänen** in **Aktiviert**.
2. Wählen Sie unter **Zertifikatverwaltungstyp** die Option _Eigenes Zertifikat verwenden_ aus.

> [!WARNING]
> Der Zertifikatverwaltungstyp „Front Door managed“ (Mit Front Door verwaltet) wird derzeit nicht für Apex- oder Stammdomänen unterstützt. Sie können nur Ihr eigenes benutzerdefiniertes SSL-Zertifikat verwenden, das auf Azure Key Vault gehostet wird, um HTTPS auf einer Apex- oder Stammdomäne für Front Door zu aktivieren.

3. Vergewissern Sie sich, dass Sie die richtigen Berechtigungen für Front Door eingerichtet haben, um wie auf der Benutzeroberfläche angegeben auf Ihren Schlüsseltresor zugreifen zu können, bevor Sie mit dem nächsten Schritt fortfahren.
4. Wählen Sie ein **Key Vault-Konto** aus, das Ihrem aktuellen Abonnement zugeordnet ist, und wählen Sie anschließend das entsprechende **Geheimnis** einschließlich der jeweiligen **Geheimnisversion** aus, um das Konto dem richtigen Zertifikat zuzuordnen.
5. Klicken Sie auf **Aktualisieren**, um die Auswahl zu speichern, und klicken Sie dann auf **Speichern**.
6. Klicken Sie nach einigen Minuten auf **Aktualisieren**, und klicken Sie dann noch mal auf die benutzerdefinierte Domäne, um den Status der Zertifikatbereitstellung anzuzeigen. 

> [!WARNING]
> Vergewissern Sie sich, dass Sie angemessene Routingregeln für Ihre Apex-Domäne erstellt oder die Domäne bereits vorhandenen Routingregeln zugeordnet haben.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Erstellen einer Azure Front Door Service-Konfiguration](quickstart-create-front-door.md).
- Informieren Sie sich über die [Funktionsweise von Azure Front Door Service](front-door-routing-architecture.md).