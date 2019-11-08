---
title: 'Tutorial: Skalieren einer Anwendung in Azure Spring Cloud | Microsoft-Dokumentation'
description: In diesem Tutorial erfahren Sie, wie Sie über das Azure-Portal eine Anwendung in Azure Spring Cloud skalieren.
ms.service: spring-cloud
ms.topic: tutorial
ms.author: jeconnoc
author: jpconnock
ms.date: 10/06/2019
ms.openlocfilehash: 31b2322bdf7b7c03ae8974d57ee1b44c2f6137b9
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607528"
---
# <a name="tutorial-scale-an-application-in-azure-spring-cloud"></a>Tutorial: Skalieren einer Anwendung in Azure Spring Cloud

In diesem Tutorial wird veranschaulicht, wie Sie mithilfe des Azure Spring Cloud-Dashboards im Azure-Portal eine beliebige Microserviceanwendung skalieren. Skalieren Sie Ihre Anwendung zentral hoch und herunter, indem Sie die Anzahl der virtuellen CPUs (vCPUs) und die Arbeitsspeichermenge ändern. Sie können Ihre Anwendung horizontal herunterskalieren oder erweitern, indem Sie die Anzahl von Anwendungsinstanzen ändern. Anschließend wissen Sie, wie jede Anwendung in Ihrem Dienst schnell manuell angepasst werden kann. Die Skalierung wird in Sekunden wirksam und erfordert weder Codeänderungen noch die erneute Bereitstellung.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:
* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen. 
* Eine bereitgestellte Azure Spring Cloud-Dienstinstanz.  Befolgen Sie unseren [Schnellstart](spring-cloud-quickstart-launch-app-cli.md), um zu beginnen.
* Mindestens eine in dieser Dienstinstanz erstellte Anwendung


## <a name="navigate-to-the-scale-page-in-the-azure-portal"></a>Navigieren zur Seite „Skalieren“ im Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

1. Navigieren Sie zur Seite **Übersicht** von Azure Spring Cloud.

1. Navigieren Sie im Menü auf der linken Seite unter der Überschrift **Einstellungen** zur Registerkarte **Apps**.

1. Wählen Sie die Anwendung aus, die Sie skalieren möchten. In diesem Beispiel wird die Anwendung mit dem Namen „account-service“ skaliert. Dadurch gelangen Sie zur Seite **Übersicht** der Anwendung.

1. Navigieren Sie im Menü auf der linken Seite unter der Überschrift **Einstellungen** zur Registerkarte **Skalieren**. Dort sollte ein Formular mit Zeilen für die einzelnen zuvor erwähnten Skalierungsattribute angezeigt werden.

## <a name="scale-your-application"></a>Skalieren Ihrer Anwendung

Sie können die Skalierungsattribute ändern. Berücksichtigen Sie Folgendes:

* **CPUs**: Pro Anwendungsinstanz sind maximal vier CPUs zulässig. Die Gesamtanzahl von CPUs für eine Anwendung ist der hier festgelegte Wert multipliziert mit der Anzahl von Anwendungsinstanzen.

* **Arbeitsspeicher/GB**: Die pro Anwendungsinstanz zulässige maximale Arbeitsspeichermenge beträgt 8 GB.  Die Gesamtmenge des Arbeitsspeichers für eine Anwendung ist der hier festgelegte Wert multipliziert mit der Anzahl von Anwendungsinstanzen.

* **Instanzenanzahl:** In der Dienstebene „Standard“ ist eine Erweiterung auf bis zu 20 Instanzen möglich. Dieser Wert ändert die Anzahl separat ausgeführter Instanzen der Microserviceanwendung.

Klicken Sie unbedingt auf die Schaltfläche **Speichern**, um Ihre Skalierungseinstellungen zu übernehmen.

Nach wenigen Sekunden werden die von Ihnen vorgenommenen Änderungen auf der Seite **Übersicht** angezeigt. Weitere Details können Sie auf der Registerkarte **Anwendungsinstanzen** einsehen. Für die Skalierung sind weder Codeänderungen noch eine erneute Bereitstellung erforderlich.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie Azure Spring Cloud-Anwendungen manuell skalieren.  Informationen zur Überwachung der Anwendung finden Sie im nächsten Tutorial.

> [!div class="nextstepaction"]
> [Tutorial: Verwenden der verteilten Ablaufverfolgung mit Azure Spring Cloud](spring-cloud-tutorial-distributed-tracing.md)
