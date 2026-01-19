# mlops-lab-06
## Etape 1:
Cette étape consiste à démarrer un cluster local via Minikube avec le pilote Docker en version v1.28.3. J'ai ensuite créé le namespace dédié churn-mlops pour isoler les ressources du projet, puis configuré le contexte courant sur ce dernier.

<img width="2044" height="1159" alt="Capture d&#39;écran 2026-01-18 034144" src="https://github.com/user-attachments/assets/8b135f3b-374e-42eb-a6dc-e4b3a4c4ad0c" />

<img width="2006" height="652" alt="Capture d&#39;écran 2026-01-18 034558" src="https://github.com/user-attachments/assets/7305d748-81ea-4e1d-9253-a38076859ccc" />

 La vérification finale confirme que l'environnement est prêt et correctement segmenté, bien qu'aucun Pod ne soit encore déployé.


## Etape 2:
Objectif : Configurer un environnement isolé et reproductible pour l'API de prédiction.
isolation via Environnement Virtuel : Création et activation de l'environnement venv_mlops afin d'éviter tout conflit de dépendances avec le système global.
Mise à jour des outils de gestion : Montée de version de pip
<img width="1896" height="1126" alt="image" src="https://github.com/user-attachments/assets/bd04aca0-6589-4f90-ae41-7a095c46d10d" />

Gestion rigoureuse des dépendances : Installation des librairies via requirements.txt, incluant FastAPI pour le service web et des versions spécifiques de scikit-learn (1.7.2) et pandas.
<img width="1719" height="857" alt="image" src="https://github.com/user-attachments/assets/df6a9778-51a1-4cfd-85c0-3fd9708c4818" />

## Étape 3 : Créer le dossier des manifests Kubernetes
Objectif : Organiser l'arborescence du projet pour accueillir les fichiers de configuration (manifests) du cluster.
Génération du dossier k8s/ à la racine du projet pour centraliser les définitions d'objets Kubernetes
<img width="1750" height="1422" alt="image" src="https://github.com/user-attachments/assets/61c0f080-aa33-4621-a9e6-64fa29bf8ad5" />


## Étape 4 : Construire l’image Docker (tag versionné)

<img width="1854" height="1036" alt="image" src="https://github.com/user-attachments/assets/d800c2c0-3a6c-4554-af9e-ab2292a72e64" />
<img width="2039" height="345" alt="image" src="https://github.com/user-attachments/assets/bb926359-3631-43f0-ab63-42115d216c2c" />


## Étape 5 : Charger explicitement l’image dans Minikube
<img width="1998" height="403" alt="image" src="https://github.com/user-attachments/assets/080b8b71-e285-45e7-bdb0-e76e52b8f3c6" />


Étape 6 : Deployment Kubernetes pour l’API churn
<img width="2011" height="1008" alt="Capture d&#39;écran 2026-01-18 035308" src="https://github.com/user-attachments/assets/fa4a1507-4597-41fb-849c-fdf918ab5273" />

<img width="1751" height="1366" alt="image" src="https://github.com/user-attachments/assets/1fe56ece-65ce-493d-a596-079a5d57303c" />

<img width="2095" height="1071" alt="Capture d&#39;écran 2026-01-18 035740" src="https://github.com/user-attachments/assets/5552254d-8e7c-4cb0-ac09-0cdad70af164" />


Étape 7 : Exposer l’API via un Service NodePort
<img width="2050" height="478" alt="Capture d&#39;écran 2026-01-18 035854" src="https://github.com/user-attachments/assets/413364b2-2957-4b09-a54b-69ec16027188" />

<img width="1824" height="976" alt="image" src="https://github.com/user-attachments/assets/e705f2d1-1523-43e9-9dad-6537398a3e7d" />

<img width="2098" height="1188" alt="Capture d&#39;écran 2026-01-18 040027" src="https://github.com/user-attachments/assets/f69d1dbe-7e05-4c61-9893-3d308dbc8260" />


<img width="2037" height="1376" alt="Capture d&#39;écran 2026-01-18 040752" src="https://github.com/user-attachments/assets/4d646e51-52a5-4fc5-8e1e-5818d0186665" />



<img width="1257" height="1216" alt="image" src="https://github.com/user-attachments/assets/dc2b2388-fe44-479a-a4a3-3a12813374b8" />


