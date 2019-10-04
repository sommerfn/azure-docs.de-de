---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/04/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 364da4bb5f3f76f0a8a189db5aa59cca305470ea
ms.sourcegitcommit: 7a6d8e841a12052f1ddfe483d1c9b313f21ae9e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2019
ms.locfileid: "70197137"
---
## <a name="copy-your-credentials-from-the-azure-portal"></a>Kopieren Ihrer Anmeldeinformationen aus dem Azure-Portal

Die Beispielanwendung muss den Zugriff auf Ihr Speicherkonto authentifizieren. Fügen Sie zum Authentifizieren der Anwendung die Anmeldeinformationen für Ihr Speicherkonto in Form einer Verbindungszeichenfolge hinzu. Führen Sie zum Anzeigen der Anmeldeinformationen Ihres Speicherkontos die folgenden Schritte aus:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Suchen Sie nach Ihrem Speicherkonto.
3. Wählen Sie im Abschnitt **Einstellungen** der Speicherkontoübersicht die Option **Zugriffsschlüssel**. Hier können Sie Ihre Kontozugriffsschlüssel und die vollständige Verbindungszeichenfolge für jeden Schlüssel anzeigen.   
4. Suchen Sie unter **key1** nach dem Wert für die **Verbindungszeichenfolge**, und wählen Sie dann die Schaltfläche **Kopieren**, um die Verbindungszeichenfolge zu kopieren. Der Wert der Verbindungszeichenfolge wird in einem späteren Schritt einer Umgebungsvariablen hinzugefügt.

    ![Screenshot: Kopieren einer Verbindungszeichenfolge aus dem Azure-Portal](media/storage-copy-connection-string-portal/portal-connection-string.png)
