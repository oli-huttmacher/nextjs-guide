# Apprendre Next.js 15 en 1 Heure

Bienvenue dans cette vidéo ! Aujourd’hui, nous allons apprendre **Next.js version 15** en une heure. Vous aurez toutes les bases pour maîtriser ce framework.

## Plan de la vidéo

- **Introduction et objectifs** : Pourquoi apprendre Next.js 15 ?
- **Setup du projet avec Tailwind CSS et Shadcn/UI**
- **Routing et gestion des pages**
- **Ajout d'une base de données avec Prisma**
- **Server Components et Server Actions**
- **Création d'une application : "Citation Master"**
- **Conclusion et ressources pour aller plus loin**

---

## Introduction : Pourquoi apprendre Next.js ?

Next.js est un framework basé sur React qui ajoute des fonctionnalités comme :
- Le routing (pages et layouts)
- Le caching et la gestion des erreurs
- Les Server Components et Server Actions
- La gestion des cookies, headers, middleware

En résumé, il fournit des outils essentiels pour les développeurs **frontend** et **full-stack**.

---

## Setup du projet

### Initialisation du projet

1. **Créer une nouvelle application Next.js :**
   ```bash
   npx create-next-app@latest citation-master
   ```
2. **Configurer Tailwind CSS :**
   ```bash
   npm install -D tailwindcss postcss autoprefixer
   npx tailwindcss init
   ```
3. **Installer Shadcn/UI :**
   ```bash
   npx shadcn init
   ```

### Configuration des fichiers
- `tailwind.config.js` : Ajoutez vos répertoires (pages, components, etc.) pour que Tailwind fonctionne correctement.
- `tsconfig.json` : Configuration TypeScript.
- `next.config.js` : Paramètres de Next.js.

---

## Routing et gestion des pages

Next.js utilise un système de **File-Based Routing** :

1. **Créer une page :**
   - Fichier : `app/page.tsx`
   - Exemple :
     ```tsx
     export default function HomePage() {
       return <div>Bienvenue sur Citation Master!</div>;
     }
     ```
2. **Créer un layout global :**
   - Fichier : `app/layout.tsx`
   - Exemple :
     ```tsx
     export default function Layout({ children }) {
       return (
         <html>
           <body>{children}</body>
         </html>
       );
     }
     ```
3. **Pages dynamiques :** Utilisez des crochets dans les noms de dossiers pour gérer les routes dynamiques.
   ```bash
   app/[citationId]/page.tsx
   ```

### Gestion des layouts
Les fichiers `layout.tsx` permettent de définir une structure commune pour les pages enfants.

---

## Ajouter une base de données avec Prisma

### Setup de Prisma

1. Installer Prisma :
   ```bash
   npm install prisma --save-dev
   npx prisma init
   ```
2. Configurer `prisma/schema.prisma` :
   ```prisma
   model Citation {
     id        Int      @id @default(autoincrement())
     text      String
     author    String
     createdAt DateTime @default(now())
   }
   ```
3. Migrer la base de données :
   ```bash
   npx prisma migrate dev --name init
   ```

### Utilisation dans Next.js
Ajoutez un client Prisma dans `lib/prisma.ts` :
```tsx
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();
export default prisma;
```

---

## Server Components et Server Actions

### Qu'est-ce qu'un Server Component ?
Un composant rendu directement sur le serveur pour optimiser les performances et la gestion des données.

### Exemple d’implémentation
1. **Créer une route d’API :**
   - Fichier : `app/api/citation/route.ts`
   - Exemple :
     ```tsx
     import prisma from '@/lib/prisma';
     import { NextResponse } from 'next/server';

     export async function POST(request) {
       const data = await request.json();
       const newCitation = await prisma.citation.create({ data });
       return NextResponse.json(newCitation);
     }
     ```

