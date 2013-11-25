## Dokumentation Aufgabe 2 ##

Tutorien
========


## Lösungsansatz ##

Es sollen fünf Termine für Tutorien zurückgegeben werden. Ein beliebiger Termin kann gestrichen werden, trotzdem stehen für die verbleibenden Termine vier Tutoren zur Verfügung. Die Überprüfung dieser Vorraussetzungen an die Lösung wird von der Klasse `PossibleSolution` übernommen. Die Klasse überprüft, ob die übergebene Terminliste den oben genannten Vorgaben entspricht. Um eine Terminliste übergeben zu können, müssen zuerst die Werte aus der GUI übernommen und gefiltert werden.


## Aufbau ##

- **Graphic.java**: Grafische Benutzeroberfläche zur Ein- und Ausgabe.
- **Tutorien.java**: Klasse, die den über die Benutzeroberfläche ausgewählten Terminplan einliest, irrelevante (nicht angefragte) Tage aussortiert und alle Möglichkeiten für fünf Termine berechnet.
- **PossibleSolution.java**: Überprüft mit der Methode 
`testPossibility()`, ob mindestens vier Tutoren verfügbar sind (unabhängig davon, welcher der fünf Termine wegfällt).


## Beispiel ##


## Nutzung ##

Sie können zum Testen des Programms die mitgelieferte GUI benutzen, welche Sie über die mitgelieferte `start.bat` aufrufen können. Die Startdatei setzt vorraus, dass 'java' als Befehl in der Befehlszeile erkannt wird. Ist dies nicht der Fall, so sollte die %PATH%-Variable des Betriebssystem angepasst werden. Die Anweisungen hierzu lassen sich auf der offiziellen Java-Seite von Oracle finden:  [Wie richte ich eine PATH-Systemvariable ein oder ändere diese?](http://www.java.com/de/download/help/path.xml)


## Code-Auszug ##

    public boolean testPossibility(){
		for(int i=0;i<termine.size();i++){
			int count = 0;
			for(int j=0;j<termine.size();j++){
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
