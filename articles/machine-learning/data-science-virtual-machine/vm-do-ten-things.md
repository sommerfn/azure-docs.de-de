---
title: Untersuchen von Daten und Modellen unter Windows
titleSuffix: Azure Data Science Virtual Machine
description: Führen Sie Aufgaben zur Datenauswertung und -modellierung in Data Science Virtual Machine unter Windows aus.
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: dc8a870d692108f3a33b89a1c3826d421dfd1f63
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73824399"
---
# <a name="ten-things-you-can-do-on-the-windows-data-science-virtual-machine"></a>Zehn Dinge, die Sie mit der Windows Data Science Virtual Machine machen können

Die Windows Data Science Virtual Machine (DSVM) ist eine leistungsfähige Data Science-Entwicklungsumgebung, in der Sie Aufgaben zur Datenauswertung und -modellierung ausführen können. Die Umgebung wird bereits mit mehreren gängigen Datenanalysetools geliefert, sodass Sie mit Ihrer Analyse für lokale, Cloud- oder Hybridbereitstellungen beginnen können. 

Die DSVM arbeitet eng mit Azure-Diensten zusammen. Sie kann Daten lesen und verarbeiten, die bereits in Azure in Azure SQL Data Warehouse, Azure Data Lake, Azure Storage oder Azure Cosmos DB gespeichert sind. Sie kann auch andere Analysetools nutzen, etwa Azure Machine Learning und Azure Data Factory.

In diesem Artikel ist beschrieben, wie Sie Ihre DSVM nutzen können, um Data Science-Aufgaben auszuführen und mit anderen Azure-Diensten zu interagieren. Sie können z.B. die folgenden Aufgaben auf der DSVM ausführen:

- Lokales Auswerten von Daten und Entwickeln von Modellen auf der DSVM mit Microsoft Machine Learning Server und Python
- Verwenden eines Jupyter-Notebooks, um in einem Browser mit Ihren Daten durch Verwenden von Python 2, Python 3 und Microsoft R zu experimentieren (Microsoft R ist eine einsatzfähige Version von R, die auf Leistung ausgelegt ist.)
- Bereitstellen von Modellen, die mit R und Python in Azure Machine Learning erstellt wurden, damit Clientanwendungen über eine einfache Webdienstschnittstelle auf Ihre Modelle zugreifen können
- Verwalten Ihrer Azure-Ressourcen über PowerShell oder das Azure-Portal
- Erweitern Ihres Speicherplatzes und Freigeben von umfangreichen Datasets/Codes für Ihr gesamtes Team durch Erstellen einer Azure Files-Freigabe als bereitstellbares Laufwerk auf Ihrer DSVM
- Freigeben von Code für Ihr Team über GitHub Zugreifen auf Ihr Repository über die vorinstallierten Git-Clients: Git Bash und Git GUI
- Zugriff auf Azure-Daten- und -Analysedienste wie Azure Blob Storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse und Azure SQL-Datenbank
- Erstellen von Berichten und eines Dashboards mithilfe der Power BI Desktop-Instanz, die auf der DSVM vorinstalliert ist, und Bereitstellen der Berichte und des Dashboards in der Cloud
- Dynamisches Skalieren Ihrer DSVM, damit Ihre Projektanforderungen erfüllt werden
- Installieren zusätzlicher Tools auf Ihrem virtuellen Computer   