2. **Gérer les formulaires dans un composant client :**
   ```tsx
   "use client";

   export default function CitationForm() {
     const [isLoading, setIsLoading] = useState(false);

     async function handleSubmit(event) {
       event.preventDefault();
       setIsLoading(true);
       const formData = new FormData(event.target);
       const response = await fetch('/api/citation', {
         method: 'POST',
         body: JSON.stringify({
           text: formData.get('text'),
           author: formData.get('author'),
         }),
       });
       setIsLoading(false);
     }

     return (
       <form onSubmit={handleSubmit}>
         <input name="text" placeholder="Texte" required />
         <input name="author" placeholder="Auteur" required />
         <button type="submit" disabled={isLoading}>
           {isLoading ? 'Envoi en cours...' : 'Soumettre'}
         </button>
       </form>
     );
   }
   ```

---

## Application "Citation Master"

- **Créer une citation**
- **Modifier une citation**
- **Supprimer une citation**
- **Partager une citation via une URL unique**

---

## Conclusion

Nous avons exploré les fonctionnalités principales de Next.js 15 :
- Configuration et setup
- Routing
- Intégration de Prisma
- Gestion des Server Components et Actions

Pour aller plus loin, inscrivez-vous à la formation gratuite sur Next.js mentionnée dans la vidéo. Merci d'avoir suivi ce tutoriel, et à bientôt ! 🎉
hello et bienvenue dans cette vidéo où on va apprendre nextjs version 15 en 1 heure tu vas vraiment savoir tout ce qu'il te faut pour maîtriser nextjs 15 on va y aller petit à petit et je suis sûr que tu vas adorer reste bien accroché car apprendre nextjs devient de plus en plus essentiel si tu es développeur frontend ou full stack c'est vraiment une compétence qu'il faut avoir je te propose sans plus tarder de voir qu'est-ce qu'on va faire dans cette vidéo donc on va commencer par cette l' avec chat cnui le but dans cette vidéo c'est pas de s'occuper du style et donc on va utiliser Tailwind et chat cnui pour avoir des styles rapides c'est vraiment le meilleur moyen actuellement d'ajouter des styles dans ton application donc c'est assez logique qu'on va partir là-dessus ensuite on va voir le routing donc comment on fait des pages des chargements des erreurs comment on gère tous ces détails avec nextgs 15 on va faire ajouter une database avec Prisma et SQL light pourquoi est-ce qu'on a ajoute une database parce que nextgs 15 en fait sans database et ben on utilise que 10 % de tout ce qu'il peut faire et c'est vraiment quand tu ajoutes une database qu'on va pouvoir justement ajouter des serveur components pour venir récupérer des datas de notre database et où on va pouvoir rajouter des serveur action pour venir modifier les données de notre database on verra aussi les AP root qui nous permettre aussi de modifier les données de database le projet qu'on va faire ensemble c'est citation master L'application qu'on va créer citation master c'est une petite application comme ceci où il est possible de venir créer des citations donc on va pas faire exactement la même interface mais par exemple ici je peux dire nextjs c'est un framework pas une librairie voilà on en parlera après de cette affirmation ici je peux écrire mon auteur et submit et tu peux voir qu'ici j'ai fake un chargement et on a nextjs c'est un framework pas une librairie on peut ensuite modifier notre citation on peut aussi la supprimer avec une petite modale qui apparaît ici et on va pouvoir aussi la visualiser comme ceci donc tu vois que là on peut partager notre citation à nos amis comme ça tu partage cette citation à toutes les personnes qui veulent en connaître plus et ils voient cette magnifique petite interface donc tu vois qu'on va s'amuser avec les layout les pages pour pouvoir créer une vraie expérience complète avant de commencer à setup le projet j'ai envie de te demander nextjs c'est quoi c'est souvent quelque chose qui pose problème au débutant et donc la réponse c'est que c'est un framew cré et là il y a plein de gens qui tombent et qui se disent mais pourquoi un framework alors déjà faut partir de la base JavaScript c'est un langage de programmation qui définit plusieurs standards comme par exemple les ternary opérator la méthode point map sur un tableau ou les variable avec Var ça c'est la base ensuite sur ceci on a react qui est une librairie qui vient ajouter des méthodes qui nous permet de gérer l'UI react son travail c'est de venir s'occuper de l'interface utilisateur venir gérer les state venir gérer la modification des données et cetera ça c'est react et maintenant on a nextgs et nextgs c'est un framewor qui vient autour de react ajouter les Appia rout le caching le routi la gestion d'erreur la gestion de loading les headers les cookies les middleware les query les search params et ça vient vraiment ajouter une surcouche un peu de backend j'ai envie de dire hein mais c'est pas totalement ça donc ça va ajouter des des fonctionnalités pour faire du web précisément contrairement à react avec lequel tu peux faire du développement mobile tu peux faire des jeux tu peux faire tout ce que tu veux et donc next j ça vient vraiment rajouter la surcouche qui nous permet de faire du développement backend ça c'est vraiment le rôle de nextjess mais de toute façon ce qui t'intéresse toi c'est de pratiquer et je te propose tout de suite de venir lancer ton terminal donc par exemple sur MacOS j'utilise high term high term c'est un joli terminal comme ceci je vais pouvoir te le zoomer et pour lancer et créer un nouveau projet on va d'abord aller dans un dossier qui qui nous permet de le faire donc ici moi je vais aller dans mon dossier YouTube dans mon dossier Youtube je vais pouvoir venir ici sur mon navigateur aller sur la documentation nextjs donc ici c'est nextgs.org on va Setup d'un projet NextJS cliquer sur Get Started installation dans installation on a ici automatique installation et on va choisir ici de copier cette commande avec les données qui sont là donc on va pouvoir ouvrir notre terminal et coller cette commande NPX create next app at latest on va lancer cette commande ici ça va te proposer des petites installations on va dire oui comme quoi on accepte d'installer on va définir le nom de l'application donc moi je vais l'appeler citation- master c'est un nom à tu peux choisir le nom que tu veux évidemment est-ce que tu veux utiliser typescript on va dire oui si tu ne sait pas faire de typescript sache que c'est pas vraiment un problème parce que ça se fait assez naturellement es lin évidemment va dire oui tin CSS on va dire oui parce que après on va ajouter chat cnui dès le départ est-ce qu'on veut coder à l'intérieur d'un source directe moi je te conseille de dire non et la prouur évidemment ça c'est obligatoire il faut dire oui turbo pack pour le développement local tu peux aussi dire oui c'est vraiment une bonne solution est-ce que tu veux customiser le import ici on va dire non et on va venir importer notre application comme ceci à ce moment-là ce qu'il va faire c'est qu'en fait il va créer un dossier il va installer toutes les dépendances et il va préparer notre projet ici avec une template de départ mais ne t'en fais pas on va recommencer tout de zéro voilà dès que c'est fait normalement ça t'affiche su une application été créée à cette url là don on va faire CD citation master ça permet de rentrer dans le dossier et ici on va pouvoir faire open point comme ceci pour l'afficher dans notre dossier YouTube ensuite on peut ouvrir Visual Studio code donc moi tu vois qu'ici j'ai le projet solution mais c'est pas très grave on va faire commande ha pour ouvrir un nouveau projet on va drag and drop citation master dans le menu qui apparaît quand on fait commande o et on va faire open normalement quand tu fais ça ça va venir t'op le projet comme ceci et on va pouvoir venir le mettre en full screen et moi ce que je vais faire c'est que je vais venir dupliquer ma page qu'on a juste ici pour venir la drag and drop à cet endroit-là pour avoir les deux pages qui sont côte à côte pour débuter ce sera assez pratique alors on va faire un petit tour sur l'application donc ça me donne envie de cacher ça pour l'instant comme ça on peut vraiment être focus désolé pour ce petit contretemps mais ici on va ouvrir ce menu et on peut voir que par défaut on a une liste de plusieurs fichiers de configuration suivi de trois dossiers alors on va Comprendre les fichiers qui sont dans un projet NextJS s'intéresser au fichier de configuration TS config ça permet de mettre la configuration pour typescript celle par défaut c'est généralement celle qu'il faut garder twin config c'est la configuration pour tailwin tu peux voir que par défaut il ajoute ici tous les dossiers qui sont dans page dans components et dans app je te conseille dès maintenant de venir rajouter sources sources qu'on va utiliser dans tous les cas pour faire des trucs si tu ajoutes des dossiers par exemple ici si on cré un dossier source il faut l'ajouter ici sinon les les classes Tellin ne vont pas être ajouté à ton B tawind maintenant on va avoir le fichier RM donc ça tu peux tout supprimer mais c'est juste des petites instructions post CSS c'est aussi pour Tailwind le package JSON ça c'est tout ce qui est concernant notre package JSON le next.config.ts ça va nous permettre de configurer certaines choses tu peux voir que normalement il y a l'auto complete comme ceci et depuis la version 15 c'est un fichier typescript git ignor ici qui ignore tous les trucs inutiles et es lint RC pour setup la config es lint ensuite on a le dossier source alors c'est celui qu'on va créer ensemble mais ici on mettra nos composants et cetera le dossier public qui définit des fichiers qui vont être accessibles de manière publique c'est-à-dire directement dans l'URL de notre application alors pour te faire une démo he si on lance l'application ici par exemple avec NPM run devev ou ou pdv tu verras moi j'écris souvent pdev mais ici tu peux voir que là il y a des fichiers SVG et en fait si on va dans l'URL comme ceci et que je fais file.svg et ben on tombe sur le logo file c'est-à-dire qu'en fait via l'URL ici on est capable d'aller directement sur ces fichiers donc ici localhost 3000/file.svg ça va nous mener tout droit sur ce fichier là donc intéressant à savoir et dans app c'est le plus intéressant on va ici avoir des fichiers qui sont nommés très spécifiquement on le verra ensemble donc on a page qui définit une page on a layout qui définit le layout de toute notre application et on a font ici qui nous permet d'avoir plusieurs fontes ici avec guest mono et gest normal et tu vois qu'ilutilise comme ceci ici donc de toute façon nous ça on va laisser ça par défaut ça nous permet d'avoir des jolies fontes alors une fois qu'on a vu tout ça on va lancer l'application et on va voir ce qui se passe sur le menu principal donc ici quand je vais dans localos 3000 et que je remets ma page juste ici tu peux voir qu'on a la page qui apparaît juste ici on peut dans cette page venir dans le fichier pagesx qui définit ce qu'il est à l'intérieur ici tout supprimer et venir rajouter juste une petite div qui dit bonsoir comme ça on a le contenu et dans layout on peut ici rajouter des petites class name comme h full ici on va rajouter une class name HF et comme ça on garde toute la taille afin de pas trop s'embrouiller avec le style pour ajouter des styles avec tawind on va pas faire un tuto tawind mais tu peux utiliser les classes ici je peux par exemple faire pading 32 j'ai red 500 et cetera et cetera mais nous ce qu'on va utiliser pour nous simplifier la vie c'est une librairie qui s'appelle chatsen ui pour l'installer ça prend vraiment 2 minutes c'est pour ça qu'on va le faire ensemble tout de suite mais on va pouvoir aller dans installation nextgs donc ici tu va aller sur ui.chatcn.com puis dans get started ici Setup de Shadcn/UI on va dans installation là on choisit nextjs et là on lance cette commande là et moi je vais choisir la variante pnpm parce que ça me permet de pas avoir trop de fichiers dans mon application donc ici tu peux faire pnpm DLX chatsen at latest init cette commande que je viens d'écrire ça va venir charger là on va choisir le style New York puis neutral CSS variable on va dire oui et ça va venir créer tout ça ici à cause de react 19 il y a quelques trucs qui ont changer peut-être que toi tu auras pas ça mais toi ici tu peux utiliser le Legacy perdeps ou force je je sais même pas pourquoi il propose mais ça ne change généralement pas grand-chose de toute façon ici notre application elle est toute petite donc on a pas de problème tu peux voir que par défaut il vient te créer un dossier lib ici et un fichier component.json donc component.json il vient te définir plein de fichiers ici plein d'alias donc ici components util ui lib source moi ici ce que je vais faire c'est que je vais modifier tout ça dans le fichier source comme ceci il va tout mettre dans le fichier source et il va pas mettre ça dans dans ma racine ça permet de pas avoir trop de fichiers donc ensuite ici je vais déplacer le fichier le dossier lib ici dans mon dossier source et comme ça on va pouvoir installer notre premier composant et donc pour ça on va lancer pnpmd le at latest add et ici on va par exemple ajouter button input card select non button input et card c'est très bien donc là quand tu écris ça alors moi tu es en train de te dire est-ce que je suis un génie qui sort tout ça de ma tête c'est juste que j'utilise souvent chat CN et donc tu peux venir ici et tu peux voir que j'ai généralement les composants s'appellent avec leur nom donc bouton text area Select et cetera donc ici on va lancer la commande on va encore une fois utiliser force c'est pas très grave et tu vois que normalement maintenant que j'ai modifié mes alias ça va venir créer mon dossier directement ici donc component/hui on a maintenant button carte et input une fois qu'on a fait tout ça on peut relancer pdev et à ce moment-là on va avoir notre application ici normalement elle va passer en white mode où il va falloir supprimer de trois styles ici ah voilà il va falloir supprimer de trois styles donc ici ici tout ce qui est par défaut on dégage voilà et comme ça on est en white mode et ici on peut aller dans tawin.config.js tu peux voir que ça a venu c'est c'est venu modifier plein de choses comme ceci alors ici il doit pas être content tu peux reload vs code si tu vois qu'il y a des petits problèmes comme ça nous ce qui nous intéresse aussi c'est d'utiliser les fonds de Family qu'on a ajouté juste ici donc avec les variables Gest font mono et guest font 100 pour ça on peut rajouter font family on peut rajouter sens comme ceci et là on va faire un tableau avec Var de et ici on va utiliser le même nom qu'ici ça va nous permettre de venir utiliser une custom fond dans notre application et ici tu peux voir que ça fonctionne pas très bien et pour le faire fonctionner il suffit de rajouter une petite classe ici dans body F à côté de ça et ça nous permet d'avoir la fond family qu'on a juste ici une fois qu'on a fait tout ça on peut aller dans page et ici on peut utiliser notre composant comme ceci et comme ça on a fini le setup de base de notre application et tu peux voir que maintenant on a des jolis composants par défaut avec par exemple un bouton ou on a aussi un champ texte comme ceci avec ici bonsoir et ici il y a pas besoin de Child run en fait comme ceci on a le magnifique bonsoir alors une fois qu'on a fait ça comme ça maintenant on a tous les on a tout qui est tout qui est bien setup on va un peu comprendre comment fonctionne la navigation le rout se fait avec les dossiers c'est ce qu'on Le routing de base (page.tsx) appelle un file based routing ou folder based routing c'està-dire qu'on va avoir ici notre application tout ce qui est dans app c'est du routing on va avoir des dossiers comme dashboard et settings qui vont définir notre route donc tu peux voir que ici app ça représente la racine ensuite quand on crée un dossier par exemple dashboard ça vient représenter un nom de Paf puis settings et ensuite pour afficher une page à cette url là on va utiliser le dossier Le routing avancées (layout.tsx, error.tsx, loading.tsx) le fichier qui s'appelle page.tsx c'est très important d'utiliser les noms qu'on va dire ici page en minuscule.tsx ça va définir une page on peut le voir ensemble par exemple là je vais créer moi ici admin/page.tsx comme ceci et à ce moment-là on a créé une nouvelle route qui est sur slash admin donc je vais ouvrir Google Chrome pour développer parce que arc c'est pas le plus pratique je sais que chaque fois il il m'embrouille un peu donc ici on a notre URL et à cet endroit-là ce qu'on doit faire impérativement dans n'importe quelle page c'est de venir ajouter un export default donc ici je vais faire export default function ici on va nommer ça page par convention et à l'intérieur ici on vient de définir une nouvelle page il faut impérativement tourner quelque chose donc par exemple ici nous on va retourner une carde avec une carde header donc importer de chat cnui afin que ce soit joli et ici on va l'appeler/ashadmin ou url de./admin donc comme ça on vient d'ajouter notre carde et maintenant tu peux voir que si dans l'URL je rajoute/ash admin ça vient totalement pas fonctionner c'est super les amis comme ça on a une bonne démo donc ici en relançant le serveur j'ai l'impression que ça marche mieux donc on va sur slash admin et tu vois qu'ici on arrive dans url/admin donc ici on peut vraiment créer des pages pour n'importe quel URL on va juste devoir utiliser des dossiers alors je sais comme ça que ça peut paraître bizarre mais si je te montre par exemple le code de nowots et ben tu peux voir qu'on va se retrouver avec plein de dossiers comme ça qui permettent de définir toutes les pages donc tu vois qu'ici on a plein de pages qui sont définies avec les dossiers donc c'est ça qu'on va reproduire ici mais c'est pour te montrer que tu peux créer n'importe quelle page avec les dossiers on va pas faire 12000 exemples mais euh c'est déjà un bon début parce que la deuxième chose intéressante c'est de venir définir un layout le fichier layout ici il va s'appliquer à toutes les pages qui sont euh on va dire en dessous de lui c'est-à-dire que ici tu vois que le layout tu peux t'imaginer que c'est une sorte de composant qui vient wrapper l'intégralité de notre application et donc tout ce qu'on va faire dans le layout sera à appliquer à toutes les pages enfant c'est-à-dire que vu qu'ici le layout on l'a mis dans app app on a vu que c'était la racine et ben évidemment en mettant un layout dans la racine ce layout va s'appliquer dans toutes les pages de notre application par contre ici ce fichier layout qui est dans dashboard et bah de manière inverse il va s'appliquer uniquement dans les pages qui vont être dans dashboard donc ici tu peux imaginer qu'il y a slash dashboard et que ici le layout va s'appliquer que sur ces pages là et donc on peut le voir juste ici on va aller dans ce fichier layout et on va ajouter un peu de style ok pour que ce soit plus plus beau gosse donc ici je vais rajouter un max Wiz LG un padding X de 4 et un padding y ici de 6 comme ça on est un peu en haut donc tu vois qu'ici on a toujours la même chose et que le style que j'ai appliqué ici dans mon layout et ben il s'applique bien sur toutes les pages ici je vais aussi pouvoir afficher par exemple un composant header donc ici je pourrais aller dans source ici je pourrais créer un fichier dans mes composants qui s'appelle header.tsx et dans ma header je vais pouvoir faire export function et donc moi pour différencier les pages des composants et ben je fais pas d'export default export function là on va faire une card avec une card header par ex exemple ici à l'intérieur de notre card on va rajouter pardon un padding de 4 et ici on va mettre un petit citation master comme ceci et on va ensuite pouvoir appliquer cette header à cet endroit là et tu vois que maintenant on a la header et ici je peux rajouter Flex flexc gap 4 et enlever ce padding y ok on va plutôt mettre ça dans une div comme ça ce sera plus joli on va faire Flex flexc gap 4 et on va enlever les settings qui sont juste ici et comme ça tu peux voir qu'on a maintenant le layout donc ici je pourrais aussi l'appeler enentre parenthèse layout de toute la page et que si je retourne sur l'URL de base on peut voir que ce layout est aussi appli



