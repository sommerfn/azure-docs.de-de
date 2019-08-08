---
title: Registrieren von Datenressourcen in Azure Data Catalog
description: 'Gewusst wie: Registrieren von Datenassets in Ihrer Azure Data Catalog-Instanz'
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: tutorial
ms.date: 08/01/2019
ms.openlocfilehash: 6dcf29c1bb98d15daf652671f31ed1647d66a81b
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68734901"
---
# <a name="tutorial-register-data-assets-in-azure-data-catalog"></a>Tutorial: Registrieren von Datenressourcen in Azure Data Catalog

In diesem Tutorial verwenden Sie das Registrierungstool, um Datenassets aus dem Azure SQL-Datenbank-Beispiel im Katalog zu registrieren. Bei der Registrierung werden wichtige strukturelle Metadaten (z.B. Namen, Typen und Speicherorte) aus der Datenquelle und des darin enthaltenen Assets extrahiert und in den Katalog kopiert. Die Datenquellen und ihre Daten bleiben an ihrem Ort, aber die Metadaten werden vom Katalog verwendet, um sie leichter ermittelbar und verständlich zu machen.

In diesem Tutorial lernen Sie Folgendes:
> [!div class="checklist"]
> * Registrieren von Datenassets 
> * Durchsuchen von Datenassets
> * Versehen von Datenassets mit Anmerkungen
> * Herstellen einer Verbindung mit Datenassets
> * Verwalten von Datenassets
> * Löschen von Datenassets

## <a name="prerequisites"></a>Voraussetzungen

Zunächst müssen Sie die [Schnellstartanleitung](register-data-assets-tutorial.md) befolgen.

