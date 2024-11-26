# BS-ITT-Basics
## Inhalt

[Fachbegriffe](#Fachbegriffe)

[Basic File Structure](#basic-file-structure)

[Leeres Skript](#leeres-skript)

[Serial Monitor](#serial-monitor)

[Sensoren und Aktoren](#externe-komponenten)

  [Mikrofon](#Mikrofon)
  
  [Temperatur](#Temperatur)
  
  [Binary Sensor](#binary-sensor)
  
  [Piezo Buzzer](#piezo-buzzer)
  
  [LED](#led)


# Generelle infos

## was sind Bibiliotheken / Libraries?
In der Arduino IDE bezeichnet eine Bibliothek eine Sammlung von vorgefertigten Funktionen, Klassen und Anweisungen, die dazu dienen, bestimmte Aufgaben oder die Steuerung von Hardware-Komponenten zu erleichtern. Diese Bibliotheken ermöglichen es Entwicklern, komplexe Funktionen wie die Steuerung von Motoren, Sensoren oder Displays zu verwenden, ohne den gesamten Code selbst schreiben zu müssen. Beispiele sind Bibliotheken für DHT11-Temperatursensoren oder Servo-Motoren, die mit wenigen Zeilen Code ansteuerbar sind.

Beispiel: Wenn man einen Servo Motor steuern möchte, kann man die Servo.h Library benutzen. Diese wandelt einen einfache Grad Zahl als input in die komplexen Motor steuerungssignale um.

## Debugging
Systematische Analyse und Behebung (Troubleshooting) von Fehlern die im Code oder auftreten können (z.B. Tippfehler, Logikfehler, Indentierungsfehler)

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



# Fachbegriffe

## **Aktoren mit Beispielen:** 
Elektrische (steuer) Signale werden in Physikalische größen umgesetzt, wie z.B. Eine LED, Motor, Lautsprecher oder Relay

## **analoge Signale:**  
Sensoren erzeugen bzw. Aktoren benötigen analoge oder digitale Signale meist in Form einer elektrischen Spannung. Analoge Signale haben einen stufenlosen und beliebig feinen Verlauf. Da  Analogsignale einen kontinuierlichen Vorgang zeitlich kontinuierlich abbilden, bezeichnet man diese auch als wert– und zeitkontinuierlich.
Beispiel für analoge Werte:

![Beispiel für einen Analogen Wert](https://github.com/AudioCrafter/BS-ArduinoIDE-Basics/blob/main/Analoge%20werte%20beispiel.png?raw=true)


## **Anschlusstechnik Bus mit Beispielen:** 
Ein Bus ist ein gemeinsamer Datenweg, über den mehrere Geräte oder Komponenten Informationen austauschen können, indem sie sich denselben Übertragungskanal teilen. Beispielsweise Ein Bus im Auto, der Steuergeräte (z. B. für Motor, ABS, oder Licht) miteinander kommunizieren lässt. oder I2C, der Sensoren und Conputer miteinander Kommunizieren lässt 

## **Anschlusstechnik Schnittstelle mit Beispielen:**
beschreibt die physischen und logischen Verbindungen, die verschiedene Geräte oder Systeme miteinander verbinden, um Daten auszutauschen. Sie umfasst sowohl die Hardware (Stecker, Kabel, Ports) als auch die Kommunikationsprotokolle, die den Datenaustausch regeln. Eine Schnittstelle ist der Punkt, an dem zwei Systeme miteinander kommunizieren können.

Beispielsweie RJ45, HDMI, USB

## **Automatisierungspyramide:**
![Automatisierungspyramiede](https://www.iph-hannover.de/de/dienstleistungen/automatisierungstechnik/automatisierungspyramide/Automatisierungspyramide.png)

## **binäre Signale:**
Binäre Signale sind digitale Signale, die nur zwei Zustände annehmen können, meist 0 und 1. Diese Zustände repräsentieren in der Regel "aus" (0) und "ein" (1) oder niedrig (Low) und hoch (High). Binäre Signale sind die Grundlage der digitalen Kommunikation und der meisten modernen elektronischen Systeme.



## **Bridge:** 
"Interface" Gerät, um zwischen verschiedenen System zu kommunizieren. Bekanntestes beispiel: Phillips Hue Bridge, Empfängt signale per W-Lan, und sendet diese Signale weiter an Lampen mit dem Funkstandart Zigbee

## **Cyberphysische Systeme und ihre Einsatzmöglichkeiten:**
Systeme, die physische Prozesse mit computergestützten Prozessen kombinieren, indem sie Daten sammeln (mit Sensoren), verarbeiten und auf physische Umgebungen reagieren (mit Aktoren). Sie finden Anwendung in Bereichen wie der Industrie 4.0, intelligente Verkehrssysteme, Medizin (z.B. chirurgische Roboter), Smart Homes, etc.

## **digitale Signale:** 
Digitale Signale können nur einzelne, fest vorgegebene Werte (vorgegebener Symbolvorrat) annehmen. Somit bezeichnet man ein digitales Signal auch Signal, das durch diskrete Werte (Zustände) repräsentiert wird.
Beispiel für digitale Werte:

![Beispiel für einen digitalen Wert](https://github.com/AudioCrafter/BS-ArduinoIDE-Basics/blob/main/Digitale%20werte%20beispiel.png?raw=true)

## **Embedded Systems:**
Spezialisierte Computer, die in andere Geräte oder Maschinen integriert sind, um bestimmte, häufig wiederkehrende Aufgaben zu erfüllen. Sie sind darauf ausgelegt, mit der Hardware zu interagieren und eine spezifische Funktion auszuführen, ohne dass ein Benutzer direkt damit interagiert.

## **ESD:** 
"Electrostatic Discharge" unkontrollierter Ausgleich elektrischer Ladung zwischen zwei unterschiedlich stark aufgeladenen Objekten. z.B. Der kleine Stromschlag beim anfassen einer Türklinke wenn man über teppich läuft

## **Feldbus:** 
Bussystem, das in einer Anlage Feldgeräte wie Messfühler (Sensoren) und Stellglieder (Aktoren) zur Kommunikation mit einem Automatisierungsgerät verbindet (rellativ Störungs-unanfällig)

## **GPIO:** 
"General Purpose Input/Output" allgemeiner digitaler Pin an z.B. einem Single Board Computer, dessen Verhalten, unabhängig, ob als Eingabe- oder Ausgabekontakt, durch logische Programmierung frei bestimmbar ist

## **I2C:** 
I²C (Inter-Integrated Circuit) ist ein einfacher Datenbus, der es Single Board Computern und Sensoren ermöglicht, über nur zwei Leitungen (Daten und Takt) miteinander zu kommunizieren.

## **Industrie 4.0:**
bezeichnet die vierte industrielle Revolution, die durch den Einsatz moderner digitaler Technologien in der Fertigungs- und Produktionsindustrie geprägt ist. Sie basiert auf der Vernetzung von Maschinen, Systemen und Produktionsprozessen durch intelligente Technologien und ist stark von der Digitalisierung und Automatisierung geprägt. Ziel ist es, die Produktionsprozesse effizienter, flexibler und autonomer zu gestalten.

## **Internet of Things:** 
Internet of Things bezeichnet ein Netzwerk aus miteinander verbundenen physischen Geräten, die über das Internet oder Netzwerk Daten senden, empfangen und oft auch untereinander kommunizieren können, um Prozesse zu automatisieren und Informationen bereitzustellen. Beispiele sind smarte Thermostate oder vernetzte Glühbirnen.

## **M2M:**
 Machine to Machine bezeichnet die direkte Kommunikation zwischen Maschinen, Geräten oder Systemen ohne die Notwendigkeit menschlicher Interaktion.

## **Master-Slave-Prinzip:**
beschreibt eine Kommunikationsstruktur, bei der ein Master-Gerät die Kontrolle über den Datenaustausch hat und die Slave-Geräte als Reaktion auf Anfragen des Masters handeln. Der Master initiiert die Kommunikation, während die Slaves auf diese Anfragen reagieren und keine eigenständige Kontrolle ausüben.

In vielen Systemen, wie etwa in seriellen Kommunikationsprotokollen oder Bus-Systemen (z. B. I²C), kann nur der Master Daten senden oder empfangen, während die Slaves in der Regel nur auf Anforderungen des Masters reagieren. Dieses Prinzip ermöglicht eine klare Hierarchie und Organisation in der Datenkommunikation.

## **Mikrocontroller:**

## **Multi-Master-Prinzip:**
Das Multi-Master-Prinzip beschreibt ein Konzept in Kommunikationssystemen, bei dem mehrere Geräte (Master) gleichzeitig die Möglichkeit haben, den Datenverkehr zu initiieren und zu steuern, anstatt dass nur ein einzelner Master alle Entscheidungen trifft. Dieses Prinzip findet sich beispielsweise in Bus-Systemen wie I²C oder CAN, wo mehrere Geräte Datenübertragungen starten können.

## **NodeMCU:**
NodeMCU ist eine Open-Source-Plattform, die auf dem ESP8266 Mikrocontroller basiert. Sie wird oft in Projekten im Bereich Internet of Things (IoT) verwendet, um vernetzte Geräte zu erstellen, die über Wi-Fi kommunizieren.

![NodeMCU ESP8266 Board](https://upload.wikimedia.org/wikipedia/commons/e/e5/Nodemcu_amica_bot_02.png)

## **NodeRED:**
eine grafische Entwicklungsumgebung für das Erstellen von IoT-Workflows und Automatisierungen. Sie ermöglicht es, durch Verbinden von sogenannten Nodes (Bausteinen) Daten aus verschiedenen Quellen wie Sensoren, APIs oder Geräten zu verarbeiten, zu transformieren und an Zielsysteme zu senden. Sceenshot:
![Screenshot der NodeRED Oberfläche](https://user-images.githubusercontent.com/4663918/63022233-76304400-be70-11e9-8516-cab988df6b1e.png)

## **Sensoren mit Beispielen:** Ein Sensor ist ein Gerät, das chemische oder physikalische Größen wie Temperatur, Feuchtigkeit, Licht oder Bewegung misst und diese in ein elektrisches Signal umwandelt, das von einem Mikrocontroller oder einem anderen Gerät verarbeitet werden kann. Sensoren sind eine zentrale Komponente in vielen IoT-Anwendungen, um Daten zu sammeln und Automatisierungen auszulösen.

## **Sicherheitsprobleme bei IoT:** 
IoT-Geräte sind oft anfällig für Sicherheitsprobleme wie unzureichende Verschlüsselung, schwache Passwörter und fehlende Updates, wodurch sie leicht Ziel von Cyberangriffen werden können.

## **Single-Board-Computer mit Beispielen:**

## **UART:** 
Universal Asynchronous Receiver Transmitter ist ein serielles Kommunikationsprotokoll, das Daten zwischen zwei Geräten über zwei Leitungen (Daten senden und empfangen) asynchron überträgt, das bedeutet dass die Datenübertragung ohne ein gemeinsames Taktsignal (Clock) erfolgt. Stattdessen synchronisieren sich Sender und Empfänger mithilfe von Start- und Stopp-Bits am Anfang und Ende jeder Datenübertragung.

