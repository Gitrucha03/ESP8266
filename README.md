EXP 01:  To test all IOs on ESP8266 Witty Cloud Development Board.
CODE :
# define led 2
# define red 15
# define green 12
# define blue 13
# define ldr  A0
 
void setup() 
{
 	pinMode(led, OUTPUT);
	pinMode(red, OUTPUT);
	pinMode(green, OUTPUT);
	pinMode(blue, OUTPUT);
	pinMode(ldr,INPUT);
	
	Serial.begin(9600);
}
 
void loop() 
{
  digitalWrite (led , HIGH);
  delay(100);
  digitalWrite (led , LOW);
  delay(100);
 
  digitalWrite (red , HIGH);
  delay(100);
  digitalWrite (red , LOW);
  delay(100);
 
  digitalWrite (green , HIGH);
  delay(100);
  digitalWrite (green, LOW);
  delay(100);
 
  digitalWrite (blue , HIGH);
  delay(100);
  digitalWrite (blue , LOW);
  delay(100);

 Serial.println(analogRead(ldr));
}

EXP 02: To connect ESP8266 Witty Cloud Development Board to WiFi and print IP address of the n/w.
CODE :

# include <ESP8266WiFi.h>           // library file for ESP8266
void setup()
{
Serial.begin(9600);           // shows output on serial monitor
  WiFi.begin ("Redmi","Ruchaa__03");  // connects to wifi n/w
  while(WiFi.status() != WL_CONNECTED)
  {
	Serial.print ('.');
	delay(200);
  }
	Serial.println();
	Serial.println("Witty board connected!");
Serial.println(WiFi.localIP()); // gives the address of the // n/w it is connected to
}
 void loop(){
 // put your main code here
 } 
// 192.168.64.52


EXP 03: To use ESP8266 Witty Cloud Development Board as a web server.

CODE:

#include <ESP8266WiFi.h>     // library file for ESP8266

#define led 2
#define red 15
#define green 12
#define blue 13
#define ldr A0

WiFiClient client;      // define client
WiFiServer server(80);  // define sever; 80 is port no. for http

void setup()
 {
  // put your setup code here, to run once:
  pinMode(led,OUTPUT);
  pinMode(red,OUTPUT);
  pinMode(blue,OUTPUT);
  pinMode(green,OUTPUT);

   Serial.begin(9600);  // shows output on serial monitor
WiFi.begin("Redmi","Ruchaa__03"); 
   while(WiFi.status()!=WL_CONNECTED)   // connects to wifi n/w

  {
    Serial.print(".");
    delay(200);
  }
  Serial.println("Wifi Connected");  
  Serial.println(WiFi.localIP());   // gives the address of the   //n/w it is connected to
  server.begin();                  // start server
}


void loop() 
{
  // put your main code here, to run repeatedly:

  client = server.available(); // server checks if any //client(request) is available
   if(client==1)                  // if client is connected
  {
   String request=client.readStringUntil('\n');  // read the //request
Serial.println(request);  // stop here &check request as text/IP                            //address

request.trim();     // remove garbage value

    if(request=="GET /ledON HTTP/1.1")
      digitalWrite(green,HIGH);
    if(request=="GET /ledOFF HTTP/1.1")
      digitalWrite(blue,HIGH);
  }
}

//192.168.49.52


EXP 04:  To make ESP8266 Witty Cloud Development Board as an access point (AP)/hotspot.

CODE :

#include <ESP8266WiFi.h>     // library file for ESP8266
#define led 2
#define red 15
#define green 12
#define blue 13
#define ldr A0

WiFiClient client;       // define client
WiFiServer server(80);   // define sever; 80 is port no.for http

void setup()
 {
  // put your setup code here, to run once:
  pinMode(led,OUTPUT);
  pinMode(red,OUTPUT);
  pinMode(blue,OUTPUT);
  pinMode(green,OUTPUT);

  Serial.begin(9600);
  WiFi.softAP("ESP","ESP8266");           // make AP
  Serial.println();
  Serial.println("Wittyboard connected");  
  Serial.println(WiFi.softAPIP());       // get IP address of AP
  server.begin();                        // start server
}

void loop() 
{
  // put your main code here, to run repeatedly:

  client = server.available();    // server checks if any //client(request) is available
  if(client==1)                     // if client is connected
  {
    String request=client.readStringUntil('\n'); // read the //request
    Serial.println(request);    //stop here &check request as //text/IP address
    request.trim();              // remove garbage value
    if(request=="GET /ledON HTTP/1.1")
      digitalWrite(green,HIGH);
    if(request=="GET /ledOFF HTTP/1.1")
      digitalWrite(blue,HIGH);
  }
}
 EXP 05: To send data from ESP8266 Witty Cloud Development Board on ThingSpeak cloud.

 CODE :

#include <ESP8266WiFi.h>   // library file for ESP8266 
#include<ThingSpeak.h>     // library file for ThingSpeak

#define led 2
#define red 15
#define green 12
#define blue 13
#define ldr A0
 
WiFiClient client;           //define client
long myChannelNumber=2491398;
const char myWriteAPIkey[]="CV";
 
void setup() 
{  // put your setup code here, to run once:
  pinMode(led,OUTPUT);
  pinMode(red,OUTPUT);
  pinMode(blue,OUTPUT);
  pinMode(green,OUTPUT);

  Serial.begin(9600);
  WiFi.begin("Redmi","Ruchaa__03");
  while(WiFi.status() != WL_CONNECTED) // check the connection 
 {
    Serial.print('.');
	delay(200);
  }
  Serial.println();
  Serial.println("witty board connected to wifi");
  Serial.println(WiFi.localIP()); // get the IP address 
  ThingSpeak.begin(client);      // start server
}
 
