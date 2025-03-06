# Gyroscope

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


