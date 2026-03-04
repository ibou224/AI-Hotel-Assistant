# Analyse comparative des modèles : Base vs Fine-Tuned

## 1. Contexte de l’évaluation

L’objectif de cette évaluation est de comparer les performances du modèle de base (*Meta-Llama-3.1-8B-Instruct*) et du modèle fine-tuné (LoRA entraîné sur la FAQ de l’Hôtel De la Promenade) à partir des réponses générées sur un ensemble de questions de test.

Les critères d’analyse retenus sont :

- Exactitude factuelle (respect strict des informations de la FAQ)
- Présence ou absence d’hallucination
- Cohérence stylistique avec l’identité de l’hôtel
- Capacité de généralisation sur des reformulations
- Gestion des questions non couvertes par la FAQ

Les réponses ont été générées avec des paramètres déterministes (`do_sample=False`) afin d’assurer une comparaison équitable et reproductible.

---

## 2. Analyse qualitative détaillée

### 2.1 Question factuelle directe : heure d’enregistrement

Les deux modèles indiquent correctement que l’enregistrement est possible à partir de 15h00.

Le modèle de base fournit une réponse correcte et relativement neutre.  
Le modèle fine-tuné propose une formulation plus concise et légèrement plus structurée, mais introduit une mention vague d’un système d’enregistrement anticipé sans précision tarifaire ou conditionnelle claire.

Interprétation :  
Le fine-tuning améliore la forme et la concision, mais ne garantit pas une stricte fidélité aux formulations exactes de la FAQ.

---

### 2.2 Informations tarifaires (stationnement, départ, late check-out)

Pour les questions concernant :
- le prix du stationnement,
- l’heure de départ,
- le coût du late check-out,

les deux modèles répondent qu’ils ne retrouvent pas l’information dans la FAQ.

Or, ces informations figurent dans le document d’entraînement.

Cela suggère :
- une mémorisation partielle des données,
- une capacité limitée de rappel précis,
- ou un effet de prudence excessive induit par le prompt strict.

Interprétation :  
Le fine-tuning n’a pas permis une consolidation robuste des détails tarifaires. L’alignement stylistique semble plus marqué que l’ancrage factuel.

---

### 2.3 Reformulation : arrivée vers midi

À la question « Puis-je arriver vers midi ? », les deux modèles répondent que l’arrivée est possible à tout moment.

Cette réponse est incorrecte au regard de la FAQ.

Il s’agit d’une hallucination typique :
- Le modèle généraliste applique une règle hôtelière générique.
- Le modèle fine-tuné ne corrige pas ce biais pré-entraîné.

Interprétation :  
Le fine-tuning par LoRA (environ 0,17 % des paramètres entraînables) ne modifie pas suffisamment les représentations internes pour neutraliser les comportements probabilistes appris lors du pré-entraînement.

---

### 2.4 Détail technique précis : hauteur du parking

Les deux modèles indiquent correctement une limite de hauteur de 2,10 mètres.

Le modèle fine-tuné se montre plus concis et légèrement mieux structuré.

Interprétation :  
Lorsque l’information est numérique, explicite et clairement représentée dans le dataset, le fine-tuning permet une restitution fidèle tout en améliorant la qualité rédactionnelle.

---

### 2.5 Questions non couvertes par la FAQ (caméras, piscine)

Pour les questions :
- « Le parking est-il surveillé par caméras ? »
- « Avez-vous une piscine ? »

les deux modèles inventent des réponses positives.

Le modèle fine-tuné ajoute parfois davantage de détails (par exemple des précisions sur la conservation des images ou des éléments descriptifs supplémentaires).

Interprétation :  
Le fine-tuning stylistique améliore la fluidité et la cohérence narrative, mais ne supprime pas les mécanismes génératifs probabilistes du modèle de base. Les hallucinations persistent et peuvent même être amplifiées par une rédaction plus détaillée.

---

## 3. Synthèse comparative

| Critère | Modèle de base | Modèle fine-tuné |
|----------|----------------|------------------|
| Style et ton | Correct mais générique | Plus aligné avec l’identité |
| Exactitude factuelle | Variable | Légèrement améliorée sur certains points |
| Rappel des tarifs | Incomplet | Incomplet |
| Hallucinations | Présentes | Toujours présentes |
| Gestion hors FAQ | Répond au lieu de refuser | Répond au lieu de refuser |

---

## 4. Interprétation technique

Le fine-tuning par LoRA agit principalement comme un mécanisme d’alignement stylistique plutôt que comme une reconfiguration profonde des connaissances internes.

Compte tenu :
- de la petite taille du dataset (30 exemples),
- du faible pourcentage de paramètres entraînables,
- de l’absence de mécanisme de retrieval externe,

le modèle conserve une forte dépendance aux distributions probabilistes générales issues du pré-entraînement.

Ainsi :
- le ton et la cohérence rédactionnelle s’améliorent,
- la concision progresse,
- mais la fidélité factuelle n’est pas garantie.

---

## 5. Conclusion

Cette comparaison montre que le fine-tuning léger par LoRA permet d’adapter efficacement le style et l’identité de marque d’un assistant conversationnel.

Cependant, il ne suffit pas à garantir une stricte fidélité aux données sources ni à éliminer les hallucinations.

Pour un déploiement en contexte réel, une architecture combinant fine-tuning stylistique et Retrieval-Augmented Generation (RAG) serait plus adaptée afin d’assurer un contrôle fiable de la véracité des réponses.

En conclusion, le fine-tuning améliore l’alignement et la cohérence stylistique, mais ne constitue pas à lui seul un mécanisme robuste de validation factuelle.