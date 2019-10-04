---
title: Dynamisch gesperrte Kennwörter – Azure Active Directory
description: Sperren Sie mit dynamisch gesperrten Kennwörtern in Azure AD unsichere Kennwörter aus Ihrer Umgebung aus.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77309a73f3c5641aa8a7667015ed02808e376348
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/26/2019
ms.locfileid: "70032878"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Beseitigen falscher Kennwörter in Ihrer Organisation

Branchenführer schärfen Ihnen ein, dasselbe Kennwort nicht mehrmals zu verwenden, ein Kennwort komplex zu gestalten und keine einfachen Kennwörter wie „Kennwort123“ zu verwenden. Wie können Organisationen gewährleisten, dass ihre Benutzer Best Practice-Anleitungen folgen? Wie können sie sicherstellen, dass Benutzer keine unsicheren Kennwörter oder sogar Varianten von unsicheren Kennwörtern nutzen?

Der erste Schritt zur Verwendung stärkerer Kennwörter besteht darin, Ihren Benutzern eine Anleitung zu geben. Die aktuelle Anleitung von Microsoft zu diesem Thema finden Sie unter folgendem Link:

[Microsoft-Kennwortleitfaden](https://www.microsoft.com/research/publication/password-guidance)

Eine gute Anleitung ist wichtig. Aber selbst dann wissen wir, dass viele Benutzer immer noch schwache Kennwörter wählen werden. Der Azure AD- Kennwortschutz schützt Ihre Organisation, indem er bekannte schwache Kennwörter und deren Varianten erkennt und blockiert sowie optional zusätzliche schwache Ausdrücke blockiert, die spezifisch für Ihre Organisation sind.

Weitere Informationen zur derzeitigen Arbeit in Bezug auf Sicherheitsmaßnahmen finden Sie im [Microsoft Security Intelligence Report](https://www.microsoft.com/security/operations/security-intelligence-report).

## <a name="global-banned-password-list"></a>Liste global gesperrter Kennwörter

Das Azure AD Identity Protection-Team analysiert fortlaufend die Azure AD-Sicherheitstelemetriedaten und sucht nach häufig verwendeten schwachen oder kompromittierten Kennwörtern, vor allem nach den schwachen Basisausdrücken, die oft als Grundlage für schwache Kennwörter verwendet werden. Wenn derartige schwache Ausdrücke gefunden werden, werden sie in die globale Liste der gesperrten Kennwörter aufgenommen. Der Inhalt der Liste global gesperrter Kennwörtern basiert nicht auf einer externen Datenquelle. Die Liste mit global gesperrten Kennwörtern basiert vollständig auf den fortlaufenden Ergebnissen der Azure AD-Sicherheitstelemetridaten und der Analysen.

Wann immer ein neues Kennwort für einen Benutzer eines Mandanten in Azure AD geändert oder zurückgesetzt wird, wird bei der Überprüfung der Stärke des Kennworts die aktuelle Version der globalen Liste der gesperrten Kennwörter als Schlüsseleingabe verwendet. Durch diese Validierung werden die Kennwörter für alle Azure AD-Kunden deutlich stärker.

> [!NOTE]
> Cyberkriminelle verwenden bei ihren Angriffen ebenfalls ähnliche Strategien. Daher veröffentlicht Microsoft den Inhalt dieser Liste nicht öffentlich.

## <a name="custom-banned-password-list"></a>Benutzerdefinierte Liste gesperrter Kennwörter

Manche Organisationen möchten die Sicherheit möglicherweise noch weiter erhöhen, indem sie die Liste global gesperrter Kennwörter mit ihren eigenen Anpassungen in der von Microsoft als benutzerdefinierte Liste gesperrter Kennwörter bezeichneten Liste ergänzen. Microsoft empfiehlt, dass Ausdrücke, die dieser Liste hinzugefügt werden, in erster Linie auf organisationsspezifische Ausdrücke ausgerichtet sind, wie z.B.:

- Markennamen
- Produktnamen
- Standorte (z.B. Unternehmenshauptsitze)
- Interne unternehmensspezifische Ausdrücke
- Abkürzungen mit einer bestimmten, unternehmensspezifischen Bedeutung.

Sobald Ausdrücke zur benutzerdefinierten Liste gesperrter Kennwörter hinzugefügt wurden, werden sie bei der Überprüfung von Kennwörtern mit den Ausdrücken in der globalen Liste gesperrter Kennwörter kombiniert.

> [!NOTE]
> Die benutzerdefinierte Liste gesperrter Kennwörter ist auf maximal 1.000 Ausdrücke beschränkt. Sie dient nicht zum Blockieren von äußerst umfangreichen Kennwortlisten. Um die Vorteile der benutzerdefinierten Liste gesperrter Kennwörter umfassend nutzen zu können, empfiehlt Microsoft, sich zunächst mit dem Algorithmus zur Kennwortvalidierung vertraut zu machen (siehe [Auswerten von Kennwörtern](concept-password-ban-bad.md#how-are-passwords-evaluated)), bevor neue Ausdrücke zur benutzerdefinierten Liste gesperrter Kennwörter hinzugefügt werden. Wenn Sie verstehen, wie der Algorithmus funktioniert, kann Ihr Unternehmen eine große Anzahl von schwachen Kennwörtern und deren Varianten effizient erkennen und blockieren.

Zum Beispiel: Nehmen wir einen Kunden namens „Contoso“, der seinen Sitz in London hat und ein Produkt namens „Widget“ herstellt. Für einen solchen Kunden wäre es sowohl verschwenderisch als auch unsicher, zu versuchen, bestimmte Varianten dieser Begriffe zu blockieren, wie z.B.:

- „Contoso!1“
- "Contoso@London"
- „ContosoWidget“
- „!Contoso“
- „LondonHQ“
- ... usw.

Stattdessen ist es wesentlich effizienter und sicherer, nur die wichtigsten Basisausdrücke zu blockieren:

- „Contoso“
- „London“
- „Widget“

Der Algorithmus zur Kennwortvalidierung blockiert dann automatisch schwache Varianten und Kombinationen der oben genannten Varianten.

Die benutzerdefinierte Liste gesperrter Kennwörter sowie die Möglichkeit, eine lokale Active Directory-Integration zu aktivieren, wird mit dem Azure-Portal verwaltet.

![Ändern der benutzerdefinierten Liste gesperrter Kennwörter unter „Authentifizierungsmethoden“](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="password-spray-attacks-and-third-party-compromised-password-lists"></a>Kennwort-Spray-Angriffe und kompromittierte Kennwortlisten von Drittanbietern

Ein wichtiger Vorteil des Kennwortschutzes von Azure AD ist es, das Sie bei der Abwehr von Kennwort-Spray-Angriffen unterstützt werden. Die meisten Kennwort-Spray-Angriffe versuchen nicht, ein bestimmtes einzelnes Konto mehr als ein paar Mal anzugreifen, da ein solches Verhalten die Wahrscheinlichkeit einer Erkennung erheblich erhöht, entweder durch eine Kontosperre oder auf andere Weise. Die Mehrheit der Kennwort-Spray-Angriffe beruht daher darauf, dass nur eine geringe Anzahl der bekanntermaßen schwächsten Kennwörter für alle Konten in einem Unternehmen übermittelt werden. Diese Technik ermöglicht es dem Angreifer, schnell nach einem leicht zu kompromittierenden Konto zu suchen und gleichzeitig mögliche Schwellenwerte für die Erkennung zu vermeiden.

Der Azure AD-Kennwortschutz wurde entwickelt, um alle bekannten schwachen Kennwörter, die wahrscheinlich bei Kennwort-Spray-Angriffen verwendet werden, auf der Grundlage von realen Sicherheitstelemetriedaten aus der Sicht von Azure AD effizient zu blockieren.  Microsoft ist bekannt, dass es Websites von Drittanbietern gibt, die Millionen von Kennwörtern auflisten, die bei früheren öffentlich bekannten Sicherheitsverletzungen kompromittiert wurden. Es ist üblich, dass Kennwortvalidierungsprodukte von Drittanbietern auf einem Brute-Force-Vergleich mit diesen Millionen von Kennwörtern basieren. Microsoft ist der Ansicht, dass solche Techniken angesichts der typischen Strategien, die bei Kennwort-Spray-Angriffen zum Einsatz kommen, nicht der beste Weg sind, die allgemeine Kennwortstärke zu verbessern.

> [!NOTE]
> Die Liste der von Microsoft global gesperrten Kennwörter basiert in keinster Weise auf Drittdatenquellen, wie Listen von kompromittierten Kennwörtern.

Auch wenn die Liste der von Microsoft global gesperrten Kennwörter im Vergleich zu einigen Bulklisten von Drittanbietern klein ist, bietet sie mehr Sicherheit, da sie aus den realen Sicherheitstelemetriedaten von tatsächlichen Kennwort-Spray-Angriffen zusammengestellt wird, und der Algorithmus zur Kennwortvalidierung von Microsoft intelligente Fuzzy-Matching-Techniken verwendet. Daher werden damit Millionen der häufigsten schwachen Kennwörter effizient erkannt und deren Verwendung in Ihrem Unternehmen blockiert. Kunden, die sich dafür entscheiden, unternehmensspezifische Ausdrücke zur benutzerdefinierten Liste gesperrter Kennwörter hinzuzufügen, profitieren ebenfalls vom gleichen Algorithmus.

Weitere Informationen zu kennwortbasierten Sicherheitsproblemen finden Sie unter [Your Pa$$word doesn't matter](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Your-Pa-word-doesn-t-matter/ba-p/731984) (Ihr Pa$$word spielt keine Rolle).

## <a name="on-premises-hybrid-scenarios"></a>Lokale Hybridszenarien

Schützen von ausschließlichen Cloudkonten ist hilfreich, aber viele Organisationen behalten Hybridszenarien einschließlich des lokalen Windows Server Active Directory bei. Die Sicherheitsvorteile des Azure AD-Kennwortschutzes können über die Installation von lokalen Agents auch auf Ihre Windows Server Active Directory-Umgebung erweitert werden. Zurzeit müssen Benutzer und Administratoren, die Kennwörter in Active Directory ändern oder zurücksetzen, dieselbe Kennwortrichtlinie einhalten wie ausschließliche Cloudbenutzer.

## <a name="how-are-passwords-evaluated"></a>Auswerten von Kennwörtern

Wann immer ein Benutzer sein Kennwort ändert oder zurücksetzt, wird das neue Kennwort auf Stärke und Komplexität überprüft, indem es mit der kombinierten Liste von Ausdrücken aus der globalen und der benutzerdefinierte Liste gesperrter Kennwörter (falls letztere konfiguriert wurde) verglichen wird.

Selbst wenn das Kennwort eines Benutzers ein gesperrtes Kennwort enthält, kann das Kennwort dennoch akzeptiert werden, wenn das Gesamtkennwort ansonsten stark genug ist. Ein neu konfiguriertes Kennwort durchläuft die folgenden Schritte, um seine Gesamtstärke zu beurteilen und festzustellen, ob es akzeptiert oder abgelehnt werden sollte.

### <a name="step-1-normalization"></a>Schritt 1: Normalisierung

Ein neues Kennwort durchläuft zunächst einen Normalisierungsprozess. Diese Technik ermöglicht es, einen kleinen Satz verbotener Kennwörter auf einen viel größeren Satz potenziell schwacher Kennwörter zu übertragen.

Die Normalisierung besteht aus zwei Teilen.  Zunächst werden alle Großbuchstaben in Kleinbuchstaben umgewandelt.  Zweitens werden häufige Zeichen ersetzt, wie z.B.:  

| Ursprünglicher Buchstabe  | Ersetzter Buchstabe |
| --- | --- |
| '0'  | 'o' |
| '1'  | 'l' |
| '$'  | 's' |
| '\@'  | 'a' |

Beispiel: Nehmen wir an, dass das Kennwort „blank“ gesperrt ist und ein Benutzer versucht, sein Kennwort in „Bl@nK“ zu ändern. Obwohl „Bl@nk“ nicht ausdrücklich verboten ist, wandelt der Normalisierungsprozess dieses Kennwort in „blank“ um, was ein gesperrtes Kennwort ist.

### <a name="step-2-check-if-password-is-considered-banned"></a>Schritt 2: Überprüfen, ob das Kennwort als gesperrt angesehen wird

#### <a name="fuzzy-matching-behavior"></a>Verhaltens der Fuzzyübereinstimmung

Die Fuzzyübereinstimmung wird für das normalisierte Kennwort verwendet, um festzustellen, ob es ein Kennwort enthält, das entweder in der globalen oder in der benutzerdefinierten Liste der gesperrten Kennwörter enthalten ist. Der Abgleichsprozess basiert auf einer Edit-Distanz eines (1) Vergleichs.  

Beispiel: Nehmen wir an, dass das Kennwort „leer“ gesperrt ist und ein Benutzer versucht, sein Kennwort in eines der folgenden zu ändern.

„abcdeg“ *(letztes Zeichen von „f“ in „g“ geändert)* „abcdefg“ *(„g“ am Ende angefügt)* „abcde“ *(nachfolgendes „f“ wurde am Ende gelöscht)*

Jedes der obigen Kennwörter entspricht nicht spezifisch dem verbotenen Kennwort „abcdef“. Da sich jedoch jedes Beispiel in einer Edit-Distanz von 1 des gesperrten Ausdrucks „abcdef“ befindet, werden sie alle als Übereinstimmung mit „abcdef“ betrachtet.

#### <a name="substring-matching-on-specific-terms"></a>Abgleich der Teilzeichenfolge (für bestimmte Ausdrücke)

Der Abgleich der Teilzeichenfolge wird für das normalisierte Kennwort verwendet, um den Vor- und Nachnamen des Benutzers sowie den Mandantennamen zu überprüfen (beachten Sie, dass der Abgleich des Mandantennamens nicht durchgeführt wird, wenn Kennwörter auf einem Active Directory-Domänencontroller überprüft werden).

Beispiel: Der Benutzer Pol möchte sein Kennwort auf „P0l123fb“ zurücksetzen. Nach der Normalisierung würde dieses Kennwort „pol123fb“ lauten. Durch den Abgleich der Teilzeichenfolge wird festgestellt, dass das Kennwort den Vornamen des Benutzers, „Pol“, enthält. Obwohl „P0l123fb“ in keiner Liste gesperrter Kennwörter enthalten war, wurde beim Abgleich der Teilzeichenfolgen „Pol“ im Kennwort gefunden. Aus diesem Grund würde dieses Kennwort zurückgewiesen werden.

#### <a name="score-calculation"></a>Bewertungsberechnung

Der nächste Schritt besteht darin, alle Fälle von verbotenen Kennwörtern in dem normalisierten neuen Kennwort des Benutzers zu identifizieren. Führen Sie dann folgende Schritte aus:

1. Jedes verbotene Kennwort, das sich im Kennwort eines Benutzers befindet, erhält einen Punkt.
2. Jedes verbleibende eindeutige Zeichen erhält einen Punkt.
3. Ein Kennwort muss mindestens fünf (5) Punkte erhalten, um akzeptiert zu werden.

Für die nächsten beiden Beispiele nehmen wir an, dass Contoso den Azure AD-Kennwortschutz verwendet und „contoso“ auf seiner benutzerdefinierten Liste hat. Nehmen wir außerdem an, „blank“ befindet sich auf der globalen Liste.

Beispiel: ein Benutzer ändert sein Kennwort in „C0ntos0Blank12“.

Nach der Normalisierung würde dieses Kennwort „contosoblank12“ lauten. Der Abgleichsprozess ermittelt, dass dieses Kennwort zwei gesperrte Kennwörter enthält: „contoso“ und „blank“. Dieses Kennwort erhält anschließend eine Bewertung:

[contoso] + [leer] + [1] + [2] = 4 Punkte. Da das Kennwort weniger als fünf (5) Punkte erhält, wird es abgelehnt.

Beispiel: ein Benutzer ändert sein Kennwort in „ContoS0Bl@nkf9“.

Nach der Normalisierung würde dieses Kennwort „contosoblankf9!“ lauten. Der Abgleichsprozess ermittelt, dass dieses Kennwort zwei gesperrte Kennwörter enthält: „contoso“ und „blank“. Dieses Kennwort erhält anschließend eine Bewertung:

[Contoso] + [leer] + [f] + [9] + [!] = 5 Punkte. Da dieses Kennwort mindestens fünf (5) Punkte erhalten hat, wird es akzeptiert.

   > [!IMPORTANT]
   > Bitte beachten Sie, dass der Algorithmus für verbotene Kennwörter zusammen mit der globalen Liste basierend auf der laufenden Sicherheitsanalyse und -forschung jederzeit in Azure geändert werden kann und wird. Für den lokalen DC-Agent-Dienst werden aktualisierte Algorithmen erst nach einer Neuinstallation der DC-Agent-Software wirksam.

## <a name="license-requirements"></a>Lizenzanforderungen

|   | Kennwortschutz für Azure AD mit globaler Liste gesperrter Kennwörter | Kennwortschutz für Azure AD mit benutzerdefinierter Liste gesperrter Kennwörter|
| --- | --- | --- |
| Reine Cloudbenutzer | Azure AD Free | Azure AD Premium P1 oder P2 |
| Über die lokale Windows Server Active Directory-Instanz synchronisierte Benutzer | Azure AD Premium P1 oder P2 | Azure AD Premium P1 oder P2 |

> [!NOTE]
> Lokale Windows Server Active Directory-Benutzer, die nicht mit Azure Active Directory synchronisiert werden, können auch die Vorteile des Azure AD-Kennwortschutzes basierend auf vorhandenen Lizenzen für synchronisierte Benutzer nutzen.

Weitere Informationen zur Lizenzierung einschließlich der Kosten finden Sie unter [Azure Active Directory – Preise](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>Anzeige für Benutzer

Wenn ein Benutzer versucht, ein Kennwort auf einen Wert zurückzusetzen, der gesperrt würde, wird die folgende Fehlermeldung angezeigt:

Ihr Kennwort ist aufgrund eines enthaltenen Worts, Ausdrucks oder Musters leider leicht zu erraten. Wiederholen Sie den Vorgang mit einem anderen Kennwort.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren der Liste benutzerdefinierter gesperrter Kennwörter](howto-password-ban-bad.md)
- [Vorschau: Bereitstellen von Azure AD-Kennwortschutz](howto-password-ban-bad-on-premises-deploy.md)
