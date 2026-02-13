# 🌾 KrushiSaathi — Application Flow Diagram

## 1. High-Level Application Architecture

```mermaid
flowchart TB
    subgraph Client["Client (Browser)"]
        UI["Next.js Frontend\n(React 19 + Tailwind CSS)"]
        CTX["Context Providers\n(User / Yard / Lab / Navigation)"]
        HOOKS["Custom Hooks\n(LiveAPI / MediaStream / Webcam)"]
    end

    subgraph Server["Next.js API Routes"]
        AUTH_API["Auth API\n(/api/auth/*)"]
        USER_API["User API\n(/api/user/*)"]
        YARDS_API["Yards API\n(/api/yards/*)"]
        LABS_API["Labs API\n(/api/soil-agent/labs/*)"]
    end

    subgraph External["External Services"]
        FIREBASE["Firebase Firestore\n(Database)"]
        GEMINI["Google Gemini API\n(AI Voice Assistant)"]
        CLOUDINARY["Cloudinary\n(Image / PDF Storage)"]
        GMAPS["Google Maps API\n(Lab Location)"]
    end

    UI <--> CTX
    CTX <--> HOOKS
    UI -- "HTTP Requests" --> AUTH_API
    UI -- "HTTP Requests" --> USER_API
    UI -- "HTTP Requests" --> YARDS_API
    UI -- "HTTP Requests" --> LABS_API
    UI -- "WebSocket" --> GEMINI
    UI --> GMAPS

    AUTH_API <--> FIREBASE
    USER_API <--> FIREBASE
    YARDS_API <--> FIREBASE
    YARDS_API --> CLOUDINARY
    LABS_API <--> FIREBASE
```

---

## 2. Authentication & Routing Flow

```mermaid
flowchart TD
    START([User visits any page]) --> MW{Middleware\nChecks JWT cookie}

    MW -- "No token &\npath ≠ /login, /signup" --> LANDING[/landing page/]
    MW -- "No token &\npath = /login or /signup" --> ALLOW_AUTH[Show Login / Signup]
    MW -- "Has token" --> VERIFY{Verify JWT}

    VERIFY -- "Invalid token" --> LOGIN_PAGE[/login page/]
    VERIFY -- "Valid token" --> ROLE{Check Role}

    ROLE -- "role = farmer &\npath = /login or /signup" --> FARMER_HOME["Redirect → / (Farmer Dashboard)"]
    ROLE -- "role = farmer &\npath = /soil-agent" --> FARMER_HOME
    ROLE -- "role = farmer &\nother paths" --> ALLOW_FARMER[Allow access]

    ROLE -- "role = soil-agent &\npath = /login or /signup" --> AGENT_HOME["Redirect → /soil-agent"]
    ROLE -- "role = soil-agent &\npath ≠ /soil-agent" --> AGENT_HOME
    ROLE -- "role = soil-agent &\npath = /soil-agent" --> ALLOW_AGENT[Allow access]
```

---

## 3. User Registration (Signup) Flow

```mermaid
flowchart TD
    SIGNUP_PAGE([Signup Page]) --> ROLE_SELECT{Select Role}

    ROLE_SELECT -- "Farmer" --> FARMER_FORM["Fill Farmer Form\n(Name, Username, Password,\nAadhaar, Address, Passbook, Photo)"]
    ROLE_SELECT -- "Soil Agent" --> AGENT_FORM["Fill Agent Form\n(Lab Name, Username, Password,\nAddress, Phone, Location)"]

    FARMER_FORM --> VALIDATE_F{Validate with Yup}
    AGENT_FORM --> VALIDATE_A{Validate with Yup}

    VALIDATE_F -- "Invalid" --> FARMER_FORM
    VALIDATE_A -- "Invalid" --> AGENT_FORM

    VALIDATE_F -- "Valid" --> UPLOAD_IMG["Upload Images\nto Cloudinary"]
    VALIDATE_A -- "Valid" --> API_SIGNUP_A

    UPLOAD_IMG --> API_SIGNUP_F["POST /api/auth/signup\n(role = farmer)"]
    API_SIGNUP_A["POST /api/auth/signup\n(role = soil-agent)"]

    API_SIGNUP_F --> HASH["Hash password\n(bcryptjs)"]
    API_SIGNUP_A --> HASH

    HASH --> STORE["Store in\nFirebase Firestore"]
    STORE --> JWT["Generate JWT\n(id + role)"]
    JWT --> COOKIE["Set HTTP-only cookie\n(token + role)"]
    COOKIE --> REDIRECT_F["Redirect → /\n(Farmer Dashboard)"]
    COOKIE --> REDIRECT_A["Redirect → /soil-agent\n(Agent Dashboard)"]
```

