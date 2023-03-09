# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
1.
La découverte d'un bug logiciel a été signalé début 2022. Ce bug affecte les serveurs Exchange installés sur site chez les clients de Microsoft, empêchant la distribution des courriers électroniques du 1er janvier 2022 au 4 janvier 2022. La raison de ce dysfonctionnement est due à un bug dans le moteur d'analyse anti-spam FIP-FS, qui est activé par défaut à partir d'Exchange Server 2013.

Le problème a été identifié comme étant lié à une variable int32 signée utilisée pour stocker la valeur d'une date. La valeur maximale de cette variable est de 2 147 483 647, ce qui est inférieur à la valeur minimale des dates de 2022 (2 201 010 001). Cette incompatibilité a entraîné l'échec du moteur d'analyse et la non-délivrance des courriers électroniques.

Le bug était local, c'est-à-dire qu'il n'affectait que le système anti-spam et non d'autres systèmes. Cependant, il a eu des conséquences globales car il entrainait le non-envoie d'emails.

Une solution de contournement a été trouvée, consistant à désactiver le moteur d'analyse FIP-FS à l'aide de commandes PowerShell spécifiques. Toutefois, cette solution non officielle présente le risque d'augmenter le nombre d'e-mails malveillants et de spams parvenant aux utilisateurs, car les courriers distribués ne seront plus analysés par le moteur de Microsoft. 

Les conséquences de ce bug sont donc importantes pour les clients et pour Microsoft, qui on dû corriger rapidement la situation pour limiter les impacts négatifs pour les utilisateurs. 

Avec des tests plus exhaustifs de dates, Microsoft aurait pu empêcher cette erreur, en effet c’est une erreur assez commune et facilement détectable si tester.

![Lien d'un article](https://www.pdq.com/blog/microsoft-exchange-2022-bug-halts-email-delivery/)

2.
Nous avons choisis de prendre l’issue COLLECTIONS-701 : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-701?filter=doneissues

Ce bug est locale puisqu’il survient sur un cas d’utilisation particulier d’une méthode qui n’avait sûrement pas été prévu ni testé par le développeur.

Le bug survient lorsque une liste de type SetUniqueList tente de s’ajouter à elle même. (ex : myList.add( myList )). Dans ce scénario, la méthode entre dans une boucle infinie qui cause l’exception StackOverFlowError.

Pour éviter ce bug, la méthode SetUniqueList.add() a été mise à jour dans les versions suivant la découverte du bug. Maintenant la méthode lève une exception lorsqu’elle reçoit elle-même en tant que paramètre. L’exception est levée avec le message : « Cannot add element to itself » pour que les développeurs puissent identifier le problème.

Le bug a été corrigé et des tests ont été ajouté pour éviter que le problème revienne. Voici le lien vers le commit en question : https://github.com/apache/commons-collections/pull/57/commits/be0cea3c907bb4ab1083384377521942d17e15bd


3.  
L'article décrit les expériences de Netflix en matière d'ingénierie du chaos, qui consistaient à introduire des défaillances contrôlées dans leurs systèmes afin de tester leur résilience. Ils ont identifié des indicateurs clés de performance pour chaque système et observé des variables telles que les temps de réponse et les taux d'erreur. L'indicateur clé principal de Netflix est combien d'utilisateur commence a regardé une vidéo chaque seconde. Cette mètrique varie lentement et de façon prévisible au cours d'une journée. 

Les principaux résultats des expériences ont montré qu'en introduisant des défaillances contrôlées, Netflix a pu identifier et corriger les faiblesses de ses systèmes avant qu'elles ne provoquent des temps d'arrêt réels ou un impact sur les clients. Elle a également été en mesure d'améliorer la résilience de ses systèmes en traitant de manière proactive les scénarios de défaillance potentiels.

Ces expériences pourraient être menées dans d'autres organisations en identifiant les composants critiques de leurs systèmes, en déterminant les indicateurs clés de performance et en introduisant des défaillances contrôlées pour voir comment les systèmes réagissent. Les variables du système à observer pendant les expériences pourraient inclure l'utilisation du CPU et de la mémoire, le trafic réseau, les taux d'erreur et les temps de réponse. En menant ces expériences, les organisations peuvent identifier les faiblesses de leurs systèmes et améliorer leur résilience globale, ce qui se traduit par une meilleure satisfaction des clients et une réduction des temps d'arrêt.

On pourrait utiliser le Chaos testing sur une application que l'on veut disponible pour les utilisateurs 24/7.

4.
Voici les principaux avantages d’avoir une spécification formelle pour WebAssembly : 

- Ça aide à garantir que les différentes implémentations de WebAssembly sont compatibles et interopérables entre elles

- WebAssembly est conçu pour être indépendant de la plate-forme et pour fonctionner sur différents appareils et architectures. Une spécification formelle peut donc aider à garantir que le code écrit en WebAssembly est portable et peut s'exécuter sur n'importe quel appareil prenant en charge WebAssembly

- Ça permet de faciliter la vérification des tests des implémentations.

Faire des tests est une partie importante du processus de développement, peu importe s’il y a une spécification formelle ou non. La spécification ne permet pas de garantir la non-existence de bug ni l’absence de vulnérabilité de sécurité.
En plus, les tests peuvent également aider à identifier les problèmes ou les incohérences dans la spécification formelle elle-même, ce qui peut être utilisé pour améliorer la spécification et la rendre plus précise et complète.

5.
Cet article présente une spécification mécanisée de WebAssembly qui fournit une description plus précise et rigoureuse du langage. Elle a amélioré la spécification formelle originale en découvrant des erreurs et des incohérences. L'auteur a dérivé plusieurs autres artefacts, dont un interprète vérifié pour WebAssembly, un validateur de traduction vérifié et un vérificateur de type vérifié.

La spécification a été vérifiée à l'aide de l'assistant de preuve Coq.

Bien que la spécification mécanisée fournisse une description plus précise et plus rigoureuse du langage, elle n'élimine pas le besoin de tests. En effet, prouver l'algorithme n'empêche pas la création de bug dans l'implémentations.

Les tests sont toujours nécessaires pour s'assurer que les implémentations de WebAssembly sont correctes et conformes à la spécification. Cependant, la spécification mécanisée peut contribuer à réduire le nombre de bogues et d'erreurs dans les algorithmes de WebAssembly, ce qui peut conduire à des logiciels plus fiables et plus sûrs.