void loop()
 {
  int value=analogRead(ldr);
  Serial.print("LDR value:");
  Serial.println(value);
  ThingSpeak.writeField(myChannelNumber,1,value,myWriteAPIkey);
  delay(200);
}

EXP 06: MQTT protocol with ESP8266 Witty Cloud Development Board and Adafruit IO.

CODE :

#include <ESP8266WiFi.h>      	// library file for ESP8266
#include "Adafruit_MQTT.h"    	// library included through Adafruit IO Arduino
#include "Adafruit_MQTT_Client.h" // library included through Adafruit IO Arduino
 
// pinout for wittyBoard 
#define led   2             // debug LED, tiny blue
#define red   15           // RGB LED red
#define green 12      	 // RGB LED green
#define blue  13         // RGB LED blue
#define ldr   A0        // LDR
 
 
#define WLAN_SSID   	"Redmi"
#define WLAN_PASS   	"Ruchaa__03"
 
#define AIO_SERVER      "io.adafruit.com"
#define AIO_SERVERPORT  1883    // mqtt: 1883, secure-mqtt: 8883
#define AIO_USERNAME	"RuuAdaa"
#define AIO_KEY         "VD "
 
WiFiClient client;                                                                                      // declare client
 
Adafruit_MQTT_Client mqtt(&client, AIO_SERVER, AIO_SERVERPORT, AIO_USERNAME, AIO_KEY);              	// declare MQTT client
Adafruit_MQTT_Publish lightintensity = Adafruit_MQTT_Publish( &mqtt, AIO_USERNAME "/feeds/lux-meter");   // declare publisher
Adafruit_MQTT_Subscribe redbutton = Adafruit_MQTT_Subscribe(&mqtt, AIO_USERNAME "/feeds/red-led");  	// declare subscriber
Adafruit_MQTT_Subscribe greenbutton = Adafruit_MQTT_Subscribe(&mqtt, AIO_USERNAME "/feeds/green-led");  // declare subscriber
Adafruit_MQTT_Subscribe bluebutton = Adafruit_MQTT_Subscribe(&mqtt, AIO_USERNAME "/feeds/blue-led");	// declare subscriber
 
void MQTT_connect();                                                                                    // bug fixes
 
void setup() {
  // put your setup code here, to run once:
  pinMode(led, OUTPUT);
  pinMode(red, OUTPUT);
  pinMode(green, OUTPUT);
  pinMode(blue, OUTPUT);
 
  Serial.begin(115200);
  delay(10);
 
  Serial.println(F("Adafruit MQTT demo"));
 
  // Connect to WiFi access point.
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(WLAN_SSID);
 
  WiFi.begin(WLAN_SSID, WLAN_PASS);
  while (WiFi.status() != WL_CONNECTED) {
	delay(500);
	Serial.print(".");
  }
  Serial.println();
 
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
 
  // Setup MQTT subscription for onoff feed.
  mqtt.subscribe(&redbutton);
  mqtt.subscribe(&greenbutton);
  mqtt.subscribe(&bluebutton);
}
 
 
void loop() {
  // put your main code here, to run repeatedly: 
  MQTT_connect();
 
  Adafruit_MQTT_Subscribe *subscription;
  while ((subscription = mqtt.readSubscription(5000))) {
	if (subscription == &redbutton) {
  	Serial.print(F("Got: "));
  	Serial.println((char *)redbutton.lastread);
  	if(strcmp((char*)redbutton.lastread, "ON"))
        digitalWrite(red, LOW);
  	else
    	digitalWrite(red, HIGH);
	}
	if (subscription == &greenbutton) {
  	Serial.print(F("Got: "));
  	Serial.println((char *)greenbutton.lastread);
  	if(strcmp((char*)greenbutton.lastread, "ON"))
    	digitalWrite(green, LOW);
  	else
    	digitalWrite(green, HIGH);
	}
	if (subscription == &bluebutton) {
  	Serial.print(F("Got: "));
  	Serial.println((char *)bluebutton.lastread);
  	if(strcmp((char*)bluebutton.lastread, "ON"))
    	digitalWrite(blue, LOW);
  	else
    	digitalWrite(blue, HIGH);
	}
  }
 
  Serial.print(F("\nSending light val "));
  Serial.print(analogRead(ldr));
  Serial.print("...");
  if (! lightintensity.publish(analogRead(ldr)))
	Serial.println(F("Failed"));
  else
	Serial.println(F("OK!"));
}
 
// Function to connect and reconnect as necessary to the MQTT server.
void MQTT_connect() {
  int8_t ret;
 
  // Stop if already connected.
  if (mqtt.connected()) {
	return;
  }
 
  Serial.print("Connecting to MQTT... ");
 
  uint8_t retries = 3;
  while ((ret = mqtt.connect()) != 0) { // connect will return 0 for connected
       Serial.println(mqtt.connectErrorString(ret));
   	Serial.println("Retrying MQTT connection in 5 seconds...");
   	mqtt.disconnect();
   	delay(5000);  // wait 5 seconds
   	retries--;
   	if (retries == 0) {
     	// basically die and wait for WDT to reset me
     	while (1);
   	}
  }
  Serial.println("MQTT Connected!");
}

}