---

## 4. Farmer Journey — Complete Flow

```mermaid
flowchart TD
    LOGIN([Farmer Logs In]) --> DASH["🏠 Farmer Dashboard\n(Home)"]

    DASH --> F1["🧪 Soil Testing"]
    DASH --> F2["🎙️ Voice Assistant\n(Gemini AI)"]
    DASH --> F3["📰 News &\nMarket Updates"]
    DASH --> F4["💬 Help &\nSupport"]
    DASH --> F5["⚙️ Settings"]

    F1 --> HOW_TO{"Know how to\ncollect samples?"}
    HOW_TO -- "No" --> GUIDE["/how-to\n(Collection Guide)"]
    HOW_TO -- "Yes" --> REG_PAGE

    GUIDE --> REG_PAGE["/register-soil-sample\n(Select Testing Lab)"]

    REG_PAGE --> MAP["View Labs on\nGoogle Maps"]
    MAP --> SELECT_LAB["Select a Lab"]
    SELECT_LAB --> REG_FORM["/register-soil-sample/[labId]\n(Fill Sample Details)"]
    REG_FORM --> YARD_NAME["Enter Yard Name\n& Sample Names"]
    YARD_NAME --> SUBMIT["POST /api/yards\n(Create Yard)"]
    SUBMIT --> TRACK_FORM["Download\nTracking Form"]

    TRACK_FORM --> PROGRESS["/test-progress/[yardId]\n(Monitor Status)"]

    PROGRESS --> STATUS{Sample Status?}
    STATUS -- "Registered" --> WAIT1["⏳ Waiting for\nlab to receive"]
    STATUS -- "In Process" --> WAIT2["🔬 Lab is\ntesting sample"]
    STATUS -- "Completed" --> VIEW_REPORT

    VIEW_REPORT["/view-report\n(All Reports List)"]
    VIEW_REPORT --> REPORT["/results/[yardId]\n(Detailed Report)"]

    REPORT --> NUTRIENTS["📊 Nutrient Charts\n(N, P, K, pH, EC, OC,\nFe, Mn, Zn, Cu, B, S)"]
    REPORT --> SUGGESTIONS["💡 AI-Generated\nSuggestions"]
    REPORT --> PDF["📄 Download\nPDF Report"]

    REPORT --> SMART_REC["/smart-recommendations\n(Crop Recommendations)"]

    F2 --> VOICE_CHAT["Real-time Voice Chat\nvia Google Gemini\nMultimodal Live API"]
```

---

## 5. Soil Agent Journey — Complete Flow

```mermaid
flowchart TD
    AGENT_LOGIN([Soil Agent Logs In]) --> AGENT_DASH["🧪 Soil Agent Dashboard"]

    AGENT_DASH --> VIEW_FARMERS["👥 View Registered Farmers\n(GET /api/yards?soil-agent=labId)"]
    AGENT_DASH --> ANALYTICS["📊 Dashboard Analytics\n(Pending / In-Process / Completed)"]

    VIEW_FARMERS --> SELECT_YARD["Select a Yard\n(Farmer's Submission)"]
    SELECT_YARD --> YARD_DETAIL["View Yard Details\n& Sample List"]

    YARD_DETAIL --> SAMPLE{Select a Sample}

    SAMPLE --> UPDATE_STATUS["Update Sample Status"]
    UPDATE_STATUS --> S1["registered → in-process"]
    UPDATE_STATUS --> S2["in-process → completed"]

    S2 --> INPUT_RESULTS["Input Test Results"]
    INPUT_RESULTS --> MACRO["Macro Nutrients\n(N, P, K)"]
    INPUT_RESULTS --> SECONDARY["Secondary Nutrients\n(S)"]
    INPUT_RESULTS --> MICRO["Micro Nutrients\n(Fe, Mn, Zn, Cu, B)"]
    INPUT_RESULTS --> PHYSICAL["Physical Parameters\n(pH, EC, OC)"]

    MACRO --> UPLOAD
    SECONDARY --> UPLOAD
    MICRO --> UPLOAD
    PHYSICAL --> UPLOAD

    UPLOAD["Upload PDF Report\nto Cloudinary"]
    UPLOAD --> SEND["PUT /api/yards/sendReport\n(Save Results + PDF URL)"]
    SEND --> NOTIFY["✅ Report Available\nfor Farmer"]
```

