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

	private String[] generateVerse(int n_lines) {
		//Determine random (odd) number of syllables for this verse
		int n1_sil = 1 + this.rand.nextInt(5)*2;
		int n2_sil = 1 + this.rand.nextInt(5)*2;
		//Nerf dynamic line lengths
		if(this.rand.nextInt(2) == 1) n2_sil = n1_sil;

		char[] set_cons = this.getConsSet();
		char[] set_vows = this.getVowsSet();
		
		String[] lines = new String[n_lines];
		for(int i_line=0; i_line < n_lines; i_line++){
			if((i_line % 2) == 0){
				lines[i_line] = this.generateLine(n1_sil, set_cons, set_vows);
			}else{
				lines[i_line] = this.generateLine(n2_sil, set_cons, set_vows);
			}
		}
		
		//Add chant (such as 'Fake That!')
		if(this.rand.nextInt(4) == 1){
			String[] new_lines = new String[lines.length + 1];
			for(int i=0; i<lines.length; i++){
				new_lines[i] = lines[i];
			}
			new_lines[lines.length] = this.chants[this.rand.nextInt(this.chants.length)];
			lines = new_lines;
		}
		return(lines);
	}

	private String generateLine(int n_syl, char[] cons_set, char[] vows_set) {
		String line = "";
		//Make sure number of syllables is odd
		if((n_syl / 2) % 1 != 0) n_syl++;

		//Choose random consonant and vowel from array fields
		String xcon = Character.toString(cons_set[this.rand.nextInt(cons_set.length)]);
		String xvow = Character.toString(vows_set[this.rand.nextInt(vows_set.length)]);
		
		for(int i_syl=0; i_syl < n_syl; i_syl++){
			//Check if we're 'within the middle'
			if((i_syl) == (int) Math.floor(n_syl/2)){
				line = line.concat(xcon + xvow + "p di ");
			}else{
				line = line.concat(xcon + xvow + " ");
			}
		}
		return(line.trim());
	}
