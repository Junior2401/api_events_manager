# TP Servelet / JAX-RS — README

## Checklist pour le rendu
- [ ] Code source complet (`src/`)
- [ ] Lancer la base HSQLDB (scripts fournis)
- [ ] Déployer / exécuter l'application (Tomcat compatible Jakarta)
- [ ] Lancer les tests unitaires (`mvn test`) et indiquer l'état actuel


## Description
Application JAX-RS / servlet pour la gestion d'évènements, tickets, utilisateurs, etc. Projet Maven packagé en WAR, base embarquée HSQLDB.

## Prérequis
- Java 17 (recommandé) ou Java 11+
- Maven 3.6+
- Un conteneur servlet compatible Jakarta Servlet API 6 (ex. Apache Tomcat 10.1+)
- (Optionnel) PowerShell sous Windows ou un shell Unix pour les scripts

## Structure importante du projet
- `pom.xml` — configuration Maven et dépendances
- `run-hsqldb-server.sh` / `run-hsqldb-server.bat` — scripts pour lancer HSQLDB
- `data/` — fichiers de la base HSQLDB (test.script, test.log, ...)
- `src/main/java/` — code source Java
- `src/main/webapp/` — ressources web et configurations
- `target/` — build output (WAR, classes, lib)

## Build (compilation / package)
Pour compiler et empaqueter l'application :

Sur Unix / WSL / Git Bash :
```bash
mvn clean package
```

Sous PowerShell (Windows) :
```powershell
mvn clean package
```

Le WAR sera généré dans `target/` (ex : `target/<nom>.war`).

Si tu veux compiler sans lancer les tests :
```bash
mvn -DskipTests clean package
```

## Lancer la base HSQLDB
Utilise les scripts fournis en fonction de ton OS.

Sous Linux / macOS :
```bash
./run-hsqldb-server.sh
```

Sous Windows (PowerShell ou double-clic) :
```powershell
.\run-hsqldb-server.bat
```

Le script `run-hsqldb-server.sh` présent dans le dépôt fait :
- `mvn dependency:copy-dependencies`
- crée le dossier `data/` si besoin
- démarre `org.hsqldb.Server` à partir du JAR dans `target/dependency`

Les fichiers de la base sont dans `data/` (test.script, test.log, ...).

## Démarrer l'application avec RestServer 
`Acceder à l'appli à travers : 
http://localhost:8081/api/`.


## Swagger / OpenAPI
Le projet contient des indications pour intégrer OpenAPI/Swagger. Points importants :
- Vérifie que la dépendance OpenAPI est présente dans `pom.xml` (ex. `io.swagger.core.v3:swagger-jaxrs2-jakarta`).
- Ajoute `OpenApiResource` (ou la ressource equivalente) dans ta `Application` JAX-RS pour exposer `/openapi` ou `/openapi.json`.
- Pour Swagger UI : copie le contenu du dossier `dist` de `swagger-ui` dans `src/main/webapp/swagger` et adapte `index.html` pour charger `http://localhost:8081/openapi.json`.
- Un petit endpoint statique (ex. `SwaggerResource`) peut servir les fichiers sous `/api/` pour accéder à l'UI.

Problème fréquent observé : `Failed to load API definition. Fetch error Not Found /openapi` — cela signifie que l'endpoint OpenAPI n'est pas exposé correctement ou que le chemin attendu est différent (`/openapi.json` vs `/openapi`).

### Exemple rapide d'intégration dans `RestApplication` :
(ajoute `OpenApiResource.class` et, si besoin, `SwaggerResource.class` dans le `getClasses()`)

## Tests
Exécuter les tests unitaires / d'intégration :
```bash
mvn test
```

## Auteur / Contact
- Auteur : Alissou ALI & Andy BONGO
