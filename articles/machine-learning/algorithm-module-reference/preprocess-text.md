---
title: 'Preprocess Text: Modulreferenz'
titleSuffix: Azure Machine Learning
description: Erfahren Sie, wie Sie das Modul „Preprocess Text“ (Text vorverarbeiten) in Azure Machine Learning verwenden, um Text zu bereinigen und zu vereinfachen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 09/01/2019
ms.openlocfilehash: 9bf81b4e4c305c864592c32a8737d1a5eeb18651
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73497684"
---
# <a name="preprocess-text"></a>Preprocess Text

In diesem Artikel wird ein Modul in Azure Machine Learning-Designer (Vorschauversion) beschrieben.

Verwenden Sie das Modul **Preprocess Text** (Text vorverarbeiten), um Text zu bereinigen und zu vereinfachen. Es unterstützt diese gängigen Textverarbeitungsvorgänge:

* Entfernen von Stoppwörtern
* Verwenden regulärer Ausdrücke zum Suchen nach und Ersetzen von spezifischen Zielzeichenfolgen
* Lemmatisierung, bei der mehrere verwandte Wörter in eine einzelne kanonische Form konvertiert werden
* Kasusnormalisierung
* Entfernen bestimmter Klassen von Zeichen, z.B. Ziffern, Sonderzeichen und Sequenzen von wiederholten Zeichen (etwa „aaaa“)
* Identifizierung und Entfernung von E-Mails und URLs

Das Modul **Preprocess Text** (Text vorverarbeiten) unterstützt zurzeit nur Englisch.

## <a name="configure-text-preprocessing"></a>Konfigurieren der Textvorverarbeitung  

1.  Fügen Sie das Modul **Preprocess Text** (Text vorverarbeiten) zu Ihrer Pipeline in Azure Machine Learning hinzu. Sie finden dieses Modul unter **Textanalyse**.

1. Stellen Sie eine Verbindung mit einem Dataset her, das mindestens eine Spalte mit Text enthält.

1. Wählen Sie die Sprache aus der Dropdownliste **Language** (Sprache) aus.

1. **Text column to clean** (Zu bereinigende Textspalte): Wählen Sie die Spalte aus, die Sie vorverarbeiten möchten.

1. **Remove stop words** (Stoppwörter entfernen): Wählen Sie diese Option aus, wenn Sie eine vordefinierte Liste mit Stoppwörtern auf die Textspalte anwenden möchten. 

    Stoppwortlisten sind sprachabhängig und anpassbar.

1. **Lemmatization** (Lemmatisierung): Wählen Sie diese Option aus, wenn Sie möchten, dass Wörter in ihrer kanonischen Form dargestellt werden. Diese Option ist nützlich, um die Anzahl eindeutiger Vorkommen von anderweitig ähnlichen Texttoken zu verringern.

    Der Lemmatisierungsprozess ist hochgradig sprachabhängig.

1. **Detect sentences** (Sätze erkennen): Wählen Sie diese Option aus, wenn das Modul beim Durchführen einer Analyse eine Satzbegrenzungsmarkierung einfügen soll.

    Dieses Modul verwendet drei senkrechte Striche (`|||`), um das Satzendezeichen darzustellen.

1. Führen Sie optionale Suchen- und Ersetzen-Vorgänge mithilfe regulärer Ausdrücke aus.

    * **Custom regular expression** (Benutzerdefinierter regulärer Ausdruck): Definieren Sie den Text, nach dem Sie suchen.
    * **Custom replacement string** (Benutzerdefinierte Ersatzzeichenfolge): Definieren Sie einen einzelnen Ersatzwert.

1. **Normalize case to lowercase** (Groß-/Kleinschreibung in Kleinbuchstaben normalisieren): Wählen Sie diese Option aus, wenn Sie ASCII-Großbuchstaben in Kleinbuchstaben umwandeln möchten.

    Wenn Zeichen nicht normalisiert werden, wird das gleiche Wort in Groß- und Kleinbuchstaben als zwei verschiedene Wörter betrachtet.

1. Sie können auch die folgenden Typen von Zeichen oder Zeichensequenzen aus dem verarbeiteten Ausgabetext entfernen:

    * **Remove numbers** (Zahlen entfernen): Wählen Sie diese Option aus, um alle numerischen Zeichen für die angegebene Sprache zu entfernen. Identifikationsnummern sind vom Fachgebiet und von der Sprache abhängig. Wenn numerische Zeichen ein integraler Bestandteil eines bekannten Worts sind, wird die Zahl möglicherweise nicht entfernt.
    
    * **Remove special characters** (Sonderzeichen entfernen): Verwenden Sie diese Option, um alle nicht alphanumerischen Sonderzeichen zu entfernen.
    
    * **Remove duplicate characters** (Doppelte Zeichen entfernen): Wählen Sie diese Option aus, um zusätzliche Zeichen in Sequenzen zu entfernen, die sich mehr als zwei Mal wiederholen. Beispielsweise würde eine Sequenz wie „aaaaa“ in „aa“ reduziert.
    
    * **Remove email addresses** (E-Mail-Adressen entfernen): Wählen Sie diese Option aus, um eine beliebige Sequenz im Format `<string>@<string>` zu entfernen.  
    * **Remove URLs** (URLs entfernen): Wählen Sie diese Option aus, um alle Sequenzen zu entfernen, die die folgenden URL-Präfixe enthalten: `http`, `https`, `ftp`, `www`
    
1. **Expand verb contractions** (Verbverschmelzungen erweitern): Diese Option gilt nur für Sprachen, die Verbverschmelzungen verwenden (zurzeit nur für Englisch). 

    Wenn Sie diese Option auswählen, können Sie beispielsweise die Phrase *„wouldn't stay there“* durch *„would not stay there“* ersetzen.

1. **Normalize backslashes to slashes** (Umgekehrte Schrägstriche in Schrägstriche normalisieren): Wählen Sie diese Option aus, um alle Instanzen von `\\` `/` zuzuordnen.

1. **Split tokens on special characters** (Token für Sonderzeichen aufteilen): Wählen Sie diese Option aus, wenn Sie Wörter bei Zeichen wie `&`, `-` usw. aufteilen möchten. Mit dieser Option können auch Sonderzeichen verringert werden, wenn sie mehr als zwei Mal wiederholt werden. 

    Beispielsweise würde die Zeichenfolge `MS---WORD` in drei Token aufgeteilt: `MS`, `-` und `WORD`.

1. Ausführen der Pipeline.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich die [Gruppe der verfügbaren Module](module-reference.md) für Azure Machine Learning an. 