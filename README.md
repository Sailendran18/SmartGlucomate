# SmartGlucomate
#include <WiFi.h>
#include "HX711.h"

const char* ssid = "YourWiFiSSID";
const char* password = "YourWiFiPassword";
const char* serverAddress = "http://yourserveraddress.com";

// Define HX711 pins
#define DOUT  32
#define CLK  33

HX711 scale(DOUT, CLK);

// Function to connect to WiFi
void connectToWiFi() {
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi");
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("Connected");
}

// Function to send data to the server
void sendData(float weight) {
  HTTPClient http;
  
  // Construct the URL
  String url = serverAddress;
  url += "/api/update_weight?value=";
  url += String(weight);
  
  // Send the request
  http.begin(url);
  int httpResponseCode = http
