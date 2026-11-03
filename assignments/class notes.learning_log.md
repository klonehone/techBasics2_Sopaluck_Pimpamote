
# Notes and Learning Log: Week 1 - 13/10/2026

Today was the first day and introduction to the Arduino device and program. 
In the lecture I learned about the Arduino UNO hardware and the components of its starter kit. 
I was only here for the lecture class and not the exercise class and had to therefore catch up at home. 

I had issues with the Arduino IDE software since it did not have the port I needed, and I had to manually download the driver itself, so I could not test out the hardware properly until the next class. 

Via the class slides I learned about the LEDs (the long leg being +)
The class made a light flash using the following code:  
```
void setup() {
  // put your setup code here, to run once:
  pinMode(13, OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(13, LOW);
  delay(300);
  digitalWrite(13, HIGH);
  delay(300);
}
```
Current ideas for the final project include a msuci-sync LED/RGB light where different colors are different aspects of music, for example purple = bass, red = hi-hats. 

## Notes and Learning Log: Week 2 - 20/10/26

Today we started out repeating LEDs and high power LEDs.
I learned about resistors, the resistor code and how to read them. 

Resistor Color Codes (thanks Jasper:)):  
<img width="500" src="https://github.com/user-attachments/assets/c3d24317-307f-41e0-b0bd-95a99e57e4c3" />  

We also explored more lights (RGB) using the breadboard and power supply. I learned how to place the resistors on the breadboard. We also looked at different wire types that come in the kit more throughly. 

We learned about GND, which is the circuit's common ground. 
We also learned different ways to power the breadboard: through Arduino UNO, power supply module, and battery/power source. 
We tried powering with battery. 

Briefly went through Ohm's Law and Power Law. 
Learned the difference between direct current and alternating current. 
We then proceeded to do series and paralel circuit. 

This code flashes three lights and then keeps them on:
`digitalWrite` is used for LED lights.  
`analogWrite` is used for RGB lights.  

```
const int redLED = 2;
const int yellowLED = 3;
const int greenLED = 4;

void setup() {
  pinMode(redLED, OUTPUT);
  pinMode(yellowLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
}

void loop() {
  digitalWrite(redLED, HIGH);
  digitalWrite(yellowLED, HIGH);
  digitalWrite(greenLED, HIGH);
  delay(500);
  digitalWrite(redLED, HIGH);
  digitalWrite(yellowLED, LOW);
  digitalWrite(greenLED, LOW);
  delay(50);
  digitalWrite(redLED, LOW);
  digitalWrite(yellowLED, HIGH);
  digitalWrite(greenLED, LOW);
  delay(50);
  digitalWrite(redLED, LOW);
  digitalWrite(yellowLED, LOW);
  digitalWrite(greenLED, HIGH);
  delay(50);
}
```

`digitalWrite` is used for LED lights.  
`analogWrite` is used for RGB lights.  

Using the RGB light, this code flashes new colors:  
```
const int redLED = 9;
const int greenLED = 10;
const int blueLED = 11;

void setup() {
  pinMode(redLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(blueLED, OUTPUT);
}

void loop() {
  setRGB(255,255,0); // yellow
  delay(300);
  setRGB(0,255,255); // cyan
  delay(300);
  setRGB(255,0,255); // purple
  delay(300);
}

void setRGB(int r, int g, int b){
  analogWrite(redLED, r);
  analogWrite(greenLED, g);
  analogWrite(blueLED, b);
}
```

## Notes and Learning Log: Week 3 - 27/10/26 

`analogWrite()` uses ~PWN pins - for analog signals. 
PWN uses analog right

digitalWrite()/digitalRead()
only high or low voltage (5/0V)
- 0 - 255 (0v to 5v)  

More of the Arduino board: 

Digital pins (right side or Arduino board)
pin 0 for writing, pin 1 for reading
- use when necessary as it can mess up the uploading process of code

Aref is not ground
- two pins ontop of aref

Analog pins and analog input pins (left lower hand side)
- There is no real analog - safe for analog input

- Power rail (left upper)
- Vin - battery
- Reset button and pin
- Ioref: reference voltage

For next week we will learn 
- Input pullup
- ADC: analog to digtal converter

_____

Potentionmeter - adjustable resistor
  Common use case - volume adjustment

Analog signals are **continuous** and vary **smoothly** over time (PWN pin like ~3, not pin 13, a normal number without the wave sign).  
Digital signals are **discrete** and are often represented with binary (0 or 1).  

