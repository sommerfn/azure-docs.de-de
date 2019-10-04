---
title: Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel
description: Verwenden Sie die Fusion-Technologie in Azure Sentinel, um die „Alarmmüdigkeit“ zu reduzieren und verwertbare Incidents zu erstellen, die auf der erweiterten Erkennung von mehrstufigen Angriffen basieren.
services: sentinel
documentationcenter: na
author: cabailey
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/24/2019
ms.author: cabailey
ms.openlocfilehash: e6ddb1b01b705d2a7857682bd84e9482e064a8db
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71241206"
---
# <a name="advanced-multistage-attack-detection-in-azure-sentinel"></a>Erweiterte Erkennung von mehrstufigen Angriffen in Azure Sentinel

Durch die Verwendung der auf maschinellem Lernen basierenden Fusion-Technologie kann Azure Sentinel mehrstufige Angriffe automatisch erkennen. Hierzu werden anomale Verhaltensweisen und verdächtige Aktivitäten kombiniert, die an verschiedenen Stellen der Kill Chain beobachtet werden. Azure Sentinel generiert dann Incidents, die auf andere Weise nur sehr schwer abgefangen werden können. Diese Incidents umfassen mindestens zwei Warnungen oder Aktivitäten. Standardmäßig weisen diese Incidents ein geringes Volumen, eine hohe Qualität und einen hohen Schweregrad auf.

Diese Erkennung ist für Ihre Umgebung angepasst und bewirkt nicht nur eine Reduzierung der False Positive-Rate, sondern kann auch Angriffe mit eingeschränkten oder fehlenden Informationen erkennen.

Ausführliche Informationen zu den Warnungen, die für die einzelnen Szenarien unterstützt werden, finden Sie im Abschnitt [Für die erweiterte Erkennung von mehrstufigen Angriffen unterstützte Szenarien](#scenarios-supported-for-advanced-multistage-attack-detection) auf dieser Seite.

## <a name="configuration-for-advanced-multistage-attack-detection"></a>Konfiguration für die erweiterte Erkennung von mehrstufigen Angriffen

Diese Erkennung ist in Azure Sentinel standardmäßig aktiviert. Befolgen Sie diese Anleitung, um den Status zu überprüfen oder zu deaktivieren, weil Sie beispielsweise eine andere Lösung nutzen, um Incidents basierend auf mehreren Warnungen zu erstellen:

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com)an, falls Sie dies noch nicht getan haben.

2. Navigieren Sie zu **Azure Sentinel** > **Konfiguration** > **Analyse**.

3. Wählen Sie in der Spalte **NAME** die Option **Active rules** (Aktive Regeln) und dann **Advanced Multistage Attack Detection** (Erweiterte Erkennung von mehrstufigen Angriffen) aus. Überprüfen Sie in der Spalte **STATUS**, ob diese Erkennung aktiviert oder deaktiviert ist.

4. Wählen Sie zum Ändern des Status diesen Eintrag und auf dem Blatt **Advanced Multistage Attack Detection** (Erweiterte Erkennung von mehrstufigen Angriffen) die Option **Bearbeiten** aus.

5. Auf dem Blatt **Assistent zum Erstellen von Regeln** ist die Statusänderung automatisch für Sie ausgewählt. Wählen Sie **Weiter: Überprüfen** und dann **Speichern** aus. 

Regelvorlagen können für die erweiterte Erkennung von mehrstufigen Angriffen nicht genutzt werden.

## <a name="scenarios-supported-for-advanced-multistage-attack-detection"></a>Für die erweiterte Erkennung von mehrstufigen Angriffen unterstützte Szenarien

Per erweiterter Erkennung von mehrstufigen Angriffen unterstützt Azure Sentinel die folgenden Szenarien, in denen Anomalieereignisse aus Azure Active Directory Identity Protection und Microsoft Cloud App Security kombiniert werden:

