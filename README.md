# smart-laundry-qr-system
## English

### 📋 Overview

A smart, semi-automated laundry management system designed for student residences. Users scan QR codes to access the system, monitor real-time machine availability, and receive notifications when their laundry is complete.

**Deployed at:** Maison du Maroc, Cité Universitaire, Paris
**Users:** 216 residents

### ✨ Key Features

- **QR Code Access Control**: Scan QR codes for temporary access (5 minutes)
- **Real-time Monitoring**: Track 3 washing machines + 3 dryers live
- **Smart Notifications**: Browser notifications and audio alerts when cycles complete
- **PIN Security**: 4-digit PIN code protection for machine operations
- **Mobile-First Design**: Optimized for smartphones with PWA capabilities
- **Timer Management**: Visual countdowns with automatic status updates
- **Dual Mode Interface**: 
  - 👁️ **View Mode**: Check machine availability
  - ✅ **Action Mode**: Start/stop machines (QR code required)

### 🎯 Problem Solved

Students no longer need to physically check the laundry room to see if machines are available. The system prevents conflicts, reduces wait times, and enables better time management.

### 🛠️ Tech Stack

- **Frontend**: React 18 (CDN-based, no build required)
- **Backend**: Firebase Realtime Database
- **Styling**: Tailwind CSS
- **QR Scanner**: HTML5 QR Code Library
- **Notifications**: Web Notifications API + Web Audio API
- **Mobile**: PWA-ready with iOS optimizations

### 📊 Architecture

```
┌─────────────┐      QR Code       ┌──────────────┐
│   User's    │ ──────Scan────────>│  Web App     │
│  Smartphone │                     │  (React)     │
└─────────────┘                     └──────┬───────┘
                                           │
                                    Real-time sync
                                           │
                                           ▼
                                  ┌────────────────┐
                                  │   Firebase     │
                                  │   Realtime DB  │
                                  └────────────────┘
                                           │
                                    Sync to all users
                                           │
                                           ▼
                              ┌──────────────────────┐
                              │  All Connected Users │
                              │  (Live Updates)      │
                              └──────────────────────┘
```

### 🚀 Quick Start

#### Prerequisites

- A Firebase account (free tier works fine)
- A web server (or Firebase Hosting)

#### Installation

1. **Clone the repository**
```bash
git clone https://github.com/YOUR_USERNAME/smart-laundry-qr-system.git
cd smart-laundry-qr-system
```

2. **Configure Firebase**
   
   Create a `.env` file in the root directory:
```env
FIREBASE_API_KEY=your_api_key_here
FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
FIREBASE_DATABASE_URL=https://your_project.firebaseio.com
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_STORAGE_BUCKET=your_project.appspot.com
FIREBASE_MESSAGING_SENDER_ID=your_sender_id
FIREBASE_APP_ID=your_app_id
```

3. **Update Firebase config in index.html**
   
   Replace the Firebase configuration in `index.html` with your credentials (or use environment variables).

4. **Deploy**

   Option A - Firebase Hosting:
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

   Option B - Any static hosting (Netlify, Vercel, GitHub Pages):
```bash
# Simply upload the index.html file
```

#### Firebase Database Structure

Set up your Firebase Realtime Database with this structure:

```json
{
  "machines": {
    "machine-1": {
      "id": 1,
      "type": "machine",
      "status": "libre",
      "endTime": null,
      "duration": null,
      "sessionId": null,
      "pin": null
    },
    "machine-2": { ... },
    "machine-3": { ... },
    "dryer-1": {
      "id": 1,
      "type": "dryer",
      "status": "libre",
      "endTime": null,
      "duration": null,
      "sessionId": null,
      "pin": null
    },
    "dryer-2": { ... },
    "dryer-3": { ... }
  },
  "accessCodes": {
    "CODE1": true,
    "CODE2": true,
    "CODE3": true
  }
}
```

#### Firebase Security Rules

```json
{
  "rules": {
    "machines": {
      ".read": true,
      ".write": true,
      "$machineId": {
        ".validate": "newData.hasChildren(['id', 'type', 'status'])"
      }
    },
    "accessCodes": {
      ".read": true,
      ".write": false
    }
  }
}
```

