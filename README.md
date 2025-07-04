# Kafka Exchange Rates Project

Ce projet démontre l'intégration de Kafka avec Spring Boot pour traiter et stocker des données de taux de change dans Elasticsearch.

## Fonctionnalités

- **Producteur Kafka** : Récupère automatiquement les taux de change depuis une API externe
- **Consommateur Kafka** : Traite les messages et les stocke dans Elasticsearch
- **Intégration Elasticsearch** : Stockage et indexation des données de taux de change
- **API REST** : Endpoints pour consulter les données Elasticsearch
- **Configuration automatique** : Création automatique de l'index Elasticsearch au démarrage

## Prérequis

- Java 23
- Apache Kafka (démarré sur localhost:9092)
- Elasticsearch (démarré sur localhost:9200)
- Maven

## Configuration

1- Ajouter les configurations kafka dans application.properties

```properties
spring.kafka.bootstrap-servers=localhost:9092
spring.kafka.consumer.group-id=tp-kafka-step1
spring.kafka.topic.name=mon-tunnel-topic

elasticsearch.url=http://localhost:9200
elasticsearch.index=exchange_rates
```

## Architecture

### 2- Producteur Kafka

- **Package** : `com.learn.kafka.producer`
- **KafkaProducerConfig** : Configuration du producteur Kafka avec sérialisation String
- **MessageProducer** : Service pour envoyer des messages vers les topics Kafka
- **WebServiceProvider** : Récupère automatiquement les taux de change au démarrage de l'application

### 3- Consommateur Kafka

- **Package** : `com.learn.kafka.consumer`
- **KafkaConsumerConfig** : Configuration du consommateur Kafka avec désérialisation String
- **MessageConsumer** : Écoute le topic Kafka et envoie les données vers Elasticsearch

### 4- Intégration Elasticsearch

- **ElasticsearchSetup** : Configuration automatique de l'index au démarrage
- **ElasticsearchController** : API REST pour consulter les données

## Endpoints disponibles

### API Elasticsearch

- `GET /api/elasticsearch/status` - Statut du serveur Elasticsearch
- `GET /api/elasticsearch/index/info` - Informations sur l'index exchange_rates
- `GET /api/elasticsearch/data` - Recherche dans les données stockées
- `GET /api/elasticsearch/count` - Nombre de documents dans l'index

````

## Démarrage du projet

1. **Démarrer Kafka et Elasticsearch**

   ```bash
   # Kafka (suivre la documentation officielle)
   # Elasticsearch (suivre la documentation officielle)
````

2. **Vérification automatique**
   - L'application récupère automatiquement les taux de change au démarrage
   - Les données sont envoyées vers le topic Kafka
   - Le consommateur traite les messages et les stocke dans Elasticsearch

## Données traitées

Le projet utilise l'API [ExchangeRate-API](https://api.exchangerate-api.com/v4/latest/USD) pour récupérer les taux de change en temps réel avec la base USD.

Structure des données :

```json
{
  "base": "USD",
  "date": "2025-06-26",
  "time_last_updated": 1719360000,
  "rates": {
    "EUR": 0.85,
    "GBP": 0.73,
    "JPY": 110.25
    // ... autres devises
  }
}
```

## Monitoring

L'application expose les endpoints Actuator pour le monitoring :

- `http://localhost:8080/actuator/health`
- `http://localhost:8080/actuator/info`
- Tous les endpoints Actuator sont exposés via la configuration

## Structure du projet

```
src/
├── main/java/com/learn/kafka/
│   ├── KafkaApplication.java           # Point d'entrée de l'application
│   ├── WebServiceProvider.java        # Service de récupération des taux de change
│   ├── config/
│   │   └── ElasticsearchSetup.java     # Configuration Elasticsearch
│   ├── consumer/
│   │   ├── KafkaConsumerConfig.java    # Configuration consommateur
│   │   └── MessageConsumer.java        # Logique de consommation
│   ├── controller/
│   │   └── ElasticsearchController.java # API REST
│   └── producer/
│       ├── KafkaProducerConfig.java    # Configuration producteur
│       └── MessageProducer.java        # Logique de production
└── resources/
    └── application.properties          # Configuration de l'application
```
