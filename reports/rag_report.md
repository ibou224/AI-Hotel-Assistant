# Rapport – Évaluation du système RAG

## 1. Objectif

L’objectif de cette section est d’évaluer la performance du système **RAG (Retrieval-Augmented Generation)** développé dans ce projet pour répondre à des questions liées aux opérations internes de l’Hôtel De la Promenade.

Le système repose sur :

- une base documentaire composée de plusieurs documents PDF internes
- une indexation vectorielle des documents
- un mécanisme de recherche sémantique
- un modèle de langage utilisé pour générer les réponses

Deux analyses principales sont présentées dans ce rapport :

1. Documentation et justification des **prompts testés**
2. Analyse de **15 conversations de test couvrant différents scénarios**

---

# 2. Documentation des prompts testés

Dans cette expérimentation, plusieurs stratégies de prompting ont été testées afin d’évaluer leur impact sur la qualité des réponses générées par le système RAG.

Trois types de prompts ont été utilisés :

- Zero-shot prompting
- Few-shot prompting
- Chain-of-Thought interne

---

## 2.1 Prompt Zero-Shot

Le prompt zero-shot est le plus simple. Il donne uniquement des instructions au modèle sans fournir d’exemples de réponse.

### Objectif

Tester la capacité du modèle à répondre correctement en se basant uniquement sur :

- les documents récupérés par le retriever
- les instructions du prompt

### Structure du prompt

Le prompt impose des règles strictes :

- répondre uniquement à partir du contexte fourni
- ne pas inventer d’information
- indiquer explicitement lorsque l’information n’est pas présente dans les documents

Exemple de règle utilisée :

> Si l'information n'est pas dans le contexte, réponds exactement :  
> "Je ne trouve pas cette information dans les documents."

### Justification

Ce prompt permet de :

- réduire les hallucinations
- garantir une utilisation stricte des documents récupérés
- tester la capacité du modèle à suivre des instructions précises

Cependant, cette approche peut parfois produire des réponses plus courtes ou moins détaillées.

---

## 2.2 Prompt Few-Shot

Le prompting few-shot ajoute **quelques exemples de questions et réponses** dans le prompt.

### Objectif

Montrer au modèle :

- le format attendu des réponses
- le style de réponse souhaité
- la manière d’utiliser les documents récupérés

### Avantages

Cette approche permet souvent :

- d’améliorer la cohérence des réponses
- d'améliorer la structure des explications
- de mieux guider le modèle dans l’utilisation du contexte

### Limites

L’ajout d’exemples augmente la taille du prompt, ce qui peut :

- augmenter le coût computationnel
- réduire la place disponible pour le contexte documentaire

---

## 2.3 Prompt Chain-of-Thought interne

Le troisième type de prompt introduit une forme de **raisonnement interne (Chain-of-Thought)**.

### Objectif

Encourager le modèle à :

1. analyser le contexte récupéré
2. identifier les passages pertinents
3. produire une réponse basée sur ces éléments

### Principe

Le raisonnement reste **interne au modèle** et n’est pas nécessairement exposé dans la réponse finale.

Cette approche vise à améliorer :

- la compréhension du contexte
- la cohérence des réponses
- la précision factuelle

---

# 3. Conversations de test

Afin d’évaluer le système RAG, **15 scénarios de questions** ont été testés.

Ces questions couvrent différents cas d’usage liés aux opérations internes de l’hôtel.

---

## 3.1 Catégories de scénarios testés

Les questions ont été choisies pour couvrir plusieurs types de situations :

### 1. Processus opérationnels

Exemples :

- Quelles sont les étapes de gestion des réservations ?
- Comment gérer une modification de réservation ?
- Comment gérer une annulation de réservation ?

Objectif :

Vérifier si le système peut restituer correctement des **procédures internes**.

---

### 2. Recherche d'information spécifique

Certaines questions demandent des informations précises contenues dans les documents.

Objectif :

Évaluer la capacité du retriever à identifier les passages pertinents.

---

### 3. Questions partiellement couvertes

Certaines questions sont volontairement ambiguës ou incomplètes.

Objectif :

Tester la robustesse du système et sa capacité à :

- utiliser le contexte
- éviter les hallucinations

---

### 4. Questions hors contexte

Certaines questions peuvent ne pas être directement couvertes par les documents.

Objectif :

Vérifier si le système respecte la règle :

> indiquer clairement que l'information n'est pas disponible.

---

# 4. Observations générales

L’analyse des conversations testées montre plusieurs tendances.

### 4.1 Qualité du retrieval

Le retriever basé sur les embeddings **sentence-transformers/all-MiniLM-L6-v2** permet généralement de récupérer des passages pertinents.

Le chunking des documents (1500 caractères avec chevauchement) permet de préserver suffisamment de contexte.

---

### 4.2 Influence du prompt

Les résultats varient selon la stratégie de prompting :

**Zero-shot**

- réponses plus courtes
- respect strict du contexte
- moins d’explications

**Few-shot**

- réponses plus structurées
- meilleure cohérence
- style plus naturel

**Chain-of-Thought**

- réponses souvent plus complètes
- meilleure capacité à synthétiser les documents

---

### 4.3 Réduction des hallucinations

Grâce à l’architecture RAG et aux instructions strictes du prompt :

- les hallucinations sont fortement réduites
- le modèle s’appuie principalement sur les documents récupérés

Cependant, certaines réponses peuvent rester partielles si :

- le retriever ne récupère pas les bons passages
- l’information est répartie sur plusieurs chunks.

---

# 5. Limites observées

Plusieurs limites ont été identifiées durant les tests.

### 1. Dépendance au retrieval

Si les documents récupérés ne contiennent pas l’information pertinente, la réponse générée sera incomplète.

---

### 2. Sensibilité au chunking

Le découpage des documents peut séparer des informations importantes sur plusieurs segments.

---

### 3. Taille du contexte

Le nombre de documents récupérés influence la qualité de la réponse.

Un contexte trop court peut manquer d’information, tandis qu’un contexte trop long peut diluer les passages pertinents.

---

# 6. Conclusion

L’expérimentation montre que l’architecture **RAG** constitue une solution efficace pour exploiter une base documentaire interne.

Les principaux avantages observés sont :

- amélioration de la précision factuelle
- réduction significative des hallucinations
- capacité à répondre à des questions complexes en s’appuyant sur les documents.

Par ailleurs, l’étude des différents prompts montre que la stratégie de prompting influence fortement la qualité des réponses.

Dans ce projet, l’approche **Few-shot et Chain-of-Thought** offre généralement les réponses les plus complètes et structurées.

Le système RAG constitue donc une base solide pour le développement d’un assistant interne capable de répondre aux questions opérationnelles à partir de la documentation de l’hôtel.