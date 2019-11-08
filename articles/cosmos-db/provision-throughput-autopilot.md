---
title: Erstellen Sie Azure Cosmos-Container und -Datenbanken mit bereitgestelltem Durchsatz im Autopilot-Modus.
description: Erfahren Sie mehr über die Vorteile, die Anwendungsfälle und die Bereitstellung von Azure Cosmos-Datenbanken und -Containern im Autopilot-Modus.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 598dc6394e8be8b3372f4ed61a522454830a22d6
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510111"
---
# <a name="create-azure-cosmos-containers-and-databases-with-provisioned-throughput-in-autopilot-mode-preview"></a>Erstellen von Azure Cosmos-Containern und -Datenbanken mit bereitgestelltem Durchsatz im Autopilot-Modus (Vorschau)

Mit Azure Cosmos DB können Sie Durchsatz für Ihre Container im manuellen oder Autopilot-Modus bereitstellen. In diesem Artikel werden die Vorteile des Autopilot-Modus und Anwendungsfälle beschrieben.

> [!NOTE]
> Der Autopilot-Modus ist derzeit in der öffentlichen Vorschau verfügbar.

Zusätzlich zur manuellen Bereitstellung von Durchsatz können Sie jetzt Azure Cosmos-Container im Autopilot-Modus konfigurieren. Bei Azure Cosmos-Containern und -Datenbanken, die im Autopilot-Modus konfiguriert wurden, wird **der bereitgestellte Durchsatz automatisch und sofort basierend auf Ihren Anwendungsanforderungen skaliert, ohne die SLAs zu beeinträchtigen**.

Es ist nicht länger erforderlich, den bereitgestellten Durchsatz manuell zu verwalten oder Probleme aufgrund von Ratenbegrenzungen zu behandeln. Im Autopilot-Modus konfigurierte Azure Cosmos-Container können sofort in Reaktion auf die Workload skaliert werden, ohne dass die Verfügbarkeit, die Wartezeit, der Durchsatz oder die Leistung der Workload insgesamt beeinträchtigt werden. Unter hoher Auslastung können im Autopilot-Modus konfigurierte Azure Cosmos-Container zentral hoch- oder herunterskaliert werden, ohne den laufenden Betrieb zu beeinflussen.

Beim Konfigurieren von Containern und Datenbanken im Autopilot-Modus müssen Sie den maximalen Durchsatz `Tmax` angeben, der nicht überschritten werden darf. Container können dann basierend auf den Anforderungen der Workload innerhalb des Bereichs `0.1*Tmax < T < Tmax` sofort skaliert werden. Mit anderen Worten: Container und Datenbanken werden basierend auf den Anforderungen der Workload unverzüglich skaliert, und zwar von einem geringen Wert wie 10 % des konfigurierten Durchsatzwerts bis zum angegebenen konfigurierten Maximalwert. Sie können die Einstellung für den maximalen Durchsatz (Tmax) für eine Autopilot-Datenbank oder einen Autopilot-Container jederzeit ändern.

## <a name="benefits-of-autopilot-mode"></a>Vorteile des Autopilot-Modus

Azure Cosmos-Container, die im Autopilot-Modus konfiguriert sind, haben die folgenden Vorteile:

* **Einfach:** Container im Autopilot-Modus machen komplexe Abläufe wie die manuelle Verwaltung des bereitgestellten Durchsatzes (RUs) und der Kapazität für verschiedene Container überflüssig.

* **Skalierbar:** Bei Containern im Autopilot-Modus lässt sich die Kapazität des bereitgestellten Durchsatzes nach Bedarf nahtlos skalieren. Dabei werden weder Clientverbindungen oder Anwendungen unterbrochen, noch vorhandene SLAs beeinträchtigt.

* **Kosteneffizient:** Wenn Sie im Autopilot-Modus konfigurierte Azure Cosmos-Container verwenden, zahlen Sie nur für die Ressourcen, die ihre Workloads benötigen, auf Stundenbasis.

* **Hochverfügbarkeit:** Azure Cosmos-Container im Autopilot-Modus verwenden dasselbe global verteilte, fehlertolerante, hochverfügbare Back-End, um die Dauerhaftigkeit und ständige Hochverfügbarkeit von Daten sicherzustellen.

## <a name="use-cases-of-autopilot-mode"></a>Anwendungsfälle für den Autopilot-Modus

Die Anwendungsfälle für die im Autopilot-Modus konfigurierten Azure Cosmos-Container umfassen:

* **Variable Workloads:** Sie führen eine gering ausgelastete Anwendung mit einer Spitzenauslastung von einer bis zu mehreren Stunden einige Male täglich oder mehrmals pro Jahr aus. Beispiele hierfür sind Anwendungen für Personalwesen, Budgetierung und Betriebsberichterstattung. Für solche Szenarien können Container verwendet werden, die im Autopilot-Modus konfiguriert sind. Es ist nicht mehr erforderlich, eine manuelle Bereitstellung für maximale oder durchschnittliche Kapazitäten vorzunehmen.

