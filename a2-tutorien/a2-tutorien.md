## Dokumentation Aufgabe 2 ##

Tutorien
========


## Lösungsansatz ##

Es sollen fünf Termine für Tutorien zurückgegeben werden. Ein beliebiger Termin kann gestrichen werden, trotzdem stehen für die verbleibenden Termine vier Tutoren zur Verfügung.

Zu Beginn des Programms werden zuerst die Werte aus der GUI übernommen und gefiltert. Vor der Berechnung aller möglichen Lösungen wird eine Fallunterscheidung vorgenommen:

- Falls nur für vier oder weniger Termine Tutoren ausgewählt wurden, kann es keine Lösung mit fünf zustande kommenden Terminen geben. In diesem Fall endet das Programm mit dem Hinweis `Zu wenige Termine ausgewählt!`.

- Falls für 5 oder 6 Termine Tutoren als verfügbar markiert wurden, werden eine mögliche Lösung (bei 5 Terminen) bzw. 6 mögliche Lösungen (bei 6 Terminen) vorgeschlagen.

Die Überprüfung der zuerst genannten Vorraussetzungen an die Lösung(en) wird von der Klasse `PossibleSolution` übernommen. Es wird die erste als gültig bestätigte Lösung ausgegeben. Ist keine der bis zu 6 Lösungen gültig, bricht das Programm mit der Meldung `Keine Terminvorschläge möglich [...]` ab.


## Aufbau ##

- **Graphic.java**: Grafische Benutzeroberfläche zur Ein- und Ausgabe.
- **Tutorien.java**: Klasse, die den über die Benutzeroberfläche ausgewählten Terminplan einliest, irrelevante (nicht angefragte) Tage aussortiert und alle Möglichkeiten für fünf Termine berechnet.
- **PossibleSolution.java**: Überprüft mit der Methode 
`testPossibility()`, ob mindestens vier Tutoren verfügbar sind (unabhängig davon, welcher der fünf Termine wegfällt).


## Beispiel ##

![Screenshot Tutorien](http://i.imgur.com/VdLJg0A.jpg)

## Nutzung ##

Sie können zum Testen des Programms die mitgelieferte GUI benutzen, welche Sie über die mitgelieferte `start.bat` aufrufen können. Die Startdatei setzt vorraus, dass 'java' als Befehl in der Befehlszeile erkannt wird. Ist dies nicht der Fall, so sollte die %PATH%-Variable des Betriebssystem angepasst werden. Die Anweisungen hierzu lassen sich auf der offiziellen Java-Seite von Oracle finden:  [Wie richte ich eine PATH-Systemvariable ein oder ändere diese?](http://www.java.com/de/download/help/path.xml)


## Code-Auszug ##

    public boolean testPossibility(){
    		//Iteriert für die Anzahl der Termine
		for(int i=0;i<termine.size();i++){
			int count = 0; //Anzahl der Tutoren
			//iteriert die Tutoren
			for(int j=0;j<termine.size();j++){
				//iteriert die Tage
				for(int k=0;k<dates.length;k++){
					if(k!=i){
						if(dates[termine.get(j)][k]){
							count++;
							break;
						}
					}
				}
			}
			if(count<4){
				return false;
			}
		}
		return true;
	}
