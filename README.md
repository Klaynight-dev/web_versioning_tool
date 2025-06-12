# 🚀 Web Versioning Tool

**Outil de versioning web moderne** qui édite un fichier JSON pour le suivi automatique de vos mises à jour patch/minor/major !

---

## 📖 Description

Cet outil permet de gérer automatiquement les versions de votre projet web en :

🔹 **Créant et maintenant** un fichier `version.json` avec l'historique complet  
🔹 **Mettant à jour automatiquement** les références de version dans votre fichier `index.html`  
🔹 **Suivant la convention** de versioning sémantique (SemVer)

---

## ⚡ Installation

1. **Téléchargez** le fichier [`version.js`](version.js) dans le répertoire racine de votre projet web
2. **Assurez-vous** d'avoir Node.js installé sur votre système

### 📁 Structure requise

```
votre-projet/
├── 📄 version.js          # L'outil de versioning
├── 🌐 index.html          # Votre fichier HTML principal
└── 📋 version.json        # Créé automatiquement
```

---

## 🎯 Utilisation

### 🛠️ Commandes disponibles

| Type | Commande | Description |
|------|----------|-------------|
| **🔧 PATCH** | `node version.js add "Description"` | Incrémente la version patch (1.0.0 → 1.0.1) |
| **✨ MINOR** | `node version.js minor add "Nouvelle fonctionnalité"` | Incrémente la version minor (1.0.1 → 1.1.0) |
| **🚨 MAJOR** | `node version.js major add "Changement majeur"` | Incrémente la version major (1.1.0 → 2.0.0) |
| **📊 INFO** | `node version.js version` | Affiche la version actuelle |
| **📋 CHANGELOG** | `node version.js changelog` | Affiche le changelog actuel |
| **🕒 NOW** | `node version.js now` | Affiche version et changelog |

### 💡 Exemples d'utilisation

```bash
# 🎬 Première utilisation - création automatique de version.json
node version.js add "Version initiale du projet"

# 🔍 Vérifier la version actuelle
node version.js version

# 📋 Vérifier le changelog actuel
node version.js changelog

# 🕒 Afficher version et changelog
node version.js now

# 🐛 Correction de bug (patch par défaut)
node version.js add "Correction du bug d'affichage sur mobile"

# ⭐ Nouvelle fonctionnalité
node version.js minor add "Ajout du système de commentaires"

# 🔥 Changement majeur
node version.js major add "Refonte complète de l'interface utilisateur"
```

---

## 💻 Récupération des informations de version

### 🌐 JavaScript (côté client)

```javascript
// 📡 Charger les données de version via fetch
async function loadVersionInfo() {
    try {
        const response = await fetch('./version.json');
        const versionData = await response.json();
        
        // 📝 Utiliser les données
        console.log('📌 Version actuelle:', versionData.version);
        console.log('📅 Date de build:', versionData.date);
        console.log('📋 Changelog:', versionData.changelog);
        
        // 🎨 Afficher dans le DOM
        document.getElementById('app-version').textContent = versionData.version;
        document.getElementById('build-date').textContent = versionData.date;
        document.getElementById('changelog').textContent = versionData.changelog;
        
        return versionData;
    } catch (error) {
        console.error('❌ Erreur lors du chargement:', error);
    }
}

// 🚀 Appeler la fonction
loadVersionInfo();
```

### 🖥️ Node.js (côté serveur)

```javascript
const fs = require('fs');

// 📖 Lire le fichier version.json
function getVersionInfo() {
    try {
        const versionData = JSON.parse(fs.readFileSync('./version.json', 'utf8'));
        return versionData;
    } catch (error) {
        console.error('❌ Erreur lors de la lecture:', error);
        return null;
    }
}

// 🎯 Utilisation
const version = getVersionInfo();
if (version) {
    console.log(`📌 Version: ${version.version}`);
    console.log(`📅 Date: ${version.date}`);
    console.log(`📋 Changelog: ${version.changelog}`);
}
```

### 📜 Affichage de l'historique

```javascript
// 🕒 Afficher l'historique complet
function displayVersionHistory() {
    fetch('./version.json')
        .then(response => response.json())
        .then(data => {
            const historyContainer = document.getElementById('version-history');
            
            data.history.forEach(version => {
                const versionElement = document.createElement('div');
                versionElement.className = 'version-item';
                versionElement.innerHTML = `
                    <h3>🏷️ Version ${version.version} (${version.type.toUpperCase()})</h3>
                    <p><strong>📅 Date:</strong> ${version.date}</p>
                    <p><strong>📋 Changelog:</strong> ${version.changelog}</p>
                `;
                historyContainer.appendChild(versionElement);
            });
        });
}
```

