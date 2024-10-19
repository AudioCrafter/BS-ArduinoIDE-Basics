# BS-ArduinoIDE-Basics
## Inhalt

Inhaltsangabe coming soon haha


[Basic File Structure](#basic-file-structure).
[Leeres Skript](#leeres-skript)
[Serial Monitor](#serial-monitor)
[Sensoren und Aktoren](#externe-komponenten)
  [Mikrofon](#Mikrofon)
  [Temperatur](#Temperatur)
  [Binary Sensor](#binary-sensor)
  [Piezo Buzzer](#piezo-buzzer)
  [LED](#led)




# Basic File Structure

Zeilen die mit // anfangen, sind ``// Kommentare``, diese haben nichts mit der ausführung des Codes zutun, und werden nur zur übersicht verwendet
```c++
void setup() {
  // so sieht ein Kommentar aus :)
}
```

# Leeres Skript
## Basis von allen Sketchen ("Programme" die für z.B. Arduinos oder ESP8266 geschrieben wurden):
Das ist der Code, der in der IDE steht, wenn ein neuer Sketch erstellt wird
```c++

  // Vor den Methoden kann bereits Code stehen, z.B. werden hier üblicherweise Variablen Inizialisiert. 

void setup() {
  // Hier steht der Code der am anfang, wenn der SBC (Single Board Computer) startet, EINMAL ausgeführt wird
}

void loop() {
  // Hier steht der Code, der nach der setup() methode DAUERHAFT in einer Schleife ausgeführt wird, bis der SBC keinen Strom mehr hat 
}
```

# Serial Monitor 
## (Printen von Text / informationen, von dem ESP an den Computer)

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


# Externe Komponenten
## (z.B. Sensoren und Aktoren)

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
Spezifisch benutzen wir den DS18B20 Sensor. Es gibt verschiedene arten von Temperatur Sensoren. Der Code kann bei verschiedenen Typen von Sensoren unterschiedlich sein. In der schule benutzen wir aber so weit nur den DS18B20

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

## Binary Sensor
Ein Binary Sensor kann alles sein was nur 2 Zustände hat, wie z.B. Ein Schalter, Druckknopf, Touch Sensor, Bewegungsmelder. 

```c++
int button_pin = 0; // Definiert den Pin an dem der Button verbunden ist

void setup() {
  pinMode(button_pin, INPUT); // Setzt den Button Pin auf Input Mode
}

void loop() {
  bool button_status = digitalRead(button_pin); // Liest aus ob der Button Gedrückt ist, und speichert dass dann in der Variable button_status
}
```


## Piezo Buzzer

Dieser Code lässt de Buzzer in einem Intervall von 500ms Piepen
```c++
int piezo_pin = 0; // Definiert den Pin an dem der Buzzer verbunden ist

void setup() {
pinMode(piezo_pin, OUTPUT); // Setzt den piezo_pin in den Output mode 
}

void loop() {
  tone(piezo_pin, 1234); // Startet die Tonausgabe am piezo_pin, mit einer Frequenz (Tonhöhe) von 1234 Hz
  delay(500); // wartet 500 ms
  noTone(piezo_pin); // Stoppt die Tonausgabe am piezo_pin wieder
  delay(500); // wartet 500 ms
}
```


## LED 

Dieser Code lässt die LED Blinken.
```c++
int led_pin = 0; // Definiert den Pin an dem die LED verbunden ist


void setup() {
  pinMode(led_pin, OUTPUT); // Setzt den led_pin in den Output mode 
}


void loop() {
  digitalWrite(led_pin, HIGH);  // LED wird angeschalten
  delay(500); // wartet 500 ms
  digitalWrite(led_pin, LOW);   // LED wird ausgeschalten
  delay(500); // wartet 500 ms
}

```
