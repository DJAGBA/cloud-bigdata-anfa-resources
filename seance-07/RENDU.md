# Rendu — Séance 7

**Nom et prénom :** <Votre nom complet>
**Identifiant GitHub :** <votre-username>
**Date de soumission :** <JJ/MM/AAAA>

## Résumé de la séance

<2-4 lignes : cluster Kafka 3 brokers déployé, flotte de bus simulée en flux continu,
tolérance aux pannes observée, Spark Structured Streaming consommant et agrégeant le flux vers MinIO.>

## Étapes principales

1. Déploiement du cluster Kafka (3 brokers, mode KRaft) + Kafka UI.
2. Création du topic `anfa-positions-bus` (3 partitions, réplication 3).
3. Premier producer/consumer Python pour comprendre la mécanique.
4. Simulation de 100 bus envoyant leur position en continu.
5. Démonstration de tolérance aux pannes (arrêt d'un broker).
6. Spark Structured Streaming : lecture console, puis agrégation en fenêtre vers MinIO.

## Captures d'écran

### 3 brokers actifs dans Kafka UI
![Brokers actifs](captures/kafka-ui-brokers.png)

### Débit de messages en augmentation
![Débit messages](captures/kafka-ui-debit.png)

### Cluster avec 2 brokers sur 3 (après arrêt volontaire)
![2 brokers sur 3](captures/kafka-ui-2-brokers.png)

### Micro-batchs affichés en console par Spark
![Console Spark Streaming](captures/spark-streaming-console.png)

### Résultats agrégés dans MinIO
![MinIO agregats](captures/minio-agregats.png)

## Réflexion personnelle

Kafka + Spark Streaming est utile quand il faut traiter des flux en temps réel, comme suivre la position des bus ou détecter des anomalies instantanément. Le pipeline batch Airflow + Spark des séances 5–6 reste adapté aux traitements différés, par exemple les rapports quotidiens. La réplication sur 3 brokers m’a montré concrètement que le cluster continue de fonctionner même si un nœud tombe. Les partitions basculent automatiquement vers un autre leader, garantissant disponibilité et absence de perte de données.
## Réponses aux exercices d'application

<À compléter d'après les énoncés fournis avec l'assignment.>

## Difficultés rencontrées

<Aucune | Décrivez brièvement.>
