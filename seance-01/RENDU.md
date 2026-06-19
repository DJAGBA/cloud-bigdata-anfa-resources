Exercices d'application - Séance 1
    Reponses aux applications

    Exercice 1:QCM conceptuel

    1.1. D. Open source obligatoire

    Justification : Selon le NIST, les caractéristiques essentielles sont l'accès réseau large, la mutualisation, le libre-service, l'élasticité et le service mesuré ; l'open source n'est pas une exigence technique du modèle

    1.2. C. SaaS

    Justification : Gmail est un logiciel complet accessible via un navigateur web, géré entièrement par le fournisseur, sans gestion d'infrastructure par l'utilisateur.

    1.3.D. FaaS (Function as a Service)

    Justification : Le modèle FaaS (Serverless) est idéal pour des exécutions basées sur des événements (trigger) sans maintenir un serveur actif, permettant de payer uniquement au temps d'exécution.

    1.4. C. Cloud hybride

    Justification : Le cloud hybride permet de garder les données sensibles sur site (Cloud privé/On-premise) tout en utilisant le Cloud public pour bénéficier de l'élasticité lors des calculs non critiques.

    1.5. B. La situation où une entreprise ne peut plus changer de fournisseur sans coûts ou risques majeurs

    Justification : Le vendor lock-in décrit une dépendance technologique ou économique forte rendant la migration vers un autre fournisseur complexe ou onéreuse.

    1.6.C. Un service open source est forcément moins performant qu'un service managé propriétaire

    Justification : C'est faux, car de nombreuses solutions open source sont extrêmement performantes et constituent souvent le cœur même des services managés propriétaires proposés par les grands fournisseurs.

        Exercice 2 : Classification de services
      
      | Service | Modèle | Justification |
| Google Compute Engine (machine virtuelle) | IaaS | Fournit des machines virtuelles brutes (infrastructure). |
| AWS Lambda | FaaS | Exécute du code en réponse à des événements sans gérer de serveur. |
| Snowflake (entrepôt de données) | SaaS | Solution d'entrepôt de données clé en main accessible via API/UI. |
| Heroku | PaaS | Plateforme permettant de déployer des applications sans gérer le système d'exploitation. |
| Microsoft 365 (Word, Excel en ligne) | SaaS | Suite bureautique complète livrée en tant que service logiciel. |
| Databricks (Spark managé) | PaaS | Plateforme analytique optimisée pour Spark, gérant le cluster et l'environnement. |
| Microsoft Azure Functions | FaaS | Équivalent d'AWS Lambda pour l'exécution de code éphémère. |
| Tableau Online | SaaS | Outil de visualisation de données hébergé et managé par l'éditeur. |

       Exercice 3 : Lecture et interprétation (moyen)

    3.1 Commande docker run
    -d : Exécute le conteneur en mode "détaché" (en arrière-plan)
    --name analyse-anfa : Donne un nom personnalisé au conteneur pour pouvoir le manipuler facilement par son nom.
    -p 8888:8888 : Mappe le port 8888 de la machine hôte vers le port 8888 du conteneur.
    -v /home/koffi/notebooks:/notebooks : Crée un volume liant le dossier local de l'hôte aux fichiers du conteneur (persistance des données).
    -e JUPYTER_TOKEN=anfa-token : Définit une variable d'environnement pour sécuriser l'accès au Jupyter Notebook avec un mot de passe.
    -jupyter/pyspark-notebook : L'image Docker utilisée comme base pour créer le conteneur.

      3.2 Lecture d'un docker-compose.yml
    a. Accès :
  
    API S3 : http://localhost:9000

    Console Web : http://localhost:9001

    b. Persistance :
    Les données ne sont pas perdues. Le volume minio-data est défini comme un volume géré par Docker (nommé). Même si le conteneur est supprimé, le volume persiste sur le disque de l'hôte et sera reconnecté au nouveau conteneur lors du prochain docker-compose up.

    c. Sécurité :
    Le mot de passe (MINIO_ROOT_PASSWORD: secret) est codé en dur dans le fichier. Il est fortement recommandé d'utiliser des variables d'environnement ou un fichier .env pour éviter de stocker des secrets en clair dans le système de gestion de versions (Git).

       Exercice 4 : Diagnostic
    a. Cause précise :
    L'étudiant utilise les identifiants racine du serveur MinIO (anfa-admin) au lieu des identifiants applicatifs créés via mc.

    b. Correction du code :
     Il faut remplacer les identifiants par ceux générés pour l'application :

     Python
    aws_access_key_id="anfa-app-key",
    aws_secret_access_key="anfa-app-secret-2026",

    c. Explication :
     MinIO différencie les accès :

     Root User : Utilisé pour l'administration globale et la connexion à la console web.

     Access Keys (IAM) : Ce sont des clés spécifiques créées pour des applications ou des services. C'est une bonne pratique de sécurité (principe du moindre privilège) : si l'application est compromise, on ne compromet pas les accès administrateur du serveur MinIO.

       Exercice 5 : Mini-cas d'architecture

    a. Limites de l'architecture actuelle
    -Absence de fraîcheur des données : Le traitement mensuel par CSV empêche toute prédiction en temps réel ou même à l'heure, car les données sont obsolètes dès l'export.

    -Dépendance au matériel (Single Point of Failure) : L'entraînement sur un ordinateur portable limite la puissance de calcul disponible et rend les modèles inutilisables si l'ordinateur de Toyi tombe en panne ou n'est pas disponible.

    b. Caractéristiques du Cloud (NIST) et  Besoins

    | Besoin de la direction  | Caractéristique du NIST  | Explication  |