### 📱 Generating QR Codes

You need to generate QR codes that contain access codes. Each QR code should encode a string like:

```
LAUNDRY_ACCESS:CODE1
```

Use any QR code generator:
- [QR Code Generator](https://www.qr-code-generator.com/)
- [QRCode Monkey](https://www.qrcode-monkey.com/)

Print and place the QR codes in your laundry room.

### 🎨 Customization

#### Change Colors

Edit the Tailwind classes in `index.html`:
```javascript
// Primary color (default: indigo)
className="bg-indigo-600"  // Change to bg-blue-600, bg-purple-600, etc.
```

#### Modify Timer Durations

```javascript
// In the DurationModal component
const durations = [
  { value: 30, label: "30min - Rapide" },
  { value: 60, label: "1h - Standard" },
  { value: 90, label: "1h30 - Intensif" }
];
```

#### Adjust Access Duration

```javascript
// Change from 5 minutes to your preference
const ACCESS_DURATION = 5 * 60 * 1000; // milliseconds
```

### 📈 Metrics & Impact

- **User Base**: 216 residents
- **Machines Managed**: 6 (3 washers + 3 dryers)
- **Average Daily Usage**: ~40 cycles
- **Conflict Reduction**: ~80% (estimated)
- **User Satisfaction**: High (based on informal feedback)

### 🔐 Security Features

1. **QR Code Access Control**: Time-limited access (5 minutes)
2. **PIN Protection**: 4-digit PIN required to start machines
3. **Session Management**: Each user session is tracked
4. **Firebase Rules**: Database access controlled via Firebase rules

### 🐛 Known Limitations

- QR codes can be shared (by design for flexibility)
- Relies on user honesty to update machine status
- No payment integration (assumes free laundry)
- Notifications require browser permissions

### 🚀 Future Enhancements

- [ ] IoT sensor integration for automatic status detection
- [ ] User accounts and history tracking
- [ ] Advanced analytics dashboard
- [ ] Multi-language support (currently FR/EN)
- [ ] Payment integration
- [ ] Reservation system
- [ ] Email/SMS notifications

### 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### 👤 Author

**Rim** - AI/Data Science Engineer & AI Project Manager
- Currently: Mastère Spécialisé in AI Project Management @ SKEMA Business School
- Background: Arts et Métiers Engineering (AI/Data Science)
- LinkedIn: https://www.linkedin.com/in/rim-zamzami-ai-solutions/

### 🙏 Acknowledgments

- Maison du Maroc residents for testing and feedback
- Firebase for the excellent real-time database
- The React and Tailwind CSS teams

---

## Français

### 📋 Aperçu

Un système de gestion de laverie semi-automatisé et intelligent conçu pour les résidences étudiantes. Les utilisateurs scannent des QR codes pour accéder au système, surveiller la disponibilité des machines en temps réel et recevoir des notifications lorsque leur linge est prêt.

**Déployé à :** Maison du Maroc, Cité Universitaire, Paris
**Utilisateurs :** 216 résidents

### ✨ Fonctionnalités Principales

- **Contrôle d'Accès par QR Code**: Scanner les QR codes pour un accès temporaire (5 minutes)
- **Surveillance en Temps Réel**: Suivi de 3 machines à laver + 3 sèche-linges en direct
- **Notifications Intelligentes**: Notifications navigateur et alertes audio en fin de cycle
- **Sécurité par Code PIN**: Protection par code PIN à 4 chiffres pour les opérations
- **Design Mobile-First**: Optimisé pour smartphones avec capacités PWA
- **Gestion des Timers**: Compte à rebours visuels avec mises à jour automatiques
- **Interface à Double Mode**: 
  - 👁️ **Mode Consultation**: Vérifier la disponibilité des machines
  - ✅ **Mode Action**: Démarrer/arrêter les machines (QR code requis)

### 🎯 Problème Résolu

Les étudiants n'ont plus besoin de se déplacer physiquement à la laverie pour vérifier si les machines sont disponibles. Le système prévient les conflits, réduit les temps d'attente et permet une meilleure gestion du temps.

### 🛠️ Stack Technique

- **Frontend**: React 18 (basé sur CDN, pas de build requis)
- **Backend**: Firebase Realtime Database
- **Styling**: Tailwind CSS
- **Scanner QR**: HTML5 QR Code Library
- **Notifications**: Web Notifications API + Web Audio API
- **Mobile**: PWA-ready avec optimisations iOS

### 🚀 Démarrage Rapide

#### Prérequis

- Un compte Firebase (le tier gratuit suffit)
- Un serveur web (ou Firebase Hosting)

#### Installation

1. **Cloner le dépôt**
```bash
git clone https://github.com/YOUR_USERNAME/smart-laundry-qr-system.git
cd smart-laundry-qr-system
```

2. **Configurer Firebase**
   
   Créer un fichier `.env` à la racine :
```env
FIREBASE_API_KEY=votre_cle_api
FIREBASE_AUTH_DOMAIN=votre_projet.firebaseapp.com
FIREBASE_DATABASE_URL=https://votre_projet.firebaseio.com
FIREBASE_PROJECT_ID=votre_projet_id
FIREBASE_STORAGE_BUCKET=votre_projet.appspot.com
FIREBASE_MESSAGING_SENDER_ID=votre_sender_id
FIREBASE_APP_ID=votre_app_id
```

3. **Mettre à jour la config Firebase dans index.html**
   
   Remplacer la configuration Firebase dans `index.html` avec vos identifiants.

4. **Déployer**

   Option A - Firebase Hosting:
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

   Option B - Tout hébergement statique (Netlify, Vercel, GitHub Pages):
```bash
# Il suffit de téléverser le fichier index.html
```

### 📱 Génération des QR Codes

Vous devez générer des QR codes contenant les codes d'accès. Chaque QR code doit encoder une chaîne comme :

```
LAUNDRY_ACCESS:CODE1
```

Utilisez n'importe quel générateur de QR code :
- [QR Code Generator](https://www.qr-code-generator.com/)
- [QRCode Monkey](https://www.qrcode-monkey.com/)

Imprimez et placez les QR codes dans votre laverie.

### 📈 Métriques & Impact

- **Base d'Utilisateurs**: 216 résidents
- **Machines Gérées**: 6 (3 lave-linges + 3 sèche-linges)
- **Usage Quotidien Moyen**: ~40 cycles
- **Réduction des Conflits**: ~80% (estimation)
- **Satisfaction Utilisateur**: Élevée (selon retours informels)

### 🔐 Fonctionnalités de Sécurité

1. **Contrôle d'Accès par QR Code**: Accès limité dans le temps (5 minutes)
2. **Protection PIN**: Code PIN à 4 chiffres requis pour démarrer les machines
3. **Gestion des Sessions**: Chaque session utilisateur est suivie
4. **Règles Firebase**: Accès à la base de données contrôlé via règles Firebase

### 🚀 Améliorations Futures

- [ ] Intégration de capteurs IoT pour détection automatique du statut
- [ ] Comptes utilisateurs et historique
- [ ] Tableau de bord analytique avancé
- [ ] Support multi-langues étendu
- [ ] Intégration de paiement
- [ ] Système de réservation
- [ ] Notifications email/SMS

### 📄 Licence

Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.

### 👤 Auteure

**Rimi** - Ingénieure AI/Data Science & Chef de Projet IA
- Actuellement : Mastère Spécialisé en Management de Projets IA @ SKEMA Business School
- Formation : Ingénieure Arts et Métiers (IA/Data Science)
- LinkedIn: https://www.linkedin.com/in/rim-zamzami-ai-solutions/

### 🙏 Remerciements

- Les résidents de la Maison du Maroc pour les tests et retours
- Firebase pour l'excellente base de données temps réel
- Les équipes React et Tailwind CSS

---

## 📸 Screenshots

*Coming soon*

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/YOUR_USERNAME/smart-laundry-qr-system/issues).

## ⭐ Show Your Support

Give a ⭐️ if this project helped you!

---

**Made with ❤️ for Maison du Maroc**

