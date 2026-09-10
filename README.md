# CraftConnect

> **AI-powered digital storefront and catalog manager for Indian artisans**

CraftConnect helps artisans turn handmade products into polished, bilingual marketplace listings—then manage orders, customers, inventory, and business growth from one focused workspace.

[![Flutter](https://img.shields.io/badge/Flutter-mobile%20%26%20web-02569B?logo=flutter&logoColor=white)](https://flutter.dev/)
[![FastAPI](https://img.shields.io/badge/FastAPI-backend-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-data%20layer-47A248?logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![License](https://img.shields.io/badge/license-private-lightgrey)](#license)



## What CraftConnect does

CraftConnect connects an artisan’s real-world craft process to a public-ready digital catalog:

1. Capture a product photo and voice description.
2. Let AI generate a bilingual product listing.
3. Review material, craft technique, pricing, and regional-language copy.
4. Publish the product to the marketplace.
5. Track orders, notifications, sales, and business insights.

## Feature highlights

### For artisans

- **AI cataloging** from product images, voice notes, and material cost.
- **Bilingual listings** with English, Hindi, and a selected regional language.
- **AI image studio** with:
  - Background removal
  - White studio-style product isolation
  - Lighting and contrast correction
  - Color enhancement
  - Sharpening and professional crop/padding
- **Smart pricing** with suggested price, craft assessment, and pricing explanation.
- **Product management** with edit, delete, listing status, material, and technique metadata.
- **Sales dashboard** with revenue, orders, products sold, and pending items.
- **Order management** with item-level status updates.
- **Notifications** for listings, order events, image enhancement, and weekly business tips.
- **Camera and microphone capture** for native mobile workflows.

### For buyers

- Marketplace search and active-product filtering.
- Category filters, material chips, artisan badges, and marketplace carousel.
- Product details with artisan attribution and full-size image viewing.
- Cart with server-calculated totals.
- Cash-on-delivery checkout.
- Order history and vertical delivery-status tracking.
- Buyer notifications and profile management.

### Languages

The app includes locale support for:

- English
- Hindi
- Marathi
- Tamil
- Bengali

## Product workflow

```text
Photo + Voice Note
        │
        ▼
AI Image Enhancement ──► Clean studio-ready product photo
        │
        ▼
AI Listing Generation ─► Bilingual copy + material + technique + price
        │
        ▼
Artisan Review
        │
        ▼
Published Marketplace Listing
        │
        ▼
Orders ──► Notifications ──► Analytics
```

## Repository layout

```text
.
├── backend/
│   ├── main.py                 # FastAPI application and API routes
│   ├── image_enhancer.py       # Background removal and photo enhancement
│   ├── database.py              # MongoDB connection and collections
│   ├── cloudinary_helper.py     # Image and audio upload helper
│   ├── requirements.txt         # Python dependencies
│   ├── .env.example             # Backend configuration template
│   └── render.yaml              # Render deployment configuration
│
├── frontend/
│   ├── lib/
│   │   ├── main.dart            # Flutter app entry point
│   │   ├── api.dart             # Shared API base URL
│   │   ├── auth.dart            # Session state
│   │   ├── l10n/                # Localization controller and translations
│   │   └── screens/             # App screens
│   ├── assets/logo.png          # CraftConnect brand mark
│   ├── android/                 # Android project and launcher icons
│   ├── ios/                     # iOS project and AppIcon assets
│   └── pubspec.yaml             # Flutter dependencies and asset declaration
│
└── README.md
```

## Requirements

### Backend

- Python 3.10+
- MongoDB
- Cloudinary account for hosted product images and audio
- Groq API key for AI listing generation

### Frontend

- Flutter 3+
- Dart 3+
- Android Studio and an Android SDK for Android builds
- Xcode and CocoaPods for iOS builds
- A physical device or simulator/emulator

## Backend setup

From the repository root:

```bash
cd backend
python -m venv .venv
```

Activate the virtual environment:

```bash
# macOS / Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Create the environment file:

```bash
cp .env.example .env
```

Configure the values in `backend/.env`:

```env
GROQ_API_KEY=your_groq_key
GROQ_VISION_MODEL=qwen/qwen3.8-27b
MONGO_URI=your_mongodb_connection_string

CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
```

Optional SMTP settings enable email delivery for password-reset OTPs:

```env
SMTP_HOST=
SMTP_PORT=587
SMTP_USERNAME=
SMTP_PASSWORD=
SMTP_FROM=
```

Start the API:

```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Health check:

```bash
curl http://127.0.0.1:8000/health
```

## Flutter setup

From the repository root:

```bash
cd frontend
flutter pub get
```

Run the app:

```bash
flutter run
```

For a browser run:

```bash
flutter run -d chrome
```

The frontend uses:

- `http://127.0.0.1:8000` for local web development.
- The configured CraftConnect backend URL for mobile builds and the deployed web app.

If you change the backend host, update:

- `frontend/lib/api.dart`
- `frontend/lib/api_config.dart`

## Android and iOS branding

The current app branding is **CraftConnect**.

Brand assets are kept in:

```text
frontend/assets/logo.png
frontend/android/app/src/main/res/mipmap-*/ic_launcher.png
frontend/ios/Runner/Assets.xcassets/AppIcon.appiconset/
```

Native display names are configured in:

```text
frontend/android/app/src/main/AndroidManifest.xml
frontend/ios/Runner/Info.plist
```

After changing native assets, run:

```bash
flutter clean
flutter pub get
```

## Build commands

### Android APK

```bash
cd frontend
flutter build apk --release
```

The release APK is generated under:

```text
frontend/build/app/outputs/flutter-apk/app-release.apk
```

For an Android App Bundle:

```bash
flutter build appbundle --release
```

### iOS

```bash
cd frontend
flutter build ios --release
```

For App Store distribution, open the generated iOS workspace in Xcode, configure signing and the bundle identifier, then archive the Runner target.

## API map

### Authentication and profile

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `POST` | `/auth/signup` | Create an artisan or buyer account |
| `POST` | `/auth/login` | Sign in |
| `POST` | `/auth/forgot-password` | Request a password-reset OTP |
| `POST` | `/auth/reset-password` | Reset a password with an OTP |
| `GET` | `/artisans/{artisan_id}` | Load profile information |
| `PUT` | `/artisans/{artisan_id}` | Update profile information |

### Products and AI cataloging

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `POST` | `/enhance-image/` | Remove the background and enhance a product photo |
| `POST` | `/process-product-listing/` | Generate a bilingual AI listing from image, audio, and cost |
| `POST` | `/products/save` | Save a catalog product with image and audio |
| `GET` | `/products` | Browse, search, and filter products |
| `GET` | `/products/{product_id}` | Load one product |
| `PUT` | `/products/{product_id}` | Update a product |
| `DELETE` | `/products/{product_id}` | Delete a product |
| `POST` | `/preview-audio/` | Convert supported audio for browser preview |

### Orders, analytics, and notifications

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `POST` | `/orders` | Create a buyer order |
| `GET` | `/orders?buyer_id=...` | Load buyer order history |
| `GET` | `/artisan/orders?artisan_id=...` | Load artisan orders |
| `PATCH` | `/orders/{order_id}/item-status` | Update an order-item status |
| `GET` | `/artisan/analytics?artisan_id=...` | Load sales analytics |
| `GET` | `/notifications?user_id=...` | Load notifications |
| `PATCH` | `/notifications/{notification_id}/read` | Mark a notification as read |
| `GET` | `/health` | Check API, database, storage, and AI-key availability |

## Data collections

CraftConnect uses MongoDB collections for:

- `artisans`
- `products`
- `orders`
- `password_resets`
- `notifications`

The backend serializes MongoDB identifiers and timestamps before returning API responses to Flutter.

## Image enhancement pipeline

The image enhancer is designed for public-facing commerce listings:

1. Correct camera EXIF orientation.
2. Resize oversized uploads to a safe processing limit.
3. Segment the product with `rembg`.
4. Clean the alpha mask and remove disconnected background artifacts.
5. Reject unreliable segmentation rather than returning a misleading result.
6. Apply white balance, CLAHE contrast, brightness, color, and sharpening adjustments.
7. Crop around the isolated product with controlled padding.
8. Compose the result on a clean studio-white background with a soft shadow.
9. Return an optimized progressive JPEG to the Flutter preview and catalog flow.

## Permissions

The mobile frontend requests:

- Camera access for product capture.
- Microphone access for artisan voice notes.

Android permissions are declared in:

```text
frontend/android/app/src/main/AndroidManifest.xml
```

iOS permissions should be reviewed in the Runner target and `Info.plist` before App Store submission.

## Development notes

- Keep API keys, database credentials, SMTP passwords, and Cloudinary secrets in environment variables only.
- Do not commit `backend/.env`.
- Keep generated directories such as `build/`, `.dart_tool/`, `.gradle/`, virtual environments, and Python caches out of release archives.
- The backend uses asynchronous FastAPI routes and moves CPU-heavy image enhancement into a worker thread.
- The frontend preserves the selected enhanced image bytes when moving from Image Studio to Auto Catalog and when saving a product.

## License

This project is currently private and intended for the CraftConnect product team. Add a formal license here before public redistribution.

---

Built for artisans who deserve better tools, better storefronts, and a wider market.
