# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. 
Le bug logiciel dans le système radar des missiles Patriot lors de la guerre du Golfe en 1991 était lié à une erreur de gestion des horodatages par le système, qui a empêché le missile de détecter la cible et d'intercepter l'attaque de missile Scud iraquien. Ce bug a eu des conséquences tragiques, entraînant la mort de 28 soldats du 14e détachement de quartier-maître de l'armée américaine.

Le bug était local, c'est-à-dire qu'il n'affectait que le système radar Patriot et non d'autres systèmes. Cependant, il a eu des conséquences globales car il a entraîné la mort de soldats américains et a également ébranlé la confiance des gens dans le système de défense antimissile Patriot.

Il s'est avéré que la cause était un calcul inexact du temps écoulé depuis le démarrage en raison d'erreurs arithmétiques de l'ordinateur. Plus précisément, le temps en dixièmes de seconde mesuré par l'horloge interne du système était multiplié par 1/10 pour obtenir le temps en secondes. Ce calcul a été effectué en utilisant un registre à virgule fixe de 24 bits. En particulier, la valeur 1/10, qui a une expansion binaire non terminale, a été hachée à 24 bits après le point de radix. La petite erreur de hachage, multipliée par le grand nombre donnant le temps en dixièmes de seconde, conduisait à une erreur importante. En effet, la batterie du Patriot avait fonctionné pendant environ 100 heures, et un calcul facile montre que l'erreur de temps résultant de l'erreur de hachage amplifiée était d'environ 0,34 seconde. (Le nombre 1/10 est égal à 1/24+1/25+1/28+1/29+1/212+1/213+..... En d'autres termes, l'expansion binaire de 1/10 est 0,00011001100110011001100110011001100..... Or, le registre de 24 bits du Patriote stocke plutôt 0,00011001100110011001100 introduisant une erreur de 0,0000000000000000000000011001100... binaire, ou environ 0,000000095 décimal. En multipliant par le nombre de dixièmes de seconde dans 100 heures, on obtient 0,000000095×100×60×60×10=0,34). Le missile se déplace à environ 1 676 mètres par seconde, et parcourt donc plus d'un demi-kilomètre dans ce laps de temps.

En testant il aurait pu empêcher cette erreur, en effet  c’est une erreur assez commune et facilement détectable si tester.


2.
Nous avons choisis de prendre l’issue COLLECTIONS-701 : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-701?filter=doneissues

Ce bug est locale puisqu’il survient sur un cas d’utilisation particulier d’une méthode qui n’avait sûrement pas été prévu ni testé par le développeur.

Le bug survient lorsque une liste de type SetUniqueList tente de s’ajouter à elle même. (ex : myList.add( myList )). Dans ce scénario, la méthode entre dans une boucle infinie qui cause l’exception StackOverFlowError.

Pour éviter ce bug, la méthode SetUniqueList.add() a été mise à jour dans les versions suivant la découverte du bug. Maintenant la méthode lève une exception lorsqu’elle reçoit elle-même en tant que paramètre. L’exception est levée avec le message : « Cannot add element to itself » pour que les développeurs puissent identifier le problème.

Le bug a été corrigé et des tests ont été ajouté pour éviter que le problème revienne. Voici le lien vers le commit en question : https://github.com/apache/commons-collections/pull/57/commits/be0cea3c907bb4ab1083384377521942d17e15bd


3.  
L'article décrit les expériences de Netflix en matière d'ingénierie du chaos, qui consistaient à introduire des défaillances contrôlées dans leurs systèmes afin de tester leur résilience. Ils ont identifié des indicateurs clés de performance pour chaque système et observé des variables telles que les temps de réponse et les taux d'erreur. Les résultats ont montré une amélioration de la résilience des systèmes. 

D'autres organisations pourraient réaliser des expériences similaires en identifiant les composants critiques, en définissant des indicateurs clés de performance et en observant les variables du système pour améliorer la résilience et réduire les temps d'arrêt.

5.
Cet article présente une spécification mécanisée de WebAssembly qui fournit une description plus précise et rigoureuse du langage. Elle a amélioré la spécification formelle originale en découvrant des erreurs et des incohérences et a conduit au développement d'autres artefacts vérifiés. La spécification a été vérifiée à l'aide de l'assistant de preuve Coq, mais des tests sont encore nécessaires pour garantir une mise en œuvre correcte. La spécification mécanisée peut contribuer à réduire les erreurs et à améliorer la fiabilité et la sécurité des logiciels.
