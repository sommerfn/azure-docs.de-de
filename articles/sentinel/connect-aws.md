---
title: Verknüpfen von Symantec AWS-Daten mit der Vorschauversion von Azure Sentinel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Symantec AWS-Daten mit Azure Sentinel verknüpfen.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2019
ms.author: rkarlin
ms.openlocfilehash: 214269bc5c854aa4d3bfd508b0adb5a53ec096df
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674113"
---
# <a name="connect-azure-sentinel-to-aws"></a>Herstellen einer Verbindung von Azure Sentinel mit AWS

Verwenden Sie den AWS-Connector, um alle Ihre AWS CloudTrail-Ereignisse in Azure Sentinel zu streamen. Dieser Verbindungsprozess delegiert den Zugriff für Azure Sentinel auf Ihre AWS-Ressourcenprotokolle und schafft so eine Vertrauensbeziehung zwischen AWS CloudTrail und Azure Sentinel. Dies erfolgt in AWS durch Erstellen einer Rolle, die Azure Sentinel die Berechtigung erteilt, auf Ihre AWS-Protokolle zuzugreifen.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Schreibzugriff auf den Azure Sentinel-Arbeitsbereich.

## <a name="connect-aws"></a>Herstellen einer Verbindung mit AWS 
 
1.  Klicken Sie in Ihrer Amazon Web Services-Konsole unter **Sicherheit, Identität und Compliance** auf **IAM**.

    ![AWS1](./media/connect-aws/aws-1.png)

2.  Wählen Sie **Rollen** aus, und klicken Sie dann auf **Rolle erstellen**.

    ![AWS2](./media/connect-aws/aws-2.png)

3.  Wählen Sie **Anderes AWS-Konto** aus. Geben Sie im Feld **Konto-ID** die **Microsoft-Konto-ID** (**123412341234**) ein, die Sie auf der AWS Connector-Seite im Azure Sentinel-Portal finden.

    ![AWS3](./media/connect-aws/aws-3.png)

4.  Vergewissern Sie sich, dass **Externe ID erforderlich** ausgewählt ist, und geben Sie dann die externe ID (Arbeitsbereich-ID) ein, die Sie auf der AWS Connector-Seite im Azure Sentinel-Portal finden.

    ![AWS4](./media/connect-aws/aws-4.png)

5.  Wählen Sie unter **Berechtigungsrichtlinie anfügen** **AWSCloudTrailReadOnlyAccess** aus.

    ![AWS5](./media/connect-aws/aws-5.png)

6.  Geben Sie ein Tag (optional) ein.

    ![AWS6](./media/connect-aws/aws-6.png)

7.  Geben Sie dann einen **Rollenname** ein, und klicken Sie auf die Schaltfläche **Rolle erstellen**.

    ![AWS7](./media/connect-aws/aws-7.png)

8.  Wählen Sie in der Liste der Rollen die Rolle aus, die Sie erstellt haben.

    ![AWS8](./media/connect-aws/aws-8.png)

9.  Kopieren Sie die **Rollen-ARN**, und fügen Sie sie in das Feld **Hinzuzufügende Rolle** im Azure Sentinel-Portal ein.

    ![AWS9](./media/connect-aws/aws-9.png)

10. Um das relevante Schema in Log Analytics für die AWS-Ereignisse zu verwenden, suchen Sie nach **AWSCloudTrail**.



## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde beschrieben, wie Sie AWS CloudTrail mit Azure Sentinel verbinden. Weitere Informationen zu Azure Sentinel finden Sie in den folgenden Artikeln:
- Erfahren Sie, wie Sie [Einblick in Ihre Daten und potenzielle Bedrohungen erhalten](quickstart-get-visibility.md).
- Beginnen Sie mit der [Erkennung von Bedrohungen mithilfe von Azure Sentinel](tutorial-detect-threats.md).

