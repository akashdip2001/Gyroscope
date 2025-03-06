# Gyroscope module check

**MPU6050**, which is a common IMU (Inertial Measurement Unit) sensor with a gyroscope and accelerometer. Below is an **Arduino Nano** program to check whether the **gyroscope** is working. It reads **gyroscope data** (X, Y, Z) from the MPU6050 and prints it to the **Serial Monitor**.

---

https://github.com/user-attachments/assets/e0cbd5ce-cf62-4fba-a10f-d5bce5d15676

### **Connections:**
| MPU6050 Pin | Arduino Nano Pin |
|------------|----------------|
| VCC        | 5V            |
| GND        | GND           |
| SDA        | A4            |
| SCL        | A5            |

---

### **Code:**
```cpp
#include <Wire.h>

const int MPU6050_ADDR = 0x68;  // MPU6050 I2C address

void setup() {
    Serial.begin(9600);  // Start Serial Monitor
    Wire.begin();        // Start I2C communication

    // Wake up MPU6050 (default in sleep mode)
    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x6B);  // PWR_MGMT_1 register
    Wire.write(0);     // Set to zero (wakes up MPU6050)
    Wire.endTransmission(true);
}

void loop() {
    int16_t gyro_x, gyro_y, gyro_z;

    // Request gyroscope data
    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x43); // Starting register for gyro data
    Wire.endTransmission(false);
    Wire.requestFrom(MPU6050_ADDR, 6, true); // Request 6 bytes

    // Read raw gyroscope values
    gyro_x = (Wire.read() << 8) | Wire.read();
    gyro_y = (Wire.read() << 8) | Wire.read();
    gyro_z = (Wire.read() << 8) | Wire.read();

    // Display gyro readings
    Serial.print("Gyro X: "); Serial.print(gyro_x);
    Serial.print(" | Gyro Y: "); Serial.print(gyro_y);
    Serial.print(" | Gyro Z: "); Serial.println(gyro_z);

    delay(500);  // Wait for half a second
}
```
---

<img align="right" alt="" width="45%" src="https://github.com/user-attachments/assets/e9f796ee-13ad-4f3b-8cc4-2a74aa05f29b">

