## Dokumentation Junioraufgabe 1 ##

SongWriter
==========


## Lösungsansatz ##

Es wird gefordert, dass der "errechnete" Songtext nur aus Silben (zusammengesetzt aus einem Konsonanten und einem Vokal) sowie 'p di' (angehängt an die mittlere Silbe einer Zeile) besteht. Zur Generierung der Silben greift die Klasse `SongWriter` auf eine Liste aller Konsonanten bzw. aller Vokale in Form von `char`-Arrays zurück, die als Attribute in der Klasse gespeichert sind.

Außerdem soll die Anzahl der verschieden Konsonanten/Vokale pro Strophe klein sein, daher werden für jede Strophe vorher 2-4 Konsonanten und 1-3 Vokale ausgewählt (dies übernehmen die Funktionen `getConsSet()` und `getVowsSet()`).

Für Variationen wie "abwechselnd 4 und 6 Zeilen" wird die Zeilenanzahl als `int`-Array definiert. Mit einer Wahrscheinlichkeit von 50% werden entweder eine einzige zufällige Zeilenzahl oder im anderen Fall zwei zufällige Zeilenzahlen definiert, welche dann abwechselnd benutzt werden.


## Aufbau ##

- **SongWriter.java**: Beinhaltet die Methode `generateLyrics()`, welche mithilfe von `generateVerse()` und `generateLine()` einen Liedtext zusammenstellt.
- **Gui.java**: Öffnet im Konstruktor eine GUI (einzelner JFrame), welche einen `Songtext generieren`-Button sowie ein Ausgabe-Feld für den erzeugten Songtext anzeigt.


## Nutzung ##

Sie können zum Testen des Programms die mitgelieferte GUI benutzen, welche Sie über die mitgelieferte `start.bat` aufrufen können. Die Startdatei setzt vorraus, dass 'java' als Befehl in der Befehlszeile erkannt wird. Ist dies nicht der Fall, so sollte die %PATH%-Variable des Betriebssystem angepasst werden. Die Anweisungen hierzu lassen sich auf der offiziellen Java-Seite von Oracle finden:  [Wie richte ich eine PATH-Systemvariable ein oder ändere diese?](http://www.java.com/de/download/help/path.xml)


## Code-Auszug ##

	public void calcSolutions() {
		ArrayList<char[]> solutions = new ArrayList<char[]>();
		//Maximal count of solutions is 2^links (that's binary!)
		int max_solutions = (int) Math.pow(2, this.links);
		
		for(int i=0; i<max_solutions; i++){//For each solution
			//Create byte (0/1-ish) from current solution int
			char[] bin = this.zerofill(Integer.toBinaryString(i), this.links).toCharArray();
			if(this.checkConsecBits(bin)){
				solutions.add(bin);
			}
		}
		
		for(int i=0; i<solutions.size(); i++){
			System.out.println("Lösung " + i + ": " + new String(solutions.get(i)));
			//System.out.println(i + "," + new String(solutions.get(i))); //CSV
		}
	}

	/**
	 * Checks byte for consecutive bits
	 * @param charArray
	 * @return 
	 */
	private boolean checkConsecBits(char[] bin) {
		for(int j=1; j<bin.length; j++){//For each bit (=link)
			if(bin[j] == '0' && bin[j-1] == '0'){
				return false;
			}
		}
		return true;
	}