* Ein [Microsoft Azure](https://azure.microsoft.com/)-Abonnement
* Sie müssen über einen eigenen [Azure Active Directory-Mandanten](../active-directory/fundamentals/active-directory-access-create-new-tenant.md) verfügen.

Um Data Catalog einzurichten, müssen Sie Besitzer oder Mitbesitzer eines Azure-Abonnements sein.

## <a name="register-data-assets"></a>Registrieren von Datenassets

### <a name="register-a-data-source"></a>Registrieren einer Datenquelle

Sie registrieren Datenassets (Tabellen) über ein [Azure SQL-Datenbank-Beispiel](../sql-database/sql-database-single-database-get-started.md). Sie können aber auch jede unterstützte Datenquelle nutzen, falls Sie es vorziehen, mit Daten zu arbeiten, die vertraut und für Ihre Rolle relevant sind. Eine Liste der unterstützten Datenquellen finden Sie unter [Von Azure Data Catalog unterstützte Datenquellen](data-catalog-dsr.md).

In diesem Tutorial verwenden wir als Name für die Azure SQL-Datenbank *RLSTest*.

Sie können Datenassets jetzt aus dem Azure SQL-Datenbank-Beispiel registrieren, indem Sie Azure Data Catalog verwenden.

1. Navigieren Sie zur [Azure Data Catalog-Startseite](http://azuredatacatalog.com), und wählen Sie die Option **Daten veröffentlichen**.

   ![Azure Data Catalog – Schaltfläche „Daten veröffentlichen“](media/register-data-assets-tutorial/data-catalog-publish-data.png)

2. Wählen Sie **Anwendung starten**, um das Registrierungstool auf Ihren Computer herunterzuladen, zu installieren und auszuführen.

   ![Azure Data Catalog – Schaltfläche „Starten“](media/register-data-assets-tutorial/data-catalog-launch-application.png)

3. Wählen Sie auf der Seite **Willkommen** die Option **Anmelden**, und geben Sie Ihre Anmeldeinformationen ein.

    ![Azure Data Catalog – Willkommensseite](media/register-data-assets-tutorial/data-catalog-welcome-dialog.png)

4. Wählen Sie auf der Seite **Microsoft Azure Data Catalog** die Option **SQL Server** und dann **Weiter**.

    ![Azure Data Catalog – Datenquellen](media/register-data-assets-tutorial/data-catalog-data-sources.png)

5. Geben Sie die SQL Server-Verbindungseigenschaften für Ihr Azure SQL-Datenbank-Beispiel ein, und wählen Sie **VERBINDEN**.

   ![Azure Data Catalog – Einstellungen für SQL Server-Verbindung](media/register-data-assets-tutorial/data-catalog-sql-server-connection.png)

6. Registrieren Sie die Metadaten Ihres Datenassets. In diesem Beispiel registrieren Sie **Product**-Objekte aus dem Namespace des Azure SQL-Datenbank-Beispiels:

    1. Erweitern Sie in der Struktur **Serverhierarchie** Ihr Azure SQL-Datenbank-Beispiel, und wählen Sie **SalesLT**.

    2. Wählen Sie bei gedrückter STRG-Taste nacheinander die Optionen **Product**, **ProductCategory**, **ProductDescription** und **ProductModel** aus.

    3. Wählen Sie den **Pfeil zum Verschieben der ausgewählten Elemente** ( **>** ). Mit dieser Aktion werden alle ausgewählten Objekte in die Liste **Zu registrierende Objekte** verschoben.

          ![Azure Data Catalog-Tutorial – Objekte durchsuchen und auswählen](media/register-data-assets-tutorial/data-catalog-server-hierarchy.png)

    4. Wählen Sie **Vorschau einschließen** aus, um eine Momentaufnahme der Daten einzubinden. Die Momentaufnahme enthält bis zu 20 Datensätze aus jeder Tabelle und wird in den Katalog kopiert.

    5. Wählen Sie **Datenprofil einschließen** aus, um eine Momentaufnahme der Objektstatistik für das Datenprofil einzubinden (z. B. Mindest-, Höchst- und Mittelwerte für eine Spalte, Anzahl von Zeilen).

    6. Geben Sie im Feld **Tags hinzufügen** die Tags **sales, product, azure sql** ein. Mit dieser Aktion werden Suchtags für diese Datenassets hinzugefügt. Tags sind eine hervorragende Möglichkeit, Benutzer beim Suchen nach einer registrierten Datenquelle zu unterstützen.

    7. Geben Sie den Namen eines **Experten** für diese Daten an (optional).

          ![Azure Data Catalog-Tutorial – zu registrierende Objekte](media/register-data-assets-tutorial/data-catalog-objects-register.png)

    8. Wählen Sie **REGISTRIEREN**. Azure Data Catalog registriert Ihre ausgewählten Objekte. In dieser Übung werden die ausgewählten Objekte aus Ihrem Azure SQL-Datenbank-Beispiel registriert. Das Registrierungstool extrahiert Metadaten aus dem Datenasset und kopiert diese Daten in den Azure Data Catalog-Dienst. Die Daten verbleiben an dem Ort, an dem sie sich gerade befinden. Die Daten bleiben unter der Kontrolle der Administratoren und Richtlinien des Ursprungssystems.

          ![Azure Data Catalog – registrierte Objekte](media/register-data-assets-tutorial/data-catalog-registered-objects.png)

    9. Wählen Sie zum Anzeigen der registrierten Datenquellenobjekte die Option **Portal anzeigen**. Vergewissern Sie sich im Azure Data Catalog-Portal, dass alle vier Tabellen und die Datenbank in der Rasteransicht angezeigt werden. (Stellen Sie sicher, dass die Suchleiste leer ist.)

        ![Objekte im Azure Data Catalog-Portal](media/register-data-assets-tutorial/data-catalog-view-portal.png)

In dieser Übung haben Sie Objekte aus dem Azure SQL-Datenbank-Beispiel registriert, sodass sie von allen Benutzern Ihrer Organisation leicht ermittelt werden können.

In der nächsten Übung erfahren Sie, wie Sie registrierte Datenassets ermitteln.

## <a name="discover-data-assets"></a>Ermitteln von Datenassets

Für die Ermittlung in Azure Data Catalog werden zwei primäre Mechanismen verwendet: Suchen und Filtern.

Die Suchfunktion ist intuitiv und leistungsstark. Suchbegriffe werden standardmäßig mit jeder Eigenschaft im Katalog abgeglichen, auch mit den von Benutzern hinzugefügten Anmerkungen.

Die Filterfunktion dient als Ergänzung der Suchfunktion. Sie können bestimmte Merkmale wie Experten, Datenquellentyp, Objekttyp und Tags auswählen, um übereinstimmende Datenassets anzuzeigen und Suchergebnisse auf übereinstimmende Assets zu begrenzen.

Mit einer Kombination aus Such- und Filteraktionen können Sie schnell durch die in Azure Data Catalog registrierten Datenquellen navigieren.

In dieser Übung verwenden Sie das Azure Data Catalog-Portal, um Datenassets zu ermitteln, die Sie in der vorherigen Übung registriert haben. Details zur Suchsyntax finden Sie unter [Data Catalog Search syntax reference](/rest/api/datacatalog/#search-syntax-reference) (Referenz zur Data Catalog-Suchsyntax).

Im Folgenden wird die Ermittlung von Datenassets im Katalog anhand einiger Beispiele erläutert.  

### <a name="discover-data-assets-with-basic-search"></a>Ermitteln von Datenassets mit der Standardsuche

Die Standardsuche ermöglicht Ihnen das Durchsuchen des Katalogs anhand eines oder mehrerer Suchbegriffe. Als Ergebnisse werden alle Ressourcen zurückgegeben, bei denen eine beliebige Eigenschaft mit den angegebenen Begriffen übereinstimmt.

1. Wählen Sie im Azure Data Catalog-Portal die Option **Start**. Wechseln Sie zur [Azure Data Catalog-Startseite](https://www.azuredatacatalog.com), falls Sie den Webbrowser geschlossen haben.

2. Geben Sie `product` in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.

    ![Azure Data Catalog – Standardtextsuche](media/register-data-assets-tutorial/data-catalog-basic-text-search.png)

3. Vergewissern Sie sich, dass alle vier Tabellen und die Datenbank in den Ergebnissen angezeigt werden. Sie können wie im folgenden Screenshot dargestellt mit den Schaltflächen auf der Symbolleiste zwischen der **Rasteransicht** und der **Listenansicht** wechseln. Das Suchschlüsselwort wird in den Suchergebnissen hervorgehoben, weil die Option **Hervorheben** **aktiviert** ist. Sie können auch die Anzahl von **Ergebnissen pro Seite** in den Suchergebnissen angeben.

    ![Azure Data Catalog – Ergebnisse der Standardtextsuche](media/register-data-assets-tutorial/data-catalog-basic-text-search-results.png)

    Links sehen Sie den Bereich **Suchvorgänge** und rechts den Bereich **Eigenschaften**. Im Bereich **Suchvorgänge** können Sie die Suchkriterien ändern und die Ergebnisse filtern. Im Bereich **Eigenschaften** werden die Eigenschaften eines ausgewählten Objekts im Raster oder in der Liste angezeigt.

4. Wählen Sie in den Suchergebnissen den Eintrag **Product** aus. Wählen Sie die Registerkarten **Vorschau**, **Spalten**, **Datenprofil** und **Dokumentation** oder den Pfeil aus, um den unteren Bereich zu erweitern.  

    ![Azure Data Catalog – unterer Bereich](media/register-data-assets-tutorial/data-catalog-data-asset-preview.png)

    Auf der Registerkarte **Vorschau** wird eine Vorschau der Daten in der Tabelle **Product** angezeigt.  
5. Wählen Sie die Registerkarte **Spalten**, um Details zu Spalten (z. B. **Name** und **Datentyp**) im Datenasset anzuzeigen.

6. Wählen Sie die Registerkarte **Datenprofil**, um die Profilerstellung von Daten (z. B. Anzahl von Zeilen, Größe der Daten, Mindestwert in einer Spalte) im Datenasset anzuzeigen.

### <a name="discover-data-assets-with-property-scoping"></a>Ermitteln von Datenassets mit der eigenschaftsbasierten Suche

Die eigenschaftsbasierte Suche ermöglicht Ihnen das Ermitteln von Datenassets, bei denen der Suchbegriff mit der angegebenen Eigenschaft übereinstimmt.

1. Deaktivieren Sie unter **Filter** für **Objekttyp** den Filter **Tabelle**.  

2. Geben Sie `tags:product` in das Suchfeld ein, und drücken Sie die **EINGABETASTE**. Alle Eigenschaften, die zum Durchsuchen des Datenkatalogs verwendet werden können, finden Sie unter [Data Catalog Search syntax reference](/rest/api/datacatalog/#search-syntax-reference) (Referenz zur Data Catalog-Suchsyntax).

3. Vergewissern Sie sich, dass die Tabellen und die Datenbank in den Ergebnissen angezeigt werden.  

    ![Data Catalog – Suchergebnisse der eigenschaftsbasierten Suche](media/register-data-assets-tutorial/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Speichern der Suche

1. Geben Sie im Bereich **Suchvorgänge** des Abschnitts **Aktuelle Suche** einen Namen für die Suche ein, und wählen Sie **Speichern**.

    ![Azure Data Catalog – Suche speichern](media/register-data-assets-tutorial/data-catalog-save-search.png)

2. Vergewissern Sie sich, dass die gespeicherte Suche unter **Gespeicherte Suchvorgänge**angezeigt wird.

3. Wählen Sie eine der Aktionen aus, die Sie für den gespeicherten Suchvorgang ausführen können (**Umbenennen**, **Löschen**, **Als Standard speichern**).

### <a name="grouping-with-parentheses"></a>Gruppierung mit Klammern

Durch die Gruppierung mit Klammern können Sie Teile der Abfrage gruppieren, um eine logische Isolierung zu erzielen (insbesondere bei booleschen Operatoren).

1. Geben Sie `name:product AND (tags:product AND objectType:table)` in das Suchfeld ein, und drücken Sie die **EINGABETASTE**.

2. Vergewissern Sie sich, dass in den Suchergebnissen nur die Tabelle **Product** angezeigt wird.

    ![Azure Data Catalog – Suchvorgänge gruppieren](media/register-data-assets-tutorial/data-catalog-grouping-search.png)

### <a name="comparison-operators"></a>Vergleichsoperatoren

Mit Vergleichsoperatoren können Sie für Eigenschaften mit numerischen Datentypen und Datumsdatentypen andere Vergleiche als „Gleichheit“ ausführen.

1. Geben Sie im Suchfeld `lastRegisteredTime:>"06/09/2016"`ein.

2. Deaktivieren Sie unter **Objekttyp** den Filter **Tabelle**.

3. Betätigen Sie die **EINGABETASTE**.

4. Vergewissern Sie sich, dass die Tabellen **Product**, **ProductCategory** und **ProductDescription** sowie die von Ihnen registrierte Azure SQL-Datenbank in den Suchergebnissen angezeigt werden.

    ![Azure Data Catalog – Vergleichssuchergebnisse](media/register-data-assets-tutorial/data-catalog-comparison-operator-results.png)

Ausführliche Informationen zur Ermittlung von Datenassets finden Sie unter [Ermitteln von Datenassets](data-catalog-how-to-discover.md). Weitere Informationen zur Suchsyntax finden Sie in der [Referenz zur Data Catalog-Suchsyntax](/rest/api/datacatalog/#search-syntax-reference).

## <a name="annotate-data-assets"></a>Versehen von Datenassets mit Anmerkungen

In dieser Übung verwenden Sie das Azure Data Catalog-Portal, um vorhandene Datenassets im Katalog mit Anmerkungen zu versehen (Hinzufügen von Informationen wie Beschreibungen, Tags oder Experten). Die Anmerkungen dienen als Ergänzung der strukturellen Metadaten, die während der Registrierung aus der Datenquelle extrahiert werden. Durch das Versehen mit Anmerkungen können die Datenassets viel leichter ermittelt und besser verstanden werden.

In dieser Übung versehen Sie ein Datenasset (ProductPhoto) mit Anmerkungen. Sie fügen dem Datenasset „ProductPhoto“ einen Anzeigenamen und eine Beschreibung hinzu.  

1. Navigieren Sie zur [Azure Data Catalog-Startseite](https://www.azuredatacatalog.com), und führen Sie eine Suche mit `tags:product` durch, um nach den von Ihnen registrierten Datenassets zu suchen.

2. Wählen Sie in den Suchergebnissen den Eintrag **ProductModel** aus.  

3. Geben Sie für **Anzeigename** den Text **Produktbilder** und für **Beschreibung** den Text **Produktfotos für Marketingmaterial** ein.

    ![Azure Data Catalog – ProductPhoto-Beschreibung](media/register-data-assets-tutorial/data-catalog-productmodel-description.png)

    Die **Beschreibung** hilft anderen Benutzern, das ausgewählte Datenasset zu finden und zu verstehen, warum und wie es verwendet werden sollte. Sie können auch weitere Tags hinzufügen und Spalten anzeigen. Sie können Datenquellen durchsuchen und filtern, indem Sie die beschreibenden Metadaten verwenden, die Sie dem Katalog hinzugefügt haben.

Auf dieser Seite können Sie auch die folgenden Schritte ausführen:

* Fügen Sie Experten für das Datenasset hinzu. Wählen Sie im Bereich **Experten** die Option **Hinzufügen**.

* Fügen Sie Tags auf Datasetebene hinzu. Wählen Sie im Bereich **Tags** die Option **Hinzufügen**. Bei einem Tag kann es sich um ein Benutzertag oder ein Glossartag handeln. Die Standard Edition von Data Catalog enthält ein Unternehmensglossar, mit dem Katalogadministratoren eine globale Unternehmenstaxonomie definieren können. Katalogbenutzer können Datenassets dann mit Glossarbegriffen versehen. Weitere Informationen finden Sie unter [Einrichten des Unternehmensglossars für gesteuertes Markieren](data-catalog-how-to-business-glossary.md)

* Fügen Sie Tags auf Spaltenebene hinzu. Wählen Sie unter **Tags** die Option **Hinzufügen** für die Spalte, die Sie mit Anmerkungen versehen möchten.

* Fügen Sie eine Beschreibung auf Spaltenebene hinzu. Geben Sie eine **Beschreibung** für eine Spalte ein. Sie können auch die Beschreibungsmetadaten anzeigen, die aus der Datenquelle extrahiert wurden.

* Fügen Sie für **Zugriff anfordern** Informationen hinzu, denen Benutzer entnehmen können, wie der Zugriff auf das Datenasset angefordert wird.
  
* Klicken Sie auf die Registerkarte **Dokumentation** , und geben Sie eine Dokumentation für das Datenasset an. Mit Azure Data Catalog-Dokumentation können Sie Ihren Datenkatalog als Inhaltsrepository zum Erstellen einer vollständigen Textbeschreibung Ihrer Datenassets verwenden.
  
Sie können auch Anmerkungen für mehrere Datenassets hinzufügen. Beispielsweise können Sie alle Datenassets auswählen, die Sie registriert haben, und einen Experten dafür angeben.

![Azure Data Catalog – mehrere Datenassets mit Anmerkungen versehen](media/register-data-assets-tutorial/data-catalog-multi-select-annotate.png)

Azure Data Catalog unterstützt einen Crowdsourcing-Ansatz für Anmerkungen. Alle Data Catalog-Benutzer können Tags (Benutzer oder Glossar), Beschreibungen und andere Metadaten hinzufügen. Hierdurch fügen Benutzer einem Datenasset Informationen zum Hintergrund und zur Nutzung hinzu und machen diese Informationen für andere Benutzer zugänglich.

Ausführliche Informationen zum Versehen von Datenassets mit Anmerkungen finden Sie unter [Hinzufügen von Anmerkungen zu Datenquellen](data-catalog-how-to-annotate.md) .

## <a name="connect-to-data-assets"></a>Herstellen einer Verbindung mit Datenassets

In dieser Übung öffnen Sie Datenassets mithilfe von Verbindungsinformationen in einem integrierten Clienttool (Excel) und einem nicht integrierten Tool (SQL Server Management Studio).

> [!NOTE]
> Bedenken Sie, dass Azure Data Catalog Ihnen keinen Zugriff auf die eigentliche Datenquelle bietet – es erleichtert es Ihnen lediglich, die Datenquelle zu ermitteln und zu verstehen. Wenn Sie eine Verbindung mit einer Datenquelle herstellen, verwendet die dafür ausgewählte Clientanwendung Ihre Windows-Anmeldeinformationen, oder Sie werden zur Eingabe Ihrer Anmeldeinformationen aufgefordert. Um auf eine Datenquelle zugreifen zu können, muss Ihnen der Zugriff gewährt werden (sofern noch nicht geschehen).

### <a name="connect-to-a-data-asset-from-excel"></a>Herstellen einer Verbindung mit einem Datenasset über Excel

1. Wählen Sie in den Suchergebnissen den Eintrag **Product** aus. Wählen Sie in der Symbolleiste die Option **Öffnen in** und dann **Excel**.

    ![Azure Data Catalog – Verbindung mit Datenasset herstellen](media/register-data-assets-tutorial/data-catalog-connect1.png)

2. Wählen Sie im Download-Popupfenster die Option **Öffnen**. Das angezeigte Fenster kann je nach Browser variieren.

3. Wählen Sie im Fenster **Microsoft Excel-Sicherheitshinweis** die Option **Aktivieren**.

    ![Azure Data Catalog – Excel-Sicherheitspopup](media/register-data-assets-tutorial/data-catalog-excel-security-popup.png)

4. Übernehmen Sie im Dialogfeld **Daten importieren** die Standardeinstellungen, und wählen Sie **OK**.

    ![Azure Data Catalog – Excel, Daten importieren](media/register-data-assets-tutorial/data-catalog-excel-import-data.png)

5. Zeigen Sie die Datenquelle in Excel an.

    ![Azure Data Catalog – Produkttabelle in Excel](media/register-data-assets-tutorial/data-catalog-connect2.png)

### <a name="sql-server-management-studio"></a>SQL Server Management Studio

In dieser Übung haben Sie eine Verbindung mit Datenassets hergestellt, die mit Azure Data Catalog ermittelt wurden. Im Azure Data Catalog-Portal können Sie direkt mithilfe der integrierten Clientanwendungen im Menü **Öffnen in** eine Verbindung herstellen. Anhand der Speicherortinformationen in den Assetmetadaten können Sie auch mit jeder anderen Anwendung Ihrer Wahl eine Verbindung herstellen. Beispielsweise können Sie SQL Server Management Studio verwenden, um eine Verbindung mit Azure SQL-Datenbank herzustellen und auf die Daten in den Datenassets zuzugreifen, die Sie in diesem Tutorial registriert haben.

1. Öffnen Sie **SQL Server Management Studio**.

2. Geben Sie im Dialogfeld **Mit Server verbinden** den Servernamen aus dem Bereich **Eigenschaften** des Azure Data Catalog-Portals ein.

3. Verwenden Sie eine geeignete Authentifizierungsmethode und die entsprechenden Anmeldeinformationen, um auf das Datenasset zuzugreifen. Falls Sie keinen Zugriff haben, fordern Sie ihn anhand der Informationen im Feld **Zugriff anfordern** an.

    ![Azure Data Catalog – Zugriff anfordern](media/register-data-assets-tutorial/data-catalog-request-access.png)

Wählen Sie **Verbindungszeichenfolgen anzeigen**, um die ADO.NET-, ODBC- und OLEDB-Verbindungszeichenfolgen anzuzeigen und zur Verwendung in Ihrer Anwendung in die Zwischenablage zu kopieren.

## <a name="manage-data-assets"></a>Verwalten von Datenassets

In diesem Schritt erfahren Sie, wie Sie die Sicherheit für Ihre Datenassets einrichten. Mit Data Catalog erhalten Benutzer keinen Zugriff auf die eigentlichen Daten. Der Besitzer der Datenquelle steuert den Datenzugriff.

Mit Data Catalog können Sie Datenquellen ermitteln und die Metadaten der im Katalog registrierten Quellen anzeigen. Mitunter sollen Datenquellen jedoch nur für bestimmte Benutzer oder Mitglieder bestimmter Gruppen sichtbar sein. In solchen Fällen können Sie in Data Catalog den Besitz von registrierten Datenassets übernehmen und die Sichtbarkeit Ihrer Assets steuern.

> [!NOTE]
> Die in dieser Übung beschriebenen Verwaltungsfunktionen sind nur in der Standard Edition von Azure Data Catalog verfügbar, nicht in der kostenlosen Edition.
> In Azure Data Catalog können Sie den Besitz von Datenassets übernehmen, Datenassets Mitbesitzer hinzufügen und die Sichtbarkeit von Datenassets festlegen.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Übernehmen des Besitzes von Datenassets und Einschränken der Sichtbarkeit

1. Navigieren Sie zur [Azure Data Catalog-Startseite](https://www.azuredatacatalog.com). Geben Sie `tags:cycles` in das Textfeld **Suche** ein, und drücken Sie die **EINGABETASTE**.

2. Wählen Sie in der Ergebnisliste ein Element aus, und wählen Sie dann in der Symbolleiste die Option **Besitz übernehmen**.

3. Wählen Sie im Bereich **Eigenschaften** unter **Verwaltung** die Option **Besitz übernehmen**.

    ![Azure Data Catalog – Besitz übernehmen](media/register-data-assets-tutorial/data-catalog-take-ownership.png)

4. Wählen Sie zum Einschränken der Sichtbarkeit im Abschnitt **Sichtbarkeit** die Option **Besitzer und folgende Benutzer** und dann **Hinzufügen**. Geben Sie die E-Mail-Adressen der Benutzer in das Textfeld ein, und drücken Sie die **EINGABETASTE**.

    ![Azure Data Catalog – Zugriff beschränken](media/register-data-assets-tutorial/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Entfernen von Datenassets

In dieser Übung verwenden Sie das Azure Data Catalog-Portal, um Vorschaudaten aus registrierten Datenassets zu entfernen und Datenassets aus dem Katalog zu löschen.

In Azure Data Catalog können Sie ein einzelnes Asset oder mehrere Assets löschen.

1. Navigieren Sie zur [Azure Data Catalog-Startseite](https://www.azuredatacatalog.com).

2. Geben Sie in das Textfeld **Suche** den Suchbegriff `tags:cycles` ein, und wählen Sie die **EINGABETASTE**.

3. Wählen Sie wie im folgenden Screenshot dargestellt in der Ergebnisliste ein Element aus, und wählen Sie in der Symbolleiste die Option **Löschen**:

    ![Azure Data Catalog – Rasterelement löschen](media/register-data-assets-tutorial/data-catalog-delete-grid-item.png)

    Wenn Sie die Listenansicht verwenden, befindet sich das Kontrollkästchen wie unten gezeigt links neben dem Element:

    ![Azure Data Catalog – Listenelement löschen](media/register-data-assets-tutorial/data-catalog-delete-list-item.png)

    Sie können wie im folgenden Screenshot gezeigt auch mehrere Datenassets auswählen und löschen:

    ![Azure Data Catalog – Mehrere Datenassets löschen](media/register-data-assets-tutorial/data-catalog-delete-assets.png)

> [!NOTE]
> Das Standardverhalten des Katalogs besteht darin, das Registrieren beliebiger Datenquellen für alle Benutzer zuzulassen und Benutzern auch das Löschen aller registrierten Datenassets zu ermöglichen. Die Verwaltungsfunktionen in der Standard Edition von Azure Data Catalog enthalten zusätzliche Optionen zum Übernehmen des Besitzes von Assets und Einschränken der Benutzer, die Assets ermitteln und löschen können.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie wichtige Funktionen von Azure Data Catalog kennen gelernt, z.B. das Registrieren, Ermitteln und Verwalten von Unternehmensdatenassets sowie das Hinzufügen von Anmerkungen. Nachdem Sie das Lernprogramm abgeschlossen haben, ist es an der Zeit, mit der Verwendung zu beginnen. Sie können noch heute starten, indem Sie die Datenquellen registrieren, die Sie und Ihr Team verwenden, und indem Sie Kollegen zur Nutzung des Katalogs einladen.

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Unterstützte Datenquellen](data-catalog-dsr.md)