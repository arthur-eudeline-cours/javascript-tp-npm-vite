# JavaScript : Découverte de NPM & ViteJS



### Objectifs 

- Comprendre ce qu'est NPM
- Installer et utiliser des libraires via NPM
- Comprendre l'intérêt de systèmes de build tels que ViteJS
- Installer et tirer parti de ViteJS
- Morceler son code en plusieurs fichiers



### Qu'est-ce que NPM ?

*NPM*, pour *Node Package Manager* est le gestionnaire de librairies de l'écosystème JavaScript. Il en existe d'autres, tels que *Yarn* ou *PNPM*, mais tous s'installent avec *NPM* (oui, il faut installer un gestionnaire de librairies via un autre gestionnaire de librairies). *NPM* est compris dans l'installation de *Node.js*. Il remplit un rôle similaire à celui de *Composer* pour PHP, *Maven* pour Java ou *CocoaPods* pour l'écosystème Apple.

Si vous avez l'œil, vous avez remarqué que la première lettre de *NPM* correspond à *Node* et plus précisément à *Node.js* qui permet d'exécuter du JavaScript côté serveur. 



1. **Créer un nouveau projet dans lequel vous créerez un nouveau fichier** `server.js` **avec le contenu suivant.**

   ```js
   console.log('Exécuté depuis le serveur !');
   
   // Termine le programme avec un code de sortie de 0 indiquant que le programme s'est terminé correctement
   process.exit(0);
   ```

   Ouvrez à présent un terminal dans votre dossier et exécutez la commande suivante :

   ```shell
   node server.js
   ```

   **Qu'est-ce qui s'affiche à l'écran ?**



### Création d'un projet NPM