---

## 6. API Request Flow

```mermaid
flowchart LR
    subgraph Authentication
        A1["POST /api/auth/signup"] --> DB1[(Firebase)]
        A2["POST /api/auth/login"] --> DB1
        A3["GET /api/auth/logout"] --> CLEAR["Clear Cookies"]
    end

    subgraph User
        U1["GET /api/user"] --> DB2[(Firebase)]
        U2["GET /api/user/checkAuth"] --> VERIFY_JWT["Verify JWT"]
    end

    subgraph Yards
        Y1["GET /api/yards\n(?farmer=id | ?soil-agent=id)"] --> DB3[(Firebase)]
        Y2["POST /api/yards"] --> DB3
        Y3["GET /api/yards/[id]"] --> DB3
        Y4["PUT /api/yards/sendReport"] --> CLD["Cloudinary"]
        Y4 --> DB3
    end

    subgraph Labs
        L1["GET /api/soil-agent/labs"] --> DB4[(Firebase)]
        L2["GET /api/soil-agent/labs/[id]"] --> DB4
    end
```

---

## 7. Data Flow — Soil Sample Lifecycle

```mermaid
stateDiagram-v2
    [*] --> Registered : Farmer submits\nsoil sample

    Registered --> InProcess : Soil Agent receives\n& starts testing

    InProcess --> Completed : Agent uploads\nresults & PDF

    Completed --> ReportViewed : Farmer views\ndetailed report

    ReportViewed --> Recommendations : Farmer checks\ncrop suggestions

    Recommendations --> [*]

    state Registered {
        [*] --> SampleCreated
        SampleCreated --> YardStored : POST /api/yards
        YardStored --> TrackingForm : Download tracking form
    }

    state InProcess {
        [*] --> StatusUpdated
        StatusUpdated --> LabTesting : Lab analyzes nutrients
    }

    state Completed {
        [*] --> ResultsEntered
        ResultsEntered --> PDFUploaded : Upload to Cloudinary
        PDFUploaded --> ReportSaved : PUT /api/yards/sendReport
    }
```

---

## 8. Voice Assistant (Gemini AI) Flow

```mermaid
flowchart TD
    USER([Farmer opens Voice Chat]) --> MIC["🎤 Capture Audio\n(MediaStream API)"]
    MIC --> PROCESS["Audio Processing\n(AudioWorklet)"]
    PROCESS --> STREAM["Stream to\nGemini Multimodal Live API\n(WebSocket)"]
    STREAM --> GEMINI["🤖 Google Gemini\nProcesses Query"]
    GEMINI --> RESPONSE["AI Response\n(Text + Audio)"]
    RESPONSE --> AUDIO_OUT["🔊 Play Audio\nResponse"]
    RESPONSE --> TEXT_OUT["📝 Display Text\nResponse"]
    AUDIO_OUT --> USER
    TEXT_OUT --> USER
```

---

## 9. End-to-End User Journey (Summary)

```mermaid
flowchart LR
    A["🌐 Landing Page"] --> B["🔐 Signup / Login"]
    B --> C{Role?}
    C -- Farmer --> D["🏠 Dashboard"]
    C -- Soil Agent --> I["🧪 Agent Dashboard"]

    D --> E["📍 Select Lab\n& Register Sample"]
    E --> F["📦 Track\nProgress"]
    F --> G["📊 View Report\n& Charts"]
    G --> H["🌱 Smart Crop\nRecommendations"]
    D --> J["🎙️ Voice\nAssistant"]

    I --> K["👥 View\nFarmers"]
    K --> L["🔬 Test\nSamples"]
    L --> M["📄 Upload\nResults"]
    M -->|Report available| G
```