> [!NOTE]
> Für viele der in diesem Artikel aufgelisteten Datenspeicher- und Analysedienste fallen zusätzliche Nutzungsgebühren an. Details finden Sie auf der Seite [Azure-Preise](https://azure.microsoft.com/pricing/).
> 
> 

## <a name="prerequisites"></a>Voraussetzungen

* Sie benötigen ein Azure-Abonnement. Sie können sich für eine [kostenlose Testversion](https://azure.microsoft.com/free/)registrieren.
* Anleitungen für die Bereitstellung einer Data Science Virtual Machine-Instanz im Azure-Portal finden Sie in [Erstellen eines virtuellen Computers](https://portal.azure.com/#create/microsoft-dsvm.dsvm-windowsserver-2016).


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="explore-data-and-develop-models-with-microsoft-machine-learning-server"></a>Auswerten von Daten und Entwickeln von Modellen mit Microsoft Machine Learning Server
Sie können Ihre Datenanalysen mit Sprachen wie R und Python direkt auf der DSVM ausführen.

Für R können Sie eine integrierte Entwicklungsumgebung wie RStudio verwenden, die im Startmenü oder auf dem Desktop zu finden ist. Alternativ können Sie R Tools für Visual Studio verwenden. Microsoft stellt zusätzliche Bibliotheken auf Basis der Open-Source-Sprache R (verfügbar über CRAN) bereit, um skalierbare Analysen und das Analysieren von Datenmengen, deren Größe den Arbeitsspeicher überschreitet, in parallelen Datenblöcken zu ermöglichen. 

Für Python können Sie eine IDE wie Visual Studio Community Edition verwenden, bei der die PTVS-Erweiterung (Python Tools for Visual Studio) vorinstalliert ist. Standardmäßig ist für PTVS nur Python 3.6 (Root-Conda-Umgebung) konfiguriert. Führen Sie die folgenden Schritte aus, um Anaconda Python 2.7 zu aktivieren:

1. Erstellen Sie benutzerdefinierte Umgebungen für jede Version, indem Sie in Visual Studio Community Edition zu **Extras** > **Python** > **Python-Umgebungen** navigieren und dann **+ Benutzerdefiniert** auswählen.
1. Geben Sie eine Beschreibung ein, und legen Sie den Umgebungspräfixpfad für Anaconda Python 2.7 auf **c:\anaconda\envs\python2** fest.
1. Wählen Sie **Automatische Erkennung** > **Anwenden** aus, um die Umgebung zu speichern.

In der [PTVS-Dokumentation](https://aka.ms/ptvsdocs) finden Sie weitere Informationen zum Erstellen der Python-Umgebungen.

Nun können Sie ein neues Python-Projekt erstellen. Wechseln Sie zu **Datei** > **Neu** > **Projekt** > **Python-Anwendung**, und wählen Sie die Python-Anwendung aus, die sie erstellen. Sie können die Python-Umgebung für das aktuelle Projekt auf die gewünschte Version (Python 2.7 oder 3.6) festlegen, indem Sie mit der rechten Maustaste auf **Python-Umgebungen** klicken und dann **Python-Umgebungen hinzufügen/entfernen** auswählen. Weitere Informationen zum Arbeiten mit PTVS finden Sie in der [Dokumentation](https://aka.ms/ptvsdocs) des Produkts.

## <a name="use-jupyter-notebooks"></a>Verwenden von Jupyter-Notebooks
Jupyter Notebook stellt eine browserbasierte integrierte Entwicklungsumgebung für Datenauswertung und -modellierung bereit. Sie können Python 2, Python 3 oder R (sowohl Open Source als auch Microsoft R Server) in einem Jupyter-Notebook verwenden.

Um das Jupyter-Notebook zu starten, wählen Sie das **Jupyter Notebook**-Symbol im **Startmenü** oder auf dem Desktop aus. An der DSVM-Eingabeaufforderung können Sie auch den Befehl ```jupyter notebook``` in dem Verzeichnis ausführen, in dem Notebooks vorhanden sind oder in dem Sie neue Notebooks erstellen möchten.  

Nachdem Sie Jupyter gestartet haben, sollte ein Verzeichnis angezeigt werden, das einige Beispielnotebooks enthält, die in der DSVM vorkonfiguriert sind. Sie können jetzt:

* Das Notebook auswählen, um den Code anzuzeigen.
* Jede Zelle ausführen, indem Sie UMSCHALT+EINGABETASTE drücken.
* Das gesamte Notebook ausführen, indem Sie **Zelle** > **Ausführen** auswählen.
* Ein neues Notebook erstellen, indem Sie das Jupyter-Symbol auswählen (linke obere Ecke), auf der rechten Seite die Schaltfläche **Neu** auswählen und schließlich die Notebook-Sprache (auch als Kernels bezeichnet) auswählen.   

> [!NOTE]
> Derzeit werden in Jupyter die Kernel Python 2.7, Python 3.6, R, Julia und PySpark unterstützt. Der R-Kernel unterstützt Programmieren sowohl in Open-Source-R als auch in Microsoft R.   
> 
> 

Nachdem Sie das Notebook geöffnet haben, können Sie mit den Bibliotheken Ihrer Wahl Ihre Daten auswerten, das Modell erstellen und das Modell testen.

## <a name="train-and-deploy-models-by-using-azure-machine-learning"></a>Trainieren und Bereitstellen von Modellen über Azure Machine Learning
Nachdem Sie Ihr Modell erstellt und überprüft haben, ist der nächste Schritt in der Regel die Bereitstellung des Modells für die Produktion. Dieser Schritt ermöglicht es Ihren Clientanwendungen, die Modellvorhersagen auf einer Echtzeitbasis oder einer Batchmodusbasis aufzurufen. Azure Machine Learning bietet einen Mechanismus zum Operationalisieren eines in R oder Python erstellten Modells.

Wenn Sie das Modell in Azure Machine Learning operationalisieren, wird ein Webdienst verfügbar gemacht. Dieser ermöglicht Clients das Ausführen von REST-Aufrufen, in denen Eingabeparameter übergeben und Vorhersagen aus dem Modell als Ausgaben empfangen werden.

### <a name="build-and-operationalize-python-models"></a>Erstellen und Operationalisieren von Python-Modellen
Hier sehen Sie einen Ausschnitt eines in einem Python Jupyter-Notebook entwickelten Codes, der durch Verwenden der Scikit-learn-Bibliothek ein einfaches Modell erstellt:

```python
# IRIS classification
from sklearn import datasets
from sklearn import svm
clf = svm.SVC()
iris = datasets.load_iris()
X, y = iris.data, iris.target
clf.fit(X, y)
```

Die Methode, die zum Bereitstellen Ihrer Python-Modelle in Azure Machine Learning verwendet wird, umschließt die Vorhersage des Modells in eine Funktion und stattet sie mit Attributen aus, die von der vorinstallierten Azure Machine Learning-Python-Bibliothek bereitgestellt werden. Die Attribute geben Ihre Azure Machine Learning-Arbeitsbereichs-ID, den API-Schlüssel sowie die Eingabe- und Rückgabeparameter an.  

```python
from azureml import services
@services.publish(workspaceid, auth_token)
@services.types(sep_l=float, sep_w=float, pet_l=float, pet_w=float)
@services.returns(int)  # 0, or 1, or 2
def predictIris(sep_l, sep_w, pet_l, pet_w):
    inputArray = [sep_l, sep_w, pet_l, pet_w]


return clf.predict(inputArray)
```

Ein Client kann jetzt den Webdienst aufrufen. Benutzerfreundliche Wrapper erstellen die REST-API-Anforderungen. Hier ist ein Beispielcode zur Nutzung des Webdiensts:

```python
# Consume through web service URL and keys
from azureml import services
@services.service(url, api_key)
@services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
@services.returns(float)
def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
pass

IrisPredictor(3,2,3,4)
```

> [!NOTE]
> Derzeit wird die Azure Machine Learning-Bibliothek nur von Python 2.7 unterstützt.   
> 
> 

### <a name="build-and-operationalize-r-models"></a>Erstellen und Operationalisieren von R-Modellen
Sie können R-Modelle, die auf der Data Science Virtual Machine oder anderswo erstellt wurden, auf Azure Machine Learning in einer Weise bereitstellen, die derjenigen für Python ähnelt. Gehen Sie wie folgt vor:

1. Erstellen Sie eine „settings.json“-Datei, um Ihre Arbeitsbereichs-ID und Ihr Authentifizierungstoken bereitzustellen. 
2. Schreiben Sie einen Wrapper für die Vorhersagefunktion des Modells.
3. Rufen Sie ```publishWebService``` in der Azure Machine Learning-Bibliothek auf, um den Wrapper für die Funktion zu übergeben.  

Verwenden Sie die folgende Vorgehensweise und die folgenden Codeausschnitte, um ein Modell als Webdienst in Azure Machine Learning einzurichten, zu erstellen, zu veröffentlichen und zu nutzen.

#### <a name="set-up"></a>Einrichtung

Erstellen Sie eine „settings.json“-Datei in einem Verzeichnis namens ```.azureml```, das sich in Ihrem Basisverzeichnis befindet. Geben Sie die Parameter aus Ihrem Azure Machine Learning-Arbeitsbereich ein.

Die „settings.json“-Datei hat folgende Struktur:

```json
{"workspace":{
"id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
"authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
}}
```

#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a>Erstellen eines Modells in R und Veröffentlichen des Modells in Azure Machine Learning

```r
library(AzureML)
ws <- workspace(config="~/.azureml/settings.json")

if(!require("lme4")) install.packages("lme4")
library(lme4)
set.seed(1)
train <- sleepstudy[sample(nrow(sleepstudy), 120),]
m <- lm(Reaction ~ Days + Subject, data = train)

# Define a prediction function to publish based on the model:
sleepyPredict <- function(newdata){
        predict(m, newdata=newdata)
}

ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)
```

#### <a name="consume-the-model-deployed-in-azure-machine-learning"></a>Nutzen des bereitgestellten Modells in Azure Machine Learning
Um das Modell aus einer Clientanwendung zu nutzen, verwenden Sie die Azure Machine Learning-Bibliothek, um den veröffentlichten Webdienst anhand seines Namens nachzuschlagen. Verwenden Sie den API-Aufruf `services`, um den Endpunkt zu bestimmen. Dann rufen Sie einfach die Funktion `consume` auf und übergeben den Datenrahmen, der vorhergesagt werden soll.

Verwenden Sie den folgenden Code, um das als Azure Machine Learning-Webdienst veröffentlichte Modell zu nutzen:

```r
library(AzureML)
library(lme4)
ws <- workspace(config="~/.azureml/settings.json")

s <-  services(ws, name = "sleepy lm")
s <- tail(s, 1) # use the last published function, in case of duplicate function names

ep <- endpoints(ws, s)

# Try this out, and compare with raw data
ans = consume(ep, sleepstudy)$ans
```

Weitere Informationen finden Sie unter [R-Pakete, die von Azure Machine Learning Studio unterstützt werden](/azure/machine-learning/studio-module-reference/r-packages-supported-by-azure-machine-learning).

## <a name="manage-azure-resources"></a>Verwalten von Azure-Ressourcen
Die DSVM ermöglicht Ihnen nicht nur, ihre Analyselösung lokal auf dem virtuellen Computer zu erstellen, sondern ermöglicht Ihnen auch, auf Dienste auf der Azure-Cloudplattform zuzugreifen. Azure stellt mehrere Compute-, Speicher-, Datenanalyse- und weitere Dienste bereit, die Sie über Ihre DSVM verwalten und auf die aus Ihrer DSVM zugreifen können.

Um Ihr Azure-Abonnement und Ihre Cloudressourcen zu verwalten, haben Sie zwei Möglichkeiten:
+ Verwenden Sie Ihren Browser, und wechseln Sie zum [Azure-Portal](https://portal.azure.com).

+ Verwenden Sie PowerShell-Skripts. Führen Sie Azure PowerShell über eine Verknüpfung auf dem Desktop oder über das **Startmenü** aus. Ausführliche Informationen finden Sie in der [Microsoft Azure PowerShell-Dokumentation](../../powershell-azure-resource-manager.md). 

## <a name="extend-storage-by-using-shared-file-systems"></a>Erweitern von Speicher durch Verwenden von freigegebenen Dateisystemen
Datenanalysten können große Datasets, Code oder andere Ressourcen innerhalb des Teams freigeben. Für die DSVM sind etwa 45 GB Speicherplatz verfügbar. Um Ihren Speicher zu erweitern, können Sie eine Azure Files-Freigabe verwenden und diese in DSVM-Instanzen einbinden oder über eine REST-API auf diese zugreifen. Sie können auch das [Azure-Portal](../../virtual-machines/windows/attach-managed-disk-portal.md) oder [Azure PowerShell](../../virtual-machines/windows/attach-disk-ps.md) verwenden, um zusätzliche dedizierte Datenträger hinzuzufügen. 

> [!NOTE]
> Der maximale Speicherplatz auf der Azure Files-Freigabe umfasst 5 TB. Die Größenbeschränkung für jede Datei beträgt 1 TB. 

Sie können dieses Skript in Azure PowerShell verwenden, um eine Azure Files-Freigabe zu erstellen:

```powershell
# Authenticate to Azure.
Connect-AzAccount
# Select your subscription
Get-AzSubscription –SubscriptionName "<your subscription name>" | Select-AzSubscription
# Create a new resource group.
New-AzResourceGroup -Name <dsvmdatarg>
# Create a new storage account. You can reuse existing storage account if you want.
New-AzStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
# Set your current working storage account
Set-AzCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

# Create an Azure Files share
$s = New-AzStorageShare <<teamsharename>>
# Create a directory under the file share. You can give it any name
New-AzStorageDirectory -Share $s -Path <directory name>
# List the share to confirm that everything worked
Get-AzStorageFile -Share $s
```

Die neu erstellte Azure Files-Freigabe können Sie nun in jeden virtuellen Computer in Azure einbinden. Es empfiehlt sich, dass Sie den virtuellen Computer im selben Azure-Rechenzentrum wie das Speicherkonto anordnen, um Latenz und Datenübertragungsgebühren zu vermeiden. Mit den folgenden Azure PowerShell-Befehlen binden Sie das Laufwerk in die DSVM ein:

```powershell
# Get the storage key of the storage account that has the Azure Files share from the Azure portal. Store it securely on the VM to avoid being prompted in the next command.
cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

# Mount the Azure Files share as drive Z on the VM. You can choose another drive letter if you want.
net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>
```

Jetzt können Sie auf dieses Laufwerk wie auf jedes normale Laufwerk auf der VM zugreifen.

## <a name="share-code-in-github"></a>Teilen von Code in GitHub
GitHub ist ein Coderepository, in dem Sie Codebeispiele und Quellen für verschiedene Tools finden, indem Sie Technologien nutzen, die von der Entwicklercommunity geteilt werden. Sie nutzt Git als Technologie zum Nachverfolgen und speichern der Versionen der Codedateien. GitHub ist auch eine Plattform, auf der Sie Ihr eigenes Repository erstellen können, um freigegebenen Code und die Dokumentation Ihres Teams zu speichern. Zudem können Sie Versionskontrolle implementieren und steuern, wer Zugriffsrechte hat, um Code anzuzeigen und bereitzustellen. 

Auf den [GitHub-Hilfeseiten](https://help.github.com/) finden Sie weitere Informationen zur Verwendung von Git. Sie können GitHub als eine der Möglichkeiten nutzen, mit Ihrem Team zusammenzuarbeiten, von der Community entwickelten Code zu verwenden und wiederum Code für die Community bereitzustellen.

Die DSVM enthält Clienttools für die Befehlszeile und die GUI, um auf das GitHub-Repository zugreifen zu können. Das Befehlszeilentool, das für Git und GitHub funktioniert, heißt Git Bash. Visual Studio ist auf der DSVM installiert und hat die Git-Erweiterungen. Symbole für diese Tools finden Sie im **Startmenü** und auf dem Desktop.

Zum Herunterladen von Code aus einem GitHub-Repository verwenden Sie den Befehl ```git clone```. Um beispielsweise das von Microsoft veröffentlichte Data Science-Repository in das aktuelle Verzeichnis herunterzuladen, können Sie den folgenden Befehl in Git Bash ausführen:

    git clone https://github.com/Azure/DataScienceVM.git

In Visual Studio können Sie den gleichen Klonvorgang ausführen. Im folgenden Screenshot sehen Sie, wie Sie in Visual Studio auf Git- und GitHub-Tools zugreifen können:

![Screenshot von Visual Studio mit angezeigter GitHub-Verbindung](./media/vm-do-ten-things/VSGit.PNG)

Weitere Informationen zur Verwendung von Git zum Arbeiten mit Ihrem GitHub-Repository finden Sie in Ressourcen, die unter „github.com“ verfügbar sind. Der [Spickzettel](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf) ist eine nützliche Referenz.

## <a name="access-azure-data-and-analytics-services"></a>Zugreifen auf Azure-Daten und -Analysedienste
### <a name="azure-blob-storage"></a>Azure Blob Storage
Azure Blob Storage ist ein zuverlässiger, wirtschaftlicher Cloudspeicherdienst für große und kleine Datenmengen. In diesem Abschnitt ist beschrieben, wie Sie Daten in Blobspeicher verschieben und auf Daten zugreifen können, die in einem Azure-Blob gespeichert sind.

#### <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie Ihr Azure Blob Storage-Konto über das [Azure-Portal](https://portal.azure.com).

   ![Screenshot des Erstellungsvorgangs des Speicherkontos im Azure-Portal](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Vergewissern Sie sich, dass das Befehlszeilentool AzCopy bereits installiert ist: ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```. Das Verzeichnis, in dem sich „azcopy.exe“ befindet, ist bereits in Ihrer PATH-Umgebungsvariablen enthalten, sodass Sie nicht den gesamten Befehlspfad eingeben müssen, wenn Sie dieses Tool ausführen möchten. Weitere Informationen zum Tool AzCopy finden Sie in der [AzCopy-Dokumentation](../../storage/common/storage-use-azcopy.md).
* Starten Sie das Tool Azure Storage-Explorer. Sie können es von der [Storage-Explorer-Webseite](https://storageexplorer.com/) herunterladen. 

   ![Screenshot von Azure Storage-Explorer beim Zugreifen auf ein Speicherkonto](./media/vm-do-ten-things/AzureStorageExplorer_v4.png)

#### <a name="move-data-from-a-vm-to-an-azure-blob-azcopy"></a>Verschieben von Daten von einem virtuellen Computer in ein Azure-Blob: AzCopy

Um Daten zwischen Ihren lokalen Dateien und Blobspeicher zu verschieben, können Sie AzCopy in der Befehlszeile oder in PowerShell verwenden:

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

Ersetzen Sie **C:\myfolder** durch den Pfad, in dem Ihre Datei gespeichert ist, **mystorageaccount** durch Ihren Blobspeicher-Kontonamen, **mycontainer** durch den Containernamen und **storage account key** durch Ihren Blobspeicher-Zugriffsschlüssel. Sie finden die Anmeldeinformationen für Ihr Speicherkonto im [Azure-Portal](https://portal.azure.com).

Führen Sie den AzCopy-Befehl in PowerShell oder an einer Eingabeaufforderung aus. Hier sehen Sie ein Beispiel für die Nutzung des AzCopy-Befehls:

```powershell
# Copy *.sql from a local machine to an Azure blob
"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

# Copy back all files from an Azure blob container to a local machine

"C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S
```

Nachdem Sie den AzCopy-Befehl ausgeführt haben, um in ein Azure-Blob zu kopieren, wird Ihre Datei in Azure Storage-Explorer angezeigt.

![Screenshot des Speicherkontos, das die hochgeladene CSV-Datei anzeigt](./media/vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

#### <a name="move-data-from-a-vm-to-an-azure-blob-azure-storage-explorer"></a>Verschieben von Daten von einem virtuellen Computer in ein Azure-Blob: Azure Storage-Explorer

Außerdem können Sie Daten aus der lokalen Datei mit Azure Storage-Explorer auf Ihren virtuellen Computer hochladen:

* Um Daten in einen Container hochzuladen, wählen Sie den Zielcontainer aus, und wählen Sie die Schaltfläche **Hochladen**aus. ![Screenshot der Schaltfläche zum Hochladen in Azure Storage-Explorer](./media/vm-do-ten-things/storage-accounts.png)
* Wählen Sie die Auslassungspunkte ( **...** ) rechts neben dem Feld **Dateien** aus, wählen Sie im Dateisystem mindestens eine Datei zum Hochladen aus, und wählen Sie **Hochladen** aus, um mit dem Hochladen der Dateien zu beginnen. ![Screenshot des Dialogfelds „Dateien hochladen“](./media/vm-do-ten-things/upload-files-to-blob.png)

#### <a name="read-data-from-an-azure-blob-machine-learning-reader-module"></a>Lesen von Daten aus einem Azure-Blob: Machine Learning-Readermodul

In Azure Machine Learning Studio können Sie das Modul für Importieren von Daten verwenden, um Daten aus Ihrem Blob zu lesen.

![Screenshot des Moduls „Daten importieren“ in Machine Learning Studio](./media/vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

#### <a name="read-data-from-an-azure-blob-python-odbc"></a>Lesen von Daten aus einem Azure-Blob: Python ODBC

Sie können die BlobService-Bibliothek verwenden, um Daten direkt aus einem Blob in ein Jupyter-Notebook oder in ein Python-Programm zu lesen.

Importieren Sie zunächst die erforderlichen Pakete:

```python
import pandas as pd
from pandas import Series, DataFrame
import numpy as np
import matplotlib.pyplot as plt
from time import time
import pyodbc
import os
from azure.storage.blob import BlobService
import tables
import time
import zipfile
import random
```

Geben Sie dann Ihre Anmeldeinformationen für das Azure-Blobspeicherkonto an, und lesen Sie Daten aus dem Blob:

```python
CONTAINERNAME = 'xxx'
STORAGEACCOUNTNAME = 'xxxx'
STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
localfilename = 'trip_data_1.csv'
LOCALDIRECTORY = os.getcwd()
LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

#download from blob
t1 = time.time()
blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
t2 = time.time()
print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))

#unzip downloaded files if needed
#with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
#    z.extractall(LOCALDIRECTORY)

df1 = pd.read_csv(LOCALFILE, header=0)
df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
print 'the size of the data is: %d rows and  %d columns' % df1.shape
```

Die Daten werden als Datenrahmen gelesen:

![Screenshot der ersten 10 Datenzeilen](./media/vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a>Azure Data Lake
Azure Data Lake Store ist ein Hyperscale-Repository für Big Data-Analyseworkloads und ist mit dem Hadoop Distributed File System (HDFS) kompatibel. Das Repository funktioniert mit Hadoop, Spark und Azure Data Lake Analytics. In diesem Abschnitt ist beschrieben, wie Sie Daten nach Azure Data Lake Storage verschieben und Analysen über Azure Data Lake Analytics ausführen.

#### <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie Ihre Azure Data Lake Analytics-Instanz im [Azure-Portal](https://portal.azure.com).

   ![Screenshot zum Erstellen einer Data Lake Analytics-Instanz im Azure-Portal](./media/vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* Das [Microsoft Azure Data Lake and Stream Analytics Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)-Plug-In ist bereits in der Visual Studio Community Edition auf dem virtuellen Computer installiert. Nachdem Sie Visual Studio gestartet und sich bei Ihrem Azure-Abonnement angemeldet haben, sollten Ihr Azure Data Analytics-Konto und der zugehörige Speicher im linken Bereich von Visual Studio angezeigt werden.

   ![Screenshot des Plug-Ins für Data Lake-Tools in Visual Studio](./media/vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

#### <a name="move-data-from-a-vm-to-data-lake-azure-data-lake-explorer"></a>Verschieben von Daten von einem virtuellen Computer nach Data Lake: Azure Data Lake-Explorer

Sie können Azure Data Lake Explorer verwenden, um [Daten aus den lokalen Dateien auf Ihrem virtuellen Computer nach Data Lake Storage hochzuladen](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal).

Sie können auch eine Datenpipeline erstellen, um die Datenverschiebung zu oder von Azure Data Lake durch Verwenden von [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) zu operationalisieren. In [diesem Artikel](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) werden Sie durch die Schritte zum Erstellen der Datenpipelines geführt.

#### <a name="read-data-from-an-azure-blob-to-data-lake-u-sql"></a>Einlesen von Daten aus einem Azure-Blob in Data Lake: U-SQL

Wenn Ihre Daten sich in Azure-Blobspeicher befinden, können Sie die Daten mit einer U-SQL-Abfrage direkt aus einem Azure-Blob lesen. Stellen Sie vor dem Erstellen Ihrer U-SQL-Abfrage sicher, dass Ihr Blobspeicherkonto mit Ihrer Azure Data Lake-Instanz verknüpft ist. Wechseln Sie zum Azure-Portal, öffnen Sie Ihr Azure Data Lake Analytics-Dashboard, wählen Sie **Datenquelle hinzufügen** aus, wählen Sie den Speichertyp **Azure Storage** aus, und geben Sie den Namen und den Schlüssel Ihres Azure-Speicherkontos ein. Danach können Sie auf die Daten verweisen, die im Speicherkonto gespeichert sind.

![Screenshot des Dialogfelds „Datenquelle hinzufügen“](./media/vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

In Visual Studio können Sie Daten aus Blobspeicher lesen, Daten bearbeiten, Features entwickeln und die resultierenden Daten entweder an Azure Data Lake oder an Azure-Blobspeicher senden. Wenn Sie auf die Daten in Blobspeicher verweisen, verwenden Sie **wasb://** . Wenn Sie auf die Daten in Azure Data Lake verweisen, verwenden Sie **swbhdfs://** .

Sie können die folgenden U-SQL-Abfragen in Visual Studio verwenden:

```usql
@a =
    EXTRACT medallion string,
            hack_license string,
            vendor_id string,
            rate_code string,
            store_and_fwd_flag string,
            pickup_datetime string,
            dropoff_datetime string,
            passenger_count int,
            trip_time_in_secs double,
            trip_distance double,
            pickup_longitude string,
            pickup_latitude string,
            dropoff_longitude string,
            dropoff_latitude string

    FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
    USING Extractors.Csv();

@b =
    SELECT vendor_id,
    COUNT(medallion) AS cnt_medallion,
    SUM(passenger_count) AS cnt_passenger,
    AVG(trip_distance) AS avg_trip_dist,
    MIN(trip_distance) AS min_trip_dist,
    MAX(trip_distance) AS max_trip_dist,
    AVG(trip_time_in_secs) AS avg_trip_time
    FROM @a
    GROUP BY vendor_id;

OUTPUT @b   
TO "swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
USING Outputters.Csv();

OUTPUT @b   
TO "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
USING Outputters.Csv();
```

Nachdem Ihre Abfrage an den Server gesendet wurde, wird der Status Ihres Auftrags in einem Diagramm angezeigt.

![Screenshot des Auftragsstatusdiagramms](./media/vm-do-ten-things/USQL_Job_Status.PNG)

#### <a name="query-data-in-data-lake-u-sql"></a>Abfragen von Daten in Data Lake: U-SQL

Nachdem das Dataset in Azure Data Lake erfasst wurde, können Sie die [U-SQL-Sprache](../../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) verwenden, um die Daten abzufragen und auszuwerten. Die U-SQL-Sprache ähnelt T-SQL, kombiniert aber einige Features aus C#, sodass Benutzer angepasste Module und benutzerdefinierte Funktionen schreiben können. Sie können die Skripts aus dem vorherigen Schritt verwenden.

Nachdem die Abfrage an den Server übermittelt wurde, wird „tripdata_summary.CSV“ wird in Azure Data Lake Explorer angezeigt. Sie können eine Vorschau der Daten anzeigen, indem Sie mit der rechten Maustaste auf die Datei klicken.

![Screenshot der CSV-Datei in Data Lake Explorer](./media/vm-do-ten-things/USQL_create_summary.png)

Die Dateiinformationen werden angezeigt:

![Screenshot der Zusammenfassungsdateiinformationen](./media/vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a>HDInsight Hadoop-Cluster
Azure HDInsight ist ein verwalteter Apache Hadoop-, Spark-, HBase- und Storm-Dienst in der Cloud. Sie können problemlos mit Azure HDInsight-Clustern der Data Science Virtual Machine arbeiten.

#### <a name="prerequisites"></a>Voraussetzungen

* Erstellen Sie Ihr Azure Blob Storage-Konto über das [Azure-Portal](https://portal.azure.com). Dieses Speicherkonto wird zum Speichern von Daten für HDInsight-Cluster verwendet.

   ![Screenshot zum Erstellen eines Speicherkontos im Azure-Portal](./media/vm-do-ten-things/Create_Azure_Blob.PNG)

* Passen Sie Azure HDInsight Hadoop-Cluster über das [Azure-Portal](../team-data-science-process/customize-hadoop-cluster.md) an.
  
   Verknüpfen Sie das erstellte Konto mit Ihrem HDInsight-Cluster, wenn dieser erstellt wird. Mit diesem Speicherkonto wird auf Daten zugegriffen, die innerhalb des Clusters verarbeitet werden können.

   ![Ausgewählte Optionen zum Verknüpfen des erstellten Speicherkontos mit einem HDInsight-Cluster](./media/vm-do-ten-things/Create_HDI_v4.PNG)

* Aktivieren Sie Remotedesktopzugriff auf den Hauptknoten des Clusters, nachdem dieser erstellt wurde. Notieren Sie sich die Remotezugriff-Anmeldeinformationen, die Sie hier angegeben haben, weil Sie diese in den späteren Schritten benötigen.

   ![Schaltfläche „Remotedesktop“ zum Aktivieren von Remotezugriff auf den HDInsight-Cluster](./media/vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* Erstellen Sie einen Azure Machine Learning-Arbeitsbereich. Ihre Machine Learning-Experimente werden in diesem Machine Learning-Arbeitsbereich gespeichert. Wählen Sie die hervorgehobenen Optionen im Portal aus, wie im folgenden Screenshot dargestellt:

   ![Erstellen eines Azure Machine Learning-Arbeitsbereichs](./media/vm-do-ten-things/Create_ML_Space.PNG)

* Geben Sie die Parameter für Ihren Arbeitsbereich ein.

   ![Eingeben der Parameter für den Machine Learning-Arbeitsbereich](./media/vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* Laden Sie Daten mit IPython Notebook hoch. Importieren Sie die erforderlichen Pakete, geben Sie die Anmeldeinformationen ein, erstellen Sie eine Datenbank in Ihrem Speicherkonto, und laden Sie dann Daten in die HDI-Cluster.

```python
# Import required packages
import pyodbc
import time as time
import json
import os
import urllib
import urllib2
import warnings
import re
import pandas as pd
import matplotlib.pyplot as plt
from azure.storage.blob import BlobService
warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


# Create the connection to Hive by using ODBC
SERVER_NAME = 'xxx.azurehdinsight.net'
DATABASE_NAME = 'nyctaxidb'
USERID = 'xxx'
PASSWORD = 'xxxx'
DB_DRIVER = 'Microsoft Hive ODBC Driver'
driver = 'DRIVER={' + DB_DRIVER + '}'
server = 'Host=' + SERVER_NAME + ';Port=443'
database = 'Schema=' + DATABASE_NAME
hiveserv = 'HiveServerType=2'
auth = 'AuthMech=6'
uid = 'UID=' + USERID
pwd = 'PWD=' + PASSWORD
CONNECTION_STRING = ';'.join(
    [driver, server, database, hiveserv, auth, uid, pwd])
connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
cursor = connection.cursor()


# Create the Hive database and tables
queryString = "create database if not exists nyctaxidb;"
cursor.execute(queryString)

queryString = """
                create external table if not exists nyctaxidb.trip
                (
                    medallion string,
                    hack_license string,
                    vendor_id string,
                    rate_code string,
                    store_and_fwd_flag string,
                    pickup_datetime string,
                    dropoff_datetime string,
                    passenger_count int,
                    trip_time_in_secs double,
                    trip_distance double,
                    pickup_longitude double,
                    pickup_latitude double,
                    dropoff_longitude double,
                    dropoff_latitude double)  
                PARTITIONED BY (month int)
                ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
            """
cursor.execute(queryString)

queryString = """
                create external table if not exists nyctaxidb.fare
                (
                    medallion string,
                    hack_license string,
                    vendor_id string,
                    pickup_datetime string,
                    payment_type string,
                    fare_amount double,
                    surcharge double,
                    mta_tax double,
                    tip_amount double,
                    tolls_amount double,
                    total_amount double)
                PARTITIONED BY (month int)
                ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
            """
cursor.execute(queryString)


# Upload data from Blob storage to an HDI cluster
for i in range(1, 13):
    queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);" % (
        i, i)
    cursor.execute(queryString)
    queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);" % (
        i, i)
    cursor.execute(queryString)
```

Alternativ dazu können Sie auch dieser [exemplarischen Vorgehensweise](../team-data-science-process/hive-walkthrough.md) folgen, um „NYC Taxi“-Daten in den HDI-Cluster hochzuladen. Zu den wichtigsten Schritten zählen:
  
* Verwenden von AzCopy, um gezippte CSV-Dateien aus dem öffentlichen Blob in Ihren lokalen Ordner herunterzuladen
* Verwenden von AzCopy, um entzippte CSV-Dateien aus dem lokalen Ordner in einen HDI-Cluster hochzuladen
* Anmelden beim Hauptknoten eines Hadoop-Clusters und Vorbereiten einer explorativen Datenanalyse

Nachdem die Daten in den HDI-Cluster geladen sind, können Sie Ihre Daten in Azure Storage-Explorer überprüfen. Außerdem wurde die Datenbank „nyctaxidb“ im HDI-Cluster erstellt.

#### <a name="data-exploration-hive-queries-in-python"></a>Untersuchen von Daten: Hive-Abfragen in Python

Da sich die Daten im Hadoop-Cluster befinden, können Sie das Paket „pyodbc“ verwenden, um Verbindungen mit Hadoop-Clustern herzustellen und Datenbanken abzufragen, indem Sie Hive verwenden, um Auswertungen und Featureentwicklung durchzuführen. Sie können die vorhandenen Tabellen anzeigen, die Sie im Schritt „Voraussetzungen“ erstellt haben.

```python
queryString = """
    show tables in nyctaxidb2;
    """
pd.read_sql(queryString, connection)
```

![Anzeigen vorhandener Tabellen](./media/vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

Betrachten wir nun die Anzahl der Datensätze in jedem Monat, und wie häufig Trinkgeld gegeben wurde, in der Fahrttabelle:

```python
queryString = """
    select month, count(*) from nyctaxidb.trip group by month;
    """
results = pd.read_sql(queryString,connection)

%matplotlib inline

results.columns = ['month', 'trip_count']
df = results.copy()
df.index = df['month']
df['trip_count'].plot(kind='bar')
```

![Darstellung der Anzahl von Datensätzen in den einzelnen Monaten](./media/vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

```python
queryString = """
    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;
    """
results = pd.read_sql(queryString, connection)

results.columns = ['tipped', 'trip_count']
df = results.copy()
df.index = df['tipped']
df['trip_count'].plot(kind='bar')
```

![Darstellung der Trinkgeldhäufigkeit](./media/vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

Außerdem können Sie die Entfernung zwischen dem Aufnahmestandort und dem Absetzstandort berechnen und sie dann mit der Fahrtstrecke vergleichen.

```python
queryString = """
                select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                    3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                    *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                    *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                    /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                    +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                    pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                    from nyctaxidb.trip
                    where month=1
                        and pickup_longitude between -90 and -30
                        and pickup_latitude between 30 and 90
                        and dropoff_longitude between -90 and -30
                        and dropoff_latitude between 30 and 90;
            """
results = pd.read_sql(queryString, connection)
results.head(5)
```

![Oberste Zeilen der Aufnahme- und Absetztabelle](./media/vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

```python
results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                   'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
df = results.loc[results['trip_distance'] <= 100]  # remove outliers
df = df.loc[df['direct_distance'] <= 100]  # remove outliers
plt.scatter(df['direct_distance'], df['trip_distance'])
```

![Darstellung von Aufnahme/Absetz-Entfernung zu Fahrtstrecke](./media/vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

Es soll nun eine mit Downsampling (1 Prozent) erstellte Datenmenge zur Modellierung vorbereitet werden. Sie können diese Daten im Machine Learning-Readermodul verwenden.

```python
queryString = """
create  table if not exists nyctaxi_downsampled_dataset_testNEW (
medallion string,
hack_license string,
vendor_id string,
rate_code string,
store_and_fwd_flag string,
pickup_datetime string,
dropoff_datetime string,
pickup_hour string,
pickup_week string,
weekday string,
passenger_count int,
trip_time_in_secs double,
trip_distance double,
pickup_longitude double,
pickup_latitude double,
dropoff_longitude double,
dropoff_latitude double,
direct_distance double,
payment_type string,
fare_amount double,
surcharge double,
mta_tax double,
tip_amount double,
tolls_amount double,
total_amount double,
tipped string,
tip_class string
)
row format delimited fields terminated by ','
lines terminated by '\\n'
stored as textfile;
"""
cursor.execute(queryString)
```

Fügen Sie nun die Inhalte des Joins in die vorstehende interne Tabelle ein.

```python
queryString = """
insert overwrite table nyctaxi_downsampled_dataset_testNEW
select
t.medallion,
t.hack_license,
t.vendor_id,
t.rate_code,
t.store_and_fwd_flag,
t.pickup_datetime,
t.dropoff_datetime,
hour(t.pickup_datetime) as pickup_hour,
weekofyear(t.pickup_datetime) as pickup_week,
from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
t.passenger_count,
t.trip_time_in_secs,
t.trip_distance,
t.pickup_longitude,
t.pickup_latitude,
t.dropoff_longitude,
t.dropoff_latitude,
t.direct_distance,
f.payment_type,
f.fare_amount,
f.surcharge,
f.mta_tax,
f.tip_amount,
f.tolls_amount,
f.total_amount,
if(tip_amount>0,1,0) as tipped,
if(tip_amount=0,0,
if(tip_amount>0 and tip_amount<=5,1,
if(tip_amount>5 and tip_amount<=10,2,
if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
from
(
select
medallion,
hack_license,
vendor_id,
rate_code,
store_and_fwd_flag,
pickup_datetime,
dropoff_datetime,
passenger_count,
trip_time_in_secs,
trip_distance,
pickup_longitude,
pickup_latitude,
dropoff_longitude,
dropoff_latitude,
3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
/sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
+cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
rand() as sample_key

from trip
where pickup_latitude between 30 and 90
    and pickup_longitude between -90 and -30
    and dropoff_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
)t
join
(
select
medallion,
hack_license,
vendor_id,
pickup_datetime,
payment_type,
fare_amount,
surcharge,
mta_tax,
tip_amount,
tolls_amount,
total_amount
from fare
)f
on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
where t.sample_key<=0.01
"""
cursor.execute(queryString)
```

Nach einer Weile können Sie sehen, dass die Daten in Hadoop-Cluster geladen wurden:

```python
queryString = """
    select * from nyctaxi_downsampled_dataset limit 10;
    """
cursor.execute(queryString)
pd.read_sql(queryString, connection)
```

![Oberen Zeilen der Daten aus der Tabelle](./media/vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

#### <a name="read-data-from-hdi-by-using-azure-machine-learning-studio-classic-reader-module"></a>Lesen von Daten aus HDI über Azure Machine Learning Studio (klassisch): Readermodul

Sie können auch das Readermodul in Azure Machine Learning Studio (klassisch) verwenden, um auf die Datenbank in einem Hadoop-Cluster zuzugreifen. Geben Sie die Anmeldeinformationen für Ihre HDI-Cluster und Ihr Azure Storage-Konto an, um das Erstellen von Machine Learning-Modellen zu ermöglichen, indem eine Datenbank in HDI-Clustern verwendet wird.

![Eigenschaften des Readermoduls](./media/vm-do-ten-things/AML_Reader_Hive.PNG)

Sie können dann das bewertete Dataset anzeigen:

![Anzeigen des bewerteten Datasets](./media/vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse-and-databases"></a>Azure SQL Data Warehouse und Datenbanken
Azure SQL Data Warehouse ist ein elastisches Data Warehouse-as-a-Service-Angebot mit einer SQL Server-Umgebung für den Einsatz in Unternehmen.

Sie können Ihr Azure SQL Data Warehouse bereitstellen, indem Sie entsprechend den Anweisungen in [diesem Artikel](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) vorgehen. Nachdem Sie Ihr SQL Data Warehouse bereitgestellt haben, können Sie diese [exemplarische Vorgehensweise](../team-data-science-process/sqldw-walkthrough.md) verwenden, um Hochladen, Auswerten und Modellieren von Daten vorzunehmen, die sich im SQL Data Warehouse befinden.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB
Azure Cosmos DB ist eine NoSQL-Datenbank in der Cloud. Sie können diese Datenbank verwenden, um mit Dokumenten wie JSON zu arbeiten und die Dokumente zu speichern und abzufragen.

Führen Sie die folgenden erforderlichen Schritte aus, damit aus der DSVM auf Azure Cosmos DB zugegriffen werden kann:

1. Das Azure Cosmos DB Python SDK ist bereits auf der DSVM installiert. Um es zu aktualisieren, führen Sie ```pip install pydocumentdb --upgrade``` an einer Eingabeaufforderung aus.
2. Erstellen Sie über das [Azure-Portal](https://portal.azure.com) ein Azure Cosmos DB-Konto und eine Azure Cosmos DB-Datenbank.
3. Laden Sie das Datenmigrationstool für Azure Cosmos DB aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) herunter, und extrahieren Sie es in das gewünschte Verzeichnis.
4. Importieren Sie JSON-Daten (Vulkandaten), die in einem [öffentlichen Blob](https://cahandson.blob.core.windows.net/samples/volcano.json) gespeichert sind, in Azure Cosmos DB. Verwenden Sie dazu die folgenden Befehlsparameter für das Migrationstool. (Verwenden Sie „dtui. exe“ aus dem Verzeichnis, in dem Sie das Datenmigrationstool für Azure Cosmos DB installiert haben.) Geben Sie die Quell-und Zielposition mit diesen Parametern ein:
   
    `/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1`

Nachdem Sie die Daten importiert haben, können Sie zu Jupyter wechseln und das Notebook *DocumentDBSample* öffnen. Es enthält Python-Code, mit dem auf Azure Cosmos DB zugegriffen wird und einige einfache Abfragen ausgeführt werden können. Weitere Informationen zu Azure Cosmos DB finden Sie auf der [Dokumentationsseite](https://docs.microsoft.com/azure/cosmos-db/) für den Dienst.

## <a name="use-power-bi-reports-and-dashboards"></a>Verwenden von Power BI-Berichten und -Dashboards 
Sie können die „Volcano JSON“-Datei aus dem vorherigen Azure Cosmos DB-Beispiel in Power BI Desktop visualisieren, um visuelle Einblicke in die Daten zu erhalten. Eine ausführliche Anleitung finden Sie im [Power BI-Artikel](../../cosmos-db/powerbi-visualize.md). Die allgemeinen Schritte sind folgende:

1. Öffnen Sie Power BI Desktop, und wählen Sie **Daten abrufen** aus. Geben Sie die URL wie folgt an: https://cahandson.blob.core.windows.net/samples/volcano.json.
2. Sie sollten die JSON-Datensätze in einer importierten Liste sehen. Konvertieren Sie die Liste in eine Tabelle, damit Power BI damit arbeiten kann.
4. Erweitern Sie die Spalten, indem Sie das Symbol für Erweitern (Pfeil) auswählen.
5. Beachten Sie, dass der Standort (location) ein **Record**-Feld (Datensatz-Feld) ist. Erweitern Sie den Datensatz und wählen Sie nur „coordinates“. **coordinates** ist eine Listenspalte.
6. Fügen Sie eine neue Spalte hinzu, um die Listenspalte mit den Koordinaten in eine durch Trennzeichen getrennte **LatLong**-Spalte zu konvertieren. Verketten Sie die beiden Elemente im Koordinatenlistenfeld mit der Formel ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.
7. Konvertieren Sie die **Elevation**-Spalte in eine Dezimalspalte, und wählen Sie die Schaltfläche **Schließen** und **Übernehmen** aus.

Anstelle der vorherigen Schritte können Sie den folgenden Code einfügen. In ihm liegen in Skriptform die Schritte vor, die im „Erweiterter Editor“ in Power BI verwendet werden, um die Datentransformationen in einer Abfragesprache zu schreiben.

```pqfl
let
    Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted to Table", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
    #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
    #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
    #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
in
    #"Changed Type"
```

Jetzt befinden sich die Daten in Ihrem Power BI-Datenmodell. Ihre Power BI Desktop-Instanz sollte folgendermaßen aussehen:

![Power BI Desktop](./media/vm-do-ten-things/PowerBIVolcanoData.png)

Sie können beginnen, Berichte und Visualisierungen mit dem Datenmodell zu erstellen. Sie können die in diesem [Power BI-Artikel](../../cosmos-db/powerbi-visualize.md#build-the-reports) beschriebenen Schritte verwenden, um einen Bericht zu erstellen.

## <a name="scale-the-dsvm-dynamically"></a>Dynamisches Skalieren der DSVM 
Sie können die DSVM gemäß Ihren Projektanforderungen zentral hoch- oder herunterskalieren. Wenn Sie die VM weder abends noch an Wochenenden verwenden, können Sie die VM über das [Azure-Portal](https://portal.azure.com) herunterfahren.

> [!NOTE]
> Es fallen Computegebühren an, wenn Sie nur die Schaltfläche zum Herunterfahren für das Betriebssystem auf der VM verwenden.  
> 
> 

Möglicherweise müssen Sie einige umfangreiche Analysen verarbeiten und benötigen mehr CPU-, Arbeitsspeicher- oder Datenträgerkapazität. Ist dies der Fall, können Sie unter verschiedenen VM-Größen in Bezug auf CPU-Kerne, GPU-basierte Instanzen für Deep Learning, Speicherkapazität und Datenträgertypen (einschließlich Solid-State-Laufwerken) wählen, die Ihre Compute- und Budgetanforderungen erfüllen. Die vollständige Liste der VMs zusammen mit deren Computepreisen pro Stunde finden Sie auf der Seite [Azure Virtual Machines – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/).

Umgekehrt kann es sein, dass sich Ihre Anforderungen hinsichtlich der VM-Verarbeitungskapazität verringern. (Beispiel: Sie haben eine Hauptworkload in einen Hadoop- oder Spark-Cluster verschoben.) Sie können den Cluster dann über das [Azure-Portal](https://portal.azure.com) zentral herunterskalieren und zu den Einstellungen Ihrer VM-Instanz wechseln. 

## <a name="add-more-tools"></a>Hinzufügen weiterer Tools
Mit Tools, die bereits in die DSVM integriert sind, können viele gängige Datenanalyseanforderungen erfüllt werden. Dadurch sparen Sie Zeit, weil Sie Ihre Umgebungen nicht eine nach der anderen installieren und konfigurieren müssen. Außerdem sparen Sie dadurch Geld, da Sie nur für Ressourcen bezahlen, die Sie verwenden.

Sie können weitere in diesem Artikel dargestellte Azure-Datendienste und -Analysedienste verwenden, um Ihre Analyseumgebung zu verbessern. In einigen Fällen benötigen Sie möglicherweise zusätzliche Tools, darunter auch einige proprietäre Partnertools. Sie haben vollen administrativen Zugriff auf den virtuellen Computer, um neue Tools zu installieren, die Sie benötigen. Sie können auch zusätzliche Pakete in Python und R installieren, die nicht vorinstalliert sind. Für Python können Sie entweder ```conda``` oder ```pip``` verwenden. Für R können Sie ```install.packages()``` in der R-Konsole verwenden, oder Sie verwenden die IDE und wählen **Pakete** > **Pakete installieren** aus.

## <a name="deep-learning"></a>Deep Learning

Zusätzlich zu den frameworkbasierten Beispielen können Sie auch eine Reihe umfassender exemplarischer Vorgehensweisen abrufen, die für die DSVM validiert sind. Diese Vorgehensweisen helfen Ihnen, Ihre Entwicklung von Deep Learning-Anwendungen in Bereichen wie Bild- und Text-/Sprachenverständnis zu beschleunigen.   


- [Betreiben neuronaler Netze in verschiedenen Frameworks](https://github.com/ilkarman/DeepLearningFrameworks): Diese exemplarische Vorgehensweise zeigt, wie Sie Code aus einem Framework zu einem anderen migrieren. Es veranschaulicht auch, wie Sie Modelle und Laufzeitleistung in verschiedenen Frameworks vergleichen. 

- [Eine Anleitung zum Erstellen einer End-to-End-Lösung zum Erkennen von Produkten in Bildern](https://github.com/Azure/cortana-intelligence-product-detection-from-images): Die Bilderkennung ist eine Technik, mit der Objekte innerhalb von Bildern lokalisiert und klassifiziert werden können. Diese Technologie hat das Potenzial, in vielen realen Geschäftsbereichen enorme Vorteile zu bringen. Mit dieser Technik können Einzelhändler beispielsweise feststellen, welches Produkt ein Kunde dem Regal entnommen hat. Diese Informationen wiederum helfen Filialen bei der Verwaltung des Warenbestands. 

- [Deep Learning for Audio](https://blogs.technet.microsoft.com/machinelearning/2018/01/30/hearing-ai-getting-started-with-deep-learning-for-audio-on-azure/): In diesem Tutorial wird gezeigt, wie Sie ein Deep Learning-Modell für die Erkennung von Audioereignissen für [Urban sound datasets](https://serv.cusp.nyu.edu/projects/urbansounddataset/urbansound8k.html) trainieren. Außerdem bietet es eine Übersicht darüber, wie mit Audiodaten gearbeitet wird.

- [Klassifizierung von Textdokumenten](https://github.com/anargyri/lstm_han): In dieser exemplarischen Vorgehensweise wird gezeigt, wie zwei Architekturen neuronaler Netze erstellt und trainiert werden: Hierarchical Attention Networks und LTSM-Netzwerke (Long Short Term Memory). Diese neuronalen Netzwerke verwenden zur Klassifizierung von Textdokumenten die Keras-API für Deep Learning. Keras ist ein Front-End für drei der am häufigsten verwendeten Deep Learning-Frameworks: Microsoft Cognitive Toolkit, TensorFlow und Theano.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel sind einige Dinge beschrieben, die mit der Microsoft Data Science Virtual Machine möglich sind. Sie können viele weitere Dinge tun, um die DSVM zu einer effizienten Analyseumgebung zu machen.

