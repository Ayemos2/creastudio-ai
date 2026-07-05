# 🎨 CréaStudio AI

Application Android (Flutter) — Générateur de contenu créatif IA (images & vidéos), prête à publier sur Google Play Store.

[![Flutter](https://img.shields.io/badge/Flutter-3.24-blue.svg)](https://flutter.dev/)
[![Platform](https://img.shields.io/badge/platform-Android-green.svg)]()
[![License](https://img.shields.io/badge/license-Commercial-orange.svg)]()

---

## 🚀 Fonctionnalités

- 🖼️ **Génération d'images IA** (DALL-E 3, 10+ styles, 5 ratios)
- 🎬 **Génération de vidéos IA** (Premium, Replicate API)
- 🎨 **Templates pros** Instagram/TikTok/YouTube
- 📁 **Galerie cloud** synchronisée (Firebase Storage)
- 💎 **Monétisation 360°** : Freemium + AdMob + Abonnements RevenueCat
- 🔒 **Sécurité** : clés API server-side, RGPD, modération
- 🌍 **Multilingue** : FR / EN
- 🌗 **Mode sombre/clair** automatique

---

## 📋 Prérequis

| Outil | Version |
|---|---|
| Flutter SDK | ≥ 3.24 |
| Dart | ≥ 3.4 |
| Android Studio | Hedgehog ou + |
| JDK | 17 |
| Node.js (Cloud Functions) | 20 |
| Compte Firebase | gratuit |
| Compte Google Play Console | 25$ unique |
| Clé OpenAI API | payant |
| Compte RevenueCat | gratuit jusqu'à 10k$/mois |
| Compte AdMob | gratuit |

---

## ⚡ Installation rapide

### 1. Cloner & installer les dépendances
```bash
cd creastudio_ai
flutter pub get
```

### 2. Configurer Firebase
```bash
# Installer FlutterFire CLI
dart pub global activate flutterfire_cli

# Lier votre projet Firebase (remplace lib/firebase_options.dart)
flutterfire configure --project=creastudio-ai
```

Activer dans la console Firebase :
- **Authentication** → Email/Password + Google
- **Firestore Database** (mode production)
- **Storage**
- **Cloud Functions**
- **Crashlytics**

### 3. Configurer .env
Créer un fichier `.env` à la racine :
```env
REVENUECAT_API_KEY=goog_xxxxxxxxxxxxxx
ADMOB_APP_ID=ca-app-pub-XXXXXXXXX~XXXXXXXXX
```

### 4. Configurer AdMob
Dans `android/app/src/main/AndroidManifest.xml`, remplacer l'App ID :
```xml
<meta-data
    android:name="com.google.android.gms.ads.APPLICATION_ID"
    android:value="ca-app-pub-VOTRE_ID~XXXXXX"/>
```

Dans `lib/core/constants/app_constants.dart`, remplacer les IDs de test par vos IDs production.

### 5. Configurer RevenueCat
1. Créer un projet sur https://app.revenuecat.com/
2. Lier votre Google Play Developer Account
3. Créer les produits : `premium_monthly`, `premium_yearly`, `premium_lifetime`
4. Copier la **Public API Key Android** dans `.env`

### 6. Déployer Cloud Functions (backend sécurisé)
```bash
cd functions
npm install
firebase login
firebase use creastudio-ai

# Configurer les secrets (clés API)
firebase functions:secrets:set OPENAI_API_KEY
firebase functions:secrets:set REPLICATE_API_TOKEN
firebase functions:secrets:set REVENUECAT_WEBHOOK_SECRET

# Déployer
firebase deploy --only functions
```

### 7. Lancer l'app en debug
```bash
flutter run
```

---

## 📦 Build pour Google Play Store

### 1. Créer une clé de signature
```bash
keytool -genkey -v -keystore ~/creastudio-key.jks \
  -keyalg RSA -keysize 2048 -validity 10000 -alias creastudio
```

### 2. Créer `android/key.properties`
```properties
storePassword=VOTRE_MOT_DE_PASSE
keyPassword=VOTRE_MOT_DE_PASSE
keyAlias=creastudio
storeFile=/chemin/absolu/vers/creastudio-key.jks
```

> ⚠️ Ne committez JAMAIS ce fichier (déjà dans .gitignore).

### 3. Build App Bundle (.aab pour Play Store)
```bash
flutter build appbundle --release
```
👉 Fichier généré : `build/app/outputs/bundle/release/app-release.aab`

### 4. Build APK (test local)
```bash
flutter build apk --release --split-per-abi
```
👉 Fichiers générés dans `build/app/outputs/flutter-apk/`

---

## 🏪 Publication Play Store — Checklist

- [ ] Créer compte Google Play Console (25$)
- [ ] Créer une nouvelle application
- [ ] Remplir la fiche : nom, descriptions FR + EN (`play_store/description_*.txt`)
- [ ] Uploader icône 512×512 PNG
- [ ] Uploader feature graphic 1024×500
- [ ] Uploader 8 captures d'écran (voir `play_store/screenshots_guide.md`)
- [ ] Vidéo promo YouTube (optionnel mais recommandé)
- [ ] Catégorie : **Art & Design**
- [ ] Classification du contenu (questionnaire)
- [ ] Politique de confidentialité hébergée publiquement (`play_store/privacy_policy.html`)
- [ ] CGU publiées (`play_store/terms_of_service.html`)
- [ ] Déclaration AdMob ID
- [ ] Configurer les produits in-app (3 abonnements + packs crédits)
- [ ] Uploader l'AAB signé
- [ ] Soumettre pour examen (24–72h en moyenne)

---

## 🏗️ Architecture

```
Clean Architecture (3 couches) :
  Presentation (UI + Riverpod) → Domain (entities + repos) → Data (datasources)

State Management : Riverpod 2.x
Navigation       : go_router
Backend          : Firebase + Cloud Functions Node.js
APIs IA          : OpenAI DALL-E 3 + Replicate (vidéo)
Paiements        : RevenueCat + Google Play Billing
Pub              : Google AdMob
Cache local      : Hive
Network          : Dio
```

---

## 📊 Monétisation

| Plan | Prix | Fonctionnalités |
|---|---|---|
| **Gratuit** | 0€ | 5 images/jour, pubs, watermark |
| **Premium Mensuel** | 9,99€/mois | Illimité, 4K, pas de pub |
| **Premium Annuel** | 79,99€/an | Idem + 33% économie |
| **Lifetime** | 199€ | Une fois, à vie |
| **Packs crédits** | 4,99€–19,99€ | 100 / 500 crédits |

Sources de revenus :
1. **Abonnements** (RevenueCat → 70-85% net après commissions Google)
2. **AdMob** (banner + interstitiel + rewarded)
3. **Achats in-app** (packs crédits, templates premium)

---

## 🆘 Dépannage

**Erreur "Default FirebaseApp is not initialized"** → Exécuter `flutterfire configure`

**AdMob ne s'affiche pas** → Vérifier l'App ID dans AndroidManifest + attendre 1h après création du compte AdMob

**Build release échoue** → Vérifier `android/key.properties` et le chemin du keystore

**Cloud Function timeout** → Augmenter `timeoutSeconds` dans `functions/src/generateImage.js`

---

## 📧 Support

- 📧 Email : support@creastudio.ai
- 🌐 Site : https://creastudio.ai

---

© 2026 CréaStudio AI. Tous droits réservés.
