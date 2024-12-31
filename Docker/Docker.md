![[Pasted image 20241231144539.png]]
# Plongée Ludique dans le Monde de Docker !

Docker, c’est un peu comme la magie noire de l’informatique : tout le monde en parle, peu de gens comprennent vraiment, mais tout le monde est épaté quand ça fonctionne. Aujourd’hui, on décortique tout ça pour que ce ne soit plus un mystère pour toi. Prêt(e) ? Allez, à l’abordage !

---

## 1. Docker, C'est Quoi ?

Imagine que tu as une application (par exemple, un site web). Elle a besoin de toute une ambiance pour fonctionner : une version spécifique de Python, une base de données PostgreSQL et d'autres trucs du genre. Avant Docker, on configurait un serveur à la main (bonjour la galère !) ou on utilisait des machines virtuelles (VM).

### VM vs Docker : La Bataille !

|**VM (Machine Virtuelle)**|**Docker (Conteneur)**|
|---|---|
|Lourd… ça embarque tout un OS|Léger, partage l'OS de l'hôte|
|Long à lancer|Rapide comme Flash !|
|Besoin de beaucoup de ressources|Optimisé pour l'efficacité|
|Idéal pour isoler totalement|Idéal pour les applications|

> **En clair** : Docker permet d’isoler ton application et ses dépendances, mais sans tout le barda d'une VM. dansPratique, non ?

### Mais Comment Ça Marche ?

Docker utilise des **conteneurs** (containers). Un conteneur, c’est comme une boîte hermétique : tu y mets tout ce qu’il faut (code, bibliothèques, outils) et tu es sûr(e) que ça marchera de la même façon sur n'importe quel environnement.
![[machines virtuelles.webp]]
---

## 2. Installation et Premier Docker (TP Inside)

### Installation

