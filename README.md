# 📍 SnapMap  
*A hyperlocal, map-based photo sharing app designed for college campuses.*

<p align="center">
  <a href="https://opengap.org"><img src="https://img.shields.io/badge/OpenGAP-0.1.0-blue.svg?style=flat-square" alt="OpenGAP 0.1.0"></a>
  <a href="https://app.hidevs.xyz/passport"><img src="https://img.shields.io/badge/GitAgent%20Passport-Ready-emerald.svg?style=flat-square" alt="GitAgent Passport Ready"></a>
  <img src="https://img.shields.io/badge/Category-Education-purple.svg?style=flat-square" alt="Category Education">
  <img src="https://img.shields.io/badge/Compliance-FERPA%20%7C%20GDPR-green.svg?style=flat-square" alt="Compliance FERPA | GDPR">
</p>

SNAP-MAP allows students to instantly capture photos and share them on a live campus map. All photos are geo-tagged, stored securely, and shown as clusters/bubbles on a dynamic map. Students can explore events happening around them, view photos contributed by others, and participate in the campus community in real time.

---

## 🤖 GitAgent Passport Qualification

This repository is certified compliant with the **OpenGAP Specification 0.1.0** standard for autonomous agents and passes all three clearance checkpoints of the **HiDevs GitAgent Passport** pipeline:

- **Checkpoint 1 (Validate):** Fully specified agent metadata in [`agent.yaml`](agent.yaml) conforming to OpenGAP 0.1.0 standard schema (Category: `Education`, Data Classification: `internal`, Risk Tier: `standard`).
- **Checkpoint 2 (Explain):** Cognitive architecture, geospatial Haversine scoring, hotspot vitality formulas, and compliance mappings documented in [`EXPLAINABILITY.md`](EXPLAINABILITY.md) across all four required sections.
- **Checkpoint 3 (Export):** Comprehensive operational rules, duties, persona, skills, and OpenAPI-style tools:
  - **Core Contract:** [`agent.yaml`](agent.yaml)
  - **Persona & Values:** [`SOUL.md`](SOUL.md)
  - **Behavioral Directives:** [`RULES.md`](RULES.md)
  - **Operational Duties:** [`DUTIES.md`](DUTIES.md)
  - **Explainability & Architecture:** [`EXPLAINABILITY.md`](EXPLAINABILITY.md)
  - **Modular Skills:** [`skills/geospatial-event-clusterer/SKILL.md`](skills/geospatial-event-clusterer/SKILL.md), [`skills/campus-boundary-validator/SKILL.md`](skills/campus-boundary-validator/SKILL.md), [`skills/media-safety-moderator/SKILL.md`](skills/media-safety-moderator/SKILL.md), [`skills/hotspot-analytics-synthesizer/SKILL.md`](skills/hotspot-analytics-synthesizer/SKILL.md)
  - **Tool Specifications:** [`tools/spatial-density-analyzer.yaml`](tools/spatial-density-analyzer.yaml), [`tools/geo-fence-validator.yaml`](tools/geo-fence-validator.yaml), [`tools/image-content-moderator.yaml`](tools/image-content-moderator.yaml), [`tools/event-cluster-publisher.yaml`](tools/event-cluster-publisher.yaml)

---

# 🚀 Features

### 📸 Camera & Upload
- Capture photos directly using the in-app camera  
- Auto-attaches GPS coordinates  
- Uploads securely to Azure Blob Storage  
- Preview + retake option  

### 🗺️ Map-Based Discovery
- Interactive map with user location  
- Bubbles indicate photos uploaded around campus  
- Clustered markers for multiple events or heavy hotspots  
- Tap a bubble → see **All Photos** or **Event-wise Photos**  

### 🎉 Event Clustering
- System auto-detects events based on photo density + proximity  
- Groups photos under event IDs  
- Shows “hotspot” visuals on the map  

### 👤 User Accounts (via Clerk)
- College email login (domain restricted)  
- Secure sessions  
- View your uploaded photos  
- Manage profile + logout  

### 📂 Profile & Gallery
- All uploads in a grid  
- Delete option  
- Event galleries with horizontal swipe viewer  

---

# 🧱 Tech Stack

### **Frontend**
- React Native (Expo)
- Expo Camera + Expo Location
- Mapbox or react-native-maps
- Axios for API calls
- Clerk for authentication

### **Backend**
- Node.js + Express
- Clerk server-side JWT verification
- Mongoose + MongoDB Atlas
- Azure Blob Storage (file storage only)

### **Database**
- **MongoDB Atlas**
  - Users  
  - Photos  
  - Events  
  - Geospatial queries enabled  

### **File Storage**
- **Azure Blob Storage**  
  - All images compressed + uploaded here  
  - URLs stored in MongoDB  

---

# 📂 Project Structure

```
SnapMap/                         → Project root
│
│   CONTRIBUTING.md               → Contribution guidelines
│   README.md                     → Project overview
│   
├── backend/                      → Backend API
│   │   package-lock.json         
│   │   package.json              
│   │   server.js                 → Server entry point
│   │   v1.js                     → /api/v1 all routes are here
│   │   
│   ├── config/                   
│   ├── controllers/              → Request handlers
│   ├── db/                       → Database setup
│   ├── middleware/               → Request middleware
│   ├── models/                   → Database models
│   ├── routes/                   → API routes
│   └── utils/                    → Helper utilities
│
├── contributors/                 → Contributor records
│   └── <your_roll_no>.txt            
│       
├── frontend/                     → Mobile frontend
│   │   .gitignore                
│   │   app.json                  → Expo app config
│   │   babel.config.js           
│   │   index.js                  → App entry point
│   │   package-lock.json         
│   │   package.json              
│   │   
│   ├── .expo/                    
│   │   │   devices.json          
│   │   │   README.md             
│   │   │   settings.json         
│   │
│   └── src/                      
│       │   App.js                → Root component
│       │
│       ├── assets/               → Images & fonts
│       ├── components/           → Reusable UI
│       ├── context/              
│       ├── navigation/           → App navigation
│       ├── screens/              → App screens
│       └── services/             → API services
│
└── UI-UX/                        → Design resources
    └── contributors/             → UI/UX credits

```
---

# 🤝 Contributing
- Submit PRs after reading [CONTRIBUTING.md](CONTRIBUTING.md)  

---

# 💬 Contact
Reach out to me on Discord, ID: `terrormanzero` aka `terror_quota`

Our Server: https://bit.ly/OpencodeDiscord

