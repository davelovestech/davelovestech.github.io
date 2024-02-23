---
layout: post
title:  "Arduino-Based Cell Culture Alarm"
date:   2012-10-14 13:53:00 -0800
categories: 
---
**<u>NOTE</u>: The following blog post was written by me in 2012. I've added it here to document how my passion for IT was already present back when I was working as a biology research associate.**

A few months ago one of my work tasks was to expand immortalized human cell culture lines up to 35 6 inch plates in order to create subtracted cDNA libraries. And I needed to repeat this for six different cell lines! 

Since, for the beginning portion of the production run, I was working by myself I was limited in the number of dishes I could maintain at once.  I found that it required a tremendous amount of time to keep 60 150 mm plates going at once, but that it was doable.  After one weekend off I came back to lab to find that my incubator's CO2 tank had run out & that all of my cells, that were supposed to be confluent, were in fact dead.  Needless to say I was furious!   Because I wanted to prevent this kind of problem in the future & I was interested in learning about electronics and computer programming I went in search of ways of setting up an incubator alarm.  

While checking out the back of my incubator I noticed an empty cable connection labelled "alarm contacts".  I thought that was pretty neat, so I read the manual and learned that there's an alarm that the manufacturer makes that you can buy & connect to the incubator.  That product is able to call or email users in the event of a failure.  I thought that was really neat, but I didn't want to buy it, so I read up on how the alarm works.

 ![alarm-contacts](/assets/arduino-incubator/alarm-contacts-junction.jpg)

