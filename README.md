# EXP 8 : CLOUD-BASED DEVICE CONTROL USING MQTT AND WI-FI COMMUNICATION

## Aim

To control an electrical device remotely through a cloud platform using MQTT communication and a Wi-Fi module.

# Hardware / Software Tools Required

- Arduino UNO / ESP32 / ESP8266 Wi-Fi Module
- USB Cable
- PC/Laptop
- Arduino IDE
- Wi-Fi Network
- Relay Module
- LED / DC Load
- Breadboard
- Jumper Wires
- Cloud Platform such as Blynk or ThingSpeak
- MQTT Broker / MQTT Service

# Circuit Diagram

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/707cbbff-a867-45bf-92d3-786a3244d2ab" />


# Procedure

## Step 1: Assemble the Circuit

1. Place the microcontroller board, Wi-Fi module, relay module, LED/load, and breadboard on the workbench.
2. Connect the required power supply and GND connections.
3. Ensure that the Wi-Fi module and controller operate at their specified voltage levels.

## Step 2: Connect the Relay Module

1. Connect the **VCC** of the relay module to the appropriate power supply.
2. Connect the **GND** of the relay module to **GND**.
3. Connect the relay input pin to a suitable digital output pin of the controller.
4. Connect the LED or other low-voltage load through the relay contacts.
5. Do not connect mains voltage directly during laboratory testing unless the setup is specifically designed and supervised for it.

## Step 3: Configure Wi-Fi Communication

1. Connect the Wi-Fi-enabled controller to the required Wi-Fi network.
2. Enter the Wi-Fi SSID and password in the program.
3. Verify that the device obtains an IP address.
4. Confirm that the controller can establish an Internet connection.

## Step 4: Configure the Cloud / MQTT Platform

1. Create an account on the selected cloud platform such as **Blynk** or **ThingSpeak**, as applicable.
2. Configure the required device, virtual control, channel, or dashboard.
3. Configure the MQTT broker/server details.
4. Set the MQTT topic used for ON/OFF control.
5. Define the payload values, for example:
   - `ON` – Switch device ON
   - `OFF` – Switch device OFF

## Step 5: Write and Upload the Program

1. Open the Arduino IDE.
2. Include the required Wi-Fi and MQTT libraries.
3. Enter the Wi-Fi credentials and MQTT broker details.
4. Configure the relay output pin.
5. Establish a connection with the Wi-Fi network.
6. Establish a connection with the MQTT broker.
7. Subscribe to the required MQTT topic.
8. Write the callback function to process ON/OFF commands.
9. Verify the program using the **Verify** button.
10. Upload the program to the controller.

## Step 6: Execute the Program

1. Power ON the controller and Wi-Fi module.
2. Open the configured cloud dashboard or MQTT client.
3. Send an **ON** command through the configured MQTT topic.
4. Observe that the relay activates and the connected device turns ON.
5. Send an **OFF** command.
6. Observe that the relay deactivates and the connected device turns OFF.
7. Monitor the Serial Monitor to verify MQTT connection and received commands.

## Step 7: Verify the Output

1. Check whether the controller successfully connects to Wi-Fi.
2. Verify the MQTT broker connection.
3. Send an ON command from the cloud platform.
4. Observe the device switching ON.
5. Send an OFF command from the cloud platform.
6. Observe the device switching OFF.
7. Record the commands and corresponding device states.

# Program
```
#include <WiFiS3.h>

char ssid[] = "Magesh";      // Replace with your WiFi name
char pass[] = "8825657396";  // Replace with your WiFi password

WiFiServer server(80);

const int ledPin = 13;

bool blinkMode = false;
unsigned long previousMillis = 0;
const long interval = 500;  // Blink every 500 ms
bool ledState = LOW;

void setup() {
  Serial.begin(9600);

  pinMode(ledPin, OUTPUT);
  digitalWrite(ledPin, LOW);

  Serial.print("Connecting to WiFi");

  while (WiFi.begin(ssid, pass) != WL_CONNECTED) {
    Serial.print(".");
    delay(3000);
  }

  Serial.println("\nConnected!");
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());

  server.begin();
}

void loop() {

  // Blink LED if blink mode is enabled
  if (blinkMode) {
    unsigned long currentMillis = millis();

    if (currentMillis - previousMillis >= interval) {
      previousMillis = currentMillis;
      ledState = !ledState;
      digitalWrite(ledPin, ledState);
    }
  }

  WiFiClient client = server.available();

  if (client) {

    String request = client.readStringUntil('\r');
    client.flush();

    if (request.indexOf("/ON") != -1) {
      blinkMode = false;
      digitalWrite(ledPin, HIGH);
    }

    if (request.indexOf("/OFF") != -1) {
      blinkMode = false;
      digitalWrite(ledPin, LOW);
    }

    if (request.indexOf("/BLINK") != -1) {
      blinkMode = true;
    }

    if (request.indexOf("/STOP") != -1) {
      blinkMode = false;
      digitalWrite(ledPin, LOW);
    }

    client.println("HTTP/1.1 200 OK");
    client.println("Content-Type: text/html");
    client.println();

    client.println("<!DOCTYPE html>");
    client.println("<html>");
    client.println("<head>");

    client.println("<meta name='viewport' content='width=device-width, initial-scale=1'>");

    client.println("<style>");
    client.println("body{font-family:Arial;text-align:center;background:#f2f2f2;}");
    client.println("h1{color:#333;}");
    client.println("button{width:200px;height:60px;font-size:22px;border:none;border-radius:12px;margin:10px;color:white;cursor:pointer;}");
    client.println(".on{background:green;}");
    client.println(".off{background:red;}");
    client.println(".blink{background:blue;}");
    client.println(".stop{background:black;}");
    client.println("</style>");

    client.println("</head>");
    client.println("<body>");

    client.println("<h1>Arduino UNO R4 WiFi</h1>");
    client.println("<h2>LED Control</h2>");

    client.println("<a href='/ON'><button class='on'>LED ON</button></a><br>");
    client.println("<a href='/OFF'><button class='off'>LED OFF</button></a><br>");
    client.println("<a href='/BLINK'><button class='blink'>BLINK</button></a><br>");
    client.println("<a href='/STOP'><button class='stop'>STOP BLINK</button></a>");

    client.println("</body>");
    client.println("</html>");

    delay(1);
    client.stop();
  }
}
```


# Observation

<img width="899" height="1599" alt="image" src="https://github.com/user-attachments/assets/1ddbb283-024a-477f-9714-c798b34072e9" />


# Result

The **cloud-based device control system was successfully implemented using MQTT and Wi-Fi communication**. The device was remotely controlled by sending ON/OFF commands through the MQTT communication channel. The experiment demonstrated the use of **IoT cloud connectivity, MQTT messaging, Wi-Fi communication, and remote device control**.