### **How It Works:**
1. The code initializes **I2C communication** using the `Wire` library.
2. It **wakes up the MPU6050** (since it's in sleep mode by default).
3. It **reads raw gyroscope data** (X, Y, Z) from the sensor registers.
4. It **prints** the gyroscope values to the Serial Monitor.
5. The program loops every **500ms**, continuously updating the readings.

---

### **How to Run:**
1. Upload this code to your **Arduino Nano** using the **Arduino IDE**.
2. Open **Serial Monitor** (`Tools > Serial Monitor`).
3. Set **baud rate** to `9600`.
4. Move/rotate the **MPU6050**, and you should see gyroscope readings change.

---

## ✅ Formatting the output cleanly  
### ✅ Adding a simple **visual representation** using text-based bars  

---

### **Updated Code with Clear Output & Visual Representation**
```cpp
#include <Wire.h>

const int MPU6050_ADDR = 0x68;  // MPU6050 I2C address

void setup() {
    Serial.begin(9600);  // Start Serial Monitor
    Wire.begin();        // Start I2C communication

    // Wake up MPU6050 (default in sleep mode)
    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x6B);  // PWR_MGMT_1 register
    Wire.write(0);     // Set to zero (wakes up MPU6050)
    Wire.endTransmission(true);

    Serial.println("MPU6050 Gyroscope Test Initialized...");
    Serial.println("Move the sensor to see changes in gyro values!");
    Serial.println("------------------------------------------------");
}

void loop() {
    int16_t gyro_x, gyro_y, gyro_z;

    // Request gyroscope data
    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x43); // Starting register for gyro data
    Wire.endTransmission(false);
    Wire.requestFrom(MPU6050_ADDR, 6, true); // Request 6 bytes

    // Read raw gyroscope values
    gyro_x = (Wire.read() << 8) | Wire.read();
    gyro_y = (Wire.read() << 8) | Wire.read();
    gyro_z = (Wire.read() << 8) | Wire.read();

    // Scale down values for better readability
    int scaled_x = gyro_x / 150;
    int scaled_y = gyro_y / 150;
    int scaled_z = gyro_z / 150;

    // Print formatted output
    Serial.println("------------------------------------------------");
    Serial.print("Gyro X: "); Serial.print(gyro_x);
    Serial.print(" | Y: "); Serial.print(gyro_y);
    Serial.print(" | Z: "); Serial.println(gyro_z);

    // Visual representation
    Serial.print("X-axis: "); drawBar(scaled_x);
    Serial.print("Y-axis: "); drawBar(scaled_y);
    Serial.print("Z-axis: "); drawBar(scaled_z);

    delay(500);  // Wait for half a second
}

// Function to print a text-based bar representation
void drawBar(int value) {
    if (value < 0) {
        Serial.print("[");
        for (int i = 0; i < -value; i++) Serial.print("-");
        Serial.println("] <");
    } else {
        Serial.print("> [");
        for (int i = 0; i < value; i++) Serial.print("+");
        Serial.println("]");
    }
}
```

https://github.com/user-attachments/assets/749e4a5e-e083-4781-97d1-cd391a4db0f7

```cpp
#include <Wire.h>

const int MPU6050_ADDR = 0x68;  // MPU6050 I2C address
int16_t gyro_x, gyro_y, gyro_z;

void setup() {
    Serial.begin(9600);  // Start Serial Monitor
    Wire.begin();        // Start I2C communication

    // Wake up MPU6050
    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x6B);  // PWR_MGMT_1 register
    Wire.write(0);     // Set to zero (wakes up MPU6050)
    Wire.endTransmission(true);

    Serial.println("MPU6050 Drone Simulation Initialized...");
    Serial.println("Move the sensor to see directional changes!");
}

void loop() {
    // Request gyroscope data
    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x43); // Starting register for gyro data
    Wire.endTransmission(false);
    Wire.requestFrom(MPU6050_ADDR, 6, true); // Request 6 bytes

    // Read gyroscope values
    gyro_x = (Wire.read() << 8) | Wire.read();
    gyro_y = (Wire.read() << 8) | Wire.read();
    gyro_z = (Wire.read() << 8) | Wire.read();

    // Define thresholds for movement
    int threshold = 800;  // Adjust based on sensitivity

    Serial.println("\n------------------------------");
    Serial.println("          DRONE STATUS        ");
    Serial.println("------------------------------");

    // Determine movement direction
    if (gyro_x > threshold) {
        Serial.println("➡️  Moving RIGHT");
    } else if (gyro_x < -threshold) {
        Serial.println("⬅️  Moving LEFT");
    }

    if (gyro_y > threshold) {
        Serial.println("⬆️  Moving FORWARD");
    } else if (gyro_y < -threshold) {
        Serial.println("⬇️  Moving BACKWARD");
    }

    if (gyro_z > threshold) {
        Serial.println("↻ Rotating CLOCKWISE");
    } else if (gyro_z < -threshold) {
        Serial.println("↺ Rotating COUNTERCLOCKWISE");
    }

    if (gyro_x < threshold && gyro_x > -threshold && 
        gyro_y < threshold && gyro_y > -threshold && 
        gyro_z < threshold && gyro_z > -threshold) {
        Serial.println("✈️  Drone is STABLE");
    }

    delay(500);  // Update every 500ms
}
```

https://github.com/user-attachments/assets/e2513c7b-a7df-48af-a298-d77224926220

✅ **How to Read Output Easily?**  
- **Numbers** show actual gyro values.  
- **Bars (`+` and `-`)** show direction and intensity of movement.  
- `" > "` means positive movement, `" < "` means negative movement.

---

# **Real-time moving rectangle** that reacts to your MPU6050 movements, like a **drone simulation**.

[![Screenshot (154)](https://github.com/user-attachments/assets/8164d561-42fa-46dd-b8dc-cf41c096a046)](https://processing.org/download)  

🚀 **why not in Adrino IDE ?**  
Since the **Serial Monitor** only displays text, it **cannot show graphical movement** directly.  
✅ Instead, I will create a **Processing sketch** (a program that runs on your PC) to visualize the rectangle moving based on your hand movements!  

---

### **🛠 SetUp**
1. **Arduino Nano + MPU6050** (with I2C connected)
2. **Processing IDE** (Download from [processing.org](https://processing.org/))
3. **Install the `Serial` Library in Processing** (It's built-in)

---

# 2D simulation

### **Step 1: Upload This Code to Arduino Nano**
This code **reads MPU6050 data** and **sends it to the PC** via Serial.
```cpp
#include <Wire.h>

const int MPU6050_ADDR = 0x68;
int16_t gyro_x, gyro_y;

void setup() {
    Serial.begin(115200);  
    Wire.begin();  

    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x6B);  
    Wire.write(0);  
    Wire.endTransmission(true);
}

void loop() {
    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x43);  
    Wire.endTransmission(false);
    Wire.requestFrom(MPU6050_ADDR, 4, true);  

    gyro_x = (Wire.read() << 8) | Wire.read();
    gyro_y = (Wire.read() << 8) | Wire.read();

    // Scale values for better control
    int scaled_x = map(gyro_x, -15000, 15000, -10, 10);
    int scaled_y = map(gyro_y, -15000, 15000, -10, 10);

    Serial.print(scaled_x);
    Serial.print(",");
    Serial.println(scaled_y);

    delay(50);  
}
```
---

### **Step 2: Run This Code in Processing**
Processing **receives Serial data** and **moves a rectangle** based on your hand movement.  

```java
import processing.serial.*;

Serial port;  
int rectX, rectY;  

void setup() {
  size(600, 600);  
  port = new Serial(this, Serial.list()[0], 115200);  // Adjust port if needed
  rectX = width / 2;
  rectY = height / 2;
}

void draw() {
  background(0);
  fill(255, 0, 0);
  rect(rectX, rectY, 50, 50);

  while (port.available() > 0) {
    String data = port.readStringUntil('\n');
    if (data != null) {
      data = trim(data);
      String[] values = split(data, ',');
      if (values.length == 2) {
        int moveX = int(values[0]);
        int moveY = int(values[1]);

        rectX += moveX;
        rectY += moveY;

        // Keep rectangle inside window
        rectX = constrain(rectX, 0, width - 50);
        rectY = constrain(rectY, 0, height - 50);
      }
    }
  }
}
```

---

### **🚀 How It Works**
- The **Arduino reads gyroscope values** and sends **scaled X, Y movement** via Serial.  
- **Processing receives these values** and moves a **rectangle** on the screen.  
- **When you tilt the MPU6050**, the rectangle **moves like a drone**!

---

### **📌 How to Run This?**
1. **Upload the Arduino code** to your Nano.  
2. **Open Processing** and **paste the Processing code** above.  
3. **Run the Processing Sketch** → You’ll see a **rectangle moving with your MPU6050**!

https://github.com/user-attachments/assets/e98f0834-b109-47a6-a3ed-fbd38ad1b027

---

# 3D simulation

---

### **🛠 What We Will Do:**
✅ **Use Processing** to create a **3D cube (or drone shape)**  
✅ **Use MPU6050 data** to control **pitch, roll, and yaw**  
✅ **Display real-time movement** that matches your hand motion  

---

### **Step 1: Upload This Code to Arduino Nano**
This code **reads** the **gyro data** from the MPU6050 and **sends it over Serial**.  

```cpp
#include <Wire.h>

const int MPU6050_ADDR = 0x68;
int16_t gyro_x, gyro_y, gyro_z;

void setup() {
    Serial.begin(115200);
    Wire.begin();

    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x6B);
    Wire.write(0);
    Wire.endTransmission(true);
}

void loop() {
    Wire.beginTransmission(MPU6050_ADDR);
    Wire.write(0x43);  
    Wire.endTransmission(false);
    Wire.requestFrom(MPU6050_ADDR, 6, true);

    gyro_x = (Wire.read() << 8) | Wire.read();
    gyro_y = (Wire.read() << 8) | Wire.read();
    gyro_z = (Wire.read() << 8) | Wire.read();

    // Scale values for smooth rotation
    float scaled_x = gyro_x / 150.0;
    float scaled_y = gyro_y / 150.0;
    float scaled_z = gyro_z / 150.0;

    Serial.print(scaled_x);
    Serial.print(",");
    Serial.print(scaled_y);
    Serial.print(",");
    Serial.println(scaled_z);

    delay(50);
}
```

---

### **Step 2: Run This 3D Simulation in Processing**
We use **Processing + PeasyCam** to create a **real-time 3D cube (drone) simulation**.  

```java
import processing.serial.*;
import peasy.*;

Serial port;
PeasyCam cam;
float roll = 0, pitch = 0, yaw = 0;

void setup() {
  size(800, 800, P3D);
  cam = new PeasyCam(this, 500);
  port = new Serial(this, Serial.list()[0], 115200);
}

void draw() {
  background(0);
  lights();
  
  translate(0, 0, 0);
  rotateX(radians(pitch));
  rotateY(radians(yaw));
  rotateZ(radians(roll));
  
  fill(0, 255, 0);
  stroke(255);
  
  box(100);  // 3D Cube (Can be replaced with a drone model)
  
  while (port.available() > 0) {
    String data = port.readStringUntil('\n');
    if (data != null) {
      data = trim(data);
      String[] values = split(data, ',');
      if (values.length == 3) {
        roll = float(values[0]);
        pitch = float(values[1]);
        yaw = float(values[2]);
      }
    }
  }
}
```

---

### **🚀 What Happens Here**
- The **Arduino reads** **pitch, roll, yaw** and sends it via Serial.  
- **Processing reads the values** and applies them to a **3D cube (your drone model)**.  
- The cube **tilts, rotates, and moves** like your **real MPU6050 sensor!**  

---

### **📌 How to Run This**
1. **Upload the Arduino code** to your Nano.  
2. **Install Processing** ([Download Here](https://processing.org/download))  
3. **Install PeasyCam Library** (Go to `Sketch > Import Library > Add Library > Search "PeasyCam" > Install`)  
4. **Run the Processing Sketch**  
5. **Move your MPU6050** → Watch the **3D cube tilt, turn, and rotate** in real-time!

---

### If You get error with Add Library

If you're missing the **PeasyCam library** in Processing. Let's fix that.  

---

### **✅ Fix: Install PeasyCam in Processing**
1. Open **Processing IDE**.  
2. Go to **Sketch > Import Library > Add Library**.  
3. In the search bar, type **"PeasyCam"**.  
4. Click **Install** and wait for it to complete.  
5. Restart Processing and try running the code again.  

---

### **🛠 Alternative Without PeasyCam (Using Manual 3D Rotation)**
If you want a **simple version without PeasyCam**, replace the code with this:

```java
import processing.serial.*;

Serial port;
float roll = 0, pitch = 0, yaw = 0;

void setup() {
  size(800, 800, P3D);
  port = new Serial(this, Serial.list()[0], 115200);
}

void draw() {
  background(0);
  lights();

  translate(width / 2, height / 2, 0);
  rotateX(radians(pitch));
  rotateY(radians(yaw));
  rotateZ(radians(roll));

  fill(0, 255, 0);
  stroke(255);
  box(100);  // 3D Cube

  while (port.available() > 0) {
    String data = port.readStringUntil('\n');
    if (data != null) {
      data = trim(data);
      String[] values = split(data, ',');
      if (values.length == 3) {
        roll = float(values[0]);
        pitch = float(values[1]);
        yaw = float(values[2]);
      }
    }
  }
}
```

### **📌 How This Works**
- **Removes PeasyCam** but keeps the **3D movement.**  
- Uses **rotateX(), rotateY(), rotateZ()** for real-time rotation.  
- Cube moves **based on MPU6050 data** like a drone.

https://github.com/user-attachments/assets/dcd72a90-6dd1-469e-a8ad-ff456e0b5d66
