---
title: Abrufen von Drohnenbildern
description: Hier erfahren Sie, wie Sie die Drohnenbilder von Partnern abrufen.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 598248a0efb3322a9c22a5e38e4986f5ba5142ab
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797467"
---
# <a name="get-drone-imagery-from-drone-partners"></a>Abrufen der Drohnenbilder von Drohnenpartnern

In diesem Artikel erfahren Sie, wie Sie Orthofotodaten von Ihren Drohnenbildpartnern im Azure FarmBeats-Datenhub platzieren. Aktuell werden folgende Bildpartner unterstützt:  

  ![FarmBeats-Projekt](./media/get-drone-imagery-from-drone-partner/drone-partner-1.png)

Durch die Integration von Drohnenbilddaten in Azure FarmBeats können Sie Orthofotodaten von Drohnenflügen über Ihren landwirtschaftlichen Betrieb im Datenhub platzieren. Die verfügbaren Daten können dann im FarmBeats Accelerator angezeigt sowie für die Datenfusion und die Erstellung von KI-/ML-Modellen verwendet werden.

## <a name="before-you-begin"></a>Voraussetzungen

  - Vergewissern Sie sich, dass Sie Azure FarmBeats bereitgestellt haben. Eine Bereitstellungsanleitung finden Sie unter [Bereitstellen von FarmBeats](prepare-for-deployment.md).
  - Vergewissern Sie sich, dass der landwirtschaftliche Betrieb, für den Sie Drohnenbilder verwenden möchten, in Ihrem FarmBeats-System definiert ist.

## <a name="enable-drone-imagery-integration-with-farmbeats"></a>Ermöglichen der Integration von Drohnenbildern in FarmBeats   

Ihr Geräteanbieter benötigt folgende Informationen, um die Integration in FarmBeats zu ermöglichen:  
 - API-Endpunkt  
 - Mandanten-ID  
 - Client-ID  
 - Geheimer Clientschlüssel  

Führen Sie die folgenden Schritte aus:

1. Laden Sie [dieses Skript](https://aka.ms/farmbeatspartnerscript) herunter, und extrahieren Sie es auf Ihrem lokalen Laufwerk. Die ZIP-Datei enthält zwei Dateien.  
2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und öffnen Sie Cloud Shell. (Die entsprechende Option finden Sie rechts oben auf der Leiste des Portals.)   

    ![FarmBeats-Projekt](./media/get-drone-imagery-from-drone-partner/navigation-bar-1.png)

3. Vergewissern Sie sich, dass die Umgebung auf **PowerShell** festgelegt ist.

    ![FarmBeats-Projekt](./media/get-drone-imagery-from-drone-partner/power-shell-new-1.png)

4. Laden Sie die beiden Dateien, die Sie weiter oben im ersten Schritt heruntergeladen haben, in Ihre Cloud Shell-Instanz hoch.  

    ![FarmBeats-Projekt](./media/get-drone-imagery-from-drone-partner/power-shell-two-1.png)

5. Navigieren Sie zum Uploadverzeichnis der Dateien. (Die Dateien werden standardmäßig im Basisverzeichnis in das nach dem Benutzer benannten Verzeichnis hochgeladen.)  
6. Führen Sie das folgende Skript aus:

    ```azurepowershell-interactive 

    PS> ./generateCredentials.ps1   

    ```

7. Befolgen Sie die Anweisungen auf dem Bildschirm, um die Werte für API-Endpunkt, Mandanten-ID, Client-ID, geheimer Clientschlüssel und EventHub-Verbindungszeichenfolge zu erfassen.

    Nachdem Sie die erforderlichen Anmeldeinformationen in das Drohnensoftwaresystem des Partners eingegeben haben, können Sie alle landwirtschaftlichen Betriebe aus dem FarmBeats-System importieren und die Details des jeweiligen Betriebs für die Flugroutenplanung und die Sammlung von Drohnenbildern verwenden.

    Nachdem die Rohbilder durch die Software des Drohnenanbieters verarbeitet wurden, lädt das Drohnensoftwaresystem die zusammengesetzten Orthofotos sowie andere verarbeitete Bilder in den Datenhub hoch.

## <a name="view-drone-imagery"></a>Anzeigen von Drohnenbildern

Sobald die Daten an den FarmBeats-Datenhub übermittelt wurden, können Sie den Szenenspeicher über die FarmBeats-Datenhub-APIs abfragen.

Alternativ können Sie das neueste Drohnenbild auf der **Detailseite für den landwirtschaftlichen Betrieb** anzeigen. Gehen Sie dazu wie folgt vor:  

1. Wählen Sie den landwirtschaftlichen Betrieb aus, in den Ihre Bilder hochgeladen wurden. Die Detailseite für den **landwirtschaftlichen** Betrieb wird angezeigt.
2. Scrollen Sie nach unten zum neuesten Abschnitt für **Präzisionskarten**.
3. Das Bild sollte im Abschnitt **Drone Imagery** (Drohnenbilder) angezeigt werden.

    ![FarmBeats-Projekt](./media/get-drone-imagery-from-drone-partner/drone-imagery-1.png)

## <a name="download-drone-imagery"></a>Herunterladen von Drohnenbildern

Wenn Sie den Abschnitt „Drone Imagery“ (Drohnenbilder) auswählen, wird ein Popupfenster geöffnet, um ein hochauflösendes Bild der Orthofotodaten der Drohne anzuzeigen.

![FarmBeats-Projekt](./media/get-drone-imagery-from-drone-partner/download-drone-imagery-1.png)

## <a name="view-all-drone-maps"></a>Anzeigen aller Drohnenkarten

Vom Drohnenanbieter hochgeladene Dateien und Bilder werden im Abschnitt „Karten“ angezeigt. Wählen Sie den Abschnitt **Karten** aus, filtern Sie nach **landwirtschaftlichem** Betrieb, und wählen Sie die entsprechenden Dateien aus, um sie anzuzeigen und herunterzuladen:

  ![FarmBeats-Projekt](./media/get-drone-imagery-from-drone-partner/view-drone-maps-1.png)

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Ihre Drohnenbilder unter Verwendung der [APIs](references-for-farmbeats.md#rest-api) des FarmBeats-Datenhubs abrufen.
