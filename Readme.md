# Final Year Project (Demo)

**Note:** This repository is for demonstration purposes only. The actual source code is not included.  
All documents (SRS, scope, etc.) are available for recruiters and reviewers.  
To access the working code, please contact me directly.

📞.  Muhammad Umar +92 3072500966
Email: umer67084@gmail.com


## Project Overview
# AI-Powered Driver Monitoring and Assistance App

🚗 **AI-Powered Driver Safety Assistant**

A comprehensive Flutter application that uses ML Kit for real-time driver drowsiness detection and provides intelligent assistance to ensure road safety.

---

## ✨ Features

### 🎯 Core Functionality

- **Real-time Drowsiness Detection:**  face detection, eye closure, and yawn detection
- **Live Camera Preview:** Front camera monitoring with futuristic UI overlay
- **Alert System:** Audio, visual, and haptic alerts for drowsiness detection
- **Emergency Response:** Automatic emergency contact notification and location sharing

### 🤖 AI Assistant

- **Intelligent Chatbot:** Context-aware assistant for driver guidance
- **Rest Suggestions:** Personalized recommendations for breaks and rest stops
- **Emergency Mode:** Priority support during critical situations
- **Voice Interaction:** Future-ready voice chat capabilities

### 📍 Location Services

- **Nearby Places:** Find rest stops, gas stations, hospitals, restaurants, and hotels
- **Google Maps Integration:** Real-time navigation and directions
- **Location Sharing:** Emergency location broadcasting to contacts
- **Live GPS Tracking:** Continuous location monitoring during drives

### 📊 Data & Analytics

- **Event Logging:** Comprehensive drowsiness event history
- **Statistics Dashboard:** Daily, weekly, and monthly analytics
- **Export Functionality:** Data export for health monitoring
- **Performance Insights:** Detection accuracy and pattern analysis

### ⚙️ Advanced Settings

- **Detection Sensitivity:** Adjustable ML model parameters
- **Notification Preferences:** Customizable alert types and volumes
- **Emergency Contacts:** Manage emergency contact list and SMS alerts
- **Dark Mode Theme:** Futuristic neon-accented UI design

---
# Emergency Contacts & Automated WhatsApp Voice Message (n8n)

## Overview
This project includes an automated emergency-notification flow using n8n to send a voice message on WhatsApp to one or more emergency contacts. The flow:
1. Receives a trigger (e.g., app button, webhook).
2. Converts a text template to audio (TTS).
3. Uploads the audio to WhatsApp (media upload) or sends via Twilio/WhatsApp API.
4. Sends the audio message to the contact(s).