Étape 8 : Injecter la configuration MLOps via ConfigMap

<img width="2059" height="1098" alt="Capture d&#39;écran 2026-01-18 041303" src="https://github.com/user-attachments/assets/27f9a27c-07a8-4c31-9c65-d4df8fbf213d" />

<img width="970" height="563" alt="image" src="https://github.com/user-attachments/assets/aafaf3a5-1ac1-4084-bfdb-d66bac1f4d19" />
<img width="2026" height="869" alt="image" src="https://github.com/user-attachments/assets/bde27877-b126-4cfa-b8af-4be8cd1941d0" />

On Modifie k8s/deployment.yaml pour injecter ces variables dans le conteneur :
<img width="2060" height="909" alt="Capture d&#39;écran 2026-01-18 041432" src="https://github.com/user-attachments/assets/581f9a97-b380-4bb8-bf64-b24182eadc04" />



Étape 9 : Gérer les secrets (MONITORING_TOKEN)
<img width="2018" height="1176" alt="Capture d&#39;écran 2026-01-18 041523" src="https://github.com/user-attachments/assets/be589857-6a74-434e-9407-826e1ca13e61" />

<img width="1311" height="742" alt="image" src="https://github.com/user-attachments/assets/2e066f5b-f5b7-4a6a-9746-2962260a3a78" />

On Ajouter la variable d’environnement dans k8s/deployment.yaml

<img width="2029" height="892" alt="image" src="https://github.com/user-attachments/assets/0a7a4bef-e269-4808-a674-9bea28400d87" />



Étape 10 : Mise en place des endpoints de santé et des probes Kubernetes pour l’API Churn

<img width="1986" height="1457" alt="image" src="https://github.com/user-attachments/assets/9225f4da-b7c1-4ca9-87e3-52ab49b6b4a4" />

<img width="2027" height="799" alt="Capture d&#39;écran 2026-01-18 042149" src="https://github.com/user-attachments/assets/1b3c454f-544c-4435-acb5-c821c84385ae" />

<img width="2304" height="238" alt="Capture d&#39;écran 2026-01-18 042209" src="https://github.com/user-attachments/assets/723ef6ee-dc5b-4b52-a3d6-bf5e149d5176" />


Étape 11 : Ajouter les probes (liveness / readiness / startup)

<img width="2274" height="1062" alt="Capture d&#39;écran 2026-01-18 042305" src="https://github.com/user-attachments/assets/99b90793-e0e6-4335-a196-cf3915349f22" />


Étape 12 : Volume persistant pour registry + logs

<img width="2083" height="458" alt="image" src="https://github.com/user-attachments/assets/16c8e05e-c33c-4ebf-bacf-73c72739fdf8" />

<img width="1193" height="811" alt="image" src="https://github.com/user-attachments/assets/f194eacc-3cdc-48c3-a794-533a631f49a1" />

<img width="2240" height="723" alt="image" src="https://github.com/user-attachments/assets/f0fe547b-90e8-4053-8450-83b9f920361b" />


<img width="1791" height="1365" alt="image" src="https://github.com/user-attachments/assets/a8bf70e2-a659-43a9-a696-bb60f4b2bb05" />

<img width="2293" height="607" alt="image" src="https://github.com/user-attachments/assets/5c8d399a-e032-4adf-9d10-999178333df6" />



Étape 13 : NetworkPolicy

<img width="2278" height="1011" alt="Capture d&#39;écran 2026-01-18 042831" src="https://github.com/user-attachments/assets/8c7f2601-1871-413e-9299-1eb6eb225619" />

<img width="1448" height="970" alt="image" src="https://github.com/user-attachments/assets/adde261e-0253-4b6f-8096-4a81b967c3d1" />


Étape 14 : Vérifications finales
<img width="2403" height="943" alt="Capture d&#39;écran 2026-01-18 043005" src="https://github.com/user-attachments/assets/4e1b4950-bb5e-4e5a-a62f-0f97c11ce936" />

<img width="2063" height="463" alt="Capture d&#39;écran 2026-01-18 031857" src="https://github.com/user-attachments/assets/de4157d5-7bfe-4320-90a9-13dbc8c4e01f" />


<img width="2276" height="640" alt="image" src="https://github.com/user-attachments/assets/7d478fd4-96a5-4eb2-8545-57c5e8ab29f4" />

