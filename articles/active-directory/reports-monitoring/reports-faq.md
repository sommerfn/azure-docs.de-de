---
title: Häufig gestellte Fragen zu Azure Active Directory-Berichten | Microsoft-Dokumentation
description: Häufig gestellte Fragen zu Azure Active Directory-Berichten
services: active-directory
documentationcenter: ''
author: cawrites
manager: MarkusVi
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: cbaf7d4e5a0dee82edf9b6b6bd65b91309e1344d
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74007692"
---
# <a name="frequently-asked-questions-around-azure-active-directory-reports"></a>Häufig gestellte Fragen zu Azure Active Directory-Berichten

Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu Azure Active Directory-Berichten (Azure AD). Weitere Informationen finden Sie unter [Azure Active Directory-Berichterstellung](overview-reports.md). 

## <a name="getting-started"></a>Erste Schritte 

**F: Ich verwende derzeit die Endpunkt-APIs unter „`https://graph.windows.net/<tenant-name>/reports/`“ zum programmgesteuerten Abrufen der Azure AD-Überwachungsberichte und der integrierten Anwendungsnutzungsberichte in unsere Berichtssysteme. Welche Umstellung sollte ich vornehmen?**

**A:** In der [API-Referenz](https://developer.microsoft.com/graph/) finden Sie Informationen dazu, wie Sie [mit den APIs auf Aktivitätsberichte zugreifen](concept-reporting-api.md). Dieser Endpunkt verfügt über zwei Berichte (**Überwachung** und **Anmeldungen**), die alle Daten des alten API-Endpunkts enthalten. Dieser neue Endpunkt verfügt auch über einen Bericht zu Anmeldeaktivitäten für die Azure AD Premium-Lizenz, über den Sie Informationen zur App-Nutzung, Gerätenutzung und Benutzeranmeldung abrufen können.

---

**F: Ich verwende derzeit die Endpunkt-APIs unter „`https://graph.windows.net/<tenant-name>/reports/`“ zum programmgesteuerten Abrufen der Azure AD-Sicherheitsberichte (bestimmte Arten von Erkennungen, z.B. kompromittierte Anmeldeinformationen oder Anmeldungen über anonyme IP-Adressen) in unsere Berichtssysteme. Welche Umstellung sollte ich vornehmen?**

**A:** Sie können die [API für Identity Protection-Risikoerkennungen](../identity-protection/graph-get-started.md) für den Zugriff auf Sicherheitserkennungen über Microsoft Graph verwenden. Dieses neue Format ermöglicht höhere Flexibilität beim Abfragen von Daten mit erweiterter Filterung, Feldauswahl und vielem mehr, und standardisiert Risikoerkennungen in einem Typ zur einfacheren Integration in SIEMs und andere Tools zur Datensammlung. Da die Daten in einem anderen Format vorliegen, können Sie Ihre alten Abfragen nicht durch eine neue Abfrage ersetzen. Allerdings [verwendet die neue API Microsoft Graph](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent), den Microsoft-Standard für diese APIs wie z.B. Office 365 oder Azure AD. So können Sie entweder Ihre aktuellen MS Graph-Investitionen erweitern oder die Umstellung auf diese neue Standardplattform beginnen.

---

**F: Wie erhalte ich eine Premium-Lizenz?**

**A:** Unter [Erste Schritte mit Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) erfahren Sie, wie Sie ein Upgrade für Ihre Azure Active Directory-Edition durchführen.

---

**F: Wie schnell werden nach Erwerb einer Premium-Lizenz Daten zu Aktivitäten angezeigt?**

**A:** Wenn Ihnen bereits mit einer kostenlosen Lizenz Daten zu Aktivitäten angezeigt werden, können Sie diese sofort sehen. Wenn Sie noch keine Daten haben, dauert es ein bis zwei Tage, bis die Daten in den Berichten angezeigt werden.

---

**F: Werden nach Erwerb einer Azure AD Premium-Lizenz die Daten des letzten Monats angezeigt?**

**A:** Wenn Sie vor Kurzem zu einer Premium-Version (einschließlich einer Testversion) gewechselt sind, werden Ihnen anfänglich Daten von bis zu 7 Tagen angezeigt. Wenn sich Daten ansammeln, können Sie die Daten der letzten 30 Tage sehen.

---

**F: Muss ich globaler Administrator sein, um die Anmeldeaktivitäten im Azure-Portal anzuzeigen oder Daten über die API abzurufen?**

**A:** Nein, Sie sind auch dann zum Zugriff auf die Berichtsdaten über das Portal oder die API berechtigt, wenn Sie **Sicherheitsleseberechtigter** oder **Sicherheitsadministrator** für den Mandanten sind. Natürlich haben auch **globale Administratoren** Zugriff auf diese Daten.

---


## <a name="activity-logs"></a>Aktivitätsprotokolle


**F: Wie lange werden Aktivitätsprotokolldaten (Überwachungs- und Anmeldevorgänge) im Azure-Portal aufbewahrt?** 

**A:** Die folgende Tabelle zeigt den Datenaufbewahrungszeitraum für Aktivitätsprotokolle. Weitere Informationen finden Sie unter [Aufbewahrungsrichtlinien für Azure AD-Berichte](reference-reports-data-retention.md).

| Bericht                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Überwachungsprotokolle             | 7 Tage        | 30 Tage             | 30 Tage             |
| Anmeldungen               | –           | 30 Tage             | 30 Tage             |
| Azure MFA-Nutzung        | 30 Tage       | 30 Tage             | 30 Tage             |

---

**F: Wie lange dauert es, bis nach Abschluss einer Aufgabe Aktivitätsdaten angezeigt werden?**

**A:** Bei Überwachungsprotokollen kann die Verzögerung 15 Minuten bis zu einer Stunde betragen. Aktivitätsprotokolle für Anmeldevorgänge können von 15 Minuten bis zu 2 Stunden bei einigen Datensätzen in Anspruch nehmen.

---

**F: Kann ich Office 365-Aktivitätsprotokollinformationen über das Azure-Portal abrufen?**

**A:** Obwohl Office 365- und Azure AD-Aktivitätsprotokolle einen Großteil der Verzeichnisressourcen gemeinsam nutzen, müssen Sie, wenn Sie eine vollständige Ansicht der Office 365-Aktivitätsprotokolle wünschen, das [Microsoft 365 Admin Center](https://admin.microsoft.com) besuchen, um Office 365-Aktivitätsprotokollinformationen abzurufen.

---

**F: Mit welchen APIs kann ich Informationen zu Office 365-Aktivitätsprotokollen abrufen?**

**A:** Über die [Office 365-Verwaltungsaktivitäts-APIs](https://docs.microsoft.com/office/office-365-management-api/office-365-management-apis-overview) können Sie auf die Office 365-Aktivitätsprotokolle zugreifen.

---

**F: Wie viele Datensätze kann ich aus dem Azure-Portal herunterladen?**

**A:** Sie können bis zu 5000 Datensätze aus dem Azure-Portal herunterladen. Die Datensätze werden nach *Aktualität* sortiert, und standardmäßig erhalten Sie die neuesten 5.000 Datensätze.

---

## <a name="risky-sign-ins"></a>Riskante Anmeldungen

**F: Es gibt eine Risikoerkennung in Identity Protection, aber ich sehe im Anmeldebericht keinen entsprechenden Anmeldevorgang. Entspricht dies dem erwarteten Verhalten?**

**A:** Ja, Identity Protection beurteilt das Risiko für alle Authentifizierungsflows, ganz gleich, ob diese interaktiv oder nicht interaktiv sind. „Alle Anmeldungen“ führt jedoch nur die interaktiven Anmeldungen auf.

---

**F: Wie erfahre ich, warum eine Anmeldung oder ein Benutzer im Azure-Portal als riskant gekennzeichnet wurde?**

**A:** Wenn Sie über ein **Azure AD Premium**-Abonnement verfügen, erfahren Sie mehr über die zugrunde liegenden Risikoerkennungen, wenn Sie unter **Benutzer mit Risikomarkierung** einen Benutzer oder im Bericht **Riskante Anmeldungen** einen Datensatz auswählen. Wenn Sie ein **Free**- oder **Basic**-Abonnement haben, können Sie zwar die Benutzer mit Risikomarkierung und die Berichte „Riskante Anmeldungen“ anzeigen, allerdings nicht die Informationen zu den zugrunde liegenden Risikoerkennungen.

---

**F: Wie werden IP-Adressen im Bericht zu Anmeldungen und riskanten Anmeldungen berechnet?**

**A:** IP-Adressen werden so ausgestellt, dass es keine definitive Verbindung zwischen einer IP-Adresse und dem physischen Standort des Computers mit dieser Adresse gibt. Das Zuordnen von IP-Adressen wird außerdem durch Faktoren wie Mobilfunkanbieter und VPNs verkompliziert, die IP-Adressen aus zentralen Pools zuweisen, die oft sehr weit von den Orten entfernt sind, an denen das Clientgerät tatsächlich verwendet wird. Derzeit erfolgt das Konvertieren einer IP-Adresse in einen physischen Standort basierend auf Ablaufverfolgungen, Registrierungsdaten, umgekehrten Suchvorgängen und anderen Informationen. 

---

**F: Was bedeutet die Risikoerkennung „Anmeldung mit erhöhtem Risiko erkannt“?**

**A:** Die Funktion „Anmeldung mit erhöhtem Risiko erkannt“ dient als Platzhalter für Anmeldungen für Erkennungen, die exklusiv Azure AD Identity Protection-Abonnenten zur Verfügung stehen, um Ihnen einen Einblick in alle riskante Anmeldungen in Ihrer Umgebung zu geben.

---

## <a name="conditional-access"></a>Bedingter Zugriff

**F: Was ist neu an diesem Feature?**

**A:** Kunden können nun im Bericht für alle Anmeldungen Probleme mit Richtlinien für bedingten Zugriff beheben. Benutzer können den Status des bedingten Zugriffs überprüfen und die Richtliniendetails für die Anmeldung sowie die jeweiligen Richtlinienergebnisse ansehen.

**F: Wie fange ich an?**

**A:** Erste Schritte:

* Navigieren Sie im [Azure-Portal](https://portal.azure.com) zum Anmeldebericht.
* Klicken Sie auf die Anmeldung für die Problembehandlung.
* Navigieren Sie zur Registerkarte **Bedingter Zugriff**. Hier werden alle Richtlinien angezeigt, die die Anmeldung und das Ergebnis der einzelnen Richtlinien betreffen. 
    
**F: Welche Werte kann der Status des bedingten Zugriffs aufweisen?**

**A:** Der Status des bedingten Zugriffs kann folgende Werte aufweisen:

* **Nicht angewendet**: Es ist keine Richtlinie für bedingten Zugriff für den Benutzer und die App vorhanden. 
* **Erfolg**: Für den Benutzer und die App ist eine Richtlinie für bedingten Zugriff vorhanden, und diese wurde erfüllt. 
* **Fehler**: Für den Benutzer und die App ist eine Richtlinie für bedingten Zugriff vorhanden, und diese wurde nicht erfüllt. 
    
**F: Welche Werte sind als Ergebnis einer Richtlinie für bedingten Zugriff möglich?**

**A:** Eine Richtlinie für bedingten Zugriff kann die folgenden Ergebnisse liefern:

* **Erfolg**: Die Richtlinie wurde erfüllt.
* **Fehler**: Die Richtlinie wurde nicht erfüllt.
* **Nicht angewendet**: Die Richtlinienbedingungen wurden möglicherweise nicht erfüllt.
* **Nicht aktiviert**: Die Richtlinie ist deaktiviert. 
    
**F: Der Richtlinienname im Bericht für alle Anmeldungen entspricht nicht dem Namen der Richtlinie für bedingten Zugriff. Warum?**

**A:** Der Richtlinienname im Bericht für alle Anmeldungen basiert auf dem Namen der Richtlinie für bedingten Zugriff zum Anmeldezeitpunkt. Die Namen müssen anfangs nicht übereinstimmen, wenn Sie den Richtliniennamen später aktualisieren, d. h. nach der Anmeldung.

**F: Meine Anmeldung wurde aufgrund einer Richtlinie für bedingten Zugriff blockiert, der Anmeldeaktivitätsbericht gibt jedoch an, dass die Anmeldung erfolgreich war. Warum?**

**A:** Der Anmeldebericht zeigt möglicherweise derzeit keine genauen Ergebnisse für Exchange ActiveSync-Szenarien an, wenn der bedingte Zugriff angewendet wird. In manchen Fällen wird zwar eventuell ein erfolgreiches Anmeldeergebnis im Bericht angezeigt, doch bei der Anmeldung ist aufgrund einer Richtlinie für bedingten Zugriff in Wirklichkeit ein Fehler aufgetreten. 
