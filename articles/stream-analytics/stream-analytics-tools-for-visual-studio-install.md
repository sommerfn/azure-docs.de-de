---
title: Einrichten der Azure Stream Analytics-Tools für Visual Studio
description: In diesem Artikel werden die Installationsanforderungen und die Einrichtung der Azure Stream Analytics-Tools für Visual Studio beschrieben.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/22/2018
ms.openlocfilehash: 447fa07953c15fe67b8a2e313fe9534164f47bbd
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71130507"
---
# <a name="install-azure-stream-analytics-tools-for-visual-studio"></a>Installieren der Azure Stream Analytics-Tools für Visual Studio

Visual Studio 2019 und Visual Studio 2017 unterstützen Azure Data Lake und Stream Analytics-Tools. In diesem Artikel wird beschrieben, wie Sie die Tools installieren und deinstallieren.

Weitere Informationen zur Verwendung der Tools finden Sie unter [Schnellstart: Erstellen eines Azure Stream Analytics-Auftrags mithilfe von Visual Studio](stream-analytics-quick-create-vs.md).

## <a name="install"></a>Installieren

Die Editionen Visual Studio Enterprise (Ultimate/Premium), Professional und Community unterstützen die Tools. Die Express-Edition und Visual Studio für Mac unterstützen die Tools nicht.

Wir empfehlen Visual Studio 2019.

### Installieren für Visual Studio 2019 und 2017<a name="recommended-visual-studio-2019-and-2017"></a>

Die Tools Azure Data Lake und Stream Analytics sind Teil der Workloads **Azure-Entwicklung** und **Datenspeicherung und -verarbeitung**. Aktivieren Sie eine der beiden Workloads während der Installation. Wenn Visual Studio bereits installiert ist, wählen Sie **Tools** > **Tools und Features abrufen** aus, um Workloads hinzuzufügen.

Laden Sie [Visual Studio 2019 (Vorschauversion 2 oder höher) und Visual Studio 2017 (15.3 oder höher)](https://www.visualstudio.com/) herunter, und folgen Sie den Installationsanweisungen.

Wählen Sie die Workload **Datenspeicherung und -verarbeitung** wie gezeigt aus:

![Ausgewählte Workload „Datenspeicherung und -verarbeitung“](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2019-install-01.png)

Wählen Sie die Workload **Azure-Entwicklung** wie gezeigt aus:

![Ausgewählte Workload „Azure-Entwicklung“](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-for-vs-2019-install-02.png)

Nachdem Sie die Workload hinzugefügt haben, aktualisieren Sie die Tools. Dieses Verfahren gilt für Visual Studio 2019:

1. Wählen Sie **Erweiterungen** > **Erweiterungen verwalten** aus.

1. Wählen Sie **Erweiterungen verwalten** die Option **Updates** und dann **Azure Data Lake- und Stream Analytics-Tools** aus.

1. Wählen Sie **Update** aus, um die aktuelle Erweiterung zu installieren.

![Visual Studio-Erweiterungen und -Updates](./media/stream-analytics-tools-for-visual-studio-install/stream-analytics-tools-vs2019-extensions-updates.png)

### Installieren für Visual Studio 2015 und 2013<a name="visual-studio-2015-2013"></a>

Die Editionen Visual Studio Enterprise (Ultimate/Premium), Professional und Community unterstützen die Tools. Die Express-Edition unterstützt die Tools nicht.

* Installieren Sie Visual Studio 2015 oder Visual Studio 2013 Update 4.
* Installieren Sie das Microsoft Azure SDK für .NET Version 2.7.1 oder höher mit dem [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx).
* Installieren Sie [Microsoft Azure Data Lake- und Stream Analytics-Tools für Visual Studio](https://www.microsoft.com/en-us/download/details.aspx?id=49504).

## Update<a name="visual-studio-2019-and-2017"></a><a name="visual-studio-2015-and-2013"></a>

Für Visual Studio 2019 und Visual Studio 2017 wird eine Erinnerung für die neue Version als Visual Studio-Benachrichtigung angezeigt.

Für Visual Studio 2015 und Visual Studio 2013 überprüfen die Tools automatisch auf neue Versionen. Befolgen Sie die Anweisungen, um die neueste Version zu installieren.

## <a name="uninstall"></a>Deinstallieren

Sie können die Azure Data Lake- und Stream Analytics-Tools deinstallieren. Wählen Sie für Visual Studio 2019 oder Visual Studio 2017 **Tools** > **Tools und Features abrufen** aus. Deaktivieren Sie unter **Änderung** die Option **Azure Data Lake- und Stream Analytics-Tools**. Sie wird entweder unter der Workload **Datenspeicherung und -verarbeitung** oder der Workload **Azure-Entwicklung** angezeigt.

Wechseln Sie zum Deinstallieren von Visual Studio 2015 oder Visual Studio 2013 zu **Systemsteuerung** > **Programme und Features**. Deinstallieren Sie **Microsoft Azure Data Lake- und Stream Analytics-Tools für Visual Studio**.
