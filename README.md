# **TinyML Speech Command Recognition on ESP32 for Voice Control**
Real-time, low-power, offline speech recognition using Edge Impulse and TensorFlow Lite for Microcontrollers.

---

## **Overview**

This project implements a TinyML-based speech command recognition system running entirely on an ESP32 with an INMP441 I2S microphone, enabling real-time, offline voice control with ultra-low power consumption.

The model is trained and optimized in Edge Impulse using a tuned MFCC pipeline and a compact 1D CNN architecture. The final quantized model performs fully on-device inference without cloud connectivity.

---

## **Features**

* Real-time on-device inference (**266 ms latency**)
* High recognition confidence (**97.1% inference confidence**)
* Fully offline (no internet required)
* Quantized int8 model optimized with EON Compiler
* Low memory usage (**–37% RAM**, **–27% ROM**)
* Strong noise rejection (**99.6% confidence**)
* Works on low-cost ESP32 hardware

---

## **Recognized Commands**

* `forward`
* `backward`
* `left`
* `right`
* `stop`
* `noise`

---

## **Dataset**

* Derived from **Google Speech Commands Dataset**
* 1,500 curated samples per class
* Noise augmentation using **Himel’s Python curation script**
* Train/validation split handled in Edge Impulse

---

## **Methodology**

### **1. MFCC Feature Extraction**

Optimized parameters:

* 20 MFCC coefficients
* 40 mel filters
* FFT length: 256
* Frame length: 17.5 ms
* Frame stride: 25 ms
* Pre-emphasis: 0.96

These settings balance accuracy, spectral richness, and MCU efficiency.

### **2. Neural Network Architecture**

* 1D CNN with **4 convolutional blocks**
* Batch Normalization + MaxPooling
* Dense Softmax output
* Dropout **0.35**
* Training: **110 epochs**, batch size **32**, LR = 0.0025

### **3. Deployment Workflow**

* Post-processing calibration (threshold, FAR, FRR)
* Quantized to int8
* EON Compiler optimization
* Exported as Arduino library
* Deployed on ESP32 with INMP441 microphone

---

## **Performance**

### **Testing Performance (Edge Impulse)**

* Accuracy: **87.14%**

### **On-Device Inference (ESP32)**

* Average confidence: **97.1%**
* Noise rejection: **99.6%**
* Latency: **266 ms**

---

## **Comparison with State-of-the-Art**

| Study              | Model                         | Platform         | Accuracy   | Notes                                                        |
| ------------------ | ----------------------------- | ---------------- | ---------- | ------------------------------------------------------------ |
| Vygon et al.       | TripletLoss ResNet            | Raspberry Pi 3B+ | 98.56%     | Runs on SBC, not MCU                                         |
| Sharifuddin et al. | 2D CNN + MFCC                 | Raspberry Pi 3B+ | 95.3%      | More powerful hardware                                       |
| Sutikno et al.     | CNN + MFCC                    | Raspberry Pi 3   | 90%        | SBC-level resources                                          |
| Al-Rousan et al.   | FNN + DWT                     | TI DSP Kit       | 98%        | High-end DSP                                                 |
| Sutikno et al.     | CNN + LSTM                    | Raspberry Pi 3   | 97.8%      | Requires larger memory                                       |
| **This project**   | **1D CNN + MFCC (Quantized)** | **ESP32 MCU**    | **87.14%** | Fully on-device, 266 ms latency, low power, optimized memory |

---

## **Hardware Requirements**

* ESP32-WROOM-32
* INMP441 I2S MEMS microphone
* USB cable
* Optional jumper wires / breadboard

---

## **Software Requirements**

* Edge Impulse Studio
* Arduino IDE
* ESP32 Arduino Core
* Python 3.x
* TensorFlow Lite for Microcontrollers

---

## **Installation & Usage**

### 1. Clone the repository

```
git clone https://github.com/<your-username>/TinyML-ESP32-Voice-Control
```

### 2. Install ESP32 board support in Arduino IDE

### 3. Open and upload the Edge Impulse `.ino` file

* Connect ESP32
* Select board: “ESP32 Dev Module”
* Upload

### 4. Test the system

* Open Serial Monitor
* Speak commands near the microphone
* Observe predicted labels and confidence values

---

## **Project Structure**

```
/dataset/          # Curated audio samples (optional)
/edge_impulse/     # Exported model files
/firmware/         # Arduino deployment code
/scripts/          # Himel-based curation scripts
/docs/             # Figures, graphs, confusion matrices
README.md
```

---

## **Applications**

* Offline voice control for IoT
* Low-power robotics
* Assistive devices
* Smart home interfaces
* Industrial control
* Wearables

---

## **Built With**

* Edge Impulse
* Himel’s dataset curation script (Python)
* TensorFlow Lite for Microcontrollers
* ESP32-WROOM-32
* INMP441 I2S Microphone
* Arduino IDE
* Google Speech Commands Dataset
* C/C++
