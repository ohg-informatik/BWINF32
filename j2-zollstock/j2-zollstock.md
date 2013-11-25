## Dokumentation Junioraufgabe 2 ##

Zollstock
=========


## Lösungsansatz ##

### Adaption zur Byte-Schreibweise ###
Im Folgenden werden nicht die 10 Segmente des Zollstocks, sondern die 9 "Gelenke" dazwischen betrachtet. Diese kennen nur die Zustände "aufgeklappt" (gestreckt) und "zugeklappt" (gefaltet), welchen man die Werte 0 und 1 zuweisen kann. Diese Zustände der 9 Gelenke lassen sich (ähnlich wie bei der Byte-Schreibweise) hintereinander aufschreiben. Für einen komplett ausgeklappten Zollstock (der so effektiv 2 Meter lang ist), erhielte man die Folge `000000000` (alle Gelenke aufgeklappt), für einen komplett zusammengefalteten Zollstock (der so effektiv 20cm lang ist) die Folge `111111111`. Eine einzelne Möglichkeit, einen Zollstock zu falten, lässt sich so mithilfe von 9 Bits darstellen (welche ein "9er-Byte" bilden).

### Lösungsfindung ###

Da jedes der 9 Bits einer Kombination entweder 1 oder 0 sein kann, erhält man eine Anzahl von 2<sup>9</sup> (= 512) verschiedenen Kombinationen. Man kann daher eine Schleife 512 mal durchlaufen und die Zählvariable in einen Binär-Code umwandeln, sodass man eine Möglichkeit der Zollstock-Faltung erhält.

Das gestellte Kriterium ist eine Limitierung der effektiven Zollstocklänge auf 50cm. Da die Segmente 20cm lang sind, können nur 2 Segmente "nebeneinander" gestreckt sein, damit die Länge von 50cm nicht überschritten wird. Dies bedeutet auch, dass nie zwei benachbarte Elemente gestreckt sein können, also den Wert `0` haben. Die Implementation dieser Überprüfung iteriert durch jedes (bis auf das erste) Bit einer Kombination und ermittelt die Kombination als ungültig, wenn das aktuell betrachtete Bit und sein "Vorgänger" gleich `0` sind, also zwei 0-Bits nebeneinander liegen (und der Zollstock an dieser Stelle 60cm oder mehr Platz einnimmt).

Die Menge, die nicht gefiltert wurde, beschreibt alle Lösungen, die dem Kriterium (Gesamtlänge &#8804; 50cm) entsprechen.

### Ergebnisse ###

Das Programm ermittelte 89 Lösungen: 


     0: 10101010        23: 011101101        45: 101101110        67: 110111111
     1: 10101011        24: 011101110        46: 101101111        68: 111010101
     2: 10101101        25: 011101111        47: 101110101        69: 111010110
     3: 10101110        26: 011110101        48: 101110110        70: 111010111
     4: 10101111        27: 011110110        49: 101110111        71: 111011010
     5: 10110101        28: 011110111        50: 101111010        72: 111011011
     6: 10110110        29: 011111010        51: 101111011        73: 111011101
     7: 10110111        30: 011111011        52: 101111101        74: 111011110
     8: 10111010        31: 011111101        53: 101111110        75: 111011111
     9: 10111011        32: 011111110        54: 101111111        76: 111101010
    10: 10111101        33: 011111111        55: 110101010        77: 111101011
    11: 10111110        34: 101010101        56: 110101011        78: 111101101
    12: 10111111        35: 101010110        57: 110101101        79: 111101110
    13: 11010101        36: 101010111        58: 110101110        80: 111101111
    14: 11010110        37: 101011010        59: 110101111        81: 111110101
    15: 11010111        38: 101011011        60: 110110101        82: 111110110
    16: 11011010        39: 101011101        61: 110110110        83: 111110111
    17: 11011011        40: 101011110        62: 110110111        84: 111111010
    18: 11011101        41: 101011111        63: 110111010        85: 111111011
    19: 11011110        42: 101101010        64: 110111011        86: 111111101
    20: 11011111        43: 101101011        65: 110111101        87: 111111110
    21: 11101010        44: 101101101        66: 110111110        88: 111111111
    22: 11101011


## Aufbau ##

- **Zollstock.java**: Beinhaltet in der Funktion `calcSolutions` die Schleife, die alle 512 Möglichkeiten in Binär-Schreibweise erzeugt, auf aufeinanderfolgende 0-Bits prüft und die gefundenen Lösungen in der Konsole darstellt.


## Nutzung ##

Sie können zum Testen des Programms die mitgelieferte `start.bat` aufrufen. Die Startdatei setzt vorraus, dass 'java' als Befehl in der Befehlszeile erkannt wird. Ist dies nicht der Fall, so sollte die %PATH%-Variable des Betriebssystem angepasst werden. Die Anweisungen hierzu lassen sich auf der offiziellen Java-Seite von Oracle finden:  [Wie richte ich eine PATH-Systemvariable ein oder ändere diese?](http://www.java.com/de/download/help/path.xml)


## Code-Auszug ##

(Variable `segments` = 10)


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

