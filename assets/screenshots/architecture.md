# CraftConnect System Architecture
## 1. System overview
CraftConnect is a Flutter marketplace and artisan business-management app backed by a FastAPI service. It combines:
- A Flutter client for Android, iOS, web, and desktop targets.
- A FastAPI backend for authentication, catalog management, orders, analytics, notifications, and media processing.
- MongoDB for application data.
- Cloudinary for hosted product images and artisan audio.
- Groq Whisper for voice-note transcription.
- Groq Vision for grounded bilingual product-listing generation.
- `rembg`, OpenCV, and Pillow for product-photo enhancement.
The primary business loop is:
```text
Artisan captures product photo + voice description
                    |
                    v
          Flutter Image Studio
                    |
                    v
             FastAPI backend
          /                     \
         v                       v
 AI image enhancement       AI catalog generation
 rembg + OpenCV + Pillow    Whisper + Groq Vision
         |                       |
         +-----------+-----------+
                     v
           Artisan reviews listing
                     |
                     v
             Product is published
                     |
                     v
          Buyer marketplace and cart
                     |
                     v
              Orders and tracking
                     |
                     v
        Artisan analytics and notifications
```
## 2. High-level architecture
```text
┌──────────────────────────────────────────────────────────────────┐
│ Flutter client                                                   │
│                                                                  │
│ Android · iOS · Web · Desktop                                    │
│                                                                  │
│ Auth · Image Studio · Auto Catalog · Marketplace · Cart · Orders  │
│ Dashboard · Notifications · Localization · Profile               │
└──────────────────────────────┬───────────────────────────────────┘
                               │ HTTPS / JSON / multipart uploads
                               v
┌──────────────────────────────────────────────────────────────────┐
│ FastAPI application                                               │
│                                                                  │
│ Auth & profiles │ Products │ Orders │ Analytics │ Notifications   │
│ AI listing      │ Image enhancement │ Audio preview              │
└───────┬────────────────┬────────────────┬────────────────┬───────┘
        │                │                │                │
        v                v                v                v
┌──────────────┐  ┌──────────────┐  ┌──────────────┐  ┌───────────┐
│ MongoDB      │  │ Groq APIs    │  │ Cloudinary   │  │ Media     │
│              │  │              │  │              │  │ tooling   │
│ Users        │  │ Whisper      │  │ Product      │  │ rembg     │
│ Products     │  │ Vision       │  │ images       │  │ OpenCV    │
│ Orders       │  │ JSON schema  │  │ Audio        │  │ Pillow    │
│ Notifications│  │ responses    │  │ URLs         │  │ FFmpeg    │
└──────────────┘  └──────────────┘  └──────────────┘  └───────────┘
```
