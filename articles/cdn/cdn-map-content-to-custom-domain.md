---
title: Tutorial – Hinzufügen einer benutzerdefinierten Domäne zum Azure CDN-Endpunkt | Microsoft-Dokumentation
description: In diesem Tutorial ordnen Sie den Inhalt eines Azure CDN-Endpunkts einer benutzerdefinierten Domäne zu.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2018
ms.author: magattus
ms.custom: mvc
ms.openlocfilehash: 22283833ebb414372de16cbe4ce7d3986cd400a9
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73837418"
---
# <a name="tutorial-add-a-custom-domain-to-your-azure-cdn-endpoint"></a>Tutorial: Hinzufügen einer benutzerdefinierten Domäne zum Azure CDN-Endpunkt
In diesem Tutorial wird veranschaulicht, wie Sie einem Azure CDN-Endpunkt (Content Delivery Network) eine benutzerdefinierte Domäne hinzufügen. Wenn Sie einen CDN-Endpunkt zum Bereitstellen von Inhalt verwenden, ist eine benutzerdefinierte Domäne erforderlich, falls Ihr eigener Domänenname in Ihrer CDN-URL sichtbar sein soll. Die Verwendung eines sichtbaren Domänennamens kann für Ihre Kunden komfortabel und für Branding-Zwecke hilfreich sein. 

