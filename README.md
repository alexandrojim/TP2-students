# UE Introduction à l’Intelligence Artificielle

## Informations générales
- Master 1, Semestre 1, 3 ECTS
- Code UE : UM4RBI12
- Chargé de Cours et resp. UE : Prof. Daniel Racoceanu
- Chargé de TP : Gabriel Jimenez
- Version : nov. 2025

## Travaux Pratiques (TP) / *Laboratories (Labs)*
### TP2 - Systèmes Bayésiens *(Bayesian systems)*

L'objectif de ce laboratoire est de comprendre les différences entre l'approche fréquentiste et l'approche bayésienne ainsi que l'utilisation des réseaux bayesiens.

*The objective of this lab is to understand the differences between the frequentist and bayesian approach as well as understanding bayesian networks.*

### General instructions
1. Tous les codes sont écrits en Python et ont été testés sur Google Collab. Veuillez suivre les instructions sur les Notebooks Jupyter correspondants à chaque exercice afin d'installer les libraries nécessaires.

    *All codes are written in Python and were tested on Google Collab. Please follow the instructions on the corresponding Jupyter Notebooks from each exercise in order to install the necessary libraries.*

2. Les CRs du TP vont consister en les codes-source Python ou Jupyter des Exos 1 et 2 (avec vos propres modifications, rajouts et commentaires, intégrés). Les 2 CRs est à déposer sur Moodle (dossier CR_TP1_votre_option (SI)). 

    *The reports of the Lab will consist of the Python or Jupyter source codes of Exo 1 and 2 (with your own adds, modifications, and comments integrated). The 2 reports are to be placed on Moodle (CR_TP1_your_option (SI) folder).*

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

One of the most powerful features of Bayesian Networks is that they explicitly show us which variables are independent of others. This concept is called **d-separation**.

Think of the edges as "pipes" for information. If a path is "blocked," the variables are **Conditionally Independent**.

Here are the three standard patterns with concrete examples:

1.  **Serial (Chain):** $A \rightarrow B \rightarrow C$
    * **Example:** $\text{Fire} \rightarrow \text{Smoke} \rightarrow \text{Alarm}$
    * **Logic:** A fire causes smoke, and smoke causes the alarm to ring.
    * **The Rule:** If we **observe the Smoke ($B$)**, the path is **blocked**.
    * *Why?* If you already see the smoke, knowing there is a Fire ($A$) doesn't give you any *extra* information about whether the Alarm ($C$) will ring. The smoke is the direct trigger. $A$ and $C$ become independent given $B$.

2.  **Diverging (Common Parent):** $A \leftarrow B \rightarrow C$
    * **Example:** $\text{Ice Cream Sales} \leftarrow \text{Summer Heat} \rightarrow \text{Sunburns}$
    * **Logic:** Hot weather causes both higher ice cream sales and more sunburns.
    * **The Rule:** If we **observe the Heat ($B$)**, the path is **blocked**.
    * *Why?* Usually, high ice cream sales correlate with sunburns (because both happen in summer). But if we *already know* it is Hot ($B$), checking Ice Cream sales ($A$) tells us nothing new about the probability of Sunburns ($C$). The heat explains both.

3.  **Converging (Collider):** $A \rightarrow B \leftarrow C$
    * **Example:** $\text{Rain} \rightarrow \text{Wet Grass} \leftarrow \text{Sprinkler}$
    * **Logic:** Both Rain and the Sprinkler can cause the grass to get wet. Rain and Sprinklers are usually independent (rain doesn't turn on the sprinkler).
    * **The Rule:** This path is **blocked by default** but becomes **unblocked** if we observe **Wet Grass ($B$)**.
    * *Why?* If we see the grass is wet ($B$), and we find out it is Raining ($A$), the probability that the Sprinkler is on ($C$) goes **down**. The rain "explains away" the wet grass, making the sprinkler unnecessary. This creates a dependency between $A$ and $C$.


**The Markov Blanket:**
The **Markov Blanket** of a node is the set of nodes that completely shields it from the rest of the network. If you know the state of all nodes in the Markov Blanket, you have all the information possible about that node; no other node in the network matters anymore.