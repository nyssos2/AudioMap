🎧 AudioMap — Audioguides 3D interactifs

AudioMap est une application web interactive qui visualise et lit des audioguides géolocalisés sur une carte 3D satellite. Elle fonctionne en tandem avec AudioKit, qui génère les audio-guides en MP3.

✨ Fonctionnalités

- Carte 3D immersive — Mapbox GL JS avec vue satellite, bâtiments extrudés en 3D et rotation libre
- Repères iconiques par pays — icônes SVG personnalisées (torii pour le Japon, Tour Eiffel pour la France...)
- Lecteur audio intégré — play/pause, avance/retour ±10s et ±30s, barre de progression, waveform animée
- Vitesse de lecture — 0.75× / 1× / 1.25× / 1.5×
- Multi-destinations — navigation par onglets entre les pays
- Sidebars rétractables — mode carte plein écran disponible
- Thème clair / sombre / automatique — suit les préférences système
- Import drag & drop — chargement de MP3 en session pour les utilisateurs externes (données non persistantes)
- Chargement depuis GitHub — les audioguides persistants sont lus depuis le repo


🗂️ Structure du repo
AudioMap/
├── index.html                  ← application complète (mono-fichier)
├── logo.png                    ← logo / favicon
└── audioguides/
    ├── index.json              ← catalogue de toutes les destinations et sites
    ├── japon/
    │   ├── kinkaku-ji_10min_adultes_guide-conferencier.mp3
    │   └── kinkaku-ji_10min_adultes_guide-conferencier.json
    └── france/
        ├── tour-eiffel_5min_enfants_vieux-sage.mp3
        └── tour-eiffel_5min_enfants_vieux-sage.json

📋 Format du fichier index.json
Ce fichier est le catalogue central lu au démarrage. Il est mis à jour automatiquement par AudioKit à chaque envoi d'un nouvel audio-guide.
json{
  "destinations": [
    {
      "key": "japon",
      "nom": "Japon",
      "sites": [
        {
          "nom": "Temple d'Or",
          "sous_titre": "Kinkaku-ji · Kyoto",
          "lat": 35.0394,
          "lng": 135.7292,
          "fichier": "kinkaku-ji_10min_adultes_guide-conferencier.mp3",
          "duree": "?:??",
          "transcription": "Konnichiwa ! Bienvenue au Kinkaku-ji..."
        }
      ]
    }
  ]
}

📦 Format des fichiers JSON individuels
Chaque MP3 est accompagné d'un fichier .json du même nom :
json{
  "nom": "Temple d'Or",
  "sous_titre": "Japon",
  "lat": 35.0394,
  "lng": 135.7292,
  "fichier": "kinkaku-ji_10min_adultes_guide-conferencier.mp3",
  "duree": "?:??",
  "transcription": "Texte complet du script de l'audio-guide..."
}

🔗 Écosystème

AudioMap fonctionne en tandem avec AudioKit, une application Streamlit qui :

- Génère des scripts d'audio-guides via Gemini 2.5 Flash
- Synthétise la voix avec Edge TTS
- Mixe la voix avec une ambiance sonore
- Intègre les coordonnées GPS dans les métadonnées ID3 du MP3
- Envoie automatiquement le MP3 + JSON vers ce repo AudioMap


🛠️ Stack technique

- ComposantTechnologieApplicationHTML + CSS + JavaScript vanilla (mono-fichier)CarteMapbox GL JS v3.3.0HébergementGitHub PagesDonnéesGitHub Contents API + raw.githubusercontent.comGéocodageNominatim (OpenStreetMap)

🚀 Déploiement

L'application est hébergée sur GitHub Pages et accessible à :
- https://nyssos2.github.io/AudioMap
Aucun serveur, aucun backend, aucune dépendance npm — tout tourne dans le navigateur.

🔒 Accès

L'application est protégée par mot de passe (hashé en SHA-256). La session reste active jusqu'à la fermeture de l'onglet.