Nachdem Sie einen CDN-Endpunkt in Ihrem Profil erstellt haben, wird der Endpunktname, bei dem es sich um eine Unterdomäne von „azureedge.net“ handelt, standardmäßig in die URL für die Bereitstellung von CDN-Inhalten eingefügt (z.B. „https:\//contoso.azureedge.net/photo.png“). Als Hilfe für Sie verfügt Azure CDN über die Möglichkeit, eine benutzerdefinierte Domäne einem CDN-Endpunkt zuzuordnen. Bei dieser Option stellen Sie Ihren Inhalt über eine benutzerdefinierte Domäne in Ihrer URL und nicht über einen Endpunktnamen bereit (z.B. „https:\//www.contoso.com/photo.png“). 

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> - Erstellen eines CNAME-DNS-Eintrags
> - Zuordnen der benutzerdefinierten Domäne zu Ihrem CDN-Endpunkt
> - Überprüfen der benutzerdefinierten Domäne

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie die Schritte in diesem Tutorial ausführen können, müssen Sie zunächst ein CDN-Profil und mindestens einen CDN-Endpunkt erstellen. Weitere Informationen finden Sie unter [Quickstart: Erstellen eines Azure CDN-Profils und -Endpunkts](cdn-create-new-endpoint.md).

Wenn Sie nicht bereits über eine benutzerdefinierte Domäne verfügen, müssen Sie zunächst bei einem Domänenanbieter eine erwerben. Informationen hierzu finden Sie beispielsweise unter [Kaufen eines benutzerdefinierten Domänennamens für Azure-Web-Apps](https://docs.microsoft.com/azure/app-service/manage-custom-dns-buy-domain).

Wenn Sie Azure zum Hosten Ihrer [DNS-Domänen](https://docs.microsoft.com/azure/dns/dns-overview) verwenden, müssen Sie das Domain Name System (DNS) des Domänenanbieters an eine Azure DNS-Instanz delegieren. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns). Fahren Sie andernfalls mit [Erstellen eines CNAME-DNS-Eintrags](#create-a-cname-dns-record) fort, wenn Sie für Ihre DNS-Domäne einen Domänenanbieter verwenden.


## <a name="create-a-cname-dns-record"></a>Erstellen eines CNAME-DNS-Eintrags

Bevor Sie eine benutzerdefinierte Domäne mit einem Azure CDN-Endpunkt verwenden können, müssen Sie zunächst einen kanonischen Namenseintrag (CNAME) bei Ihrem Domänenanbieter erstellen, mit dem auf Ihren CDN-Endpunkt verwiesen wird. Ein CNAME-Eintrag ist eine Art von DNS-Eintrag, mit dem ein Quelldomänenname einem Zieldomänennamen zugeordnet wird. Für Azure CDN ist der Quelldomänenname Ihr benutzerdefinierter Domänenname, und der Zieldomänenname entspricht dem Hostnamen Ihres CDN-Endpunkts. Nachdem der von Ihnen erstellte CNAME-Eintrag von Azure CDN überprüft wurde, wird der für die benutzerdefinierte Quelldomäne (z.B. „www\.contoso.com“) bestimmte Datenverkehr an den angegebenen Hostnamen des CDN-Zielendpunkts geleitet (z.B. „contoso.azureedge.net“). 

Eine benutzerdefinierte Domäne und die dazugehörige Unterdomäne können jeweils nur einem Endpunkt zugeordnet werden. Sie können aber unterschiedliche Unterdomänen derselben benutzerdefinierten Domäne für unterschiedliche Azure-Dienstendpunkte verwenden, indem Sie mehrere CNAME-Einträge nutzen. Außerdem können Sie eine benutzerdefinierte Domäne mit unterschiedlichen Unterdomänen demselben CDN-Endpunkt zuordnen.

> [!NOTE]
> Für benutzerdefinierte Domänen kann jeder Aliaseintragstyp verwendet werden, wenn Sie Azure DNS als Domänenanbieter nutzen. In dieser exemplarischen Vorgehensweise wird der Eintragstyp CNAME verwendet. Wenn Sie die Eintragstypen A oder AAAA verwenden, führen Sie die gleichen unten gezeigten Schritte aus, und ersetzen Sie CNAME durch den Eintragstyp Ihrer Wahl. Wenn Sie einen Aliaseintrag verwenden, um eine Stammdomäne als benutzerdefinierte Domäne zu verwenden, und SSL aktivieren möchten, müssen Sie die manuelle Überprüfung verwenden, die [diesem Artikel](https://docs.microsoft.com/azure/cdn/cdn-custom-ssl?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate#custom-domain-is-not-mapped-to-your-cdn-endpoint) beschrieben wird. Weitere Informationen finden Sie unter [Verweisen an der Zonenspitze auf Azure CDN-Endpunkte](https://docs.microsoft.com/azure/dns/dns-alias#point-zone-apex-to-azure-cdn-endpoints).

## <a name="map-the-temporary-cdnverify-subdomain"></a>Zuordnen der temporären Unterdomäne „cdnverify“

Es müssen bestimmte Aspekte berücksichtigt werden, wenn Sie eine vorhandene Domäne zuordnen, die sich in der Produktion befindet. Während Sie Ihre benutzerdefinierte Domäne im Azure-Portal registrieren, kann es für die Domäne zu einer kurzen Ausfallzeit kommen. Ordnen Sie Ihre benutzerdefinierte Domäne zuerst dem Hostnamen Ihres CDN-Endpunkts mit der Azure-Unterdomäne „cdnverify“ zu, um eine temporäre CNAME-Zuordnung zu erstellen und so eine Unterbrechung des Webdatenverkehrs zu vermeiden. Mit dieser Methode können Benutzer ohne Unterbrechung auf Ihre Domäne zugreifen, während die DNS-Zuordnung durchgeführt wird. 

Falls Sie Ihre benutzerdefinierte Domäne zum ersten Mal verwenden und dafür kein Produktionsdatenverkehr aktiv ist, können Sie die benutzerdefinierte Domäne auch direkt Ihrem CDN-Endpunkt zuordnen. Fahren Sie mit [Zuordnen der permanenten benutzerdefinierten Domäne](#map-the-permanent-custom-domain) fort.

Erstellen Sie wie folgt einen CNAME-Eintrag mit der Unterdomäne „cdnverify“:

1. Melden Sie sich an der Website des Domänenanbieters für Ihre benutzerdefinierte Domäne an.

2. Suchen Sie mithilfe der Dokumentation des Anbieters nach der Seite für die Verwaltung von DNS-Einträgen, oder suchen Sie nach Bereichen der Website mit der Bezeichnung **Domänenname**, **DNS** oder **Namensserververwaltung**. 

3. Erstellen Sie einen CNAME-Eintrag für Ihre benutzerdefinierte Domäne, und füllen Sie die Felder wie in der folgenden Tabelle gezeigt aus (Feldnamen können variieren):

    | `Source`                    | type  | Destination                     |
    |---------------------------|-------|---------------------------------|
    | cdnverify.www.contoso.com | CNAME | cdnverify.contoso.azureedge.net |

    - Quelle: Geben Sie den Namen Ihrer benutzerdefinierten Domäne, einschließlich der Unterdomäne „cdnverify“, im folgenden Format ein: cdnverify.&lt;Name der benutzerdefinierten Domäne&gt;. Beispiel: „cdnverify.www.contoso.com“.

    - Geben Sie Folgendes ein:  Geben Sie *CNAME* ein.

    - Ziel: Geben Sie den Hostnamen Ihres CDN-Endpunkts, einschließlich der Unterdomäne „cdnverify“, im folgenden Format ein: cdnverify. _&lt;Endpunktname&gt;_ .azureedge.net. Beispiel: „cdnverify.contoso.azureedge.net“.

4. Speichern Sie die Änderungen.

Für die Domänenregistrierungsstelle GoDaddy gilt beispielsweise folgende Vorgehensweise:

1. Melden Sie sich an, und wählen Sie die gewünschte benutzerdefinierte Domäne aus.

2. Wählen Sie im Abschnitt „Domänen“ die Option **Manage All** (Alle verwalten) und dann **DNS** | **Manage Zones** (Zonen verwalten).

3. Geben Sie unter **Domänenname** Ihre benutzerdefinierte Domäne ein, und wählen Sie anschließend **Auswählen**.

4. Wählen Sie auf der Seite **DNS-Verwaltung** die Option **Hinzufügen** und dann in der Liste **Typ** den Eintrag **CNAME**.

5. Füllen Sie für den CNAME-Eintrag die folgenden Felder aus:

    ![CNAME-Eintrag](./media/cdn-map-content-to-custom-domain/cdn-cdnverify-cname-entry.png)

    - Geben Sie Folgendes ein:  Übernehmen Sie *CNAME*.

    - Host: Geben Sie die gewünschte Unterdomäne Ihrer benutzerdefinierten Domäne ein (einschließlich des Unterdomänennamens „cdnverify“). Beispiel: „cdnverify.www“.

    - Points to (Verweist auf): Geben Sie den Hostnamen Ihres CDN-Endpunkts ein (einschließlich des Unterdomänennamens „cdnverify“). Beispiel: „cdnverify.contoso.azureedge.net“. 

    - TTL: Übernehmen Sie *1 Stunde*.

6. Wählen Sie **Speichern** aus.
 
    Der CNAME-Eintrag wird der Tabelle mit den DNS-Einträgen hinzugefügt.

    ![Tabelle mit DNS-Einträgen](./media/cdn-map-content-to-custom-domain/cdn-cdnverify-dns-table.png)


## <a name="associate-the-custom-domain-with-your-cdn-endpoint"></a>Zuordnen der benutzerdefinierten Domäne zu Ihrem CDN-Endpunkt

Nachdem Sie Ihre benutzerdefinierte Domäne registriert haben, können Sie sie dem CDN-Endpunkt hinzufügen. 

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com/) an, und navigieren Sie zu dem CDN-Profil mit dem Endpunkt, den Sie einer benutzerdefinierten Domäne zuordnen möchten.
    
2. Wählen Sie auf der Seite **CDN-Profil** den CDN-Endpunkt aus, der der benutzerdefinierten Domäne zugeordnet werden soll.

   Die Seite **Endpunkt** wird geöffnet.
    
3. Wählen Sie **Benutzerdefinierte Domäne**. 

   ![Schaltfläche für benutzerdefinierte CDN-Domäne](./media/cdn-map-content-to-custom-domain/cdn-custom-domain-button.png)

   Die Seite **Benutzerdefinierte Domäne hinzufügen** wird geöffnet.

4. Unter **Endpunkthostname** ist der Endpunkthostname, der als Zieldomäne Ihres CNAME-Eintrags verwendet werden soll, bereits angegeben. Er wird von der CDN-Endpunkt-URL abgeleitet: *&lt;Endpunkthostname&gt;* .azureedge.net. Diese Einstellung kann nicht geändert werden.

5. Geben Sie unter **Benutzerdefinierter Hostname** Ihre benutzerdefinierte Domäne (einschließlich Unterdomäne) ein, die als Quelldomäne Ihres CNAME-Eintrags verwendet werden soll. Beispiel: „www\.contoso.com“ oder „cdn.contoso.com“. Verwenden Sie nicht den Namen der Unterdomäne „cdnverify“.

   ![Dialogfeld für benutzerdefinierte CDN-Domäne](./media/cdn-map-content-to-custom-domain/cdn-add-custom-domain.png)

6. Wählen Sie **Hinzufügen**.

   Azure überprüft, ob der CNAME-Eintrag für den eingegebenen Namen der benutzerdefinierten Domäne vorhanden ist. Wenn der CNAME-Eintrag korrekt ist, wird Ihre benutzerdefinierte Domäne überprüft. 

   Es kann einige Zeit dauern, bis die neuen Einstellungen der benutzerdefinierten Domäne für alle CDN-Edgeknoten übernommen werden: 
    - Bei Profilen vom Typ **Azure CDN Standard von Microsoft** ist die Weitergabe in der Regel in zehn Minuten abgeschlossen. 
    - Bei **Azure CDN Standard von Akamai**-Profilen ist die Weitergabe in der Regel in einer Minute abgeschlossen. 
    - Bei Profilen vom Typ **Azure CDN Standard von Verizon** und **Azure CDN Premium von Verizon** ist die Weitergabe in der Regel in zehn Minuten abgeschlossen.   


## <a name="verify-the-custom-domain"></a>Überprüfen der benutzerdefinierten Domäne

Nachdem Sie die Registrierung der benutzerdefinierten Domäne abgeschlossen haben, sollten Sie sicherstellen, dass die benutzerdefinierte Domäne auf Ihren CDN-Endpunkt verweist.
 
1. Stellen Sie sicher, dass öffentliche Inhalte zum Zwischenspeichern auf dem Endpunkt verfügbar sind. Wenn der CDN-Endpunkt beispielsweise einem Speicherkonto zugeordnet ist, wird der Inhalt vom Azure CDN in einem öffentlichen Container zwischengespeichert. Stellen Sie zum Testen der benutzerdefinierten Domäne sicher, dass der Container den öffentlichen Zugriff zulässt und mindestens eine Datei enthält.

2. Navigieren Sie in Ihrem Browser mithilfe der benutzerdefinierten Domäne zur Adresse der Datei. Wenn Ihre benutzerdefinierte Domäne beispielsweise `www.contoso.com` ist, sollte die URL für die zwischengespeicherte Datei der folgenden URL ähneln: `http://www.contoso.com/my-public-container/my-file.jpg`. Vergewissern Sie sich, dass Ergebnis mit dem Ergebnis des direkten Zugriffs auf den CDN-Endpunkt unter *&lt;Endpunkthostname&gt;* .azureedge.net identisch ist.


## <a name="map-the-permanent-custom-domain"></a>Zuordnen der permanenten benutzerdefinierten Domäne

Wenn Sie sich vergewissert haben, dass die Unterdomäne „cdnverify“ Ihrem Endpunkt erfolgreich zugeordnet wurde (oder wenn Sie eine neue benutzerdefinierte Domäne verwenden, die nicht für die Produktion bestimmt ist), können Sie die benutzerdefinierte Domäne direkt dem Hostnamen Ihres CDN-Endpunkts zuordnen.

Erstellen Sie wie folgt einen CNAME-Eintrag für Ihre benutzerdefinierten Domäne:

1. Melden Sie sich an der Website des Domänenanbieters für Ihre benutzerdefinierte Domäne an.

2. Suchen Sie die Seite für die Verwaltung von DNS-Einträgen mithilfe der Dokumentation des Anbieters, oder suchen Sie nach Bereichen der Website mit der Bezeichnung **Domänennamen**, **DNS**, oder **Namensserververwaltung**. 

3. Erstellen Sie einen CNAME-Eintrag für Ihre benutzerdefinierte Domäne, und füllen Sie die Felder wie in der folgenden Tabelle gezeigt aus (Feldnamen können variieren):

    | `Source`          | type  | Destination           |
    |-----------------|-------|-----------------------|
    | <www.contoso.com> | CNAME | contoso.azureedge.net |

   - Quelle: Geben Sie den Namen Ihrer benutzerdefinierten Domäne ein (z. B. „www\.contoso.com“).

   - Geben Sie Folgendes ein:  Geben Sie *CNAME* ein.

   - Ziel: Geben Sie den Hostnamen Ihres CDN-Endpunkts ein. Er muss das folgende Format haben: _&lt;Endpunktname&gt;_ .azureedge.net. Beispiel: „contoso.azureedge.net“.

4. Speichern Sie die Änderungen.

5. Wenn Sie zuvor einen temporären CNAME-Eintrag für die Unterdomäne „cdnverify“ erstellt haben, sollten Sie ihn löschen. 

6. Wenn Sie diese benutzerdefinierte Domäne zum ersten Mal in der Produktion verwenden, können Sie die Schritte unter [Zuordnen der benutzerdefinierten Domäne zu Ihrem CDN-Endpunkt](#associate-the-custom-domain-with-your-cdn-endpoint) und [Überprüfen der benutzerdefinierten Domäne](#verify-the-custom-domain) ausführen.

Für die Domänenregistrierungsstelle GoDaddy gilt beispielsweise folgende Vorgehensweise:

1. Melden Sie sich an, und wählen Sie die gewünschte benutzerdefinierte Domäne aus.

2. Wählen Sie im Abschnitt „Domänen“ die Option **Manage All** (Alle verwalten) und dann **DNS** | **Manage Zones** (Zonen verwalten).

3. Geben Sie unter **Domänenname** Ihre benutzerdefinierte Domäne ein, und wählen Sie anschließend **Auswählen**.

4. Wählen Sie auf der Seite **DNS-Verwaltung** die Option **Hinzufügen** und dann in der Liste **Typ** den Eintrag **CNAME**.

5. Füllen Sie die Felder für den CNAME-Eintrag aus:

    ![CNAME-Eintrag](./media/cdn-map-content-to-custom-domain/cdn-cname-entry.png)

    - Geben Sie Folgendes ein:  Übernehmen Sie *CNAME*.

    - Host: Geben Sie die Unterdomäne der gewünschten benutzerdefinierten Domäne ein. Beispiel: „www“ oder „cdn“.

    - Points to (Verweist auf): Geben Sie den Hostnamen Ihres CDN-Endpunkts ein. Beispiel: „contoso.azureedge.net“. 

    - TTL: Übernehmen Sie *1 Stunde*.

6. Wählen Sie **Speichern** aus.
 
    Der CNAME-Eintrag wird der Tabelle mit den DNS-Einträgen hinzugefügt.

    ![Tabelle mit DNS-Einträgen](./media/cdn-map-content-to-custom-domain/cdn-dns-table.png)

7. Wählen Sie bei Vorhandensein des CNAME-Eintrags „cdnverify“ daneben das Stiftsymbol und dann das Papierkorbsymbol aus.

8. Wählen Sie **Löschen**, um den CNAME-Eintrag zu löschen.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

In den vorherigen Schritten haben Sie einem CDN-Endpunkt eine benutzerdefinierte Domäne hinzugefügt. Wenn Sie die Zuordnung Ihres Endpunkts zu einer benutzerdefinierten Domäne aufheben möchten, können Sie die benutzerdefinierte Domäne mit den folgenden Schritten entfernen:
 
1. Wählen Sie in Ihrem CDN-Profil den Endpunkt mit der benutzerdefinierten Domäne aus, die Sie entfernen möchten.

2. Klicken Sie auf der Seite **Endpunkt** unter „Benutzerdefinierte Domänen“ mit der rechten Maustaste auf die benutzerdefinierte Domäne, die Sie entfernen möchten, und wählen Sie im Kontextmenü dann die Option **Löschen**.  

   Die Zuordnung der benutzerdefinierten Domäne zum Endpunkt wird aufgehoben.


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> - Erstellen eines CNAME-DNS-Eintrags
> - Zuordnen der benutzerdefinierten Domäne zu Ihrem CDN-Endpunkt
> - Überprüfen der benutzerdefinierten Domäne

Fahren Sie mit dem nächsten Tutorial fort, um zu erfahren, wie Sie HTTPS für eine benutzerdefinierte Azure CDN-Domäne konfigurieren.

> [!div class="nextstepaction"]
> [Tutorial: Konfigurieren von HTTPS in einer benutzerdefinierten Azure CDN-Domäne](cdn-custom-ssl.md)