* **Unvorhersehbare Workloads:** Sie führen Workloads aus, bei denen die Datenbanknutzung über den ganzen Tag erfolgt, allerdings auch Aktivitätsspitzen auftreten, die nur schwer vorhersehbar sind. Ein Beispiel hierfür ist eine Verkehrsinfo-Website, bei der die Aktivitäten ansteigen, sobald sich der Wetterbericht ändert. Bei Containern, die im Autopilot-Modus konfiguriert sind, wird die Kapazität angepasst, wenn die Anwendung eine Spitzenlast verzeichnet, und wieder horizontal herunterskaliert, wenn die Aktivitäten wieder auf das normale Maß zurückgehen.

* **Neue Anwendungen:** Sie stellen eine neue Anwendung bereit und sind sich nicht sicher, wie viel bereitgestellten Durchsatz (d. h. wie viele RUs) Sie benötigen. Bei Containern, die im Autopilot-Modus konfiguriert sind, können Sie die Skalierung automatisch an den Kapazitätsbedarf und die Anforderungen Ihrer Anwendung anpassen.

* **Selten genutzte Anwendungen:** Sie verfügen über eine Anwendung, die mehrmals pro Tag, Woche oder Monat für wenige Stunden genutzt wird, beispielsweise eine Anwendung/Website/Blogwebsite mit geringem Volumen.

* **Entwicklungs-und Testdatenbanken:** Entwickler verwenden die Azure Cosmos-Konten während der Arbeitszeit, benötigen Sie nachts oder an Wochenenden jedoch nicht. Bei Containern, die im Autopilot-Modus konfiguriert sind, wird die Skalierung auf ein Minimum beschränkt, wenn sie nicht verwendet werden.

* **Geplante Produktionsworkloads/-abfragen:** Wenn Sie über eine Reihe geplanter Anforderungen/Vorgänge/Abfragen für einen einzelnen Container verfügen und es Leerlaufphasen gibt, in denen Sie den Durchsatz auf ein absolutes Minimum beschränken möchten, können Sie dies jetzt problemlos tun. Wenn eine geplante Abfrage/Anforderung an einen Container übermittelt wird, der im Autopilot-Modus konfiguriert ist, wird der Durchsatz soweit wie nötig zentral hochskaliert, und der Vorgang wird ausgeführt.

Lösungen für die vorherigen Probleme erfordern nicht nur eine sehr lange Implementierungszeit, sondern erhöhen auch die Komplexität der Konfiguration oder Ihres Codes. Außerdem ist häufig ein manuelles Eingreifen erforderlich, um die Probleme zu beheben. Im Autopilot-Modus werden die oben aufgeführten Szenarien standardmäßig unterstützt, sodass Sie sich nicht mehr um diese Probleme kümmern müssen.

## <a name="comparison--containers-configured-in-manual-mode-vs-autopilot-mode"></a>Vergleich – im manuellen Modus und im Autopilot-Modus konfigurierte Container

|  | Im manuellen Modus konfigurierte Container  | Im Autopilot-Modus konfigurierte Container |
|---------|---------|---------|
| **Bereitgestellter Durchsatz** | Manuell bereitgestellt | Wird je nach Workload-Verwendungsmustern proaktiv und reaktiv skaliert. |
| **Ratenbegrenzung von Anforderungen/Vorgängen (429)**  | Kann passieren, wenn die Nutzung die bereitgestellte Kapazität überschreitet. | Passiert nicht.  |
| **Kapazitätsplanung** |  Sie müssen eine anfängliche Kapazitätsplanung durchführen und den benötigten Durchsatz bereitstellen. |    Sie müssen sich nicht um die Kapazitätsplanung kümmern. Das System übernimmt automatisch die Kapazitätsplanung und -verwaltung. |
| **Preise** | Manuell bereitgestellte RU/s pro Stunde | Bei Konten mit einer Schreibregion zahlen Sie für den genutzten Durchsatz auf Stundenbasis nach dem Tarif für Autopilot-RU/s pro Stunde. <br/><br/>Bei Konten mit mehreren Schreibregionen fallen keine zusätzlichen Kosten für Autopilot an. Sie zahlen für den genutzten Durchsatz auf Stundenbasis nach demselben Tarif für Multimaster-RU/s pro Stunde. |
| **Für folgende Workloadtypen am besten geeignet** |  Vorhersehbare und stabile Workloads|   Nicht vorhersehbare und variable Workloads  |

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Erstellen einer Datenbank oder eines Containers mit dem Autopilot-Modus

Sie können Autopilot für Datenbanken oder Container konfigurieren, während Sie diese erstellen. Führen Sie die folgenden Schritte für eine neue Datenbank oder einen neuen Container aus, aktivieren Sie Autopilot, und geben Sie den maximalen Durchsatz an.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) oder beim [Azure Cosmos-Explorer](https://cosmos.azure.com/) an.

1. Navigieren Sie zu Ihrem Azure Cosmos-Konto, und öffnen Sie die Registerkarte **Daten-Explorer**.

1. Wählen Sie **Neue Datenbank** aus, und geben Sie einen Namen für die Datenbank ein. Wählen Sie für die **Autopilot**-Option **Aktiviert** aus, und geben Sie den maximalen Durchsatz an, den die Datenbank bei Verwendung der Autopilot-Option nicht überschreiten darf.

   ![Erstellen einer Datenbank im Autopilot-Modus](./media/provision-throughput-autopilot/create-database-autopilot-mode.png)

1. Klicken Sie auf **OK**.

Auf ähnliche Weise können Sie auch einen Container mit bereitgestelltem Durchsatz im Autopilot-Modus erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).