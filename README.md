# AMIGO GOLD — Déploiement Netlify

## Structure
```
index.html                        ← L'application complète
netlify.toml                      ← Config Netlify (scheduled function)
package.json                      ← Dépendance @netlify/blobs
netlify/functions/
  draw.mjs                        ← Tirage auto toutes les 5 min (cron)
  get-draws.mjs                   ← API HTTP pour les tirages manqués
```

## Déployer en 3 étapes

1. **Créer un repo GitHub** et pousser ces fichiers
2. **Connecter à Netlify** → "Add new site" → "Import from Git"
3. **C'est tout** — Netlify détecte automatiquement le `netlify.toml`

## Fonctionnement

- La **Scheduled Function** `draw` tire 12 numéros toutes les 5 minutes et les stocke dans **Netlify Blobs**
- Quand vous ouvrez le site, l'app appelle `/api/get-draws?since={lastVisit}` pour récupérer les tirages manqués
- Si vous aviez validé une mise avant de fermer, le résultat est calculé automatiquement
- Le timer local (Web Worker) reste synchronisé avec l'horloge réelle

## Pré-requis Netlify

- Compte Netlify gratuit (Free tier inclut les Scheduled Functions et Netlify Blobs)
- Aucune variable d'environnement requise (Netlify Blobs est automatique en prod)

## Tester localement

```bash
npm install -g netlify-cli
npm install
netlify dev
```
