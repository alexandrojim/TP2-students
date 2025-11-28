# UE Introduction à l’Intelligence Artificielle

## Informations générales
- Master 1, Semestre 1, 3 ECTS
- Code UE : UM4RBI12
- Chargé de Cours et resp. UE : Prof. Daniel Racoceanu
- Chargé de TP : Gabriel Jimenez
- Version : nov. 2025

## Travaux Pratiques (TP) / *Laboratories (Labs)*
### TP2 - Systèmes Bayésiens *(Bayesian systems)*

L'objectif de ce laboratoire est de comprendre les différences entre l'approche fréquentiste et l'approche bayésienne et l'application au traitement d'images.

*The objective of this lab is to understand the differences between the frequentist and bayesian approach and the application to image processing.*

### General instructions
1. Tous les codes sont écrits en Python et ont été testés sur Google Collab. Veuillez suivre les instructions sur les Notebooks Jupyter correspondants à chaque exercice afin d'installer les libraries nécessaires.

    *All codes are written in Python and were tested on Google Collab. Please follow the instructions on the corresponding Jupyter Notebooks from each exercise in order to install the necessary libraries.*

2. Les CRs du TP vont consister en les codes-source Python ou Jupyter des Exos 1 et 2 (avec vos propres modifications, rajouts et commentaires, intégrés). Les 2 CRs est à déposer sur Moodle (dossier CR_TP1_votre_option (ISI, SAR, IPS, app)). 

    *The reports of the Lab will consist of the Python or Jupyter source codes of Exo 1 and 2 (with your own adds, modifications, and comments integrated). The 2 reports are to be placed on Moodle (CR_TP1_your_option (ISI, SAR, IPS, app) folder).*

### Context and Theoretical Background

Before diving into the code, it is essential to understand the fundamental difference between the Frequentist and Bayesian approaches, and how we structure complex dependencies using Bayesian Networks.

1. Frequentist vs. Bayesian: A Shift in Perspective

    - *Frequentist Approach:* Treats parameters (like the probability of a coin landing on heads) as fixed but unknown constants. Probability is defined as the limit of relative frequency over infinite trials. We use data to estimate these fixed parameters (e.g., Maximum Likelihood Estimation) and rely on p-values to reject null hypotheses.
    - *Bayesian Approach:* Treats parameters as random variables describing a degree of belief. We start with a Prior belief (before seeing data), collect evidence (Likelihood), and update our belief to form a Posterior distribution.
    
2. Bayesian Networks (BNs)

    A Bayesian Network is a Probabilistic Graphical Model that represents a set of variables and their conditional dependencies via a Directed Acyclic Graph (DAG).

    - Nodes: Represent random variables.
    - Edges: Represent conditional dependencies (causality).
    - CPTs (Conditional Probability Tables): Quantify the relationship between a node and its parents.
    
    BNs allow us to perform Inference: updating the probabilities of unobserved variables when we observe the state of others (evidence).

3. Conditional Independence and d-Separation

One of the most powerful features of Bayesian Networks is that they explicitly show us which variables are independent of others. This concept is called **d-separation** (directed separation).

Think of the edges in the graph as "pipes" through which information flows. If a path is "blocked," information cannot flow, and the variables are **Conditionally Independent**.

There are three main patterns that determine if information flows:

- **Serial (Chain):** $A \rightarrow B \rightarrow C$
    * *Default:* $A$ influences $C$.
    * *With Evidence:* If we observe **$B$**, the path is **blocked**. $A$ and $C$ become independent. (Knowing $A$ gives us no *new* info about $C$ if we already know $B$).
- **Diverging (Common Parent):** $A \leftarrow B \rightarrow C$
    * *Default:* $A$ and $C$ are correlated (due to common cause $B$).
    * *With Evidence:* If we observe **$B$**, the path is **blocked**. $A$ and $C$ become independent.
- **Converging (Collider/V-structure):** $A \rightarrow B \leftarrow C$
    * *Default:* $A$ and $C$ are independent (different causes). The path is **blocked**.
    * *With Evidence:* If we observe **$B$** (or a descendant of $B$), the path is **unblocked**. $A$ and $C$ become dependent. This is the logic behind "Explaining Away."

**The Markov Blanket:**
The **Markov Blanket** of a node is the set of nodes that completely shields it from the rest of the network. It consists of:
* Its Parents
* Its Children
* Its Children's Parents (Spouses)

If you know the state of all nodes in the Markov Blanket, knowing anything else in the network is irrelevant to that node.