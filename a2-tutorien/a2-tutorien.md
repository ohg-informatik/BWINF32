## Dokumentation Aufgabe 2 ##

Tutorien
========

## Lösungsansatz ##

Es sollen aufgrund der Auswahlen der Tutoren fünf von sechs Terminen zurückgegeben werden, wobei es immer mindestens vier Tutoren geben muss, egal welcher der fünf Termine nicht stattfindet. 

## Aufbau ##

- **Graphic.java**: Graphische Benutzeoberfläche zur Ein- und Ausgabe.
- **Tutorien.java**: Übernimmt den in der Benutzeroberfläche ausgewählten Terminplan, sortiert irrelevante Tage aus und generiert alle Möglichkeiten für funf Termine.
- **PossibleSolution.java**: Überprüft mit der Methode 
`testPossibility()`, ob egal welcher der fünf Termine wegfällt mindestens vier Tutoren verfügbar sind.

## Beispiel ##

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
