# **TinyML Speech Command Recognition on ESP32 for Voice Control**

**Real-time, offline, low-power voice interface for industrial robots, autonomous systems, and assistive wheelchairs**

---

## **Overview**

This project demonstrates that **speech recognition can run entirely on a low-cost ESP32 microcontroller**, enabling **real-time, offline voice control** for embedded systems where cloud connectivity, latency, and high power consumption are unacceptable.

Using **Edge Impulse**, **MFCC-based feature extraction**, a **1D CNN optimized for microcontrollers**, and the **EON Compiler**, this system provides a fast, lightweight, and deployable TinyML solution suitable for safety-critical and accessibility-focused applications.

The result is a fully operational **voice-controlled embedded module** that can be integrated into:

* **Industrial robots**
* **Smart mobility devices**
* **Assistive wheelchairs**
* **IoT automation systems**
* **Human-machine interfaces requiring hands-free operation**

This work proves that reliable voice interaction does not require cloud servers, high-end SBCs, or expensive hardware—**microcontrollers can be enough**.

---

## **Key Features**

* **Real-time inference** on microcontroller (266 ms total latency)
* **High on-device confidence** (97.1% average)
* **Strong noise rejection** (99.6%)
* **Fully offline**, no cloud or network dependency
* **Fully quantized int8 model**
* **Optimized memory footprint** (–37% RAM, –27% ROM)
* Support for **six core commands** used in robotic control
* Deployable for **industrial safety, mobility aids, autonomous navigation**

---

## **Innovation and Relevance**

Traditional speech recognition requires cloud services or powerful SBCs such as Raspberry Pi. These approaches fail in environments where:

* Network reliability varies
* Privacy is required
* Latency cannot be tolerated
* Power is limited (battery-operated systems)
* Hardware cost must remain minimal

This project shows that an ESP32—just a few dollars in cost—can run a complete **TinyML-based voice interface**.
This makes it a promising component for:

* **Voice-controlled wheelchairs** (hands-free navigation, accessibility)
* **Factory robots** (operator commands without physical controls)
* **Autonomous carts and AGVs**
* **Maintenance tools and wearable devices**
* **Robotic arms and collaborative robots (cobots)**

It bridges the gap between **human voice commands** and **low-power embedded intelligence**.

---

## **Recognized Commands**

The system supports the following commands essential for robotic navigation and control:

* `forward`
* `backward`
* `left`
* `right`
* `stop`
* `noise` (background rejection)

---

## **Dataset**

* Based on six classes from the **Google Speech Commands Dataset**
* 1,500 curated samples per command
* Noise-mixed augmentation using **Himel’s audio curation script** (Python)
* Real-world noise injection to increase robustness

Train/validation split handled in Edge Impulse.

---

## **Methodology**

### **1. MFCC Feature Extraction**

Optimized through iterative trade-offs:

* 20 MFCC coefficients
* 40 mel filterbanks
* FFT length: 256
* Frame length: 17.5 ms
* Frame stride: 25 ms
* Pre-emphasis: 0.96

These settings maximize speech information richness while keeping compute load manageable for ESP32.

---

### **2. Neural Network Architecture**

A compact **1D Convolutional Neural Network**:

* 4 Conv1D blocks
* BatchNorm + ReLU
* MaxPooling layers
* Dense Softmax output
* Dropout: 0.35
* Training: 110 epochs
* Batch size: 32
* Optimizer: Adam (LR = 0.0025)

Designed specifically for TinyML deployment.

---

### **3. Deployment Pipeline (Edge Impulse)**

* Model calibration (FAR/FRR, threshold 0.46, suppression windows)
* Quantization to **int8**
* Compiled with **EON Compiler**
* Memory footprint reduced by 37% RAM / 27% Flash
* Exported as **Arduino library**
* Integrated with ESP32 firmware
* Connected to **INMP441 I2S MEMS microphone**
* Real-time inference loop on-device

---

## **System Performance**

### **Edge Impulse Testing**

* **Accuracy:** 87.14%
* High precision, recall, and F1-scores
* Strong generalization to unseen data

### **On-Device ESP32 Performance**

* **97.1% average inference confidence**
* **99.6% noise rejection**
* **266 ms total latency** (MFCC + NN)
* Runs fully offline, low power

---

## **Comparison with State-of-the-Art**

| Study              | Model                         | Platform         | Accuracy   | Notes                                              |
| ------------------ | ----------------------------- | ---------------- | ---------- | -------------------------------------------------- |
| Vygon et al.       | TripletLoss ResNet            | Raspberry Pi 3B+ | 98.56%     | High-power SBC, not MCU                            |
| Sharifuddin et al. | 2D CNN + MFCC                 | Raspberry Pi 3B+ | 95.3%      | Requires more compute                              |
| Sutikno et al.     | CNN + MFCC                    | Raspberry Pi 3   | 90%        | SBC-level hardware                                 |
| Al-Rousan et al.   | FNN + DWT                     | TI DSP Kit       | 98%        | High-end DSP solution                              |
| Sutikno et al.     | CNN + LSTM                    | Raspberry Pi 3   | 97.8%      | Larger memory footprint                            |
| **This project**   | **1D CNN + MFCC (Quantized)** | **ESP32 (MCU)**  | **87.14%** | Real-time, ultra-low-power, fully on-device TinyML |

**Key takeaway:**
Competes with higher-performance platforms while running on a microcontroller, enabling voice interfaces in places where SBCs are too power-hungry or expensive.

---

## **Applications**

This TinyML system is suitable for embedded voice control in:

### **Assistive Technologies**

* Voice-controlled wheelchairs
* Rehabilitation robots
* Hands-free mobility devices

### **Industrial Robotics**

* Voice-operated AGVs
* Factory robots requiring safe operator interaction
* Human-machine voice interfaces

### **IoT + Smart Environments**

* Smart appliances
* Home automation
* Energy-efficient voice-controlled hubs

### **Wearables**

* Fitness trackers
* Medical alert devices
* Navigation aids

---

## **Hardware Requirements**

* ESP32-WROOM-32
* INMP441 I2S MEMS microphone
* USB cable
* (Optional) Robot platform for movement control

---

## **Software Requirements**

* Edge Impulse Studio
* Arduino IDE
* ESP32 Arduino Core
* TensorFlow Lite for Microcontrollers
* Python 3.x

---

## **Installation & Usage**

### 1. Clone the repository

```
git clone https://github.com/Maglez-UP/TinyML-ESP32-Voice-Control-EdgeAIHackathon
```

### 2. Install ESP32 support in Arduino IDE

### 3. Load and upload the Edge Impulse firmware library

### 4. Run the system

Open the Serial Monitor, speak commands, and observe live predictions.
If connected to a robot, the commands will control its motion in real time.

---

## **Project Structure**

```
/data_speech_commands/         # Audio samples (optional)
/edge_impulse/                 # Exported model files
/Arduino_script/               # Arduino deployment code
README.md
```

---

## **Built With**

* Edge Impulse
* Himel’s dataset curation script
* TensorFlow Lite for Microcontrollers
* ESP32-WROOM-32
* INMP441 I2S Microphone
* Arduino IDE
* Google Speech Commands Dataset
* C/C++ firmware
