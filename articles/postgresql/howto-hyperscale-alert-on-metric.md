---
title: Konfigurieren von Metrikwarnungen für Azure Database for PostgreSQL – Hyperscale (Citus)
description: In diesem Artikel wird beschrieben, wie Sie Metrikwarnungen für Azure Database for PostgreSQL – Hyperscale (Citus) konfigurieren und auf diese zugreifen.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d5f45df16ac294f50e99cc7e05ab6eba43c0ae85
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511263"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-postgresql---hyperscale-citus"></a>Verwenden des Azure-Portals zum Einrichten von Warnungen zu Metriken für Azure Database for PostgreSQL – Hyperscale (Citus)

In diesem Artikel erfahren Sie, wie Sie mit dem Azure-Portal Azure Database for PostgreSQL-Warnungen einrichten können. Sie können auf der Grundlage von Überwachungsmetriken für Ihre Azure-Services eine Warnung empfangen.

Wir richten eine Warnung ein, die ausgelöst wird, wenn der Wert für eine bestimmte Metrik einen Schwellenwert überschreitet. Die Warnung wird ausgelöst, wenn die Bedingung erstmals erfüllt ist. Anschließend wird sie weiterhin ausgelöst.

Sie können konfigurieren, dass bei einer Warnung die folgenden Aktionen ausgeführt werden, wenn sie ausgelöst wird:
* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren.
* Senden von E-Mails an weitere von Ihnen angegebene Adressen
* Aufrufen eines Webhooks

Sie haben folgende Möglichkeiten zum Konfigurieren von Warnungsregeln und Abrufen zugehöriger Informationen:
* [Azure-Portal](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure-Befehlszeilenschnittstelle](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure Monitor-REST-API](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Erstellen einer Warnungsregel anhand einer Metrik aus dem Azure-Portal
1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) den zu überwachenden Azure Database for PostgreSQL-Server aus.

2. Wählen Sie im Abschnitt **Überwachung** in der Randleiste die Option **Warnungen** aus, wie unten gezeigt:

   ![„Warnungsregeln“ auswählen](./media/howto-hyperscale-alert-on-metric/2-alert-rules.png)

3. Wählen Sie **Neue Warnungsregel** (Symbol „+“) aus.

4. Die Seite **Regel erstellen** wird geöffnet, wie unten gezeigt. Füllen Sie die erforderlichen Informationen aus:

   ![Formular „Metrikwarnung hinzufügen“](./media/howto-hyperscale-alert-on-metric/4-add-rule-form.png)

5. Wählen Sie im Abschnitt **Bedingung** die Option **Hinzufügen** aus.

6. Wählen Sie eine Metrik aus der Liste der Signale aus, bei denen eine Warnung erfolgen soll. Wählen Sie in diesem Beispiel „Speicher in Prozent“ aus.
   
   ![Metrik auswählen](./media/howto-hyperscale-alert-on-metric/6-configure-signal-logic.png)

7. Konfigurieren Sie die Warnungslogik:

    * **Operator** (Beispiel: „Größer als“)
    * **Schwellenwert** (Beispiel: 85 Prozent)
    * **Aggregationsgranularität**: Der Zeitraum, in dem die Metrikregel erfüllen werden muss, bevor die Warnung ausgelöst wird (Beispiel: „Innerhalb der letzten 30 Minuten“)
    * und **Häufigkeit der Auswertung** (Beispiel: „1 Minute“)
   
   Wählen Sie anschließend **Fertig** aus.

   ![Metrik auswählen](./media/howto-hyperscale-alert-on-metric/7-set-threshold-time.png)

8. Wählen Sie im Abschnitt **Aktionsgruppen** die Option **Neu erstellen** aus, um eine neue Gruppe zum Empfangen von Benachrichtigungen zu Warnungen zu erhalten.

9. Tragen Sie in das Formular „Aktionsgruppe hinzufügen“ einen Namen, Kurznamen, ein Abonnement und eine Ressourcengruppe ein.

    ![Aktionsgruppe](./media/howto-hyperscale-alert-on-metric/9-add-action-group.png)

10. Konfigurieren Sie den Aktionstyp **E-Mail/SMS/Push/Sprachanruf**.
    
    Wählen Sie „E-Mail an Azure Resource Manager-Rolle“ aus, um Benachrichtigungen an Besitzer, Mitwirkende und Leser des Abonnements zu senden.
   
    Wählen Sie **OK** aus, wenn Sie fertig sind.

    ![Aktionsgruppe](./media/howto-hyperscale-alert-on-metric/10-action-group-type.png)

11. Geben Sie einen Namen, einen Beschreibung und den Schweregrad für die Warnungsregel an.

    ![Aktionsgruppe](./media/howto-hyperscale-alert-on-metric/11-name-description-severity.png) 

12. Wählen Sie **Benachrichtigungsregel erstellen** aus, um die Benachrichtigung zu erstellen.

    Innerhalb weniger Minuten wird die Warnung aktiv und wie oben beschrieben ausgelöst.

## <a name="manage-your-alerts"></a>Verwalten Ihrer Warnungen

Nachdem Sie eine Warnung erstellt haben, können Sie sie auswählen und folgende Aktionen ausführen:

* Ein Diagramm anzeigen, das den Schwellenwert der Metrik und die tatsächlichen Werte vom Vortag zeigt, die für diese Warnung relevant sind.
* Die Warnungsregel **bearbeiten** oder **löschen**.
* Die Warnung **deaktivieren** oder **aktivieren**, wenn Sie den Empfang von Benachrichtigungen vorübergehend beenden oder fortsetzen möchten.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über das [Konfigurieren von Webhooks in Warnungen](../azure-monitor/platform/alerts-webhooks.md).
* Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.