> Note: WhatsApp "voice messages" are typically sent as audio media via the WhatsApp Business API (or via Twilio's WhatsApp API). You will need a WhatsApp Business API or Twilio account and API credentials.

---

## Emergency contacts (example format)
Replace the example entries below with your real contacts.

- name: "Ali Khan"
  relation: "Father"
  phone: "+923001234567"
  preferred_language: "ur"
  message_template: "This is an emergency. Please call me back immediately."

- name: "Sara Ahmed"
  relation: "Friend"
  phone: "+447XXXXXXXXX"
  preferred_language: "en"
  message_template: "Emergency! Please check in at once."

(You can store these in a JSON/YAML file or a small DB; n8n can fetch them at run time.)

---

## n8n Workflow (high-level)
1. Trigger node
   - Webhook or whatever event triggers the emergency flow.

2. Set or Function node
   - Load contact list, select target(s), prepare message text (use contact.preferred_language for localization).

3. Text-to-Speech (TTS) node / HTTP request
   - Use Google Cloud Text-to-Speech, Amazon Polly, ElevenLabs, or another TTS provider.
   - Output binary audio (mp3/ogg).

   Example (pseudo):
   - HTTP Request to TTS API → receive audio binary.

4. Upload media to WhatsApp (or Twilio)
   - WhatsApp Cloud API:
     - POST /v13.0/{PHONE_NUMBER_ID}/media (multipart/form-data) to upload binary. Returns media id.
     - Then POST /v13.0/{PHONE_NUMBER_ID}/messages with body:
       {
         "messaging_product": "whatsapp",
         "to": "<recipient_phone>",
         "type": "audio",
         "audio": { "id": "<MEDIA_ID>" }
       }
     - Header: Authorization: Bearer <WHATSAPP_TOKEN>

   - Twilio WhatsApp API alternative:
     - Upload or host the audio and send as media message through Twilio's Messages API (or use Twilio Programmable Voice if you want an actual call).
## 🏗️ Architecture

### Project Structure

```
─.dart_tool
├───.idea
│   ├───caches
│   └───libraries
├───android
│   └───app
│       └───src
│           └───main
│               ├───java
│               │   └───io
│               │       └───flutter
│               │           └───plugins
│               └───kotlin
│                   └───com
│                       └───example
│                           └───driver_monitoring_app
├───assets
│   ├───fonts
│   └───sounds
├───build
├───ios
│   ├───Flutter
│   │   └───ephemeral
│   └───Runner
└───lib
    ├───app
    ├───assets
    │   ├───fonts
    │   └───sound
    ├───core
    │   ├───constants
    │   ├───services
    │   ├───theme
    │   └───utils
    ├───data
    │   ├───database
    │   ├───models
    │   └───repositories
    ├───presentation
    │   ├───providers
    │   ├───Screens
    │   └───widgets
    └───services
        ├───location_service
        ├───ml_service
        ├───notification_service
        └───sms_service

```


### Tech Stack

- **Frontend:** Flutter 3.16+ with Dart
- **State Management:** Provider pattern
- **Database:** Hive (local NoSQL)
- **ML/AI:**  (Face Detection)
- **Maps:** Google Maps API
- **Camera:** Camera plugin with ML integration
- **Notifications:** Flutter Local Notifications
- **Location:** Geolocator plugin
- **SMS:** SMS Advanced plugin
- **UI:** Custom neon-themed components with animations

---

## 📱 Screens

1. Home Screen - Main dashboard with monitoring controls
2. Detection Screen - Live camera feed with ML overlay
3. Chatbot Screen - AI assistant for driver guidance
4. Maps Screen - Nearby places finder with categories
5. Logs Screen - Event history and analytics
6. Settings Screen - App configuration and preferences
7. Emergency Screen - Critical situation response
8. Splash Screen - App loading and initialization

---

## 🚀 Setup Instructions

### Prerequisites

- Flutter SDK 3.16.0 or higher
- Dart 3.0.0 or higher
- Android Studio / VS Code with Flutter plugins
- Physical device (camera required for FACE  detection)

### Installation

1. **Clone the repository**
   
   cd FYP
   ```
2. **Install dependencies**
   ```sh
   flutter pub get
   ```
3. **Configure API Keys**
    - Get Google Maps API key from Google Cloud Console
    - Replace `YOUR_GOOGLE_MAPS_API_KEY` in:
        - `android/app/src/main/AndroidManifest.xml`
        - `lib/core/constants/app_constants.dart`
4. **Generate Hive adapters**
   ```sh
   flutter packages pub run build_runner build
   ```
5. **Run the app**
   ```sh
   flutter run
   ```

### Platform-Specific Setup

**Android**
- Minimum SDK: 21
- Target SDK: 34
- Permissions automatically requested at runtime

**iOS**
- Minimum iOS: 11.0
- Camera and location permissions configured in `Info.plist`
- Background modes enabled for continuous monitoring

---

## 🔧 Configuration

### ML Kit Detection Parameters

Edit in `app_constants.dart`:
```dart
static const double eyeClosureThreshold = 0.4;
static const double yawnDetectionThreshold = 0.6;
static const int drowsinessDetectionFrames = 5;
```

### Notification Settings

Customize alert behaviors:
```dart
static const String alertSoundPath = 'assets/sounds/alert_beep.mp3';
static const Duration alertCooldown = Duration(seconds: 10);
```

---

## 🛡️ Security & Privacy

- Local Data Storage: All detection data stored locally using Hive
- No Cloud Dependency: ML processing happens on-device
- Privacy First: No personal data transmitted to servers
- Secure SMS: Emergency contacts managed locally
- Permissions: Minimal required permissions requested

---

## 🔄 State Management

Uses Provider pattern for reactive state management:
- `DrowsinessProvider`: ML detection state and camera management
- `LocationProvider`: GPS and nearby places functionality
- `SettingsProvider`: App preferences and configuration
- `ChatbotProvider`: AI assistant conversation management

---

## 🧪 Testing

### Manual Testing Scenarios

1. Detection Accuracy: Test with various lighting conditions
2. Emergency Response: Verify SMS and location sharing
3. Performance: Monitor battery usage during detection
4. UI Responsiveness: Test on different screen sizes

---



## 🔮 Future Enhancements

### Planned Features

- Advanced ML Models: Custom drowsiness detection training
- Wearable Integration: Smartwatch heart rate monitoring
- Fleet Management: Multi-driver dashboard for companies
- Cloud Sync: Optional cloud backup with encryption
- Voice Commands: Complete voice-controlled interface
- Passenger Mode: Detection for multiple occupants

### Technical Improvements

- Offline Maps: Cached map data for remote areas
- Edge Computing: On-device AI model optimization
- Real-time Sync: Multi-device synchronization
- Advanced Analytics: Machine learning insights

---

## 📄 License

This project is licensed under the MIT License.

---



## 📞 Support

For support and questions: Muhammad Umar 03072500966
Email: umer67084@gmail.com

---


---

> ⚠️ **Important:**  
> This app is designed to assist drivers but should not replace alertness and responsible driving. Always prioritize road safety and follow traffic regulations.
