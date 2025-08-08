# Arduino UNO Line Follower Robot

[![Arduino](https://img.shields.io/badge/Arduino-UNO-blue?style=for-the-badge)](https://circuitdigest.com/microcontroller-projects/arduino-uno-line-follower-robot) [![Embedded C](https://img.shields.io/badge/Language-EmbeddedC-orange?style=for-the-badge)]() [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

This project demonstrates how to build an **Autonomous Line Follower Robot** using an **Arduino UNO**, **IR sensors**, and an **L293D motor driver**. The robot detects and follows a black line on a white surface by controlling the speed and direction of its motors based on real-time IR sensor feedback.

![Line Follower Working](https://circuitdigest.com/sites/default/files/inlineimages/u5/Line-Follower-working.gif)

---

## 🚀 Features

- Detects and follows black lines on white surfaces
- Uses dual IR sensors for left/right tracking
- Supports movement: **Forward**, **Left Turn**, **Right Turn**, **Stop**
- Powered by Arduino UNO with L293D motor driver
- Cost-effective and beginner-friendly
- Can be upgraded into a **maze-solving robot**

---

## 🛠️ Hardware Requirements

| Component              | Quantity | Description                                  |
|------------------------|----------|----------------------------------------------|
| Arduino UNO            | 1        | Main microcontroller board                   |
| L293D Motor Driver IC  | 1        | Dual H-bridge motor driver                   |
| IR Sensor Module       | 2        | Detects black/white line contrast            |
| BO Motors (60 RPM)     | 2        | Drives robot wheels                          |
| Motor Wheels           | 2        | Mounted on BO motors                         |
| Castor Wheel           | 1        | Front balancing wheel                        |
| Robot Chassis          | 1        | Base platform for mounting components        |
| 7.4V or 9V Battery     | 1        | Powers entire system                         |
| Jumper Wires, Screws   | -        | Assembly and wiring                          |

---

## ⚙️ Working Principle

The robot uses **IR sensors** to detect lines based on infrared reflectivity. Black surfaces absorb IR light, while white surfaces reflect it. When the robot's sensors detect black (low signal), it interprets a line and adjusts its motors accordingly. Basic behavior includes:

- **Both sensors on white** → move forward  
- **Left sensor on black** → turn left  
- **Right sensor on black** → turn right  
- **Both sensors on black** → stop  

IR Sensor Pinout:

![IR Sensor Pinout](https://circuitdigest.com/sites/default/files/inlineimages/u4/IR-Sensor-Pinout.png)

---

## 🧭 Navigation Logic

| Sensor State (Left, Right) | Action        |
|----------------------------|---------------|
| White, White               | Move Forward  |
| Black, White               | Turn Left     |
| White, Black               | Turn Right    |
| Black, Black               | Stop          |

### Forward

![Forward Navigation](https://circuitdigest.com/sites/default/files/inlineimages/u3/Line-Follower-Navigation.jpg)

### Left

![Left Navigation](https://circuitdigest.com/sites/default/files/inlineimages/u3/Line-Follower-Left-Navigation.png)

### Right

![Right Navigation](https://circuitdigest.com/sites/default/files/inlineimages/u3/Line-Follower-Right-Navigation.png)

### Stop

![Stop Navigation](https://circuitdigest.com/sites/default/files/inlineimages/u3/Line-Follower-STOP-Navigation.png)

---

## 🔌 Circuit Connection

- IR Sensor Output → Arduino Pins 2 (Left) and 4 (Right)  
- Motor Driver ENA/ENB → Arduino Pins 5 and 8  
- Motor Driver IN1-IN4 → Arduino Pins 6, 7, 9, 10  
- VCC (5V), GND appropriately connected  
- BO Motors connected to motor driver outputs  

![Circuit Diagram](https://circuitdigest.com/sites/default/files/circuitdiagram_mic/Line-Follower-Circuit-Diagram.png)

**L293D Pinout:**

![L293D Pinout](https://circuitdigest.com/sites/default/files/inlineimages/u3/L293D-Pinout.png)

---

## 💻 Arduino Code

```c
#define enA 5
#define in1 6
#define in2 7
#define in3 9
#define in4 10
#define enB 8
#define R_S 4 // Right sensor
#define L_S 2 // Left sensor

void setup() {
  pinMode(R_S, INPUT);
  pinMode(L_S, INPUT);
  pinMode(enA, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);
  pinMode(enB, OUTPUT);
  digitalWrite(enA, HIGH);
  digitalWrite(enB, HIGH);
}

void loop() {
  if ((digitalRead(R_S) == 0) && (digitalRead(L_S) == 0)) {
    forward();
  }
  else if ((digitalRead(R_S) == 1) && (digitalRead(L_S) == 0)) {
    turnRight();
  }
  else if ((digitalRead(R_S) == 0) && (digitalRead(L_S) == 1)) {
    turnLeft();
  }
  else if ((digitalRead(R_S) == 1) && (digitalRead(L_S) == 1)) {
    Stop();
  }
}

void forward() {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
}

void turnRight() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
}

void turnLeft() {
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
}

void Stop() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);
}
```

---

## 🧪 Troubleshooting

| Issue                     | Cause                  | Solution                                |
|--------------------------|------------------------|------------------------------------------|
| Not following the line   | IR sensor not tuned    | Adjust sensor potentiometers             |
| Erratic movement         | Low battery            | Charge or replace battery                |
| Circling or spinning     | Motor wire polarity    | Reverse motor wires                      |
| No motion                | Power or code issue    | Check enable pins and connections        |
| Line loss on curves      | Sensor position issue  | Set sensor height ~5mm above surface     |

---

## 📈 Enhancements

- PID algorithm for better control  
- Adjustable motor speed via potentiometer  
- LCD display for real-time debug info  
- Ultrasonic sensor for obstacle detection  
- Switch to L298N for more current capacity  

![L298N Wiring](https://circuitdigest.com/sites/default/files/Line-Following-Robot-Connection-Diagram.jpg)

---

## 🧪 Technical Specifications

| Parameter           | Value                                |
|---------------------|---------------------------------------|
| Controller          | Arduino UNO                           |
| Motor Driver        | L293D / Optional L298N                |
| Sensors             | 2x IR Sensor Modules                  |
| Motors              | 6V DC BO Motors (60 RPM)              |
| Operating Voltage   | 7.4V to 9V (battery powered)          |
| Motion Types        | Forward, Left, Right, Stop            |
| Estimated Cost      | $25 - $30                             |
| Build Time          | 2-3 hours                             |

---

## 🔗 Links

- 📖 [Complete Tutorial on Circuit Digest](https://circuitdigest.com/microcontroller-projects/arduino-uno-line-follower-robot)
- 📘 [IR Sensor with Arduino](https://circuitdigest.com/microcontroller-projects/interfacing-ir-sensor-module-with-arduino)
- ⚙️ [Motor Driver L293D vs L298N](https://circuitdigest.com/microcontroller-projects/interfacing-l298n-motor-driver-with-arduino)
- 💡 [More Arduino Robotics Projects](https://circuitdigest.com/arduino-robotics-projects)

---

## ⭐ Support

If you found this helpful, please ⭐ star this repository and share it with others!

---

**Built with 🤖 by [Circuit Digest](https://circuitdigest.com/)**  
_Making Electronics Simple_

---

### 🔖 Keywords

`line follower robot arduino` `L293D motor control` `IR sensor robot`  
`arduino uno robot project` `embedded C line tracking bot` `robot navigation logic`  
`autonomous arduino car` `basic robotics project` `DIY robot line follower`
