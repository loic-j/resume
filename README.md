# CV en ligne — Guide de déploiement Cloudflare

## Structure du projet

```
cv/
├── public/
│   ├── index.html     ← CV complet (HTML + CSS + JS en un seul fichier)
│   ├── _headers       ← En-têtes de sécurité (Cloudflare Pages)
│   └── photo.jpg      ← ← Ajoute ta photo ici
└── wrangler.toml      ← Config Cloudflare Workers
```

---

## 1. Personnaliser le contenu

Ouvre `public/index.html` et remplace tous les `[brackets]` :

| Placeholder           | À remplacer par                          |
|-----------------------|------------------------------------------|
| `[LastName]`          | Ton nom de famille                       |
| `[your@email.com]`    | Ton adresse email                        |
| `[username]`          | Ton pseudo GitHub                        |
| `[profile]`           | Ton profil LinkedIn                      |
| `[your-domain.com]`   | Ton domaine Cloudflare                   |
| `[Company Name]`      | Tes entreprises (×4 langues)             |
| `[Location]`          | Tes localisations                        |
| `[N]`                 | Nombre d'ingénieurs managés              |
| `[Year]` / `[Degree]` | Tes diplômes et certifications           |

**Photo de profil :**
1. Ajoute `photo.jpg` dans `public/`
2. Dans `index.html`, remplace `<div class="avatar-placeholder">L</div>` par :
   ```html
   <img src="photo.jpg" alt="Ton Nom">
   ```

---

## 2. Déployer sur Cloudflare Workers (recommandé en 2025)

```bash
# 1. Installer Wrangler (CLI Cloudflare)
npm install -g wrangler

# 2. Se connecter à Cloudflare
npx wrangler login

# 3. Déployer (depuis le dossier cv/)
npx wrangler deploy

# → URL de preview immédiate : https://my-cv.[account].workers.dev
```

---

## 3. Connecter ton domaine

Dans le **dashboard Cloudflare** :
1. Workers & Pages → ton worker → Settings → Domains & Routes
2. Ajoute ton domaine custom : `cv.ton-domaine.com` ou `ton-domaine.com`
3. DNS se configure automatiquement si le domaine est déjà chez Cloudflare

---

## 4. Déploiement automatique via Git (optionnel)

```bash
# Initialiser un repo git
git init && git add . && git commit -m "init cv"

# Pousser sur GitHub
gh repo create my-cv --public --push

# Dans Cloudflare dashboard :
# Workers & Pages → Create → Pages → Connect to Git → my-cv
# Build command : (laisser vide)
# Output directory : public
```
→ Chaque `git push` déclenche un redéploiement automatique.

---

## 5. Exporter en PDF

Ouvre le CV dans Chrome/Edge :
1. `Ctrl+P` (ou `Cmd+P` sur Mac)
2. Destination : **Enregistrer en PDF**
3. Taille : A4
4. Marges : Minimum
5. ✅ Graphiques en arrière-plan

Le CSS `@media print` est déjà configuré pour un rendu propre.

---

## 6. Ajouter Google Analytics (optionnel)

Dans `<head>` de `index.html`, après le titre :
```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```
