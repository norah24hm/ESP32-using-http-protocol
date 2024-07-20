# ESP32-using-http-protocol
Use http request in ESP32 code to get the data from database.
## Task idea 
TO use http request in ESP32 code to get the data from database, in our case the data we need is the last data enterd the db.
## Steps:
* The db we need to work on is the one we used in this previous project: https://github.com/norah24hm/control-panel, specifically the php page that displayed the last direction.
* Since we are working on a simulator: https://wokwi.com we need to use an esp32 board to use it for the http request also we'll use a led for testing purposes, the second step is to make sure that the LED's anode (positive leg) is connected to the correct digital output pin and the cathode (negative leg) is connected to a ground (GND) pin on the ESP32 board.
* Fristly i tried hosting the page on vercel but it didn't work as this is my first time working on this website:
---------------------------------------------------------------------------------------
![image](https://github.com/user-attachments/assets/581d3178-7a5d-4090-a2f2-b40c2b90f3ec)

---------------------------------------------------------------------------------------
* Using the ip address of my device with the page link did not work as well
* The last attempt is to use the company link that is using the company domain to print the last direction .

## The code: 
```
#include<WiFi.h>
#include <HTTPClient.h>
const char* ssid = "Wokwi-GUEST";
const char* pass = "";
const char* serverUrl = "https://s-m.com.sa/f.html";
unsigned const long interval = 2000;
unsigned long zero = 0;

const int ledPin = 25; // choose a pin for the LED
void setup(){
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT); // set the LED pin as an output
  WiFi.begin(ssid, pass);
  while(WiFi.status() != WL_CONNECTED){
    delay(100);
    Serial.println(".");
  }
  Serial.println("WiFi Connected!");
  Serial.println(WiFi.localIP());
}
void loop() {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    http.begin(serverUrl);
    int httpResponseCode = http.GET();
    if (httpResponseCode > 0) {
      String response = http.getString();
      Serial.println(response); // print the last inserted value
      if (response == "forward") {
        digitalWrite(ledPin, HIGH); // turn the LED on
      } else {
        digitalWrite(ledPin, LOW); // turn the LED off
      }
    } else {
      Serial.println("Error: " + String(httpResponseCode));
    }
    http.end();
  }
  delay(10000); // wait 10 seconds before sending the next request
} 

```
## Code explanation:
----------------------------------------------------------------------------------------
1. The code starts by including the necessary libraries for Wi-Fi and HTTP client functionality.
2. It then defines the Wi-Fi network credentials (SSID and password), the server URL, and an interval for sending requests.
3. The setup() function initializes the serial communication, sets the LED pin as an output, and connects to the Wi-Fi network. It prints a message to the serial terminal when the connection is established.
4. The loop() function runs repeatedly and checks if the Wi-Fi connection is active. If it is, it sends an HTTP GET request to the specified server URL using the HTTPClient library.
5. The code then checks the HTTP response code. If it's greater than 0, it means the request was successful, and the code retrieves the response as a string.
6. The code then checks if the response is equal to "forward". If it is, it turns the LED on by setting the voltage on the LED pin to HIGH. Otherwise, it turns the LED off by setting the voltage to LOW.
7. If the HTTP response code is not greater than 0, the code prints an error message to the serial terminal.
Finally, the code waits for 10 seconds before sending the next request.

* In summary, this code connects to a Wi-Fi network, sends an HTTP request to a server, and controls an LED based on the response received.
----------------------------------------------------------------------------------------
## Demo of the wokwi project:

https://github.com/user-attachments/assets/1e2b390f-ca24-4669-b85c-b557463f6c71





