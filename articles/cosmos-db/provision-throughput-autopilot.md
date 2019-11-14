---
title: Erstellen von Azure Cosmos-Containern und -Datenbanken im Autopilot-Modus
description: Erfahren Sie mehr über die Vorteile, die Anwendungsfälle und die Bereitstellung von Azure Cosmos-Datenbanken und -Containern im Autopilot-Modus.
author: kirillg
ms.author: kirillg
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: e0c3c88119c3d064326442881854920b411f5ed4
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73748375"
---
# <a name="create-azure-cosmos-containers-and-databases-in-autopilot-mode-preview"></a>Erstellen von Azure Cosmos-Containern und -Datenbanken im Autopilot-Modus (Vorschauversion)

Mit Azure Cosmos DB können Sie Durchsatz für Ihre Container im manuellen oder Autopilot-Modus bereitstellen. In diesem Artikel werden die Vorteile des Autopilot-Modus und Anwendungsfälle beschrieben.

> [!NOTE]
> Der Autopilot-Modus ist derzeit in der öffentlichen Vorschau verfügbar. Informationen zur Aktivierung der Autopilot-Funktion für Ihr Azure Cosmos-Konto finden Sie im Abschnitt [Aktivieren des Autopilot-Modus](#enable-autopilot) dieses Artikels. Sie können den Autopilot-Modus nur für neue Datenbanken und Container aktivieren. Er ist für vorhandene Container und Datenbanken nicht verfügbar.

Zusätzlich zur manuellen Bereitstellung von Durchsatz können Sie jetzt Azure Cosmos-Container im Autopilot-Modus konfigurieren. Bei Azure Cosmos-Containern und -Datenbanken, die im Autopilot-Modus konfiguriert wurden, wird **der bereitgestellte Durchsatz automatisch und sofort basierend auf Ihren Anwendungsanforderungen skaliert, ohne die SLAs zu beeinträchtigen**.

Es ist nicht länger erforderlich, den bereitgestellten Durchsatz manuell zu verwalten oder Probleme aufgrund von Ratenbegrenzungen zu behandeln. Im Autopilot-Modus konfigurierte Azure Cosmos-Container können sofort in Reaktion auf die Workload skaliert werden, ohne dass die Verfügbarkeit, die Wartezeit, der Durchsatz oder die Leistung der Workload insgesamt beeinträchtigt werden. Unter hoher Auslastung können im Autopilot-Modus konfigurierte Azure Cosmos-Container zentral hoch- oder herunterskaliert werden, ohne den laufenden Betrieb zu beeinflussen.

Beim Konfigurieren von Containern und Datenbanken im Autopilot-Modus müssen Sie den maximalen Durchsatz `Tmax` angeben, der nicht überschritten werden darf. Container können dann basierend auf den Anforderungen der Workload innerhalb des Bereichs `0.1*Tmax < T < Tmax` sofort skaliert werden. Mit anderen Worten: Container und Datenbanken werden basierend auf den Anforderungen der Workload unverzüglich skaliert – von einem von Ihnen konfigurierten geringen Wert (z. B. 10 % des maximalen Durchsatzwerts) bis zum konfigurierten Maximalwert. Sie können die Einstellung für den maximalen Durchsatz (Tmax) für eine Autopilot-Datenbank oder einen Autopilot-Container jederzeit ändern.

Während der Vorschauphase von Autopilot ist für den angegebenen maximalen Durchsatz im Container oder in der Datenbank der Betrieb innerhalb der berechneten Speicherbegrenzung zulässig. Wenn die Speicherbegrenzung überschritten wird, wird der maximale Durchsatz automatisch auf einen höheren Wert festgelegt. Bei Verwendung des Durchsatzes auf Datenbankebene im Autopilot-Modus wird die Anzahl von Containern, die in einer Datenbank zulässig ist, wie folgt berechnet: (0,001 · max. Durchsatz). Wenn Sie beispielsweise 20.000 RU/s im Autopilot-Modus bereitstellen, kann die Datenbank über 20 Container verfügen.

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
| **Bereitgestellter Durchsatz** | Manuell bereitgestellt | Automatische und sofortige Skalierung basierend auf den Workload-Verwendungsmustern. |
| **Ratenbegrenzung von Anforderungen/Vorgängen (429)**  | Kann passieren, wenn die Nutzung die bereitgestellte Kapazität überschreitet. | Wird nicht durchgeführt, wenn der verbrauchte Durchsatz unter dem maximalen Durchsatz liegt, den Sie für den Autopilot-Modus ausgewählt haben.   |
| **Kapazitätsplanung** |  Sie müssen eine anfängliche Kapazitätsplanung durchführen und den benötigten Durchsatz bereitstellen. |    Sie müssen sich nicht um die Kapazitätsplanung kümmern. Das System übernimmt automatisch die Kapazitätsplanung und -verwaltung. |
| **Preise** | Manuell bereitgestellte RU/s pro Stunde | Bei Konten mit einer Schreibregion zahlen Sie für den genutzten Durchsatz auf Stundenbasis nach dem Tarif für Autopilot-RU/s pro Stunde. <br/><br/>Bei Konten mit mehreren Schreibregionen fallen keine zusätzlichen Kosten für Autopilot an. Sie zahlen für den genutzten Durchsatz auf Stundenbasis nach demselben Tarif für Multimaster-RU/s pro Stunde. |
| **Für folgende Workloadtypen am besten geeignet** |  Vorhersehbare und stabile Workloads|   Nicht vorhersehbare und variable Workloads  |

## <a name="a-idenable-autopilot-enable-autopilot-from-azure-portal"></a><a id="enable-autopilot"> Aktivieren des Autopilot-Modus über das Azure-Portal

Sie können den Autopilot-Modus unter Ihren Azure Cosmos-Konten ausprobieren, indem Sie ihn über das Azure-Portal aktivieren. Führen Sie die folgenden Schritte aus, um die Autopilot-Option zu aktivieren:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an.

2. Navigieren Sie zu Ihrem Azure Cosmos-Konto, und öffnen Sie die Registerkarte **Neue Features**. Wählen Sie **Autopilot** und **Registrieren** aus. Dies ist im folgenden Screenshot dargestellt:

![Erstellen eines Containers im Autopilot-Modus](./media/provision-throughput-autopilot/enable-autopilot-azure-portal.png)

## <a name="create-a-database-or-a-container-with-autopilot-mode"></a>Erstellen einer Datenbank oder eines Containers mit dem Autopilot-Modus

Sie können Autopilot für Datenbanken oder Container konfigurieren, während Sie diese erstellen. Führen Sie die folgenden Schritte für eine neue Datenbank oder einen neuen Container aus, aktivieren Sie Autopilot, und geben Sie den maximalen Durchsatz an.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) oder beim [Azure Cosmos-Explorer](https://cosmos.azure.com/) an.

1. Navigieren Sie zu Ihrem Azure Cosmos-Konto, und öffnen Sie die Registerkarte **Daten-Explorer**.

1. Wählen Sie die Option **Neuer Container** aus, und geben Sie einen Namen und einen Partitionsschlüssel ein. Wählen Sie die Option **Autopilot** und dann den maximalen Durchsatz aus, der vom Container bei der Verwendung der Autopilot-Option nicht überschritten werden darf.

   ![Erstellen eines Containers im Autopilot-Modus](./media/provision-throughput-autopilot/create-container-autopilot-mode.png)

1. Klicken Sie auf **OK**.

Auf ähnliche Weise können Sie auch eine Datenbank mit bereitgestelltem Durchsatz im Autopilot-Modus erstellen.

## <a name="next-steps"></a>Nächste Schritte

* Erfahren Sie mehr über [logische Partitionen](partition-data.md).
* Erfahren Sie mehr über das [Bereitstellen von Durchsatz für einen Azure Cosmos-Container](how-to-provision-container-throughput.md).
* Erfahren Sie mehr über das [Bereitstellen des Durchsatzes für eine Datenbank in Azure Cosmos](how-to-provision-database-throughput.md).
