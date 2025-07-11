# Évaluation - Construction d'un Robot en ASCII avec Microservices

## Objectif
Créer une architecture de microservices avec Spring Boot permettant de construire et afficher un robot en ASCII.

## Architecture des Microservices

### 1. Service Tête (`robot-head-service`)
- **Port** : 8081
- **Endpoint** : `GET /head/{id}`
- **Paramètre** : `id` (1, 2 ou 3)
- **Retour** : Représentation ASCII de la tête choisie

### 2. Service Corps (`robot-body-service`)
- **Port** : 8082
- **Endpoint** : `GET /body/{id}`
- **Paramètre** : `id` (1, 2, 3 ou 4)
- **Retour** : Représentation ASCII du corps choisi

### 3. Service Bras Gauche/Droit (`robot-left-arm-service`)
- **Port** : 8083
- **Endpoint** : `GET /right-arm/{id}` pour le bras droit
- **Endpoint** : `GET /left-arm/{id}` pour le bras gauche
- **Paramètre** : `id` (1, 2 ou 3)
- **Retour** : Représentation ASCII du bras gauche choisi

### 4. Service Bras Droit (`robot-right-arm-service`)
- **Port** : 8084
- **Endpoint** : `GET /right-arm/{id}`
- **Paramètre** : `id` (1, 2 ou 3)
- **Retour** : Représentation ASCII du bras droit choisi

### 5. Service Système de Déplacement (`robot-movement-service`)
- **Port** : 8085
- **Endpoint** : `GET /movement/{id}`
- **Paramètre** : `id` (1, 2 ou 3)
- **Retour** : Représentation ASCII du système de déplacement choisi

### 6. Service Principal (`robot-builder-service`)
- **Port** : 8080
- **Endpoint** : `POST /build-robot`
- **Fonction** : Orchestrer les appels aux autres services et assembler le robot complet

### 7. Service de Sauvegarde (`robot-storage-service`)
- **Port** : 8086
- **Endpoints** :
    - `POST /robots` - Sauvegarder un robot
    - `GET /robots` - Récupérer tous les robots sauvegardés
    - `GET /robots/{id}` - Récupérer un robot par son ID
    - `DELETE /robots/{id}` - Supprimer un robot
- **Fonction** : Gérer la persistance des configurations de robots

## Exigences Techniques

### Chaque Microservice doit :
- Utiliser Spring Boot 3.x
- Être un module Maven intégré avec son propre `pom.xml`
- Renvoyer les données ASCII fournies
- Avoir un contrôleur REST
- Gérer les erreurs (ID invalides) avec des codes HTTP appropriés
- Avoir un port distinct configuré dans `application.properties`

### Le Service Principal doit :
- Utiliser RestTemplate ou WebClient pour appeler les autres services
- Assembler le robot complet en combinant les parties ASCII
- Retourner le robot ASCII final en format texte brut
- Gérer les erreurs de communication entre services
- Valider le JSON d'entrée

### Le Service de Sauvegarde doit :
- Utiliser un système de fichiers JSON pour la persistance
- Stocker les configurations dans un dossier `data/` avec des fichiers JSON
- Gérer les opérations CRUD complètes
- Générer automatiquement les IDs (incrémental)
- Avoir une structure : `data/robot-{id}.json`

## Structure de Stockage
```
robot-storage-service/
 ├── src/main/java/...
 ├── data/
 │ ├── robot-1.json
 │ ├── robot-2.json
 │ └── ...
 └── pom.xml
```

## Exemple de Fichier JSON Stocké

Fichier : `data/robot-123.json`
```json
{
  "id": 123,
  "name": "Mon Robot Favori",
  "configuration": {
    "headId": 1,
    "bodyId": 2,
    "leftArmId": 1,
    "rightArmId": 3,
    "movementId": 2
  },
  "createdAt": "2024-01-15T10:30:00"
}
```

## Structure des Projets Maven
Chaque service doit être un projet Maven séparé :
```
robot-services
├── robot-arm-service/
│   ├── pom.xml
│   └── src/main/java/...
├── robot-body-service/
│   ├── pom.xml
│   └── src/main/java/...
├── robot-builder-service/
│   ├── pom.xml
│   └── src/main/java/...
├── robot-head-service/
│   ├── pom.xml
│   └── src/main/java/...
├── robot-movement-service/
│   ├── pom.xml
│   └── src/main/java/...
├── robot-storage-service/
│   ├── pom.xml
│   └── src/main/java/...
```
## Configuration des Ports

Chaque service doit avoir son fichier `application.properties` :
- robot-head-service : port 8081
- robot-body-service : port 8082
- robot-arm-service : port 8083 (gère les deux bras)
- robot-movement-service : port 8085
- robot-builder-service : port 8080
- robot-storage-service : port 8086