2. **Ouvrez un terminal dans votre dossier de projet et saisissez la commande suivante** : 

   ```shell
   npm init
   ```

   Cette commande est interactive, vous allez pouvoir lui passer différentes valeurs. Pour aller plus vite, vous pouvez utiliser `npm init -y` pour que NPM remplisse pour vous toutes les valeurs par défaut.

   Observez à présent le contenu de votre dossier. Un nouveau fichier a été créé : `package.json`. Voici un exemple réduit au strict minimum (ne copiez pas le code, je vous rappelle que les commentaires n'existent pas en JSON) : 

   ```javascript
   // package.json
   {
     // Le nom du projet. Par défaut, ça sera le nom du dossier
     "name": "tp-npm",
     
     // Indique le type du projet
     "type": "module",
     
     // Le fichier principal de l'application, son point d'entrée
     "main": "src/main.js",
     
     // Ensembles de commandes qui peuvent être exécutées via "npm run [command]"
     "scripts": {},
     
     // Liste des dépendances du projet
     "dependencies": {},
     
     // Liste des dépendances liées au développement. Elles ne seront pas installées en production
     "devDependencies": {}
   }
   ```

   >**Info :**
   >
   >Le `name` du projet correspond au nom qui sera utilisé pour installer votre projet si jamais vous le publier sur NPM ou tout autre dépôt. Par convention, si votre projet est lié à une organisation (entreprise ou famille de projets), vous pouvez le préfixer par `@[nom-orga]/[nom-projet]`, par exemple, si je voulais indiquer que ce projet est rattaché à l'IUT, je le renommerai ainsi : `@iut-bourgogne/tp-npm`.



3. **Créez les fichiers suivants :**
   - `index.html` et **ajoutez-y la structure HTML de base**. 
   - `src/main.js`



### Installer une librairie via NPM

> #### Installer toutes les dépendances d'un projet
>
> Vous l'avez déjà utilisée, pour installer une librairie, nous utilisons la commande `npm install` ou `npm i` en version raccourcie. Pour utiliser cette commande sans arguments, vous avez juste besoin d'avoir un fichier `package.json` dans votre dossier courant. Cela installera toutes les dépendances présentes dans ce dernier. 
>
> Si vous avez un fichier `package-lock.json`, alors NPM se basera sur ce dernier pour charger les versions exactes des dépendances utilisées par le projet.
>
> 
>
> #### Installer une dépendance spécifique 
>
> En passant des arguments à la commande `npm install`, vous pouvez installer une ou plusieurs librairies :
>
> ```shell
> # Installe la dernière version de lodash
> npm install lodash
> 
> # Équivalent à (donc la dernière version stable publiée)
> npm install lodash@latest
> 
> # Installer plusieurs librairies : npm install [lib1] [lib2]
> npm install lodash @popperjs/core
> 
> # Installer une dépendance avec une version spécifique [lib]@[version]
> npm install lodash@3
> ```
>
> 
>
> #### Comprendre la syntaxe de versions de NPM
>
> Cette syntaxe est à la fois utilisée dans le `package.json` et dans la ligne de commande. Elle se base sur SEMVER. Un super site pour comprendre la syntaxe et la tester : https://semver.npmjs.com/ ! En bas de page, vous trouverez un aide mémoire sur la syntaxe.
>
> Le plus gros à retenir, pour SEMVER c'est qu'il y a 3 portions importantes. Si on prend une version `1.2.3`, voilà ce que ça représente :
>
> - Le `1` représente la **version majeure**, quand ce nombre change d'une version à une autre, **c'est qu'il y a eu des changements non-rétrocompatibles dans la librairie**, c'est-à-dire que certaines fonctions ont pu changer ou n'existent plus, ce qui peut faire sérieusement planter votre application. Il est donc nécessaire de tester encore plus son code après ce type de mise à jour. Quand il est inférieur à 1, on considère que la librairie est encore en cours de développement et que c'est le *no-man's land* des modifications non rétrocompatibles. Entendez-par là que vous devez être très attentif aux mises à jour de librairies en dessous de 1.0.0.
> - Le `2` est la **version mineure**, une modification de ce nombre indique l'ajout de fonctionnalités rétrocompatibles. Vous pouvez, en théorie, mettre à jour sans problème ces librairies si le développeur respecte bien la notation (ce n'est pas toujours le cas, ça serait trop facile !).
> - Le `3` est le **correctif (ou patch)**, il indique une correction de bug rétrocompatible et il est donc souvent rentable de faire ces mises à jour. 
>
> 
>
> Vous pouvez utiliser cette syntaxe combinée à des sélecteurs pour indiquer à NPM de ne pas dépasser certaines versions :
>
> - `^` indique que vous acceptez **n'importe quelle version supérieure du moment que ça reste sur la même majeure**. Ce sélecteur est utilisé par défaut.
>
>   **Exemple :** `npm i lodash@^3.3.0` acceptera de `3.3.0` à `3.10.1`
>
> - `~` indique que vous acceptez **n'importe quelle version supérieure du moment que ça reste sur la même mineure**.
>
>   **Exemple :** `npm i lodash@~3.9` acceptera de `3.9.0` à `3.9.3`
>
> - `>`, `<`, `=`, `>=`, `<=` pour indiquer que vous acceptez *jusqu'à* ou *à partir de* certaines versions
>
>   **Exemple :** `npm i lodash@">3.8"` acceptera toutes les versions au-dessus de `3.8` jusqu'à la dernière version publiée à ce jour
>
> - `-` pour indiquer un intervalle de versions acceptées 
>
>   **Exemple :** `npm i lodash@"3.3 - 3.6"` acceptera toute version entre `3.3.0` et `3.6.0` inclus
>
> - Enfin la plus facile, vous pouvez utiliser un *wildcard* `*` 
>
>   **Exemples :** 
>
>   - `npm i lodash@*` accepte n'importe quelle version stable, très utile dans un projet ayant beaucoup de dépendances
>   - `npm i lodash@3.*` accepte n'importe quelle **version mineure stable** de la version `3`
>   - `npm i lodash@3.3.*` accepte n'importe quel patch de la version `3.3`
>
> (Vous pouvez également utiliser ces syntaxes dans les versions des dépendances de votre `package.json`)
>
> 
>
> ### Mettre à jour les dépendances
>
> Vous pouvez mettre à jour toutes les dépendances de votre projet avec la commande `npm update`. NPM cherchera alors s'il existe des versions supérieures à celles installées, mais toujours en respectant les contraintes que vous avez spécifiés. 
>
> ```shell
> # Met à jour toutes les dépendances du projet
> npm update
> 
> # Met à jour uniquement la dépendance spécifiée
> npm update lodash
> ```
>
> Vous pouvez également modifier la contrainte de version d'une dépendance directement dans le `package.json` et lancer par la suite un `npm install`, NPM réinstallera toute dépendance dont le sélecteur de version n'est plus conforme à ce qui est installé dans le projet.
>
> 
>
> #### Connaître la version des dépendances installées
>
> ```shell
> # Affiche toutes les dépendances du package.json et leurs dépendances
> npm list 
> 
> # Affiche la version de la dépendance spécifiée
> npm list lodash
> ```
>
> 
>
> #### Supprimer une dépendance
>
> Vous pouvez utiliser la commande `npm remove [lib]` ou retirer la ligne de la librairie que vous voulez supprimer dans le `package.json` et faire un `npm install`.
>
> ```shell
> npm remove lodash
> ```



