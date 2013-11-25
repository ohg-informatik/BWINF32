## Dokumentation Aufgabe 1 ##

Rationalisierung
================


## Lösungsansatz ##

Ein von uns generierter Bruch soll dem folgenden Format entsprechen: **D = <sup>Z</sup>&frasl;<sub>N</sub>**, wobei Z ein ganzzahliger Zähler, N ein ganzzahliger Nenner und D die gegebene Dezimalzahl ist.

Dies lässt sich äquivalent umformen zu: **N = <sup>Z</sup>&frasl;<sub>D</sub>**

Sei Z nun eine frei gewählte Ganzzahl, lässt sich N berechnen. Die Gleichung wäre für jedes Z erfüllt (der Bruch wäre äquivalent mit der ursprünglichen Dezimalzahl), jedoch ist eine Lösung gesucht, bei der das resultierende N eine möglichst geringe Ganzzahl ist.

Für die Implementation bedeutet dies, dass als Abbruchbedingung `N % 1 == 0` ("Ist N eine Ganzzahl?") anzugeben ist. In einer Schleife wird solange Z inkrementiert, bis die Abbruchbedingung erfüllt ist. Der errechnete Bruch ist maximal vereinfacht, da N die kleinste Ganzzahl ist, die die o.g. Gleichung erfüllt.

### Geschwindigkeits-Optimierung (freiwillige Ergänzung) ###

Bei frühen Testdurchläufen mit Dezimalzahlen wie `3000,5` war die Rechenzeit bis zum Erhalt des Ergebnisses auffällig hoch, was sich durch die langwierige Inkrementation des Zählers auf 6001 erklären lässt. Zur Beschleunigung der Rationalisierung von großen Dezimalzahlen wird daher der Bruch nur für die "Nachkommastellen" berechnet und die "Vorkommastellen" werden zuletzt mit dem Nenner multipliziert und addiert.

Da der Modulo-Operator von Java bei Dezimalzahlen (z.B. floats) sehr ungenau ist, wird er nicht verwendet, um die die Zahl "am Komma zu trennen". Stattdessen wird die gegebene Dezimalzahl in einen String umgewandelt und dieser am Zeichen '.' getrennt, sodass man die Zahlen vor und nach dem Komma als Strings erhält, welche man mit der `Integer`-Klasse wieder als `int` speichern kann. Der zweite String (der "Nachkommawert") erhält vorher das Prefix "0.", da der Dezimaloperator vorher durch die Trennung entfernt wurde.

#### Beispiel für `3000,5`: ####
Vorkommawert: `vw = 3000`<br>
Nachkommawert: `nw = 0,5`<br>
Rationalisierter Nachkommawert: `nw = 1/2`<br>
Nenner und Zähler des Bruchs `nw`: `n = 2, z = 1`<br>
Ergebnis: `x = ((vw * n) + z) / n = ((3000*2) + 1)/2 = 6001/2 = 3000,5`


## Aufbau ##

- **Rationalisierung.java**: Beinhaltet die Methode `rationalisiere(double zahl)`, welche den rationalisierten Bruch der gegebenen Dezimalzahl in der Form `int[]{Zähler, Nenner}` zurückgibt.
- **Gui.java**: Öffnet im Konstruktor eine GUI (einzelner JFrame), welche eine Eingabezeile für die Dezimalzahl, einen 'Rationalisieren'-Button und ein Ausgabefeld für den rationalisierten Bruch anzeigt.


## Nutzung ##

Sie können zum Testen des Programms die mitgelieferte GUI benutzen, welche Sie über die mitgelieferte `start.bat` aufrufen können. Die Startdatei setzt vorraus, dass 'java' als Befehl in der Befehlszeile erkannt wird. Ist dies nicht der Fall, so sollte die %PATH%-Variable des Betriebssystem angepasst werden. Die Anweisungen hierzu lassen sich auf der offiziellen Java-Seite von Oracle finden:  [Wie richte ich eine PATH-Systemvariable ein oder ändere diese?](http://www.java.com/de/download/help/path.xml)


## Code-Auszug ##

    public static int[] rationalisiere(double zahl) {
		//Nehme nur "Wert nach dem Komma" zur Bruchermittlung
		//Hinweis: Modulo ist in diesem Fall ungenau, daher wird die Zahl
		//mithilfe der String-Klasse am '.' getrennt
		
		//nw ^= Nachkommawert, vw ^= Vorkommawert
		double nw = Double.parseDouble("0." + new Double(zahl).toString().split("[.]")[1]);		
		int vw = (int) (zahl - nw);
		
		int zaehler = 1, nenner = 1;
		
		//Fange Ganzzahlen ab (verhindert eine Endlosschleife)
		if(nw == 0){
			zaehler = 0;
		}else{
			while (true) {
				double n = zaehler / nw;
				if (n % 1 == 0) {
					nenner = (int) n;
					break;
				} else {
					zaehler++;
				}
			}
		}
		
		//Rechne Ganzzahl bzw. "Wert vor dem Komma" wieder hinzu
		zaehler += (int) (nenner * vw);
		
		return (new int[] { zaehler, nenner });
    }