- [Unmöglicher Ortswechsel zu atypischen Orten, gefolgt von anomaler Office 365-Aktivität](#impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity)
- [Anmeldeaktivität für unbekannten Standort, gefolgt von anomaler Office 365-Aktivität](#sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity)
- [Anmeldeaktivität mit infiziertem Gerät, gefolgt von anomaler Office 365-Aktivität](#sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity)
- [Anmeldeaktivität von anonymer IP-Adresse, gefolgt von anomaler Office 365-Aktivität](#sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity)
- [Anmeldeaktivität eines Benutzers mit kompromittierten Anmeldeinformationen, gefolgt von anomaler Office 365-Aktivität](#sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity)

Sie müssen den [Azure AD Identity Protection-Datenconnector](connect-azure-ad-identity-protection.md) und die [Cloud App Security](connect-cloud-app-security.md)-Connectors konfiguriert haben.

In den folgenden Beschreibungen wird mit Azure Sentinel der tatsächliche Wert aus Ihren Daten angezeigt, der auf dieser Seite in Form von Variablen in Klammern dargestellt ist. Dies ist beispielsweise der tatsächliche Anzeigename anstelle von \<*account name*> und die tatsächliche Zahl anstelle von \<*number*>.

### <a name="impossible-travel-to-atypical-location-followed-by-anomalous-office-365-activity"></a>Unmöglicher Ortswechsel zu atypischen Orten, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einem unmöglichen Ortswechsel zu atypischen Orten (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden:

- **Unmöglicher Ortswechsel zu atypischen Orten, der zu einer Exfiltration des Office 365-Postfachs führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers.
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden.

- **Unmöglicher Ortswechsel zu atypischen Orten, der zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number*> Verwaltungsaktivitäten durchgeführt.

- **Unmöglicher Ortswechsel zu atypischen Orten mit Massenlöschung von Dateien**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> am atypischen Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Unmöglicher Ortswechsel zu atypischen Orten mit Massendownload von Dateien**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Unmöglicher Ortswechsel zu atypischen Orten, der zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>. 
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung eine ungewöhnliche Menge (\<*number of activities*>) von Identitätswechselaktivitäten durchgeführt.

- **Unmöglicher Ortswechsel zu atypischen Orten mit Massenfreigabe von Dateien**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Unmöglicher Ortswechsel zu atypischen Orten, der zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem unmöglichen Ortswechsel zum atypischen Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.


### <a name="sign-in-activity-for-unfamiliar-location-followed-by-anomalous-office-365-activity"></a>Anmeldeaktivität für unbekannten Standort, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einer Anmeldeaktivität an einem unbekannten Ort (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden.

- **Anmeldeereignis von einem unbekannten Ort, das zu einer Exfiltration des Exchange Online-Postfachs führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> am unbekannten Ort \<*location*>, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers.
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden. 

- **Anmeldeereignis von einem unbekannten Ort, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> Verwaltungsaktivitäten durchgeführt.

- **Anmeldeereignis von einem unbekannten Ort, das zur Massenlöschung von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Anmeldeereignis von einem unbekannten Ort, das zum Massendownload von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Anmeldeereignis von einem unbekannten Ort, das zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>.
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung für mehr als \<*number of*> unterschiedliche Konten ein Identitätswechsel durchgeführt.

- **Anmeldeereignis von einem unbekannten Ort, das zur Massenfreigabe von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Anmeldeereignis von einem unbekannten Ort, das zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> vom unbekannten Ort \<*location*>. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.

### <a name="sign-in-activity-from-infected-device-followed-by-anomalous-office-365-activity"></a>Anmeldeaktivität mit infiziertem Gerät, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einer Anmeldeaktivität mit einem infizierten Gerät (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden:

- **Anmeldeereignis mit einem infizierten Gerät, das zu einer Exfiltration des Office 365-Postfachs führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von einem Gerät, das unter Umständen mit Schadsoftware infiziert ist, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers.
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden. 

- **Anmeldeereignis mit einem infizierten Gerät, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> Verwaltungsaktivitäten durchgeführt.

- **Anmeldeereignis mit einem infizierten Gerät, das zur Massenlöschung von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Anmeldeereignis mit einem infizierten Gerät, das zum Massendownload von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Anmeldeereignis mit einem infizierten Gerät, das zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung für mehr als \<*number of*> unterschiedliche Konten ein Identitätswechsel durchgeführt.

- **Anmeldeereignis mit einem infizierten Gerät, das zur Massenfreigabe von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Anmeldeereignis mit einem infizierten Gerät, das zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> mit einem Gerät, das unter Umständen mit Schadsoftware infiziert ist. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.

### <a name="sign-in-activity-from-anonymous-ip-address-followed-by-anomalous-office-365-activity"></a>Anmeldeaktivität von anonymer IP-Adresse, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einer Anmeldeaktivität von einer anonymen IP-Adresse (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden:

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu einer Exfiltration des Office 365-Postfachs führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers.
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden. 

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> Verwaltungsaktivitäten durchgeführt.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zur Massenlöschung von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zum Massendownload von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung für mehr als \<*number of*> unterschiedliche Konten ein Identitätswechsel durchgeführt.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zur Massenfreigabe von Dateien führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Anmeldeereignis von einer anonymen IP-Adresse, das zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis auf ein Anmeldeereignis von \<*account name*> von der anonymen Proxy-IP-Adresse \<*IP address*>. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.

### <a name="sign-in-activity-from-user-with-leaked-credentials-followed-by-anomalous-office-365-activity"></a>Anmeldeaktivität eines Benutzers mit kompromittierten Anmeldeinformationen, gefolgt von anomaler Office 365-Aktivität

Es gibt sieben mögliche Azure Sentinel-Incidents, für die Warnungen zu einer Anmeldeaktivität eines Benutzers mit kompromittierten Anmeldeinformationen (Azure AD Identity Protection) und per Microsoft Cloud App Security generierte anomale Office 365-Warnungen kombiniert werden:

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Exfiltration des Office 365-Postfachs führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen verwendet wurden, gefolgt von der Festlegung einer verdächtigen Weiterleitungsregel im Posteingang eines Benutzers. 
    
    Dies kann bedeuten, dass das Konto kompromittiert wurde und das Postfach genutzt wird, um Informationen Ihrer Organisation abzuschöpfen (Exfiltration). Der Benutzer \<*account name*> hat eine Weiterleitungsregel für den Posteingang erstellt oder aktualisiert, mit der alle eingehenden E-Mails an die externe Adresse \<*email address*> weitergeleitet werden. 

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zu verdächtiger Verwaltungsaktivität für die Cloud-App führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> Verwaltungsaktivitäten durchgeführt.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Massenlöschung von Dateien führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien gelöscht.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zum Massendownload von Dateien führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung \<*number of*> eindeutige Dateien heruntergeladen.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zu einem Office 365-Identitätswechsel führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden. 
    
    Anschließend wurde über das Konto \<*account name*> in nur einer Sitzung für mehr als \<*number of*> unterschiedliche Konten ein Identitätswechsel durchgeführt.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zur Massenfreigabe von Dateien führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden.
    
    Anschließend wurden über das Konto \<*account name*> in nur einer Sitzung mehr als \<*number of*> eindeutige Dateien freigegeben.

- **Anmeldeereignis eines Benutzers mit kompromittierten Anmeldeinformationen, das zum Vorhandensein von Ransomware in der Cloud-App führt**
    
    Diese Warnung ist ein Hinweis darauf, dass für das Anmeldeereignis mit \<*account name*> kompromittierte Anmeldeinformationen genutzt wurden. 
    
    Anschließend wurden über das Konto \<*account name*> \<*number of*> Dateien hochgeladen und insgesamt \<*number of*> Dateien gelöscht. 
    
    Dieses Aktivitätsmuster weist auf einen möglichen Angriff mit Ransomware hin.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun mehr über die erweiterte Erkennung von mehrstufigen Angriffen erfahren haben, ist für Sie ggf. die folgende Schnellstartanleitung interessant. Darin wird veranschaulicht, wie Sie Einblicke in Ihre Daten und in potenzielle Bedrohungen erhalten: [Erste Schritte mit Azure Sentinel](quickstart-get-visibility.md).

Arbeiten Sie das folgende Tutorial durch, wenn Sie bereit zum Untersuchen der Incidents sind, die für Sie erstellt wurden: [Untersuchen von Vorfällen mit Azure Sentinel](tutorial-investigate-cases.md).