4. **Installons notre première librairie** : *Lodash*. Cette librarie est assez populaire et contient un ensemble de fonctions utilitaires telles que `camelCase()` qui convertit une chaîne au format Camel Case ("*salut les loulous*" deviendrait "*salutLesLoulous*"). Pour installer votre librairie, **lancez la commande suivante** :

   ```shell
   npm install lodash
   
   # Ou
   npm i lodash
   ```

   

5. **Consultez à nouveau votre fichier `package.json` vous devriez y voir une nouvelle ligne :**

   ```javascript
   {
     // ...
     "dependencies": {
       "lodash": "^4.17.21" // peu importe si la version n'est pas exactement la même
     }
     // ...
   }
   ```

   Ce qu'indique cette ligne, c'est que votre projet nécessite au moins la version `4.17.21` pour fonctionner. À chaque vous que vous ajouterez une nouvelle dépendance, NPM s'assurera que cette dépendance est compatible avec celles déjà installées. Par exemple, si une librairie se base elle aussi sur Lodash, mais n'est compatible qu'avec Lodash 3.0, alors NPM refusera l'installation et il vous faudra utiliser une version compatible de Lodash ou opter pour une autre librairie. 



6. **Regardez aussi votre dossier, vous devriez y constater deux modifications :**

   - un fichier `package-lock.json`, **ce fichier est aussi très important** ! Il contient la résolution des versions des librairies qui correspondent aux contraintes écrites dans le `package.json` (dans notre cas, la contrainte est `^4.17.21`, on verra ce qu'elle veut dire plus tard). Autrement dit, si vous transmettez le projet à un collègue sans le `package-lock.json` avec, lorsque la personne exécutera `npm install`, une nouvelle résolution des versions s'effectuera et il se pourrait qu'il ait des versions de librairies légèrement différentes des votres, ce qui pourrait induire de nouveaux bugs ou comportements inattendus dans votre projet.

     

   - un dossier `node_modules` est apparu à la racine du projet. Si vous regardez ce dossier, vous y verrez des sous-dossiers qui représentent toutes les librairies que vous installez, mais aussi leurs dépendances ainsi que les dépendances de leurs dépendances, d'où ce meme :

     ![](https://img.devrant.com/devrant/rant/r_1558252_FgBYz.jpg)



7. **Adoptons les bonnes pratiques**, pour prévoir l'utilisation de git avec notre projet, **créez un fichier** `.gitignore` **à la racine du projet** avec le contenu suivant :

   ```
   **/node_modules/
   ```

   Cela empêchera Git de suivre les modifications apportées à tous les dossiers `node_modules` du projet (qu'ils soient à la racine ou imbriqués dans des sous-dossiers). **Il ne faut jamais versionner le dossier** `node_modules`, en plus d'être lourd, certaines librairies peuvent varier selon l'OS sur lesquelles vous les installez. 

   De même, si vous devez copier, compresser ou déplacer un projet avec un `node_modules`, sachez qu'il sera toujours plus rapide d'ignorer/supprimer ce dossier et de refaire un `npm i` dans le nouveau dossier. 



### Utilisation d'une librairie installée avec NPM

8. **Ouvrez votre fichier** `index.html` et **chargez lodash avant votre** `main.js` :

   ```html
   <!doctype html>
   <html lang="fr">
   	<head>
     	<!-- ... -->
       <script type="text/javascript" src="node_modules/lodash/lodash.js" defer></script>
       <script type="text/javascript" src="src/main.js" defer></script>
     </head>
     <body>
       <!-- ... -->
     </body>
   </html>
   ```



9. **Lancez un serveur avec la commande** `npx serve` (qui est un outil de NPM dédié à l'exécution des paquets *Node Package eXecute*) et ouvrez dans un navigateur `http://localhost:3000`. 
   **Ouvrez une console et commencez à écrire** `_.`, vous devriez voir une myriade de méthodes comme `add()` ou `after()`. C'est parce que Lodash est chargé sur la page et qu'il est stocké sur l'objet global `window` et toutes ses fonctions sont groupées sur l'objet `_`. 

   

10. **Utilisez lodash dans votre fichier** `main.js` en utilisant par exemple la fonction `camelCase()` évoquée plus haut pour en afficher le résultat en console. Vous trouverez ici la liste de toutes les fonctions disponibles dans Lodash https://lodash.com/docs/4.17.15.



11. Arrivé à ce point, vous devriez être assez déçus par la façon d'utiliser les librairies NPM et en entrevoir quelques défauts. **Relevez les principaux défauts de cette approche**.



### Utilisation de ViteJS 

Créé par Evan You, le créateur de VueJS, ViteJS est un outil de build JavaScript. Ce type d'outil répond à une ancienne problématique qu'avaient les développeurs JavaScript de morceler leur code JavaScript en fichiers plus petits, ce que l'on fait avec des `include` ou `require` en PHP par exemple. 

Cependant, à l'époque, pour chaque fichier, il fallait ajouter une nouvelle balise `<script>` tout en faisant attention à leur ordre pour être sûr que l'ordre de dépendance soit bien respecté, car il n'existait pas de telle fonction en JavaScript et il n'était donc pas possible de les utiliser dans le navigateur.  

C'est en réponse à cette problématique que les outils de build JavaScript ont étés créés. Ils ont apporté avec eux des fonctions comme `require` et `import` qui permettaient à un fichier JavaScript d'en charger un autre. À l'époque, ces fonctions n'étaient toujours pas supportées par les navigateurs et c'est cette famille d'outils qui venaient fusionner les sous-fichiers JavaScript en un seul gros fichier que l'on appelle le *bundle*, et qui donna leur nom aux outils JavaScript de build qu'on appelle plus communément les *JavaScript bundlers*.

Outre la fusion de ces fichiers en bundles, les bundlers permettent d'autres types d'optimisation comme la transpilation (par exemple conversion du *TypeScript* en *JavaScript*), conversion du code JavaScript pour qu'il soit compatible avec tous les navigateurs via *Babel* ou encore minification des fichiers pour optimiser leur chargement. 

ViteJS, même s'il est en bonne voie, n'est pas le bundler le plus populaire aujourd'hui. Cette place revient à Webpack qui domine le peloton avec ViteJS et Rollup. Cependant, Webpack est extrêmement dur à prendre en main et nous le verrons plus tard dans le cursus, d'autant plus que dans la plupart des cas, ce seront vos frameworks qui le paramètreront pour vous. 



12. **Ajoutez ViteJS à votre projet existant** :

    - Installez ViteJS en tant que **dépendance de développement** 

      ```shell
      npm install vite --save-dev
      
      # ou 
      npm i vite -D
      ```

      Cela ajoutera vite dans l'entrée `devDependencies` du `package.json`, car nous n'avons pas besoin que l'utilisateur final installe ViteJS pour utiliser notre projet !

    - **Ajoutez deux entrées à l'objet** `scripts` présent dans votre `package.json` (si vous en avez déjà, vous pouvez les remplacer si vous voulez) :

      ```javascript
      {
        // ...
        "scripts" : {
          "dev" : "vite",
          "build" : "vite build"
        },
        // ...
      }
      ```

    - Dans votre fichier `index.html` modifiez vos balises `<script>` :

      - **Supprimez la balise script chargeant lodash**
      - **Modifier la valeur de l'attribut** `type` **du script chargeant le fichier** `src/main.js` **pour** `module` 

    

13. **Lancez le serveur inclus dans ViteJS** (assurez-vous d'avoir coupé le serveur de `npx serve` auparavant avec un `CTRL + C` dans le terminal qui exécute le serveur)

    ```shell
    npm run dev
    ```

    En regardant dans votre `package.json` vous devriez comprendre pourquoi cette commande est disponible.

    Ouvrez l'URL qui s'affiche dans le terminal, ça pourrait être `http://localhost:5173`.



14. **Ouvrez la console**, une erreur devrait apparaître indiquant que `_` n'est pas définit. 



15. **Chargez Lodash** dans votre fichier `src/main.js` pour que votre script fonctionne comme avant. Pour cela, utiliser la syntaxe `import` : 

    ```js
    import _ from "lodash";
    
    // Votre script
    console.log( _.camelCase("Salut les loulous") );
    ```



**Important** avec les bundlers, vous pouvez charger toute librairie contenue dans le `node_modules` via `from`. À partir du moment où vous ne mettez pas de chemin relative (par exemple `./lodash`), le bundler considérera que vous parlez d'une librairie. Ici, en écrivant `from "lodash"`, on va chercher le fichier indiqué dans l'entrée `main` du `package.json` de la librairie Lodash. Dans certaines librairies vous pourrez importer depuis des fichiers directs de la librairie également (par exemple `from "lodash/camelCase"` qui chargera le fichier `node_modules/lodash/camelCase.js`).

**Vous pouvez (et devriez) aussi au possible n'importe que les fonctions que vous comptez réellement utiliser**. Cela permet au bundler de n'extraire de la librairie que le code que vous utilisez réellement. C'est ce qu'on appelle du *tree shaking*. Pour ça, on utilise cette syntaxe :

```js
import { camelCase } from "loadash";

// Plus besoin de "_"
console.log( camelCase("Salut les loulous") );
```



### Morceler son code en plusieurs fichiers

16. **Créez un nouveau fichier** `src/functions.js` et ajoutez-y le code d'exemple suivant :

    ```javascript
    function myAwesomeFunction1() {}
    
    function myAwesomeFunction2() {}
    
    function myAwesomeFunction3() {}
    ```

    

17. **Importez votre nouveau fichier dans votre** `src/main.js` en y ajoutant la ligne :

    ```js
    // Spécifier l'extension est facultatif quand c'est du JS ou TS
    import { myAwesomeFunction1 } from "./functions";
    
    // Si on veut utiliser plusieurs functions du fichier 
    import {
      myAwesomeFunction1,
      myAwesomeFunction2 as myFunction // Je la renomme en plus court 
    } from "./functions";
    ```



18. **Faites maintenant un** `console.log( myAwesomeFunction1 );`, vous devriez avoir une erreur en console, qui vous indique que le module ne fournit pas d'export nommé "myAwesomeFunction1". C'est normal ! Par défaut, rien n'est exporté depuis le fichier `src/functions.js`. 



19. **Dans votre fichier** `src/functions.js` **ajoutez le mot clé** `export` **devant les fonctions que vous voulez utiliser dans d'autres fichiers.** Normalement, vous ne devriez plus avoir d'erreur. 



>Ici, nous avons vu les bases de la syntaxe import, il y en a d'autres, mais nous les verrons plus tard. Vous avez les grosses bases pour morceler votre code et utiliser des librairies depuis 



### Build son projet pour l'envoyer en production

L'utilisation de bundlers amène avec elle la notion de *version de développement* et *version de production*, ça y est vous rentrez dans la cours des grands ! Plus haut, lorsque nous avons lancé le serveur de Vite, nous avons exécuté la commande `npm run dev`, ce qui indique implicitement que nous exécutons le projet en version de développement (je vous le donne en mile). 

Une version de développement est loin d'être optimisée. Souvent, le code n'est pas minifié et tout un tas d'optimisations ne sont pas exécutées. Mais surtout, dans certains cas (spoiler alert c'est le nôtre), une version de développement n'est pas fonctionnelle en production.

Il faut donc, avant de déployer notre projet, le *build* vers une version de production et c'est le code qui résulte de cette opération qui termine sur le serveur final. Avec cette approche, pas besoin d'envoyer le `node_modules` sur le serveur et votre site est compatible nativement avec le navigateur.



20.  **Arrêtez le serveur de développement ViteJS** (en faisant `CTRL + C` dans le terminal qui exécute ViteJS), lancez un serveur "classique" avec `npx serve` et **ouvrez** `http://localhost:3030`  **dans votre navigateur.** Ouvrez la console : votre code ne fonctionne plus. Vous pouvez couper le serveur.



21. **Exécutez la commande**  `npm run build` et **regardez votre dossier**, vous devriez y voir un nouveau dossier `dist`. C'est ce dossier que vous enverrez en production sur votre serveur. **Exécutez la commande** `npx serve` depuis le dossier `dist` et ouvrez votre navigateur, votre projet devrait fonctionner. 



Ajoutez également le dossier `dist` au `.gitignore`. **Ne versionnez jamais les dossiers/fichiers qui sont le résultat d'un build**.

```
**/dist
```



### Utiliser CSS avec ViteJS (Bonus, mais promis ça va vite et c'est cool)

Une des fonctionalités phares de ViteJS, c'est ce qu'on appelle le *Hot Module Replacement (H.M.R)* ou *Hot Reload*. Vous avez dû le remarquer, lorsque vous exécutez `npm run dev`, vous n'avez plus besoin de rafraîchir votre page : dès que vous faites une modification dans un fichier du projet, votre page reflète les modifications sans même avoir besoin de s'autorafraîchir (c'est ça le *Hot Reload*). 

Ce qui est très sympathique, c'est que vous pouvez aussi le faire avec du CSS !



22. **Créez un fichier** `src/style.css` avec le code suivant, **pas besoin de le charger dans le HTML avec une balise** `<link>` : 

    ```css
    /* Ceci est le CSS de mon projet */
    
    body {
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
    }
    
    button {
        background-color: #0f172a;
        padding: 16px 32px;
        border-radius: 6px;
        border: none;
        color: #fff;
        text-transform: uppercase;
        letter-spacing: 0.1em;
        font-weight: bold;
        transition: transform 300ms, background-color 300ms, filter 300ms;
        transform: scale(1);
        filter: drop-shadow(0 10px 5px rgba(0,0,0,0.2));
        cursor: pointer;
    }
    
    button:hover {
        background-color: #334155;
        transform: scale(1.05);
        filter: drop-shadow(0 20px 10px rgba(0,0,0,0.2));
    }
    
    button:active {
        transform: scale(0.95);
        filter: drop-shadow(0 5px 3px rgba(0,0,0,0.2));
    }
    ```

    **Ajoutez un** `<button>` **dans votre body HTML** avec pour texte "*mon super bouton*", "*super*" étant englobé dans un `<span>`.



23. À présent, **importez votre CSS dans votre** `src/main.js`. :warning: **ATTENTION :** vous ne pouvez faire cela que grâce à votre bundler ! C'est tout à fait commun en JavaScript, mais jamais sans l'utilisation d'un bundler !

    ```js
    // src/main.js
    import './style.css';
    ```



24. **Dans votre navigateur, inspectez le code HTML et cherchez comment le code CSS est inclus sur la page**. Comment le CSS est-il inclus dans la page ?

    > Pro tips de gamer : dans l'inspecteur, faites un `CTRL + F` et cherchez *CSS de mon projet*



25. **Re-exécutez la commande** `npm run build` puis `npx serve` dans le dossier `dist` et remarquez comment le CSS est maintenant inclu dans la page. D'où l'importance de build votre projet ! Vous remarquerez que votre code est automatiquement minifié également. C'est cadeau, ça me fait plaisir. 



### Utiliser SCSS avec Vite (Bonus)

SCSS est un pré-processeur de CSS, au même titre que SASS, LESS ou Stylus. C'est une surcouche syntaxique du CSS qui va permettre le support de nouvelles opérations comme les variables (bon maintenant ça existe nativement dans le navigateur), les fonctions et surtout, le CSS imbriqué. Cependant, cette syntaxe n'est pas comprise nativement par les navigateurs, il faut donc transpiler (transformer un langage vers un autre) en CSS natif compréhensible par le navigateur. 



26. **Renommez** `src/style.css` **en** `src/style.scss` et modifier le code comme ceci : 

    ```scss
    // Pas besoin de modifier le body
    
    // Variables
    $backgroundColor: #0f172a;
    $padding: 16px;
    $duration: 300ms;
    
    button {
      background-color: $backgroundColor;
      // Calcul natif sans calc()
      padding: $padding $padding * 2;
      border-radius: 6px;
      border: none;
      color: #fff;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      font-weight: bold;
      transition: transform $duration, background-color $duration, filter $duration;
      transform: scale(1);
      filter: drop-shadow(0 10px 5px rgba(0, 0, 0, 0.2));
      cursor: pointer;
    
      // "&" sera remplacé par le selecteur parent, ici ça donnera "button:hover"
      &:hover {
        // Fonction d'éclaircissement des couleurs
        background-color: lighten($backgroundColor, 20);
        transform: scale(1.05);
        filter: drop-shadow(0 20px 10px rgba(0, 0, 0, 0.2));
      }
    
      &:active {
        transform: scale(0.95);
        filter: drop-shadow(0 5px 3px rgba(0, 0, 0, 0.2));
      }
    
      // Sera converti en "button span"
      & span {
        color: #a5b4fc;
      }
    }
    ```



27. Vous devriez avoir une erreur. 

    ```
    [vite] Internal server error: Preprocessor dependency "sass" not found. Did you install it?
    ```

    **Trouvez comment résoudre cette erreur** (https://stackoverflow.com/questions/65589265/vite-how-to-use-sass)

    Vous devrez peut-être relancer vite.



C'est bon c'est fini ! Bravo 🎉

![](https://media.tenor.com/cZK8lK6_xbIAAAAC/bravo-clap.gif)
