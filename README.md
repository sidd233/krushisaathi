# 🌾 KrushiSaathi

**Empowering Indian Farmers with Technology**

KrushiSaathi is a comprehensive agri-tech platform aimed at supporting Indian farmers by providing real-time weather forecasts, crop recommendations, market prices, and government scheme updates in their native languages. With a voice assistant and easy-to-use interface, KrushiSaathi ensures even semi-literate or illiterate farmers can make informed decisions to improve yield, reduce losses, and increase profitability.

 **Link to the Repo of ML models:** https://github.com/Lord3008/AGRI_AI 

---

## 🚀 Features

- 📱 **Multilingual Support** – Interface in native languages (Odia, Hindi, etc.)
- 🎙️ **Voice Assistant** – For hands-free access and ease of use
- 🌦️ **Weather Forecasting** – Location-based alerts and predictions
- 🧪 **Soil Health Insights** – Integration with soil health data
- 📈 **Crop Recommendation** – Based on season, soil, and weather
- 🧑‍🌾 **Government Scheme Updates** – Timely info on subsidies, schemes
- 🛒 **Market Price Alerts** – Real-time mandi prices for key crops
- 💬 **Query Forum / Chatbot** – For instant help and farmer community interaction

---

## 🛠️ Tech Stack

- **Frontend**: React Native / Flutter (for mobile), React.js (for web)
- **Backend**: Node.js / Express
- **Database**: MongoDB / Firebase
- **APIs**: OpenWeather, AgriMarket, Soil Health Card API
- **ML**: TensorFlow.js / Python backend for crop prediction

---

## 🧑‍💻 Installation

```bash
# Clone the repository
git clone https://github.com/siddharth-narayan-mishra/krushisaathi.git
cd KrushiSaathi

# Install dependencies
npm install

# Run the development server
npm run dev
```

## 🧪 Demo

You can try the working prototype at:
👉 [KrushiSaathi](https://krushisaathi-891779892886.us-central1.run.app/)

---
## 📷 Screenshots

![ss2](https://github.com/user-attachments/assets/f046f419-b38a-40bf-86a2-aa47443a8e71)

![ss1](https://github.com/user-attachments/assets/c18a3a0e-4166-411a-9f50-9db33287d372)

![ss3](https://github.com/user-attachments/assets/fa2f74e5-fa72-45a3-97a8-d3241bb78106)

![ss4](https://github.com/user-attachments/assets/3f776c9e-7eec-4e52-aeba-5026a46e0e2a)

![ss5](https://github.com/user-attachments/assets/ee9698ab-8b4d-44ba-91d9-dc266c5c0ae4)

![ss6](https://github.com/user-attachments/assets/60fc65c5-9aaa-4c64-b6b1-7d7954f52892)

---

## 📊 Flow Diagram

For a comprehensive visual overview of the application architecture, authentication flow, user journeys, and API interactions, see the [Flow Diagram](FLOW_DIAGRAM.md).

---

## 📄 Project Structure

```
├── .dockerignore
├── .gitignore
├── Dockerfile
├── README.md
├── favicon.ico
├── globals.css
├── layout.tsx
├── middleware.ts
├── next.config.ts
├── package.json
├── package-lock.json
├── postcss.config.mjs
├── tsconfig.json

├── app
│   ├── (auth)/login/page.tsx
│   ├── (auth)/signup/page.tsx
│   ├── (dashboard)/soil-agent/page.tsx
│   ├── (features)/
│   │   ├── register-soil-sample/[id]/page.tsx
│   │   ├── register-soil-sample/page.tsx
│   │   ├── results/[id]/page.tsx
│   │   ├── smart-recommendations/page.tsx
│   │   ├── test-progress/[id]/page.tsx
│   │   └── view-report/page.tsx
│   ├── (static)/how-to/page.tsx
│   ├── (static)/landing/page.tsx
│   ├── (static)/soil-testing/page.tsx
│   ├── api/
│   │   ├── auth/{login, logout, signup}/route.ts
│   │   ├── soil-agent/labs/[id]/route.ts
│   │   ├── soil-agent/labs/route.ts
│   │   ├── user/{checkAuth, route}.ts
│   │   └── yards/{[id], route, sendReport}/route.ts
│   └── page.tsx

├── components/
│   ├── audio-pulse/
│   ├── auth/
│   ├── common/
│   ├── control-tray/
│   ├── farmerDashboard/
│   ├── landing/
│   ├── soilAgent/
│   ├── soilTestingRegistration/
│   └── ui/

├── config/
│   ├── ImagesUrl.ts
│   ├── landingPageConfig.ts
│   └── statesData.ts

├── context/
│   ├── LabContext.tsx / LabState.tsx
│   ├── LiveAPIContext.tsx
│   ├── NavigationContext.tsx / NavigationState.tsx
│   ├── UserContext.tsx / UserState.tsx
│   ├── YardContext.tsx / YardState.tsx

├── hooks/
│   ├── use-live-api.ts
│   ├── use-media-stream-mux.ts
│   ├── use-screen-capture.ts
│   └── use-webcam.ts

├── lib/
│   ├── audio-{recorder, streamer}.ts
│   ├── audioworklet-registry.ts
│   ├── multimodal-live-client.ts
│   ├── store-logger.ts
│   ├── firebase/FirebaseConfig.js
│   └── worklets/{audio-processing, vol-meter}.ts

├── models/
│   ├── Labs.ts
│   ├── User.ts
│   └── Yard.ts

├── public/assets/icons/
│   └── *.svg

├── types/
│   └── multimodal-live-types.ts

├── utils/
│   ├── customImageLoader.js
│   ├── ststs-card.tsx
│   └── utils.ts
```

## 🌱 Vision

> “To bridge the gap between traditional farming and modern technology, making agricultural knowledge and services accessible to every farmer in India.”

---

## 🤝 Contributing

We welcome contributions! If you'd like to improve KrushiSaathi:

1. Fork this repo
2. Create a new branch: `git checkout -b feature/your-feature`
3. Make your changes and commit: `git commit -m 'Add your feature'`
4. Push to your branch: `git push origin feature/your-feature`
5. Open a pull request 🙌


---

## 📜 License

This project is licensed under the [MIT License](LICENSE).

---

> Made with ❤️ for the farmers of India 🇮🇳
