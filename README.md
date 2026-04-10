<div align="center">

<img src="https://raw.githubusercontent.com/nyssos2/AudioMap/main/logo.png" alt="AudioKit Logo" width="120" />

# 🎧 AudioMap

> Application web d'audio-guides géolocalisés sur carte 3D interactive

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-live-c9a96e?style=flat-square&logo=github)](https://nyssos2.github.io/AudioMap)
[![Vanilla JS](https://img.shields.io/badge/Vanilla-JS-c9a96e?style=flat-square&logo=javascript&logoColor=black)](https://developer.mozilla.org/fr/docs/Web/JavaScript)
[![Mapbox GL](https://img.shields.io/badge/Mapbox%20GL-v3.3-c9a96e?style=flat-square)](https://docs.mapbox.com/mapbox-gl-js/)

---

## Présentation

**AudioMap** est une application web légère (mono-fichier HTML/JS/CSS) qui permet d'explorer une collection d'audio-guides géolocalisés sur une carte satellite 3D. Elle fonctionne en tandem avec **AudioKit**, un script Python de génération et d'export automatique d'audio-guides.

L'application est conçue pour être déployée sans serveur, directement via **GitHub Pages**, avec les fichiers audio stockés dans le repo lui-même.

---

## Fonctionnalités

### 🗺️ Carte interactive
- Carte satellite 3D **Mapbox GL JS v3.3** avec bâtiments extrudés
- Vue inclinée (pitch 55°), rotation, zoom
- Repères SVG personnalisés par destination (torii japonais, tour Eiffel…)
- Géolocalisation de l'utilisateur
- Survol animé des repères

### 🎵 Lecteur audio
- Lecture / pause, avance/recul de 10s et 30s
- Barre de progression interactive avec waveform animée
- Contrôle de la vitesse de lecture (0.75×, 1×, 1.25×, 1.5×)
- Affichage des coordonnées GPS, transcription et métadonnées
- Téléchargement du fichier audio

### 📱 Responsive mobile
- Drawer latéral avec liste des destinations et sites
- Mini-lecteur persistant en bas d'écran
- Lecteur plein écran expansible
- Thème sombre / clair / auto

### 🎨 Interface
- Thème sombre raffiné (accent doré `#c9a96e`)
- Typographies Outfit + DM Sans
- Transitions et animations soignées
- Authentification par mot de passe (hash SHA-256)

---

## Structure du repo

```
AudioMap/
├── index.html              # Application principale
├── admin.html              # Interface d'administration
├── logo.png                # Logo de l'application
└── audioguides/
    ├── index.json          # Index centralisé de tous les audio-guides
    ├── japon/
    │   └── *.mp3
    └── france/
        └── *.mp3
```

### Format de `index.json`

```json
{
  "destinations": [
    {
      "key": "japon",
      "sites": [
        {
          "nom": "Temple d'Or",
          "sous_titre": "Kinkaku-ji · Kyoto",
          "lat": 35.0394,
          "lng": 135.7292,
          "fichier": "guide-temple-dor_6min_adultes.mp3",
          "duree": "6:08",
          "transcription": "Bienvenue au Temple d'Or…"
        }
      ]
    }
  ]
}
```

> ⚠️ Les noms de fichiers MP3 ne doivent pas contenir d'accents ni d'espaces (utiliser des tirets).

---

## Administration (`admin.html`)

L'interface d'administration permet de gérer la collection sans toucher au code ni à GitHub directement.

### Accès
```
https://nyssos2.github.io/AudioMap/admin.html
```

### Connexion
- **Mot de passe admin** : identique à celui d'AudioMap
- **Personal Access Token GitHub** : token avec permission `repo > contents (read & write)`  
  → GitHub › Settings › Developer settings › Tokens (classic)

> Le token reste uniquement en mémoire session — il n'est jamais écrit dans le code.

### Fonctionnalités
- Tableau de tous les audio-guides par destination
- **Suppression** : retire l'entrée de `index.json` et supprime le MP3 du repo
- **Ajout** : upload du MP3 et mise à jour automatique de `index.json`
- **Nettoyer JSON orphelins** : supprime les fichiers `.json` individuels laissés par d'anciennes versions d'AudioKit

---

## AudioKit

**AudioKit** (`audiokit_v5_6.py`) est le script Python compagnon, déployé sur Streamlit, qui permet de :

1. Générer un script d'audio-guide via l'API Claude (Anthropic)
2. Synthétiser la voix via un moteur TTS
3. Ajouter une musique de fond
4. Exporter le MP3 final
5. L'envoyer automatiquement vers AudioMap (MP3 + entrée `index.json`)

Le nom du fichier MP3 est automatiquement slugifié (sans accents, sans espaces) :
```
{sujet}_{durée}min_{public}_{personnalité}.mp3
```

---

## Déploiement

### Prérequis
- Compte GitHub avec repo public
- Token Mapbox (à configurer dans `index.html`)
- GitHub Pages activé sur la branche `main`

### Configuration Mapbox
Dans `index.html`, ligne ~80 :
```javascript
mapboxgl.accessToken = 'votre_token_mapbox';
```

---

## Stack technique

| Composant | Technologie |
|-----------|-------------|
| Carte | Mapbox GL JS v3.3 |
| Frontend | HTML / CSS / JS vanilla (mono-fichier) |
| Hébergement | GitHub Pages |
| Stockage | GitHub repo (MP3 + JSON) |
| API | GitHub REST API v3 |
| Auth | SHA-256 côté client |
| Typographies | Outfit, DM Sans, DM Mono |

---

## Roadmap

- [ ] Recherche par mot-clé
- [ ] Filtres par durée / public cible
- [ ] Mode hors-ligne (Service Worker)
- [ ] Support multi-langues

---

## Licence

Usage privé — tous droits réservés.

---

*Développé avec ❤️ et beaucoup de café ☕*
