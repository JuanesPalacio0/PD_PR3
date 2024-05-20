# Programa 1: Servidor Web con WiFi

Este programa configura un ESP32 como un servidor web en modo estación, que muestra una página HTML simple con una imagen.

## Código

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include <WebServer.h>

// SSID & Password
const char* ssid = "Xiaomi_11T_Pro";  // Enter your SSID here
const char* password = "12345678";  // Enter your Password here

WebServer server(80);  // Object of WebServer (HTTP port, 80 is default)

// HTML & CSS contents which display on web server
String HTML = "<!DOCTYPE html>\
<html>\
<body>\
<h1>My Primera guai Pagina con ESP32 - Station Mode &#128522;</h1>\
<a href=\"https://www.freepik.es\"><img src=\"https://img.freepik.com/foto-gratis/ojo-belleza-pintado-purpura-vibrante-elegante-generado-ia_188544-9707.jpg?size=626&ext=jpg\" alt=\"Descripción de la imagen\"></a>\
</body>\
</html>";

// Handle root url (/)
void handle_root() {
    server.send(200, "text/html", HTML);
}

void setup() {
    Serial.begin(115200);
    Serial.println("Try Connecting to ");
    Serial.println(ssid);

    // Connect to your Wi-Fi modem
    WiFi.begin(ssid, password);

    // Check if Wi-Fi is connected to Wi-Fi network
    while (WiFi.status() != WL_CONNECTED) {
        delay(1000);
        Serial.print(".");
    }
    Serial.println("");
    Serial.println("WiFi connected successfully");
    Serial.print("Got IP: ");
    Serial.println(WiFi.localIP());  // Show ESP32 IP on serial

    server.on("/", handle_root);

    server.begin();
    Serial.println("HTTP server started");
    delay(100);
}

void loop() {
    server.handleClient();
}

  **Este programa realiza las siguientes funciones:**

 -Conecta el ESP32 a una red Wi-Fi.
 -Configura un servidor web que escucha en el puerto 80.
 -Muestra una página HTML con un título y una imagen cuando se accede a la raíz del servidor.


# Programa 1:Comunicacion serial bluethooth
Este programa permite la comunicación serial entre el ESP32 y otro dispositivo a través de Bluetooth clásico.


#include <Arduino.h>
#include <WiFi.h>
#include <WebServer.h>

// This example code is in the Public Domain (or CC0 licensed, at your option.)
// By Evandro Copercini - 2018
//
// This example creates a bridge between Serial and Classical Bluetooth (SPP)
// and also demonstrate that SerialBT have the same functionalities of a normal Serial

#include "BluetoothSerial.h"
BluetoothSerial SerialBT;

void setup() {
    Serial.begin(115200);
    SerialBT.begin("BlutuDelDiablo"); // Bluetooth device name
    Serial.println("The device started, now you can pair it with Bluetooth!");
}

void loop() {
    if (Serial.available()) {
        SerialBT.write(Serial.read());
    }
    if (SerialBT.available()) {
        Serial.write(SerialBT.read());
    }
    delay(20);
}


**Este programa realiza las siguientes funciones: **

* Inicializa la comunicación serial a través del puerto USB y el Bluetooth.
* Configura el ESP32 como un dispositivo Bluetooth con el nombre "BlutuDelDiablo".
* Crea un puente de comunicación entre la entrada y salida serial del ESP32 y la conexión Bluetooth, permitiendo transmitir y recibir datos.