| Prédictions à l'heure  | Accès réseau large | Permet d'accéder aux services de calcul n'importe où, n'importe quand. |
| Tableau de bord partagé  | Libre-service à la demande | Les analystes accèdent aux outils sans intervention manuelle du service IT. |
| Calcul lors des pics  | Élasticité rapide | Le cloud ajuste automatiquement les ressources selon la demande. |
| Maîtrise des coûts  | Service mesuré | On ne paie que pour les ressources consommées pendant le calcul. |

    c. Modèles de services proposés

    (i) Tableau de bord partagé : SaaS (ex: Tableau Online, PowerBI). Il permet un accès collaboratif via navigateur sans aucune gestion 
    
    d'infrastructure par l'équipe.

    (ii) Calcul des prédictions à l'heure : FaaS (ex: AWS Lambda, Azure Functions). Il est idéal pour déclencher un script de calcul chaque heure, sans payer pour un serveur inactif le reste du temps.

    (iii) Stockage des données clients : IaaS ou PaaS (ex: Instance S3 ou base de données gérée). Cela permet un contrôle granulaire des accès et de la sécurité, nécessaire à la conformité.

    d. Modèle de déploiement recommandé

    Je recommande un Cloud Hybride. Il permet de conserver les données clients les plus sensibles dans un environnement privé ou local (pour répondre aux contraintes de conformité) tout en utilisant la puissance de calcul et l'élasticité d'un cloud public pour entraîner les modèles et générer les prédictions en cas de pic de charge.

    e. Stratégies anti-vendor lock-in

    -Conteneurisation (Docker/Kubernetes) : encapsuler les applications de prédiction pour qu'elles puissent tourner sur n'importe quel fournisseur cloud sans modification.

    -Utilisation de standards Open Source : privilégier des outils comme MinIO (pour le stockage S3 compatible), PostgreSQL (base de données), ou Apache Spark, qui sont supportés partout.

    -Infrastructure as Code (IaC) : utiliser Terraform pour décrire l'infrastructure, ce qui permet de reproduire facilement l'environnement chez un autre fournisseur si nécessaire.

    