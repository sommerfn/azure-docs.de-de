---
title: Erweitern Ihres Experiments mit R
titleSuffix: ML Studio (classic) Azure
description: Erweitern der Funktionalität von Azure Machine Learning Studio (klassisch) über die Sprache R mithilfe des Moduls Execute R Script.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 1a64303702ffa5ccff9c79e0946d5df45eac9479
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73621642"
---
# <a name="azure-machine-learning-studio-classic-extend-your-experiment-with-r"></a>Azure Machine Learning Studio (klassisch): Erweitern Ihres Experiments mit R 
Sie können die Funktionalität von Azure Machine Learning Studio (klassisch) über die Sprache R mithilfe des Moduls [Execute R Script][execute-r-script] erweitern.

Dieses Modul akzeptiert mehrere Eingabedatasets und gibt als Ergebnis ein einzelnes Dataset aus. Sie können für den Parameter **R Script** des Moduls [Execute R Script][execute-r-script] ein R-Skript eingeben.

Sie können auf jeden Eingangsport des Moduls mithilfe von Code wie folgt zugreifen:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Auflisten aller derzeit installierten Pakete
Die Liste der installierten Pakete kann sich ändern. Eine Liste der derzeit installierten Pakete finden Sie unter [R Packages supported by Azure Machine Learning Studio (classic)](https://msdn.microsoft.com/library/azure/mt741980.aspx) (Von Azure Machine Learning Studio (klassisch) unterstützte R-Pakete).

Sie können auch die vollständige und aktuelle Liste der installierten Pakete abrufen, indem Sie folgenden Code im Modul [Execute R Script][execute-r-script] eingeben:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Damit wird die Liste der Pakete an den Ausgabeport des Moduls [Execute R Script][execute-r-script] gesendet.
Um die Liste der Pakete anzuzeigen, verbinden Sie ein Modul für die Konvertierung wie [Convert to CSV][convert-to-csv] mit der linken Ausgabe des Moduls [Execute R Script][execute-r-script]. Führen Sie das Experiment aus, klicken Sie auf die Ausgabe des Konvertierungsmoduls, und wählen Sie dann **Download** aus. 

![Herunterladen der Ausgabe des Moduls „Convert to CSV“](./media/extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](https://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Importieren von Paketen
Sie können noch nicht installierte Pakete mithilfe der folgenden Befehle im Modul [Execute R Script][execute-r-script] importieren:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Dabei enthält die Datei `my_favorite_package.zip` Ihr Paket.




<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
