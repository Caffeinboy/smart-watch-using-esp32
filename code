#define BLYNK_TEMPLATE_ID "YOUR_TEMPLATE_ID"
#define BLYNK_TEMPLATE_NAME "Smart Watch"
#define BLYNK_AUTH_TOKEN "YOUR_AUTH_TOKEN"

#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <Wire.h>
#include "MAX30105.h"
#include <heartRate.h>
#include <Adafruit_MLX90614.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <MPU6050.h>

// ---------------- WiFi ----------------
char ssid[] = "YOUR_WIFI_NAME";
char pass[] = "YOUR_WIFI_PASSWORD";

// ---------------- OLED ----------------
#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// ---------------- Sensors ----------------
MAX30105 particleSensor;
Adafruit_MLX90614 mlx = Adafruit_MLX90614();
MPU6050 mpu;

BlynkTimer timer;

// ---------------- Variables ----------------
float bodyTemp = 0;
int bpm = 0;
int spo2 = 0; 
int stepCount = 0;
bool fallDetected = false;

long lastBeat = 0;
float beatsPerMinute;
int beatAvg;

float ax, ay, az;
float totalAcceleration;

// ---------------- Step Detection ----------------
float stepThreshold = 1.35;
bool stepState = false;

// ---------------- Fall Detection ----------------
float fallThreshold = 2.5;

// =====================================================
// Read MAX30102 Heart Rate
// =====================================================
void readHeartRate() {
  long irValue = particleSensor.getIR();

  if (checkForBeat(irValue) == true) {
    long delta = millis() - lastBeat;
    lastBeat = millis();

    beatsPerMinute = 60 / (delta / 1000.0);

    if (beatsPerMinute < 255 && beatsPerMinute > 40) {
      bpm = (int)beatsPerMinute;
      beatAvg = bpm;
    }
  }

  if (irValue < 50000) {
    bpm = 0; // finger not detected
  }
}

// =====================================================
// Read Temperature
// =====================================================
void readTemperature() {
  bodyTemp = mlx.readObjectTempC();
}

// =====================================================
// Read MPU6050 for Steps + Fall
// =====================================================
void readMotion() {
  int16_t AcX, AcY, AcZ;
  mpu.getAcceleration(&AcX, &AcY, &AcZ);

  ax = AcX / 16384.0;
  ay = AcY / 16384.0;
  az = AcZ / 16384.0;

  totalAcceleration = sqrt((ax * ax) + (ay * ay) + (az * az));

  // Step Detection
  if (totalAcceleration > stepThreshold && !stepState) {
    stepCount++;
    stepState = true;
  }

  if (totalAcceleration < 1.0) {
    stepState = false;
  }

  // Fall Detection
  if (totalAcceleration > fallThreshold) {
    fallDetected = true;
  } else {
    fallDetected = false;
  }
}

// =====================================================
// OLED Display
// =====================================================
void updateDisplay() {
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(WHITE);

  display.setCursor(0, 0);
  display.print("BPM: ");
  display.println(bpm);

  display.print("SpO2: ");
  display.print(spo2);
  display.println(" %");

  display.print("Temp: ");
  display.print(bodyTemp);
  display.println(" C");

  display.print("Steps: ");
  display.println(stepCount);

  display.print("Fall: ");
  display.println(fallDetected ? "YES" : "NO");

  display.display();
}

// =====================================================
// Send to Blynk
// =====================================================
void sendToBlynk() {
  Blynk.virtualWrite(V0, bpm);
  Blynk.virtualWrite(V1, spo2);
  Blynk.virtualWrite(V2, bodyTemp);
  Blynk.virtualWrite(V3, fallDetected ? 1 : 0);
  Blynk.virtualWrite(V4, stepCount);
}

// =====================================================
// Setup
// =====================================================
void setup() {
  Serial.begin(115200);
  Wire.begin();

  // OLED
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED failed");
    while (1);
  }

  display.clearDisplay();
  display.display();

  // MAX30102
  if (!particleSensor.begin(Wire, I2C_SPEED_FAST)) {
    Serial.println("MAX30102 not found");
    while (1);
  }

  particleSensor.setup();
  particleSensor.setPulseAmplitudeRed(0x0A);
  particleSensor.setPulseAmplitudeGreen(0);

  // MLX90614
  if (!mlx.begin()) {
    Serial.println("MLX90614 not found");
  }

  // MPU6050
  Serial.println("Initializing MPU6050...");
  mpu.initialize();

  if (!mpu.testConnection()) {
    Serial.println("MPU6050 connection failed");
  } else {
    Serial.println("MPU6050 connected");
  }

  // WiFi + Blynk
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  timer.setInterval(1000L, []() {
    readHeartRate();
    readTemperature();
    readMotion();
    updateDisplay();
    sendToBlynk();

    Serial.print("BPM: ");
    Serial.print(bpm);
    Serial.print(" | Temp: ");
    Serial.print(bodyTemp);
    Serial.print(" | Steps: ");
    Serial.print(stepCount);
    Serial.print(" | Fall: ");
    Serial.println(fallDetected ? "YES" : "NO");
  });
}

// =====================================================
// Loop
// =====================================================
void loop() {
  Blynk.run();
  timer.run();
}