It turns out that, through some complicated internal mechanism, a relay is switched between alarm and normal states.  I'll note that it's possible to alter the factory alarm settings, but that it comes set up to alert mode for power loss, CO2 level changes and temperature alterations.  Typically, according to the figure above, the communication wire is connected to the red wire, but in states of alarm it connects to the green wire.  There's some pretty neat physics to how these devices work, but it's not necessary to delve into that right now.  However, if you're interested, here's a [great place to start](https://en.wikipedia.org/wiki/Relay).

With the knowledge that I had a wire that could be connected to two different end points, pending incubator state, I began looking into how different types of contact methods work.  The first device I found was a [DTMF generator](https://www.elektronika.ba/505/phone-call-alerter/).  The acronym might sound esoteric to you, but you've got loads of experience with this thing.  Ever wonder why phones make different pitched beeping sounds when you type the different keys?  Well, that's [DTMF signalling](https://en.wikipedia.org/wiki/Dual-tone_multi-frequency_signaling)! Though this fashion is very interesting, given my lack of experience with electronics I wanted to find something easier.  That's how I stumbled upon Arduino boards.  I found this Make project that showed readers how to make an [email alert for when their doorbell was pushed](https://www.youtube.com/watch?v=0IIwuAmIro4).

Reading about this project, I noticed that the doorbell alert trigger was surprisingly similar to the incubator relay.  Without having any understanding of electronics or computer science I enlisted the help of a few technically competent friends for guidance.  At first I copied the code from the doorbell project line for line and mimicked their board setup.  Conceptually, the doorbell project works as follows: the doorbell button, when pushed, brings a 5 volt line into contact with pin3 which, through the arduino program goes to a www.pushingbox.com web link that triggers an email.  Pushingbox is a great free service that lets you create a variety of responses to web links being accessed.

I'm skipping out a lot on the pushingbox stuff.  Check out the [project link](https://makezine.com/projects/notifying-doorbell-with-pushingbox/) for more info.  After I had the bare bones doorbell alarm setup (a 5 volt line that I could connect or remove from pin3) I brought the first incubator into the game.  Following the manual's schematic, I spliced some old phone cable and connected the Arduino 5 volt line to the communication line of the incubator alarm.  I connected pin3 to the normally open side of the relay.  So to be clear pin3 will only receive the 5 volts when the incubator alarm is active.  At this stage all I had to do was to use the doorbell code & create a pushingbox response that would email my lab group when pin3 was receiving the 5V.  All of this was done with the help of BH and SK.

With this in operation I wanted to add a coworkers incubator to this system, but I found that it wasn't that easy.  I did the same phone jack splice to Arduino board, this time with pin4. Here's a picture of the Arduino:

 ![arduino](/assets/arduino-incubator/arduino-ethernet-shield.jpg)
 
And it works! Here's a YouTube video of it in action:

[![DIY Incubator Alarm](/assets/arduino-incubator/youtube-pic.PNG)](https://www.youtube.com/watch?v=PeRlTA8hjJU "DIY Incubator Alarm")

And here is the code that I used:

```
////
//
// General code from http://www.pushingbox.com for Arduino + Ethernet Shield (official)
//
////

#include <SPI.h>
#include <Ethernet.h>

  /////////////////
 // MODIFY HERE //
/////////////////
byte mac[] = { 0x00, 0xAA, 0xBB, 0xCC, 0xDE, 0x19 };   // Be sure this address is unique in your network

//Your secret DevID from PushingBox.com. You can use multiple DevID  on multiple Pin if you want
#define DAVID_POWER_OUT "vBF05E85729C42B0"        //Scenario : "David loses power"          
#define KELSEY_POWER_OUT "v25B99B9FDD02DFF"       //Scenario : "Kelsey loses power"           
#define DAVID_POWER_BACK "vEE1E78E06E62DA5"       //Scenario : "David regains power"         
#define KELSEY_POWER_BACK "v8F4FE0578DA22D5"      //Scenario : "Kelsey regains power"  

//Numeric Pin where you connect your switch
#define pinDevid1 3  // Dave oohh no!
#define pinDevid2 4  // Kelsey ohh no!

// Debug mode
#define DEBUG true
  ///////
 //End//
///////

char serverName[] = "api.pushingbox.com";
boolean pinDevid1State = false;
boolean pinDevid2State = false;

// Initialize the Ethernet client library
// with the IP address and port of the server
// that you want to connect to (port 80 is default for HTTP):
EthernetClient client;

void setup() {
  Serial.begin(9600);
  pinMode(pinDevid1, INPUT);
  pinMode(pinDevid2, INPUT);
 
  // start the Ethernet connection:
  if (Ethernet.begin(mac) == 0) {
    Serial.println("Failed to configure Ethernet using DHCP");
    // no point in carrying on, so do nothing forevermore:
    while(true);
  }
  else{
    Serial.println("Ethernet ready");
  }
  // give the Ethernet shield a second to initialize:
  delay(1000);
}

void loop()
{
      ////
      // Listening for the pinDevid1 state
      ////
      if (digitalRead(pinDevid1) == HIGH && pinDevid1State == false) // switch on pinDevid1 is ON
      {
        if(DEBUG){Serial.println("pinDevid1 is HIGH");}
        pinDevid1State = true;
        //Sending request to PushingBox when the pin is HIGHT
        sendToPushingBox(DAVID_POWER_OUT);
      }
       if (digitalRead(pinDevid1) == LOW && pinDevid1State == true) // switch on pinDevid1 is OFF
      {
        if(DEBUG){Serial.println("pinDevid1 is LOW");}
        pinDevid1State = false;
        //Sending request to PushingBox when the pin is LOW
        sendToPushingBox(DAVID_POWER_BACK);
      }
     
     
      ////
      // Listening for the pinDevid2 state
      ////
      if (digitalRead(pinDevid2) == HIGH && pinDevid2State == false) // switch on pinDevid2 is ON
      {
        if(DEBUG){Serial.println("pinDevid2 is HIGH");}
        pinDevid2State = true;
        //Sending request to PushingBox when the pin is HIGHT
        sendToPushingBox(KELSEY_POWER_OUT);
      }
       if (digitalRead(pinDevid2) == LOW && pinDevid2State == true) // switch on pinDevid2 is OFF
      {
        if(DEBUG){Serial.println("pinDevid2 is LOW");}
        pinDevid2State = false;
        //Sending request to PushingBox when the pin is LOW
        sendToPushingBox(KELSEY_POWER_BACK);
      }
}


//Function for sending the request to PushingBox
void sendToPushingBox(String devid){
    if(DEBUG){Serial.println("connecting...");}

  if (client.connect(serverName, 80)) {
    if(DEBUG){Serial.println("connected");}

    if(DEBUG){Serial.println("sendind request");}
    client.print("GET /pushingbox?devid=");
    client.print(devid);
    client.println(" HTTP/1.1");
    client.print("Host: ");
    client.println(serverName);
    client.println("User-Agent: Arduino");
    client.println();
  }
  else {
    if(DEBUG){Serial.println("connection failed");}
  }
 
  // if there are incoming bytes available
  // from the server, read them and print them:
  if(DEBUG){
    if (client.available()) {
    char c = client.read();
    Serial.print(c);
    }
  }

    if(DEBUG){Serial.println();}
    if(DEBUG){Serial.println("disconnecting.");}
    client.stop();
} 
```