#include <SoftwareSerial.h>

int ledpin = 2;      // LED connected to D2
int relaypin = 4;    // Relay connected to D3
String command;

// Create software serial on pins 10 (RX) and 11 (TX)
SoftwareSerial BTSerial(10, 11); // RX, TX

void setup() {
  Serial.begin(9600);     // For debugging on Serial Monitor
  BTSerial.begin(9600);   // HC-05 default baud rate

  pinMode(ledpin, OUTPUT);
  pinMode(relaypin, OUTPUT);

  digitalWrite(ledpin, LOW);   // LED off at start
  digitalWrite(relaypin, HIGH); // Relay off at start

  Serial.println("Waiting for Bluetooth commands...");
  Serial.println("Commands: (LED ON, LED OFF, FAN ON, FAN OFF)");
}

void loop() {
  if (BTSerial.available()) {              // Check if data is coming from HC-05
    command = BTSerial.readStringUntil('\n');
    command.trim(); // remove spaces/newlines

    if (command.equalsIgnoreCase("LED ON")) {
      digitalWrite(ledpin, HIGH);
      Serial.println("LED turned ON");
    }
    else if (command.equalsIgnoreCase("LED OFF")) {
      digitalWrite(ledpin, LOW);
      Serial.println("LED turned OFF");
    }
    else if (command.equalsIgnoreCase("FAN ON")) {
      digitalWrite(relaypin, LOW);
      Serial.println("Fan turned ON via relay");
    }
    else if (command.equalsIgnoreCase("FAN OFF")) {
      digitalWrite(relaypin, HIGH);
      Serial.println("Fan turned OFF via relay");
    }
    else {
      Serial.println("Unknown command. Use LED ON/OFF or FAN ON/OFF.");
    }
  }
  else  if (Serial.available()) {
    command = Serial.readStringUntil('\n');
    command.trim(); // remove spaces/newlines


    if (command.equalsIgnoreCase("LED ON")) {
      digitalWrite(ledpin, HIGH); // energize coil
      Serial.println("LED turned ON ");
    }
    else if (command.equalsIgnoreCase("LED OFF")) {
      digitalWrite(ledpin, LOW); // de-energize coil
      Serial.println("LED turned OFF ");
    }
    else if (command.equalsIgnoreCase("fan on")) {
      digitalWrite(relaypin, LOW); // energize coil
      Serial.println("fan turned ON via relay");
    }
    else if (command.equalsIgnoreCase("fan off")) {
      digitalWrite(relaypin, HIGH); // de-energize coil
      Serial.println("fan turned OFF via relay");
    }
    else {
      Serial.println("Unknown command. Use ON or OFF.");
    }
  }

}
