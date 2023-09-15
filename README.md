# Kafka Log Enricher (kafka_log_enricher_TF-IDF.py)

Une interface web permettant d'extraire des messages d'un topic Kafka spécifique, d'afficher ces messages et de proposer un enrichissement avec des informations supplémentaires. Les messages enrichis sont ensuite renvoyés dans un autre topic Kafka.

Configuration et initialisation : Le script commence par importer les bibliothèques nécessaires, lire la configuration depuis un fichier config.ini, configurer le Consumer et le Producer Kafka, et initialiser quelques autres paramètres.

Récupération des données : fetch_initial_training_data() est une fonction qui récupère les messages d'un topic Kafka et renvoie une liste de logs et leurs étiquettes.

Apprentissage Actif : Le script utilise l'apprentissage actif pour aider à catégoriser les messages. Lorsqu'un message est reçu, si le modèle est assez sûr de sa prédiction (avec une probabilité supérieure à 95 %), il étiquette automatiquement le message. Sinon, il sollicite une intervention humaine pour l'étiquetage.

Envoi des données : Le script prend également en charge l'envoi de messages étiquetés à un topic Kafka.

App Flask : Enfin, le script utilise Flask pour créer une interface web simple qui permet à un utilisateur d'étiqueter les messages pour lesquels le modèle n'est pas sûr.

Exécution : Lorsque le script est exécuté, il vérifie d'abord si le modèle a déjà été formé et sauvegardé. Si c'est le cas, il charge le modèle. Sinon, il utilise les données récupérées du topic Kafka pour former un nouveau modèle.

## Fonctionnalités

- Lire les messages d'un topic Kafka en temps réel.
- Afficher le contenu du champ "log" de messages formatés en JSON.
- Proposer d'enrichir chaque message avec trois informations : sévérité, type d'événement et la Catégorie générale en composant
- Renvoyer le message enrichi dans un topic Kafka défini.

## Prérequis

- Le topic Kafka "ENRICHED" doit disposer d'au moins 1000 messages catégorisés. Une fois ce volume de message disponible vous pourrez commenter la ligne :

  ```bash
    ACTIVE_LEARNING_ENABLED = False
  ```

- Le message dans le topic Kafka doit avoir le format suivant :

   ```bash
    {
        "log": "my_log_message",
        "my_key": "my_value",
        ...
    }
   ```

   ou

      ```bash
    {
        "message": "my_log_message",
        "my_key": "my_value",
        ...
    }
   ```

- Python 3.x
- Flask
- confluent-kafka
- configparser

## Installation

1. **Clonez le répertoire**:

   ```bash
   git clone https://github.com/NeilOrley/kafka-log-enricher.git
   cd kafka-log-enricher
   ```

2. **Configuration**:

   Mettez à jour le fichier `config.ini` avec les informations appropriées pour vos serveurs Kafka, groupes et topics.

3. **Activez l'environnement virtuel**:

   - Naviguez vers le dossier du projet et créer un environnement virtuel :
    ```bash
    cd kafka-log-enricher
    python -m venv venv
    ```

   - Sur Windows:
     ```bash
     .\venv\Scripts\Activate
     ```

   - Sur macOS ou Linux:
     ```bash
     source venv/bin/activate
     ```

4. **Installez les dépendances**:

   - Sur Windows:
   ```bash
   .\venv\Scripts\python.exe -m pip install --upgrade pip
   pip install -r .\app\requirements.txt
   ```

   - Sur macOS ou Linux:
   ```bash
   pip install -r app/requirements.txt
   ```

5. **Exécution**:

   ```bash
   python kafka_log_enricher.py
   ```

## Utilisation

1. Exécutez l'application:

   ```bash
   python kafka_log_enricher.py
   ```

2. Naviguez vers `http://localhost:5000` dans votre navigateur pour accéder à l'interface utilisateur.

3. Les logs de Kafka seront automatiquement récupérés du topic spécifié. Enrichissez chaque log en sélectionnant sa gravité et son type d'événement, puis cliquez sur "Send" pour envoyer le log enrichi vers le topic Kafka de sortie.


## Configuration

La configuration de l'application est gérée par le fichier `config.ini`. Ce fichier contient des sections pour le Consumer Kafka et le Producer Kafka, ainsi que des paramètres spécifiques comme les noms des topics.

Exemple de configuration :

```ini
[CONSUMER]
bootstrap.servers = your_broker
group.id = your_group
auto.offset.reset = earliest
topic = your_topic

[PRODUCER]
bootstrap.servers = your_broker
output_topic = your_output_topic

[CLASSIFIERS]
severities = INFO,DEBUG,WARNING,ERROR,CRITICAL
event_types = Heartbeat,State change,Authentication and Authorization,Operations on files and apps,Network Communication,Security and Anomalies,Performance and Resources,User Interactions,Useless
categories = Hardware Infrastructure,Software Infrastructure,Connectivity and Security,Datas,Application & Middleware,Monitoring & Logging,Automation & CI/CD,Uncategorized
```

## Contribuer

Si vous souhaitez contribuer à ce projet, veuillez suivre les étapes suivantes :

1. **Fork** le projet.
2. Créez votre **Feature Branch** (`git checkout -b feature/AmazingFeature`).
3. Commitez vos changements (`git commit -m 'Add some AmazingFeature'`).
4. Poussez dans la **Branch** (`git push origin feature/AmazingFeature`).
5. Ouvrez une **Pull Request**.


Pour intégrer un "disclaimer" ou un avertissement indiquant que le texte a été généré en utilisant ChatGPT, vous pouvez ajouter une note soit au début soit à la fin du document. Voici quelques suggestions sur la manière dont vous pourriez le formuler, en fonction de votre préférence :


> ---
>
> _Note : Ce texte a été généré avec l'aide de ChatGPT, un modèle linguistique développé par OpenAI._
