# Stremio Glass — reconstruction complète PWA (v4)

Cette archive est un client web indépendant destiné à être publié sur GitHub Pages. Elle n'est ni une application Stremio officielle, ni une IPA iOS.

## Contenu
- `index.html` : interface iPad, catalogues Cinemeta, recherche, fiches, épisodes, gestionnaire d'addons, lecteur direct et tentative torrent WebRTC.
- `sw.js` : cache PWA versionné, précache tolérant aux fichiers manquants et fallback hors ligne.
- `manifest.webmanifest` : configuration installable.
- `icon-192.png`, `icon-512.png` : icônes.

## Déploiement GitHub Pages
1. Fais une sauvegarde de ton dépôt actuel.
2. Décompresse l'archive.
3. Remplace à la racine du dépôt les cinq fichiers : `index.html`, `sw.js`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`.
4. Commit les changements sur la branche publiée par GitHub Pages.
5. Attends le déploiement, puis ouvre l'adresse GitHub Pages dans Safari.
6. Si l'ancienne version reste en cache : ferme l'app installée, supprime son icône de l'écran d'accueil, puis dans Réglages > Safari > Effacer historique et données de sites, efface les données du site concerné. Rouvre le site et ajoute-le à l'écran d'accueil.

## Utilisation
- Accueil : catalogues déclarés dans les manifestes des addons actifs.
- Recherche : recherche sur les catalogues qui déclarent `extra: search` (Cinemeta inclus).
- Addons : colle une URL HTTPS se terminant par `manifest.json` (ou une URL de manifeste configurée), puis Ajouter.
- Fiche : affiche les métadonnées et récupère les sources des addons déclarant la ressource `stream`.
- URL directe : le lecteur HTML5 tente de lire les URL HTTP(S) compatibles avec Safari.
- Torrent : pour un stream Stremio contenant `infoHash`, le client tente WebTorrent 3.0.21 via le module ESM `esm.sh`. Les trackers WebRTC trouvés dans `sources` sont utilisés, avec deux trackers WebTorrent publics en secours.

## Limites importantes
1. **CORS** : les addons doivent autoriser les appels navigateur avec les en-têtes CORS. Une PWA GitHub Pages ne peut pas contourner CORS. Il faut un backend/proxy HTTPS maîtrisé si un addon bloque les requêtes. Ne mets pas de secrets dans le code public.
2. **Torrent navigateur** : WebTorrent dans un navigateur communique via WebRTC et ne parle pas aux pairs BitTorrent TCP/UDP classiques. Le flux ne démarrera que si le torrent possède des pairs WebRTC joignables et si Safari/iPadOS autorise les connexions nécessaires. La compatibilité réelle doit être testée sur l'iPad cible. Documentation : https://webtorrent.io/docs et https://webtorrent.io/faq.
3. **Fichiers vidéo** : Safari ne lit pas tous les codecs/conteneurs. Une URL directe peut donc être valide sans être décodable.
4. **Sous-titres** : l'application récupère les ressources `subtitles` des addons et ajoute des pistes `<track>` quand une URL est fournie. Le serveur de sous-titres doit permettre le chargement cross-origin.
5. **Séries** : les épisodes listés dans les métadonnées sont présentés individuellement et leurs propres IDs sont utilisés pour les requêtes stream.
6. **Sécurité / légalité** : utilise uniquement des sources et contenus auxquels tu as légalement accès. Les addons sont des services tiers indépendants.
7. **Pas d'IPA** : cette archive est une PWA. Pour une app native iPadOS/AltStore PAL, il faut un projet natif compilé et signé sur macOS/Xcode, puis suivre les conditions de distribution Apple/AltStore.

## Vérifications réalisées dans cet environnement
- Contrôle de syntaxe du JavaScript inline avec Node.js.
- Contrôle de syntaxe du service worker.
- Validation JSON du manifeste PWA.
- Contrôle de l'intégrité du ZIP.

Non réalisé : test Safari réel, test réseau live des addons, lecture effective d'un torrent sur iPad, compilation iOS native. Les résultats réseau peuvent varier selon les addons et leurs règles CORS.