## Livrables

**Dépot github :**
1. Code source complet pour tous les microservices
2. Fichiers `pom.xml` configurés avec les bonnes dépendances
3. Fichiers `application.properties` avec les ports

## Dépendances Maven Requises

Pour chaque service, inclure dans le `pom.xml` :
- spring-boot-starter-web
- spring-boot-starter-test

Pour le service de sauvegarde, ajouter :
- spring-boot-starter-data-jpa
- pas de base de données obligatoirement, vous avez le choix entre un fichier JSON ou une base de données en mémoire (H2, SQLite, etc.)


## Exemple de Structure de Contrôleur

Chaque service doit avoir un contrôleur REST qui retourne les éléments ASCII correspondants selon l'ID demandé.

## Gestion des Erreurs

- Retourner HTTP 404 pour les ID invalides
- Retourner HTTP 500 pour les erreurs de communication entre services
- Valider les données JSON d'entrée

## Éléments ASCII Fournis
### Têtes (3 modèles)
**Tête 1 :**
```
  ╭╭╭╮╮╮
 (┃▀  ▀┃)
  ╰━┯┯━╯
```
**Tête 2 :**
```
 ╭╭╭╭╮╮╮╮
 ││▀  ▀││
 ╰╰━┯┯━╯╯
```
**Tête 3 :**
```
 ╔══════╗
 ║░░░░░░║
 ╚══╦╦══╝
```
### Corps (4 modèles)
**Corps 1 :**
```
╦═══╬╬═══╦
╩═══╬╬═══╩
    ║║    
    ║║    
    ║║    
    ║║
```
**Corps 2 :**
```
┬───┼┼───┬
┴───┼┼───┴
    ││    
    ││    
    ││    
    ││    
```
**Corps 3 :**
```
┳━━━╋╋━━━┳
┻━━━╋╋━━━┻
    ┃┃    
    ┃┃    
    ┃┃    
    ┃┃    
```
**Corps 4 :**
```
╦╦╦╦╬╬╦╦╦╦
╬╬╬╬╬╬╬╬╬╬
╠╬╬╬╬╬╬╬╬╣
╠╬╬╬╬╬╬╬╬╣
╠╬╬╬╬╬╬╬╬╣
╚╩╩╩╬╬╩╩╩╝
```

### Bras (4 modèles)
**Bras Droit 1 :**
```
───┐
─┐ │
 │ │
 ┢━┪
 ┗ ┛
```
**Bras Gauche 1 :**
```
┌───
│ ┌─
│ │
┢━┪
┗ ┛
```
**Bras Droit 2 :**
```
───┐
─┐ │
 ├╥┤
 ╰╫╯
  ╿
```
**Bras Gauche 2 :**
```
┌───
│ ┌─
├╥┤
╰╫╯
 ╿
```
**Bras Droit 3 :**
```
───╖
─╖ ║
 ╟╥╢
 ╟╫╢
 ╚╬╝
```
**Bras Gauche 3 :**
```
╓───
║ ╓─
╟╥╢
╟╫╢
╚╬╝
```
**Bras Droit 4 :**
```
───┐
─┐▓│
╾┤▒│
╭┤░│
╽╰ ╿
```
**Bras Gauche 4 :**
```
┌───
│▓┌─
│▒├╼
│░├╮
╿ ╯╽
```

### Systèmes de Déplacement (3 modèles)
**Déplacement 1 :**
```
╔══╗┃┃╔══╗
║╪╪╠┻┻╣╪╪║
╚══╝  ╚══╝
```
**Déplacement 2 :**
```
╭─╮ ┃┃ ╭─╮
│ ┝━┻┻━┥ │
╰─╯    ╰─╯
```
**Déplacement 3 :**
```
  ┏━┻┻━┓
  ┃    ┃
  ┛    ┗ 
```

## Format JSON pour la Requête

```json
{
  "headId": 1,
  "bodyId": 2,
  "leftArmId": 1,
  "rightArmId": 3,
  "movementId": 2
}
```
## Robot obtenu
```
      ╭╭╭╮╮╮
     (┃▀  ▀┃)
      ╰━┯┯━╯     
┌───┬───┼┼───┬───╖ 
│ ┌─┴───┼┼───┴─╖ ║
│ │     ││     ╟╥╢
┢━┪     ││     ╟╫╢
┗ ┛     ││     ╚╬╝
        ││
    ╭─╮ ┃┃ ╭─╮
    │ ┝━┻┻━┥ │
    ╰─╯    ╰─╯
```