1. **Windows** : Installe [Docker Desktop](https://www.docker.com/products/docker-desktop/).
    
2. **MacOS** : Idem, Docker Desktop.
    
3. **Linux** :
    
    ```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```
    
4. Vérifions que tout va bien :
    
    ```bash
    docker --version
    ```
    
    Si tu vois une version, ça roule !
    

### TP : Hello World Docker ✨

On va commencer doucement avec un conteneur « Hello World ».

1. Tape cette commande magique :
    
    ```bash
    docker run hello-world
    ```
    
2. Docker va :
    
    - Chercher une image nommée `hello-world` sur **Docker Hub** (le magasin d'images Docker, un peu comme GitHub mais pour les conteneurs).
        
    - La télécharger si elle n'est pas là.
        
    - L'exécuter et afficher un message sympa.
        

### Explication à la Sherlock

- **Image** : Le « modèle » pour créer un conteneur.
    
- **Conteneur** : Une instance de l'image, qui tourne et fait son boulot.
    

---

## 3. Créons Notre Propre Image Docker

### Dockerfile, La Recette Magique

Un **Dockerfile**, c’est un fichier texte qui explique comment construire une image, écrit en YAML. Exemple :

**Fichier : **`**Dockerfile**`

```yaml
# On part d'une image existante
FROM python:3.9-slim

# On ajoute des fichiers au conteneur
COPY app.py /app/app.py  # Copie le fichier app.py dans le dossier /app du conteneur

# On se place dans le répertoire de l'app
WORKDIR /app  # Change le répertoire de travail pour /app

# On exécute un script Python
CMD ["python", "app.py"]  # Commande par défaut pour exécuter l'application
```

### Gérer les Volumes dans Docker

Un volume permet de persister des données entre les redémarrages d’un conteneur ou de partager des données entre plusieurs conteneurs. Il fonctionne comme un lien symbolique : ce qui est modifié dans le conteneur se reflète directement sur le système hôte et vice-versa. En gros, c'est un moyen de connecter un dossier de ton PC à un dossier dans le conteneur pour partager ou sauvegarder des données. Voici un exemple d’utilisation dans un Dockerfile :

```yaml
# On part d'une image existante
FROM python:3.9-slim

# On ajoute des fichiers au conteneur
COPY app.py /app/app.py  # Copie le fichier app.py dans le dossier /app du conteneur

# On se place dans le répertoire de l'app
WORKDIR /app  # Change le répertoire de travail pour /app

# Déclare un volume pour partager des données avec l'hôte
VOLUME ["/app/data"]  # Dossier partagé pour persister les données

# On exécute un script Python
CMD ["python", "app.py"]  # Commande par défaut pour exécuter l'application
```

Lorsque tu utilises un conteneur avec un volume, tu peux monter un répertoire de ton hôte comme suit :

```bash
docker run -v /chemin/local:/app/data mon-premier-docker
```

### TP : Construis et Lâche ton Docker !

1. Crée un fichier `app.py` avec ce contenu :
    
    ```python
    print("Salut depuis un conteneur Docker !")
    ```
    
2. Construis ton image :
    
    ```bash
    docker build -t mon-premier-docker .
    ```
    
3. Lance ton conteneur :
    
    ```bash
    docker run mon-premier-docker
    ```
    

Tadaa ! Tu viens de créer ton premier conteneur. Facile, non ?

---

## 4. Docker Compose : L'Orchestre des Conteneurs

### Pourquoi Docker Compose ?

Quand tu as plusieurs conteneurs (ex : une API + une base de données), c’est galère de les gérer à la main. Docker Compose est là pour ça.

### Fichier `compose.yml`

Un fichier **YAML** pour tout décrire :

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: testdb
      MYSQL_USER: user
      MYSQL_PASSWORD: password
```

### TP : Lance une App Multi-Conteneurs

1. Crée un projet avec ce fichier `docker-compose.yml`.
    
2. Construis et lance tout :
    
    ```
    docker compose up -d  # "-d" pour lancer en mode détaché, c’est-à-dire en arrière-plan
    ```
    
3. Docker Compose gère tout pour toi !
    

---

## 5. Commandes Docker Indispensables

- **Lister les conteneurs actifs** :
    
    ```
    docker ps
    ```
    
- **Stopper un conteneur** :
    
    ```
    docker stop <ID>
    ```
    
- **Supprimer un conteneur** :
    
    ```
    docker rm <ID>
    ```
    
- **Supprimer une image** :
    
    ```
    docker rmi <IMAGE>
    ```
    

---

## 6. TP Final : Créer une App Web Dockerisée

### Objectif

Une app Node.js qui tourne avec Docker et une base de données MySQL.

1. Crée ces fichiers :
    
    - `**app.js**` (Node.js):
        
       ```js
const express = require('express');
const app = express();
const mysql = require('mysql');

// Configurer la connexion MySQL
const db = mysql.createConnection({
  host: 'db',
  user: 'user',
  password: 'password',
  database: 'testdb'
});

db.connect((err) => {
  if (err) {
    console.error('Erreur de connexion à MySQL:', err);
    return;
  }
  console.log('Connecté à MySQL !');
});

app.get('/', (req, res) => {
  db.query('SELECT "Bienvenue sur Docker avec Node.js et MySQL !" AS message', (err, results) => {
    if (err) {
      res.status(500).send('Erreur lors de la requête SQL');
      return;
    }
    res.send(results[0].message);
  });
});

const PORT = 5000;
app.listen(PORT, () => {
  console.log(`App running on http://localhost:${PORT}`);
});

       ```

`**package.json**`
```json
{
  "name": "docker-node-app",
  "version": "1.0.0",
  "main": "app.js",
  "dependencies": {
    "express": "^4.17.1",
    "mysql": "^2.18.1"
  }
}
```

- `**Dockerfile**` :
    
    ```yaml
    FROM node:20
    WORKDIR /app
    COPY package.json .
    RUN npm install
    COPY . .
    CMD ["node", "app.js"]
    ```
    
- `**compose.yml**` :
    
    ```yaml
services:
  web:
    build: .
    ports:
      - "5000:5000"
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: testdb
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ```
    

2. Lance tout :
    
    ```
    docker compose up -d
    ```
    
3. Va sur [http://localhost:5000](http://localhost:5000). Ton app tourne !
    

---

### Conclusion

Docker, c’est un outil puissant, mais accessible si on y va pas à pas. Avec ces bases, tu peux créer, tester et déployer des applications sans te prendre la tête. Et surtout, n'oublie pas : amuse-toi bien avec tes conteneurs !