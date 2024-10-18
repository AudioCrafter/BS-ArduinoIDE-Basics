# BS-ArduinoIDE-Basics

# Basic File Structure

Zeilen die mit // anfangen, sind Kommentare, diese haben nichts mit der ausführung des Codes zutun, und werden nur zur übersicht verwendet
```c++
void setup() {
  // so sieht ein Kommentar aus :)
}
```

### Leeres Skript / Basis vonn allen Sketchen ("Programme" die für z.B. Arduinos oder ESP8266 geschrieben wurden):
Das ist der Code, der in der IDE steht, wenn ein neuer Sketch erstellt wird
```c++
void setup() {
  // Hier steht der Code der am anfang, wenn der SBC (Single Board Computer) startet, EINMAL ausgeführt wird
}

void loop() {
  // Hier steht der Code, der nach der setup() methode DAUERHAFT in einer Schleife ausgeführt wird, bis der SBC keinen Strom mehr hat 
}
```

# Serial Monitor (Printen von Text / informationen, von dem ESP an den Computer)

```c++
void setup() {
    Serial.begin(9600);
}

void loop() {
    // Mit Anführungszeichen können fest definierte Texte ausgegeben werden
    Serial.println("Hallo! ich bin fester Text");

    // Ohne Anführungszeichen wird versucht eine Variable zu drucken, Beispielsweise Der Helligkeitswert von einem Lichtsensor 
    Serial.println(variable);

    // Es können auch Text und Variablen gleichzeitig gedruckt werden, Diese verknüpfung wird mit + ermöglicht.
    // Da Fester Text aber ein String ist, und variablen meisten Int oder Float, müssen diese erst mit String(variable) zu einem String konvertiert werden
    Serial.println("Helligkeit: " + String(helligkeitswert));
    // Diese Zeile würde z.B. ausgeben "Helligkeit: 69"

    delay(1000);
}
```


# Externe Komponenten (z.B. Sensoren und Aktoren)

## Mikrofon

```c++
int mic_Pin = A0; //Da das Mikrofon Analoge werte ausgibt, muss es an einem Analogen Pin angeschlossen werden, am ESP8266 ist das der Pin A0
void setup() {

}

void loop() {
    int mic_value = analogRead(mic_Pin);
    // Hier wird der Lautstärke Wert Vom Mikrofon, welches an den definierten Pin angeschlossen wurde ausgelesen, und in der Variable "mic_value" gespeichert
    // Diese Variable kann dann z.B. in einer If Else funktion benutzt werden
}

```

## Temperatur

```c++
#include <OneWire.h>          // Inkludiert die OneWire-Bibliothek, die für die Kommunikation mit 1-Wire-Geräten, wie dem DS18B20-Temperatursensor, benötigt wird
#include <DallasTemperature.h> // Inkludiert die DallasTemperature-Bibliothek, die spezifisch für die DS18B20-Sensoren entwickelt wurde und den Umgang mit diesen vereinfacht


int temp_pin = 0; // Definiert den Pin, an dem der DS18B20-Sensor angeschlossen ist

OneWire oneWire(temp_pin);    // Initialisiert das OneWire-Objekt für die Kommunikation mit dem Sensor über den angegebenen Pin
DallasTemperature DS18B20(&oneWire);  // Erstellt ein DallasTemperature-Objekt, das den OneWire-Bus verwendet, um mit den DS18B20-Temperatursensoren zu kommunizieren


void setup() {
  DS18B20.begin(); // Initialisiert den DS18B20-Sensor auf dem OneWire-Bus

void loop() {

  DS18B20.requestTemperatures();  // Sendet eine Anfrage an den Sensor, die Temperatur zu messen

  float temp_current = DS18B20.getTempCByIndex(0);
  // Speichert die Temperatur in Grad Celsius vom ersten Sensor (Index 0), der am Bus angeschlossen ist in der Variable "temp_current"
  // Diese Variable kann dann z.B. in einer If Else funktion benutzt werden
}
```



