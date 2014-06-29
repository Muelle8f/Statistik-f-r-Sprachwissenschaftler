% Hausaufgabe 20
% Janina Müller <Muelle8f@students.uni-marburg.de>
% 2014-06-27

Falls die Umlaute in dieser und anderen Dateien nicht korrekt dargestellt werden, sollten Sie File > Reopen with Encoding > UTF-8 sofort machen (und auf jeden Fall ohne davor zu speichern), damit die Enkodierung korrekt erkannt wird! 


```
## Loading required package: Matrix
## Loading required package: Rcpp
```


# Die nächsten Punkte sollten beinahe automatisch sein...
1. Kopieren Sie diese Datei in Ihren Ordner (das können Sie innerhalb RStudio machen oder mit Explorer/Finder/usw.) und öffnen Sie die Kopie. Ab diesem Punkt arbeiten Sie mit der Kopie. Die Kopie bitte `hausaufgabe20.Rmd` nennen und nicht `Kopie...`
2. Sie sehen jetzt im Git-Tab, dass die neue Datei als unbekannt (mit gelbem Fragezeichen) da steht. Geben Sie Git Bescheid, dass Sie die Änderungen in der Datei verfolgen möchten (auf Stage klicken).
3. Machen Sie ein Commit mit den bisherigen Änderungen (schreiben Sie eine sinnvolle Message dazu -- sinnvoll bedeutet nicht unbedingt lang) und danach einen Push.
4. Ersetzen Sie meinen Namen oben mit Ihrem. Klicken auf Stage, um die Änderung zu merken.
5. Ändern Sie das Datum auf heute. (Seien Sie ehrlich! Ich kann das sowieso am Commit sehen.)
6. Sie sehen jetzt, dass es zwei Symbole in der Status-Spalte gibt, eins für den Zustand im *Staging Area* (auch als *Index* bekannt), eins für den Zustand im Vergleich zum Staging Area. Sie haben die Datei modifiziert, eine Änderung in das Staging Area aufgenommen, und danach weitere Änderungen gemacht. Nur Änderungen im Staging Area werden in den Commit aufgenommen.
7. Stellen Sie die letzten Änderungen auch ins Staging Area und machen Sie einen Commit (immer mit sinnvoller Message!).
8. Vergessen Sie nicht am Ende, die Lizenz ggf. zu ändern!

*Für diese Hausaufgabe werden Sie auch während der Bearbeitung Internetzugang brauchen, weil die Quellenangaben und Daten dynamisch aus dem Netz runtergeladen werden!*

Ich schlage weiterhin vor, dass Sie die R-Code-Blöcke erst in der Konsole testen und zum Laufen bringen, bevor Sie sie in die Rmd-Datei einfügen. Die Behebung von Fehlern ist einfach leichter in der Konsole *und* bei den Rmd-Dateien wird evtl. alles jedes mal neu berechnet, was bei den Modellen hier evtl. ein paar Minuten dauern könnte. Sparen Sie sich Zeit und testen Sie das Knitten der Rmd-Datei erst, wenn Sie kurz eine Kaffeepause brauchen

Im folgenden steht RE für engl. *random effect* und FE für engl. *fixed effect*.

# Höflichkeit und Stimmhöhe
<a href="http://dx.doi.org/10.1016/j.wocn.2012.08.006">Winter & Grawunder (2012)</a> untersuchten die phonetischen Eigenschaften der formellen und informellen Register im Koreanischen. Unter anderem wurde auch die Grundfrequenz ($F_0$) der Stimme bei jedem Trial festgestellt. Die Daten zur Grundfrequenz bei sechs Probanden und sieben Items hat Bodo Winter im Netz verfügbar gemacht. 

Wir können Daten direkt von seiner Webseite in R laden: 


```r
stimmen <- read.csv("http://www.bodowinter.com/tutorial/politeness_data.csv")
```


Wie immer schauen wir uns erstmal die Zusammenfassung der Daten an.


```r
summary(stimmen)
```

```
##  subject gender    scenario attitude   frequency    
##  F1:14   F:42   Min.   :1   inf:42   Min.   : 82.2  
##  F2:14   M:42   1st Qu.:2   pol:42   1st Qu.:131.6  
##  F3:14          Median :4            Median :203.9  
##  M3:14          Mean   :4            Mean   :193.6  
##  M4:14          3rd Qu.:6            3rd Qu.:248.6  
##  M7:14          Max.   :7            Max.   :306.8  
##                                      NA's   :1
```


Die Items in dieser Studie sind "Szenarien", also Situationen wo man mehr oder weniger höflich ist: obwohl es einen fixen Faktor für Höflichkeit gibt, ist man evtl. nicht in jeder Situation gleich (un)höflich. Somit haben wir einen zufälligen Faktor, weil wir nicht alle möglichen Szenarien austesten können, obgleich wir erkennen, dass das Szenario einen Teil der Varianz erklären kann. `attitude` ist auch ein Faktor mit zwei Stufen `inf` für *informal* und `pol` für *polite*.  Geschlecht spielt eine bekannte Rolle bei Stimmhöhe und wurde demzufolge auch als `gender` mit aufgenommen. Die Messung der Stimmhöhe in Hertz ist eine metrische Variable, die als `frequency` im data frame zu finden ist. 

Allerdings merken wir, dass `scenario` falsch kodiert ist, was wir korrigieren müssen:

stimmen$scenario <- as.factor(stimmen$scenario)

## Auswirkung der experimentellen Manipulation
Um einen groben Eindruck zu bekommen, können wir schnell einen Boxplot machen:





