Demo of the oscilations:  
```
void setup() {
  pinMode(3, OUTPUT);
}

void loop() {
  analogWrite(3, 255); // always on (5v)
  analogWrite(3, 127); // half on, half off
  analogWrite(3, 50); // sometimes on, mostly off
  analogWrite(3, 0); // always off (0v)
}
```

Blinking LED:  
```
const int LED_PIN = 3; // ~ PWM pin (between power values 0-255)
void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  analogWrite(LED_PIN, 200);
  delay(300);
  analogWrite(LED_PIN, 5);
  delay(300);
}
```

Fade in/out an LED:  
```
const int LED_PIN = 3;

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  for (int i=0; i <= 255; i += 5) { // (START; END; STEP)
    analogWrite(LED_PIN, i);
    delay(15);
  }

  for (int i=255; i >= 0; i -= 5) {
    analogWrite(LED_PIN, i);
    delay(15);
  }
}
```

Fading an LED using a potentiometer:  
```
const int POT = A0;
const int LED_PIN = 3;
int reading = 0;
int brightness = 0;

void setup() {
  pinMode(POT, INPUT);
  Serial.begin(9600);
}

void loop() {
  reading = analogRead(POT);
  Serial.print("Potentiometer: ");
  brightness = map(reading, 0, 1023, 0, 255);
  Serial.println(reading);
  analogWrite(LED_PIN, brightness);
}
```
We detected light using a photoresistor using the same code as above (we detect using the same pins). This is the code

```

const int POT = A0;
const int LED_PIN = 3;
int reading = 0;
int brightness = 0;

void setup() {
  // put your setup code here, to run once:
  pinMode(POT, INPUT);
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  reading = analogRead(POT);
  Serial.print("Photoresistor");
  Serial.println(reading);

  // brightness = map(reading, 0, 1023, 0, 225);
  if(reading > 400) { //enough light
    digitalWrite(LED_PIN, LOW);
  } else {
    digitalWrite(LED_PIN, HIGH);

  }
  analogWrite(LED_PIN, brightness);
}

```

We also went through coding elements of int - start, end, step - and also if/else condition in Arduino code, as you can see in the code (new for this session). 


## Notes and Learning Log: Week 3 - 03/11/26

We got introduced to Buzzers and differentiating passive and active buzzers:
Active Buzzer: it can produce sound directly when
connected to power (built in oscillator, defined pitch)
Passive Buzzer: it requires a signal in order to make sound

Soundwaves: easiest soundwave to see is pulse. 

Usage of PWN and GND can cause problems for interpretation
We connect a passive buzzer to the Arduino and breadboard - generates a square wave of the specified frequency (non-blocking function):

```
const int buzzerPin = 8;

void setup() {
  // put your setup code here, to run once:
  pinMode(buzzerPin, OUTPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  tone(buzzerPin, 200, 1000);
  delay(2000);
}
```

With serial read (also different pitches): 

```
const int buzzerPin = 8;
int reading = 0;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  pinMode(buzzerPin, OUTPUT);
  delay (1000);
  Serial.println("Type the pitch you want to hear!");
}

void loop() {
  // put your main code here, to run repeatedly:
  if (Serial.available() > 0) {
    reading = Serial.parseInt();
    Serial.println(reading);
    tone (buzzerPin, reading, 500);
    }
}
```

We the moved on to ultrasonic distance sensor, connected it to the buzzzer and got it woirking with the following code: 

```
/***
Ultrasonic distance sensor
***/

// Define the Trig and Echo pin connections
const int trigPin = 9;
const int echoPin = 10;
const int buzzerPin = 8;

// Define variables to store duration and distance
long duration; // for potentially large number
int distance;

void setup() {
  // Start the serial communication
  Serial.begin(9600);

  // Define the Trig and Echo pins as Output and Input
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode (buzzerPin, OUTPUT);
}

void loop() {
  // Clear the trigPin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  // Sets the trigPin HIGH for 10 microseconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);

  Serial.print("Duration ");
  Serial.print(duration);
  Serial.print(" microseconds | ");

  // Calculating the distance
  distance = duration * 0.034 / 2; // Speed of sound wave divided by 2 (go and back)

  if (distance < 30) {
    digitalWrite(buzzerPin, HIGH);
  } else {
    digitalWrite(buzzerPin, LOW);
  }

  // Displays the distance on the Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Delay 500 milliseconds before next measurement
  delay(500);
}

```
The boards: 
<img width="300" src="https://github.com/user-attachments/assets/c4f1b80a-ecdf-440f-a383-ef8d0d648533" />



