---
title: 'Show HN: JBR-001 – An open-source 3D printable desktop robot'
link: https://projecthub.arduino.cc/syntheticaidata/jbr-001-a-desktop-companion-robot-powered-by-arduino-uno-q-b11c96
source: hnrss-org
published: 2026-09-29T10:05:56Z
updated: 2026-09-29T10:05:56Z
first_seen: 2026-09-30T20:48:45.272500234Z
authors:
- gvuksic
summary: 'We built a desktop companion robot that is powered by Arduino UNO Q. Project is open source: source code, 3D printable files and assembly instructions are available at Arduino Project Hub. Robot is equipped with camera, distance sensor, buzzer, moves head and arms with 3 servo motors and has an animated display. It can recognise objects and respond to it. We build it with idea this should be fun project people can 3D print and build at home themself. Comments URL: https://news.ycombinator.com/item?id=49890707 Points: 115 # Comments: 28'
content: extracted
html: 2026-09-29-show-hn-jbr-001-an-open-source-3d-printable-desktop-robot.html
preview:
  file: 2026-09-29-show-hn-jbr-001-an-open-source-3d-printable-desktop-robot.preview-305911645c07.webp
  width: 256
  height: 192
  color: '#334973'
images:
- source: https://projects.arduinocontent.cc/cover-images/5dff6f3b-25f5-492f-96f2-0b5db5018528.jpg
  original:
    file: 2026-09-29-show-hn-jbr-001-an-open-source-3d-printable-desktop-robot.image-6ff8dfdf1891.jpg
    width: 900
    height: 675
  color: '#000816'
---

```
1#include <Servo.h>
2#include <Modulino.h>
3#include "Arduino_LED_Matrix.h"
4
5// JBR-001 hardware
6Servo headServo;
7Servo leftArmServo;
8Servo rightArmServo;
9
10ModulinoBuzzer buzzer;
11ModulinoDistance distanceSensor;
12
13ArduinoLEDMatrix matrix;
14
15// Servo pins
16const int HEAD_SERVO_PIN = 9;
17const int LEFT_ARM_SERVO_PIN = 10;
18const int RIGHT_ARM_SERVO_PIN = 11;
19
20// Servo resting positions
21const int HEAD_CENTER = 90;
22const int LEFT_ARM_CENTER = 90;
23const int RIGHT_ARM_CENTER = 90;
24
25// Distance detection
26const float DETECTION_DISTANCE = 200.0;
27const float RESET_DISTANCE = 250.0;
28
29bool objectDetected = false;
30
31// Small heart
32// flipped to match display orientation
33uint8_t heartSmall[8][13] = {
34  {0,0,0,0,0,0,0,0,0,0,0,0,0},
35  {0,0,0,0,0,1,1,1,0,0,0,0,0},
36  {0,0,0,0,1,1,1,1,1,0,0,0,0},
37  {0,0,0,1,1,1,1,1,1,1,0,0,0},
38  {0,0,1,1,1,1,1,1,1,1,1,0,0},
39  {0,0,1,1,1,1,0,1,1,1,1,0,0},
40  {0,0,0,1,1,0,0,0,1,1,0,0,0},
41  {0,0,0,0,0,0,0,0,0,0,0,0,0}
42};
43
44
45// Large heart
46// flipped to match display orientation
47uint8_t heartLarge[8][13] = {
48  {0,0,0,0,1,1,1,1,1,0,0,0,0},
49  {0,0,0,1,1,1,1,1,1,1,0,0,0},
50  {0,0,1,1,1,1,1,1,1,1,1,0,0},
51  {0,1,1,1,1,1,1,1,1,1,1,1,0},
52  {1,1,1,1,1,1,1,1,1,1,1,1,1},
53  {1,1,1,1,1,1,1,1,1,1,1,1,1},
54  {0,1,1,1,1,1,0,1,1,1,1,1,0},
55  {0,0,1,1,1,0,0,0,1,1,1,0,0}
56};
57
58// Heartbeat animation
59unsigned long heartbeatTimer = 0;
60int heartbeatStep = 0;
61
62void heartbeat() {
63  unsigned long now = millis();
64
65  switch (heartbeatStep) {
66
67    case 0:
68      matrix.renderBitmap(heartLarge, 8, 13);
69      heartbeatTimer = now;
70      heartbeatStep = 1;
71      break;
72
73    case 1:
74      if (now - heartbeatTimer >= 120) {
75        matrix.renderBitmap(heartSmall, 8, 13);
76        heartbeatTimer = now;
77        heartbeatStep = 2;
78      }
79      break;
80
81    case 2:
82      if (now - heartbeatTimer >= 100) {
83        matrix.renderBitmap(heartLarge, 8, 13);
84        heartbeatTimer = now;
85        heartbeatStep = 3;
86      }
87      break;
88
89    case 3:
90      if (now - heartbeatTimer >= 160) {
91        matrix.renderBitmap(heartSmall, 8, 13);
92        heartbeatTimer = now;
93        heartbeatStep = 4;
94      }
95      break;
96
97    case 4:
98      if (now - heartbeatTimer >= 700) {
99        heartbeatStep = 0;
100      }
101      break;
102  }
103}
104
105// Play JBR-001's friendly greeting
106void playHello() {
107  buzzer.tone(523, 120);   // C5
108  delay(150);
109
110  buzzer.tone(659, 120);   // E5
111  delay(150);
112
113  buzzer.tone(784, 180);   // G5
114  delay(210);
115
116  buzzer.tone(1047, 250);  // C6
117  delay(270);
118}
119
120// Perform JBR-001's startup movement
121void makeMovement() {
122
123  // Look left, right, then forward
124  headServo.write(70);
125  delay(400);
126
127  headServo.write(110);
128  delay(400);
129
130  headServo.write(HEAD_CENTER);
131  delay(400);
132
133  // Move both arms
134  leftArmServo.write(70);
135  rightArmServo.write(110);
136  delay(500);
137
138  leftArmServo.write(110);
139  rightArmServo.write(70);
140  delay(500);
141
142  // Return to resting position
143  leftArmServo.write(LEFT_ARM_CENTER);
144  rightArmServo.write(RIGHT_ARM_CENTER);
145  delay(500);
146}
147
148void setup() {
149  Modulino.begin();
150
151  buzzer.begin();
152  distanceSensor.begin();
153  matrix.begin();
154
155  // Show the heart while JBR-001 starts
156  matrix.renderBitmap(heartSmall, 8, 13);
157
158  // Move each servo to its resting position
159  headServo.attach(HEAD_SERVO_PIN);
160  headServo.write(HEAD_CENTER);
161  delay(400);
162
163  leftArmServo.attach(LEFT_ARM_SERVO_PIN);
164  leftArmServo.write(LEFT_ARM_CENTER);
165  delay(400);
166
167  rightArmServo.attach(RIGHT_ARM_SERVO_PIN);
168  rightArmServo.write(RIGHT_ARM_CENTER);
169  delay(400);
170
171  delay(500);
172
173  // Say hello and come to life
174  playHello();
175  makeMovement();
176}
177
178void loop() {
179
180  // Keep the heart beating
181  heartbeat();
182
183  // React when someone approaches
184  if (distanceSensor.available()) {
185    float distance = distanceSensor.get();
186
187    if (distance < DETECTION_DISTANCE && !objectDetected) {
188      objectDetected = true;
189      playHello();
190    }
191
192    // Ready for the next greeting once they move away
193    if (distance > RESET_DISTANCE) {
194      objectDetected = false;
195    }
196  }
197}
```