### 🎨 Exemple d'intégration HTML complète

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🚀 Mon Projet Web</title>
    <style>
        .version-badge { 
            background: #007acc; 
            color: white; 
            padding: 4px 8px; 
            border-radius: 12px; 
            font-size: 0.8em; 
        }
        .version-item { 
            background: #f8f9fa; 
            border-left: 4px solid #007acc; 
            padding: 15px; 
            margin: 10px 0; 
            border-radius: 4px; 
        }
    </style>
</head>
<body>
    <header>
        <h1>🚀 Mon Projet Web</h1>
        <p>📌 Version: <span id="app-version" class="version-badge">1.0.0</span></p>
        <p>📅 Dernière mise à jour: <span id="build-date"></span></p>
    </header>
    
    <main>
        <section id="changelog-section">
            <h2>📋 Derniers changements</h2>
            <p id="changelog"></p>
        </section>
        
        <section id="history-section">
            <h2>🕒 Historique des versions</h2>
            <div id="version-history"></div>
        </section>
    </main>

    <script>
        // 🎬 Charger et afficher les informations de version
        async function initializeVersionInfo() {
            try {
                const response = await fetch('./version.json');
                const versionData = await response.json();
                
                // 🎨 Mettre à jour les éléments du DOM
                document.getElementById('app-version').textContent = versionData.version;
                document.getElementById('build-date').textContent = versionData.date;
                document.getElementById('changelog').textContent = versionData.changelog;
                
                // 📜 Afficher l'historique
                const historyContainer = document.getElementById('version-history');
                versionData.history.forEach(version => {
                    const div = document.createElement('div');
                    div.className = 'version-item';
                    
                    const typeEmoji = {
                        patch: '🔧',
                        minor: '✨',
                        major: '🚨',
                        initial: '🎬'
                    };
                    
                    div.innerHTML = `
                        <strong>${typeEmoji[version.type] || '📦'} v${version.version}</strong> 
                        <span style="color: #666;">📅 ${version.date}</span>
                        <br>
                        <em>📝 ${version.changelog}</em>
                    `;
                    historyContainer.appendChild(div);
                });
                
            } catch (error) {
                console.error('❌ Erreur:', error);
            }
        }
        
        // 🚀 Initialiser au chargement de la page
        document.addEventListener('DOMContentLoaded', initializeVersionInfo);
    </script>
</body>
</html>
```

---

## 📄 Format du fichier version.json

```json
{
  "version": "1.2.3",               // 📌 Version actuelle
  "date": "2025-06-11",             // 📅 Date de build
  "changelog": "Description",       // 📋 Dernier changement
  "history": [                      // 🕒 Historique complet
    {
      "version": "1.2.3",
      "date": "2025-06-11",
      "changelog": "Description du changement",
      "type": "patch"               // 🔧 patch | ✨ minor | 🚨 major | 🎬 initial
    }
  ]
}
```

---

## 📏 Convention de versioning

L'outil suit la convention **Semantic Versioning (SemVer)** :

| Type | Description | Exemple |
|------|-------------|---------|
| 🚨 **MAJOR** | Changements incompatibles | `1.0.0` → `2.0.0` |
| ✨ **MINOR** | Nouvelles fonctionnalités compatibles | `1.0.0` → `1.1.0` |
| 🔧 **PATCH** | Corrections de bugs | `1.0.0` → `1.0.1` |
| 🎬 **INITIAL** | Version initiale du projet | `1.0.0` |

---

## ✨ Fonctionnalités

- ✅ **Création automatique** du fichier `version.json`
- ✅ **Historique complet** avec dates et changelogs
- ✅ **Mise à jour automatique** du fichier `index.html`
- ✅ **Support SemVer** complet
- ✅ **Validation** des arguments obligatoires
- ✅ **Commandes flexibles** pour consulter version et changelog
- ✅ **Messages d'aide** intégrés
- ✅ **API JavaScript** simple et efficace

---

## 📜 Licence

Ce projet est sous licence **MIT**.

## 👨‍💻 Auteur

Développé avec ❤️ par **[Klaynight](https://github.com/klaynight-dev)**