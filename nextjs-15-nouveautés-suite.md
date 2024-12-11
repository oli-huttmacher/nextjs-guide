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
une fois qu'on a fait ça comme ça maintenant on a tous les on a tout qui est tout qui est bien setup on va un peu
comprendre comment fonctionne la navigation le rout se fait avec les dossiers c'est ce qu'on
appelle un file based routing ou folder based routing c'està-dire qu'on va avoir
ici notre application tout ce qui est dans app c'est du routing on va avoir
des dossiers comme dashboard et settings qui vont définir notre route donc tu peux voir que ici app ça représente la
racine ensuite quand on crée un dossier par exemple dashboard ça vient représenter un nom de Paf puis settings
et ensuite pour afficher une page à cette url là on va utiliser le dossier
Le routing de base (page.tsx)
le fichier qui s'appelle page.tsx c'est très important d'utiliser les noms qu'on va dire ici page en
minuscule.tsx ça va définir une page on peut le voir ensemble par exemple là je vais créer moi ici
admin/page.tsx comme ceci et à ce moment-là on a créé une nouvelle route
qui est sur slash admin donc je vais ouvrir Google Chrome pour développer parce que arc c'est pas le plus pratique
je sais que chaque fois il il m'embrouille un peu donc ici on a notre URL et à cet endroit-là ce qu'on doit
faire impérativement dans n'importe quelle page c'est de venir ajouter un export default donc ici je vais faire
export default function ici on va nommer ça page par convention et à l'intérieur
ici on vient de définir une nouvelle page il faut impérativement tourner quelque chose donc par exemple ici nous
on va retourner une carde avec une carde header donc importer de chat cnui afin que ce soit joli et ici on va
l'appeler/ashadmin ou url de./admin donc comme ça on vient d'ajouter notre carde et maintenant tu
peux voir que si dans l'URL je rajoute/ash admin ça vient totalement
pas fonctionner c'est super les amis comme ça on a une bonne démo donc ici en
relançant le serveur j'ai l'impression que ça marche mieux donc on va sur slash admin et tu vois qu'ici on arrive dans
url/admin donc ici on peut vraiment créer des pages pour n'importe quel URL on va juste devoir utiliser des dossiers
alors je sais comme ça que ça peut paraître bizarre mais si je te montre par exemple le code de nowots et ben tu
peux voir qu'on va se retrouver avec plein de dossiers comme ça qui permettent de définir toutes les pages donc tu vois qu'ici on a plein de pages
qui sont définies avec les dossiers donc c'est ça qu'on va reproduire ici mais c'est pour te montrer que tu peux créer
n'importe quelle page avec les dossiers on va pas faire 12000 exemples mais euh c'est déjà un bon début parce que la
deuxième chose intéressante c'est de venir définir un layout le fichier
layout ici il va s'appliquer à toutes les pages qui sont euh on va dire en
dessous de lui c'est-à-dire que ici tu vois que le layout tu peux t'imaginer que c'est une sorte de composant qui
vient wrapper l'intégralité de notre application et donc tout ce qu'on va faire dans le layout sera à appliquer à
toutes les pages enfant c'est-à-dire que vu qu'ici le layout on l'a mis dans app app on a vu que c'était la racine et ben
évidemment en mettant un layout dans la racine ce layout va s'appliquer dans
toutes les pages de notre application par contre ici ce fichier layout qui est
dans dashboard et bah de manière inverse il va s'appliquer uniquement dans les
pages qui vont être dans dashboard donc ici tu peux imaginer qu'il y a slash dashboard et que ici le layout va
s'appliquer que sur ces pages là et donc on peut le voir juste ici on va aller
dans ce fichier layout et on va ajouter un peu de style ok pour que ce soit plus plus beau gosse donc ici je vais
rajouter un max Wiz LG un padding X de 4
et un padding y ici de 6 comme ça on est un peu en haut donc tu vois qu'ici on a
toujours la même chose et que le style que j'ai appliqué ici dans mon
layout et ben il s'applique bien sur toutes les pages ici je vais aussi pouvoir afficher par exemple un
composant header donc ici je pourrais aller dans source ici je pourrais créer
un fichier dans mes composants qui s'appelle header.tsx et dans ma header je vais pouvoir faire export function et donc
moi pour différencier les pages des composants et ben je fais pas d'export default export function là on va faire
une card avec une card header par ex exemple ici à l'intérieur de notre card
Le routing avancées (layout.tsx, error.tsx, loading.tsx)
on va rajouter pardon un padding de 4 et ici on va mettre un petit citation
master comme ceci et on va ensuite pouvoir appliquer cette header à cet endroit là et tu vois que maintenant on
a la header et ici je peux rajouter Flex flexc gap 4 et enlever ce padding y ok
on va plutôt mettre ça dans une div comme ça ce sera plus joli on va faire Flex flexc gap 4 et on va enlever les
settings qui sont juste ici et comme ça tu peux voir qu'on a maintenant le layout donc ici je pourrais aussi
l'appeler enentre parenthèse layout de toute la page et que si je retourne sur l'URL de base on peut voir que ce layout
est aussi appliqué et donc maintenant ce layout il est appliqué dans toutes nos pages d'ailleurs dans la page principale
ici on va rajouter un Link et pour styliser un Link comme un bouton tu peux
utiliser class name button variant ici on va faire dies LG variant
outline et en fait ça va appliquer les mêmes class name qu'un bouton mais pour un Link c'est assez drôle et donc ici on
va faire admin et je vais faire un bouton slash admin comme ça là on a un bouton slash admin et quand on clique on
arrive sur admin c'est vraiment pour te montrer que tu vois qu'on peut vraiment naviguer donc si ici je remplace ça par
une carde pour un peu plus pouvoir s'y retrouver avec à l'intérieur une card
header une card title qui va être slash URL de /ash fin de card header card
content Bam Bam Bam Bam et du coup là on a admin et on arrive sur admin et tu
peux voir que le layout est toujours appliqué donc ça c'est très intéressant c'est le fichier layout ensuite on a
plein d'autres petits fichiers qui s'ajoute à notre hibambelle de petit fichier c'est notamment le loading alors
le loading.tsx c'est un fichier qui nous permet de définir l'interface lors du
loading donc ici encore une fois export default function loading à l'intérieur de ce loading je vais pouvoir retourner
par exemple encore une fois une carde sauf qu'à l'intérieur de la carde header je vais afficher une carte title avec
ici écrit loading point point point point point point comme ça on comprend qu'il y a un chargement qui se produit
donc si ici je fait ce loading il se passe rien parce que ce loading s'applique uniquement quand cette page
charge pour simuler un chargement on peut utiliser un serveur component qu'on verra ensuite après et ici faire une new
promise de P qui va ici sur set timeouts donc ici de r pardon pour resolve on va
par exemple attendre une seconde et tu vois quand je fais ça et je refresche quand je clique là ça va m'afficher
loading pendant une seconde et ensuite ça va m'afficher la page et donc ce loading state il nous permettra dès
qu'on aura ajouté la database pour venir charger des données de venir afficher une page de chargement le temps que les
données soient chargées donc ça c'est très important maintenant de la même manière on peut gérer les erreurs et
donc pour gérer les erreurs on peut faire un fichier error.tsx celui-ci va ressembler à
Le routing avec des paramètres dans l'URL /[someId]/page.tsx
loading sauf qu'il va falloir utiliser use client pour faire un client component c'est très important et donc
là on va pouvoir faire un petit icône warning error warning comme ceci et tu
peux voir qu'à ce moment-là bah il se passe rien il y a toujours le loading mais en fait si après le loading ici je
fais trop new error de invalide path name comme ceci et ben tu peux voir que
là on a le composant erreur qui s'affiche tu peux voir que là ça fait error et
qu'ensuite on a ce trop new error qui s'affiche et donc on va pouvoir gérer la page l'error le loading et le layout
c'est vraiment les quatre fichiers principal et vu qu'on a que une heure dans cette vidéo et ben je ne peux pas me permettre de tout te montrer ce que
je dois te montrer par contre c'est le deuxième moyen de nommer des dossiers c'est de venir passer des paramètres
nous dans notre application on va gérer des citations et ces citations elles vont avoir un ID unique lors de ces ID
uniques et bah en web on va généralement utiliser l'URL pour stocker les
informations ou les paramètres qu'on a sur la page donc si je suis sur la page
de l'utilisateur qui a l'id1 j'ai envie que mon url elle ressemble à
ucode.sh/dhboard/user/1 pour vraiment avoir le bon projet et donc pour faire ça on peut utiliser des paramètres dans
les dossiers on va utiliser des crochets comme ceci des crochets à gauche et des crochets à
droite pour dire en fait cette partie-là de l'URL est dynamique et ensuite on va
pouvoir récupérer les données avec ici la page en utilisant les paramètres qui
sont des chromise JavaScript on va le voir il faut à tout prix que ce soit un serveur component pour gérer des
paramètres donc on va le faire ensemble ici on va créer par exemple un nouveau
fichier donc au cas où dans vs code quand tu crées un fichier tu peux directement appeler ça avec des dossiers
on va faire citation/ash citation
id/ash page.tsx et là ensuite je vais faire export default function ici on va faire
le a5 citation page on peut juste l'appeler page et ici on va pouvoir return toujours la même chose avec une
card une card header une card title et
ici je vais écrire un je t'invite vraiment d'ailleurs à faire tout ce que je fais avec moi en même temps j'espère
que c'est déjà le cas depuis le début mais ça te permet de pratiquer de comprendre ce qui est en train de se produire donc ici dans ma page je vais
pouvoir enlever cette error je vais pouvoir enlever ce loading et je vais pouvoir rajouter une card
content ici avec un Link qui va aller sur
href/admin/citation/1 avec ici citation 1 et dans class name je vais faire button variant avec ici size LG variant
Récupérer les paramètre dans notre page
outline donc comme ça je refresche et maintenant on a citation 1 et je peux venir faire Flex Flex call gap 4 pour
venir créer plusieurs fichiers plusieurs PAF comme citation Melvin donc ici je
vais faire/ashcitation/melvin et citation Patat ou
citation Youyu ID comme ceci on va mettre un super long ID pour te montrer
qu'on peut vraiment tout récupérer donc là tu peux voir que je fais une redirection dans l'URL à
avec/ash1/melvin ou ce uu ID donc si je clique ici on peut voir que ça marche que ça m'affiche bien la page et que
dans l'URL on a bien tout ce que je t'ai dit juste avant mais le problème ici
c'est qu'on n pas récupéré le paramètre de l'URL pour récupérer le paramètre de
l'URL on peut venir ici dans les props rajouter props de points là on va
rajouter params de points et là on va dire que params il est de type d'une
promise qui vient retourner citation ID string donc là c'est vraiment ce qu'il
faut écrire là donc ça paraît un peu des de la magine noire parce qu'on peut écrire n'importe quoi mais c'est bien ce
que nous donne nextjs on peut le tester parce que si je fais console log de props ici et que j'affiche ici la
console bon alors tu peux voir que c'est un peu le le bordel mais on peut voir qu'il y a bien params qui est égal à une
promice et on peut même voir qu'on a le C ID qui est ici mais donc pour récupérer les params on va faire await
props.pams à ce moment-là je vais pouvoir ici afficher params et donc tu peux voir que ici on a l'erreur qui a
été catché parce que je peux pas afficher de d'objet donc ici je peux faire json. stringify de params nul 2
comme ceci normalement cette fois ça va mieux marcher et donc si je refresh ici on peut bien bien voir que params
contient citation ID et ce citation ID il est bien égal à l'ID qu'on avait
juste auudessus alors je sais pas pourquoi je viens de le coller mais on a l'ID si ici je vais sur citation 1 on voit bien qu'on a citation ID 1 et si si
je vais sur citation Melvin on peut bien voir qu'on a citation ID Melvin donc là on va vraiment pouvoir passer des
paramètres dans l'URL on peut aussi dans une page récupérer des search params qui
vont être ici une promise d'un record de string qui va sur string ou string er
comme ceci donc la syntaxe c'est un peu compliqué mais donc ici je peux venir récupérer les search params est égal
await props.search params et là je vais pouvoir afficher deux titres donc là tu
vois que c'est vide mais dans l'URL on est capable de rajouter des search params avec un point d'interrogation avec par exemple ici Search est égal à
test plus bonsoir et tu vois qu'ici on a search est égal à test plus bonsoir on
peut aussi mettre des tableaux comme Search est égal à 1 2 3 avec des virgules et tu vois que là ça vient de
donner un tableau alors alors non ça ne donne pas de tableau je sais pas comment on fait des tableaux dans l'URL mais ça nous permet de récupérer les search
params donc ça c'était les deux trucs que j'ai envie de te montrer donc ici on peut faire ça avec l'URL on peut évidemment faire plusieurs fichiers dans
plusieurs fichiers comme je te l'explique juste ici donc user id/csis/ coursese ID ça quand tu fais ça ça
voudrait dire que tu es en train de visionner le user numéro 1 et tu regardes toutes les coursises qu'il a
enfin tu regardes les coursises qu'il a à l'ID numéro 4 donc ça permettrait de voir la liaison entre user et coursiss
on a un troisième truc qu'on va pas faire ensemble en démo parce que c'est plus avancé on a qu'une heure ça fait
déjà la moitié du temps donc ça passe très vite mais ici c'est de faire point point point point allall pour venir
récupérer tous les paramètres donc ici quand tu fais point point point all ça vient vient venir récupérer toutes les
parties qui sont après user donc ici vu que j'ai mis point point point après user ça vient récupérer toutes les
parties et ça va te les mettre dans un tableau séparé par des virgule donc ici 4 coursis 8 de la même chose ici ça va
te faire XC 1 2 3 6 lol 6 dragon donc tu vois qu'on retrouve tout euh qui se
reproduit ici et ici une fois qu'on a vu tout ça on va pouvoir passer à la suite
donc ici on a vu déjà la page loading et cetera et cetera et sache qu'il y a pas mal de trucs avancés il peut y avoir les
group rotes il peut y avoir les parallèles rotes les intercepting rotes euh les metadata et cetera tout ça on n
pas le temps de le voir si je veux m'occuper de Prisma en 1 heure et c'est moins important si vraiment tu as envie
de le voir tu peux te rendre sur mlv.sh/nexjs et ici tu auras accès à une
formation gratuite qui a été suivie par 5387 personnes donc c'est une formation
super complète sur nextjs tu peux t'inscrire c'est totalement gratuit voilà il il y a plein de reviews et tu
auras des cours sur nextgs les tests unitaires Rea query et cetera je t'invite vraiment à t'inscrire si tu as envie de pratiquer c'est vraiment le
meilleur moyen d'apprendre c'est de pratiquer de passer à l'action et donc pour ça tu peux cliquer dans le premier lien dans la description pour passer à
l'action pratiquer et tu pourras le faire dès la fin de cette vidéo tu le recevras par email ce qui nous intéresse maintenant c'est d'ajouter Prisma pour
pouvoir gérer la création de notre première citation parce que jusqu'à maintenant notre application elle est
très statique hein on n pas ici de de citation et le but c'est qu'on ait ici la liste de toutes nos citations et
qu'on aie un bouton pour créer une citation donc je te propose de commencer par là de venir ici créer
citation/ne/p.tsx et comme ça ici on va pouvoir gérer la création d'une citation on va gérer la création d'une citation
Création de formulaire
premièrement avec des serveurs component pour ça on va faire export default
function page et ici on va retourner en form tu peux importer le composant form
comme ceci import form from next/ form comme ceci ensuite dans ce form ici tu
peux voir qu'il a besoin d'une action et ici on va pouvoir créer A5 comme ceci on va faire use Server alors ça ça va nous
permettre de créer une action comme ceci et à l'intérieur de ce formme ici on va pouvoir créer différentes différents
champs donc ce que je vais commencer par faire c'est ici ajouter un label à mon
application ça va me permettre de pouvoir utiliser le composant Label et en fait je t'ai dit qu'on allait
utiliser une app route ça va te permettre de tout voir donc on utilisera pas une serveur action pour l'instant donc je me suis un peu trompé dans mon
histoire on va pas faire de serveur action donc ici on va faire un label avec si citation et on va faire un
deuxième champ avec si author suivi d'un bouton de type submit avec écrit submit
ici on va pouvoir faire name author ici on va pouvoir faire name citation et à ce moment-là on va pouvoir
aller dans page ici supprimer les deux premiers liens et faire create citation
admin/citation/ne tu peux voir que malgré que j'utilise ici citation ID celui ça celui-ci va être prioritaire
sur celui-là c'est-à-dire que maintenant évidemment j'ai j'ai coupé le serveur donc ça ça marchotte ça marchotte plus
trop trop mon histoire mais ici dans create citation on peut voir qu'on a bien notre citation ici qui nous arrive
sur cette page là afin d'être cohérent avec nos autres pages je te propose d'ajouter un card avec ici class name
ici class name padding 4 ou padding 6 plutôt et comme ça ou sinon on va faire
on va faire un truc standard on va faire un card header un card title avec si
creat citation et ici un card content avec ici notre formulaire donc ici ce
qu'on peut faire dans notre formulaire c'est venir remplacer cette action par une URL par exemple ap/
comme ceci et avec ici for méthode qui va être égale à post et après petite
analyse finalement on va pas utiliser ce composant forme je suis désolé il permet de faire autre chose mais ici pour ma
démo j'ai vraiment envie de te montrer de faire comment faire ça avec les API Rootes donc ici tu vois qu'on fait form
action api/ citation avec la méthode POST ap/citation ça va être un dossier qui
nous va nous permettre de créer une citation donc là pour l'instant il se passe rien d'ailleurs je suis en train
de me dire que j'ai pas mis mes class name au bon endroit donc on va le mettre dans le form et ici tu vois que si
j'écris test test submit ça va nousediriger sur une page 404 Not F nous
on va venir créer cette page donc dans app pour créer une app rot donc quelque chose qui va être accessible VI API on
va faire api/citation/roout.ts et donc à ce
moment-là l'AP route comme ceci c'est un dossier qu'on peut mettre dans API mais on peut le mettre n'importe où et en
Les API Route endpoint (route.ts)
définissant le fichier root.ts dans ce fichier là on peut exporter des fonctions qui définissent
les différentes méthodes et ces méthodesl peuvent être appelés donc nous ce qu'on va faire pour tester c'est
export function A5 function get et ici
ce qu'attend comme réponse une AP rote c'est next response point JSON et ici on va faire
ok TR et là tu vois que tu peux utiliser n'importe quel outil pour pour tester des request donc par exemple httpi ici
je peux mettre une URL comme localos 3000/api/citation et là je vais faire
command enter sur httpi pour tester et on peut voir qu'on a bien ok trou ici on
est côté backend donc je peux très bien faire en de process node en comme ceci et tu vois
qu'à ce moment-l et ben hop ça me donne développement donc ici je vais pouvoir appeler mes request nous ce qui nous
c'est de créer un endpint post et pour récupérer le body on va prendre la request qui va être égale à next request
et ici on va récupérer le JSON qui a été envoyé dans la request avec
request.json on peut ensuite sans autre console log le JSON qui va être console log dans le backend et on va retourner
ici du coup pour développer nextwp.gson et ici on va mettre Jon à
l'intérieur maintenant normalement quand je fais ça dans notre application si si je fais submit ça marche pas parce qu'il
faut utiliser un form data donc là j'ai j'ai buggé mais ce qu'il faut qu'on récupère c'est le form data pas Jon
parce qu'en fait quand on fait ça on récupère un for data donc ici tu vois qu'on avait une petite erreur donc quand tu as des erreurs pas de panique tu
ouvres la console tu lis l'erreur et tu vois qu'ici moi je reconnais avec mon expérience que cette syntaxe là ça
ressemble typiquement à un forme data donc j'écris forme data et normalement on a résolu notre problème donc ici je
vais venir ici je vais faire submit et tu peux voir qu'ici on a aucune doné c'est c'estz normal parce que pour récupérer les datas il faut du coup
faire comme ceci donc ici on va faire citation est égal à json.get citation et auutor est égal à
json.gate de auutthor et ici on va appeler ça form data en faant ça si je
retourne data et je reviens là et je submite on peut voir qu'on a test test donc là si ma citation c'est
gemel Jean vendam submit on a gel Jean vendam donc on récupère les données dans
notre app donc c'est pas un moyen standard de faire en XJS mais c'est juste pour te
montrer comment créer une appot et ensuite on va modifier ce formulaire pour te montrer comment faire d'autres
choses donc ici on est bien en backend et vu qu'on est en backend on est capable ici de venir sauvegarder les
données dans Prisma et c'est le but qu'on fasse juste ici et donc pour ça on va aller sur Google et on va faire
Prisma setup next JS alors normalement on va cliquer ici on va pouvoir aller
sur un bouton get started with Prisma on devrait avoir Quick Start le Quick Start
Setup de Prisma
c'est le meilleur tuto on va choisir ici SQL Lite très important sinon tu vas avoir plein de problèmes et ici on va
pouvoir passer ça ce qui nous intéresse ici c'est de faire NPM install Prisma save dev donc ici on va lancer le
serveur on va ici lancer pnpm install Prisma save Dave donc on va installer
Prisma en développement ça va prendre un petit moment et puis là vu que j'ai utilisé p PM il s'énerve un petit peu
mais c'est pas grave ça nous installe Prisma dans notre environnement comme ceci et en attendant on peut déjà copier
la deuxième commande donc ici NPX Prisma init Data Source provider SQLite c'est
très juste et donc ici on va faire pnpmdlx ù tu peux utiliser la commande par
défaut et ici ça va venir setup Prisma donc moi ici j'ai une petite erreur si tu utilises Prisma tu vois que j'ai
encore une petite erreur et donc cette erreur elle venait que j'avais pas la bonne version de not JS donc si j'ai
fait NVM use node donc au cas où tuas NVM voilà si tu as le problème upgrade
juste ta version de notre JS et ici en faisant init on a maintenant le fichier qui est apparu tac Chac tac dans Prisma
on a le schemma Prisma alors Prisma on va pas faire tout un tutoriel dessus j'ai d'autres vidéos spécialement dédié
à Prisma donc on va pas prendre 12000 ans mais on va créer un modèle citation avec ici ID qui va être donc ici on va
s'inspirer de ce qu'ils ont fait comme exemple parce que en vrai je crée rarement des modèles donc ici on va copier un modèle d'exemple on va le
coller ici on va faire citation in default à l'intérieur il y aura le texte
ici qu'on va faire string il y aura le auutor qui va être une string il y aura
le created ADS qui sera at created ad qui sera une date comme ceci et je sais
pas si ça marche ça non faut faire default now si je me trompe pas voilà
donc je je fais pas ça tout tous les jours on peut venir comme ceci dans mon autre application pour venir voir un peu
ce que ça donne mais ici le create dat c'est dat time ok c'est pour ça donc comme ça on a créé notre citation et une
fois qu'on a fait ça je peux faire Prisma migrate dev on va devoir donner un nom à cette migration on va l'appeler
initial migration et à ce moment-là ça va venir sauvegarder ta migration ce qui
te permettra d'avoir une database en local maintenant si tu retournes sur leur site tu peux pe cliquer sur le
deuxième lien quand tu tapes ça best practice for instancing Prisma client in
nextjs et donc ici on peut copier ce fichier db.ts avec ce code qui est juste là c'est très bien et là on va aller
dans notre source on va aller dans lib on va créer un nouveau fichier qui s'appelle
prisma.ts et on va coller l'intégralité de ce fichier ça va venir exporter un
composant ici Prisma et moi je vais enlever le default parce que j'aime pas trop quand c'est exporté par défaut donc
ici je fais export con Prisma maintenant que tu as ça on peut venir ici dans la PI root venir faire await Prisma on
importe ici Prisma point citation pointcate ici on va pouvoir passer data
et dans les datas on a juste deux données obligatoires à passer c'est le author bah ça tombe bien parce que on a
récupéré le data.or et le texte qui est égal à data.citation comme ceci alors ici il
est pas content parce que par défaut ça donne ce type là alors pas de souci donc nous ce qu'on va faire au lieu de de
passer un objet comme ceci je vais pas non plus te faire faire des mauvaise pratique mais ici on va faire ça et on
va transformer ça en string comme ça on a pas de souci et de la même manière AOR
comme ceci on va faire ça et on va le transformer en string comme ça c'est juste et là comme ça on va pouvoir créer
stocker le résultat dans New citation et dans cette nouvelle citation on va pouvoir retourner ici c'est une bonne
pratique que un app endpint retourne la citation comme ceci et donc à ce moment-là tu peux voir que si je relance
le serveur c'est toujours mieux que je supprime ça et que je refresche ma petite page pour créer une citation on a
ici CRE citation citation auutor je peux venir écrire citation j'aime l'eau et on
va faire JeanClaude vendam et tu vois qu'ici ça nous a bien créé une citation
on peut ensuite lancer Prisma studio dans la console Prisma studio ça va lancer un petit serveur comme ceci qui
nous permet de voir qu'est-ce qu'il y a dans notre database et tu peux voir qu' on a persisterer les données de gemlo JCVD avec la date l'ID on a toutes les
données qui ont été sauvegardées et donc ça prouve qu'on a bien fait fonctionner notre application et donc là on a maintenant notre nouvelle citation et on
a un moyen de venir sauvegarder donc ça c'est comme ça qu'on peut créer des API root ces API Rootes bah elles sont
accessibles par n'importe qui hein comme notre application c'est-à-dire que là dans HTTP je peux venir faire un méthode
une méthode POST dans body ici je peux faire un form data et tu vois que sans autes je peux venir créer ici citation
ici je peux venir créer une citation comme un biscuit ça n'a pas d'esprit c'est juste un biscuit mais avant
c'était du lait un œuf et dans les œufs il y a de la vie potentielle donc ça c'est une vraie citation de Jean-Claude
VAM on va faire autor JCVD et ensuite on va lancer ceci
directement dans notre interface et tu peux voir que ça m'a dit connection Refused et bam pourquoi parce que j'ai
oublié de relancer mon serveur mais si je le refais et que je relance mon serveur on peut voir que ça vient créer
la citation avec l'ID numéro 2 et donc ça fonctionne bien euh évidemment on peut créer toutes les citations qu'on
veut mais ce qui nous intéresse c'est de créer quand même un formulaire un peu plus
interactif il faut savoir que depuis le début on est bien dans des serveur components et c'est pour ça qu'on n pas
de validation de données tu vois tout est statique ici on est dans un serveur component pour le savoir il nous suffit
Refactor avec le isLoading ou pending state
de venir afficher la console et quand on fait console.log bonsoir ici tu vas voir qu' il va s'afficher ici et on peut voir
que ça nous affiche que c'est dans le serveur on peut voir aussi que il apparaît ici si je passe mon composant
en use client ici on peut voir que le bonsoir maintenant s'affiche aussi là
sans la petite indication serveur donc nous on va laisser ça en en client component comme ceci et de cette manière
ici on pourrait faire un formulaire peut-être comment comment puis-je dire plus
dynamique si on veut qui va venir utiliser ici la props action qui peut
être asynchrone qui va prendre en paramètrre form data et qui va pouvoir
appeler une méthode donc par exemple ici je vais pouvoir venir faire
con create citation est égal à une méthode qui prend en form data en
paramètres et qui va venir fetch les données donc qui va venir fetch
l'url/ api/citation avec ici come body json.
stringify donc ici citation formdataget citation et ici auutor form
datatag auutor comme ceci avec ici méthode donc viru ici on va faire une
méthode qui va être post et là on va pouvoir faire const result avec un a wait ici A5 ici con JSON est égal à
await.json et ici on va pouvoir faire console. log du JSON comme ceci et donc
là je vais pouvoir appeler await create citation de form data en faisant ça ici
je peux venir rajouter un state react vu qu'on est dans un serveur un client component on peut venir rajouter un
state par exemple is loading set is loading est égal à state et comme ça par
défaut il va être à false et ensuite on va faire set is loading true et ici on
va faire à la fin set is loading false ça c'est la première manière de
faire qui serait pas fausse et ici du coup quand ça charge on peut disable le
bouton disable si c'est is loading et donc à ce momentl quand je fais tout ça et ben on peut voir
qu'ici je peux venir créer une citation donc on va reprendre une citation de Jean-Claude Vanam ici une baffe ça te
Bou 3 hectares moi avec 3 hectares je te fais 2000 kg de riz avec 3 hectares je
te nourris l'Avignon tu vois donc ici on va faire ça JCVD comme ceci submit tu peux voir
qu'ici on a une grosse erreur parce que apparemment mon app citation m'a retourné une 500 dans la face parce que
on passe maintenant un gisson c'est vrai que j'ai j'ai zappé j'ai la modification mais maintenant il faut
utiliser Jon parce que on passe bienon et donc ici on peut
faireonor ici on peut faireonation comme ceci donand on fait
ça on peut venir retourner ici mettre le texte de JeanClaude VAM
JCVD et tu peux voir ça a fonctionné alors de toute évidence on n pas vu le
petit state de loading is loading loading et sinon on va afficher submit
et pour encore plus le voir ici je vais rajouter un petit timeout avec ici une promise qui va faire 7 Time Out de r
après 1000 milliseces et comme ça on a une seconde d'attente on va juste faire test test et tu peux voir que ici si on
enlève le eit parce que sinon ça ça marche pas précisément comme on veut mais là quand j'écris Melvin Melvin
submit on peut voir que ça affiche le loading state il y a une meilleure manière de faire avec react 19 et next
c'est de venir créer ici un composant qui va s'appeler submit button ici on va pouvoir récupérer pending de use form
status on va pouvoir prendre notre bouton retourner ici notre bouton Remplacer EAS loading par pending et
mettre notre submit bouton ici l'avantage de faire ça c'est qu'en fait
quand ici on vient await notre action quand tu utilise la propre action elle
vient faire une sorte de donc ici d'ailleurs maintenant je peux utiliser le composant forme avec majuscule mais
bon de toute façon il arrive jamais à l'importer c'est pas très grave et quand on utilise la props avec action ça vient
envoyer à notre application à tous les enfants ici donc tout ce qui est à
l'intérieur de notre formulaire notre formulaire est en chargement et donc comme ça maintenant avec ce petit code
ici je peux faire test test on voit que le submit il fonctionne bien et dès que c'est submit ça vient automatiquement vider l'input donc ça c'est vraiment la
meilleure manière de faire la meilleure manière de faire plus plus c'est même d'utiliser des serveur action pour
utiliser des serveurs action on peut venir ici créer un
Refactor dans une Server Actions
citation.action.ts comme ceci et ici on va pouvoir commencer ce fichier par use
Server on va ensuite pouvoir faire export const create
citation lui il va prendre en paramètrre donc ici on va lui faire citation qui
est égal à text string et auutor string donc ici on peut créer une fonction
pardon on va créer des fonctions tout le temps pour pas te perdre avec les différentes syntaxes et donc ici je peux
prendre ce que j'ai fait dans la route donc ici on va prendre ça on va copier ce que j'ai fait dans la route on va le
mettre ici on va importer Prisma on va passer notre fonction en asynchrone et
ici et ben dans les datas je peux mettre citation.utor et citation Tex je peux
ensuite retourner new citation et à ce moment-là je vais appeler ça create station action je mets
toujours un un suffixe à la fin pour bien m'en rendre compte et en faans ça et ben je peux supprimer tout ça et
juste faire cons JSON est égal à await de create citation action et ici je vais
pouvoir passer les paramètres donc ici author formdata.get auutor et donc ici on va
changer ça en texte comme ça c'est bon et ici on va faire texte form
datata.get de texte avec ici des strings string ici on va faire des
strings comme ceci et en faisant ça ici je peux écrire test test on peut voir
dans la console que ça submit ça nous envoie bien la création et ça fonctionne comme avant une fois qu'on a fait ça
dans cette serveur action on a même l'avantage de pouvoir préciser une redirection et donc ici je pourrais
rediriger sur slash admin comme ceci avec la méthode redirect qui va nous
permettre de dire à la réponse de au lieu de retourner une réponse comme je le fais ici et ben ça sert à rien si il
y a une nouvelle citation qui a été créé on redirige et sinon on peut return
error error while creating de citation donc si si je
fais test test submit ça me ramène sur la page et je peux en créer une donc ça c'était pour l'exemple d'une serveur
action enfin d'une API rote puis d'une serveur action tout ça dans un composant react qui a été un serveur component et
qui est maintenant un client component quand on utilise un client component on se rappelle qu'il est possible de venir
ici afficher par exemple utiliser des hook des use effect des use form statut
des use tout ce que tu veux qui vont être disponibles tu peux voir d'ailleurs qu'on peut voir qu'ici si le JSON est
défini c'est une erreur et donc évidemment on pourrait faire if json. error ici on pourrait afficher som error
ur ou on pourrait juste afficher l'erreur comme ceci son error et donc typiquement quand est-ce qu'il y a une erreur et ben si par exemple ici je
passe un number comme ceci et B ça va m'afficher une erreur ici du coup il
faudrait catcher cette erreur parce que ici create il va juste trop directement une erreur donc ici il faudrait plutôt
faire ça un try catch dans le cas où on catche on affiche ça et dans le cas où
on catche pas euh on affiche ça et ici on a même pas besoin de paramètres et
donc à ce moment-là en faisant ça ça va mieux marcher donc si ici je fais test test tu vois que ça va bien m'afficher
une erreur error while creating de components parce que ici je retourne une erreur alors que si je ne fais pas
d'erreur donc comme j'avais fait avant et ben à ce moment-là ça va me rediriger sur la page principale normalement si je
refresche alors peut-être qu'il faut plutôt que je mette le redirect ici après réflexion ça va mieux fonctionner
parce que le redirect en fait en xGS faut faire attention parce que il trop une erreur et et cette erreur elle va
être catchée par le try catch le résultat de catcher l'erreur par le try catch c'est qu'on va retourner ici donc
Les Server Components
il faut bien le mettre en dessous comme ça normalement ça va marcher donc si si je fais test test bom je suis redirigé ici justement dans ma page admin ici je
suis capable de venir récupérer toutes les citations et les afficher const citation est égal à await de Prisma
point citation find many ici on peut venir
faire ORDER BY created desk et ici dans mon C
content et ben vu que je suis dans un serveur component je suis capable de faire des queries à ma database puis
faire citation map de citation qui va venir ici sur une card avec class name P4 et
citation Tex comme ceci avec citation.id et donc là tu vois qu'on a
toutes les citations qui viennent d'apparaître on peut évidemment rajouter des styles un peu plus jolis donc par
exemple ici on va faire ça comme ça et là je vais sélectionner ça et je vais lui dire voilà j'ai rajouté des petits
styles comme ceci et en dessous on peut faire un pcitation.utor avec généralement une
sorte de double slash comme ceci je je sais pas très bien le caractère que c'est mais ici on va mettre deux tirets
comme ça ici on voit qu'on a l'auteur avec JCVD qui nous a fait plein de citations donc ça on est dans un serveur
component et on va pouvoir à l'intée serve component par exemple créer un client component qu'on va pouvoir render
dans la page on n pas obligé de rendre toute la page serveur donc ici je pourrais par exemple créer delete
citation button TSX comme ceci là je vais pouvoir créer un composant qui
delete donc export function delete citation button
comme ceci là on va pouvoir faire un petit stage is confirme is confirme est
ég state false et con ON DELETE et là on
appellera pour l'instant on va faire un alerte Delite comme ceci et on va retourner un bouton avec ici une trash
donc il y a pas d'icône trash on va pas installer lucide c'est pas grave on va juste mettre une croix on va faire une
variante ici is confirme si il est en train de confirmer on fait ça sinon on affiche outline tu vas voir ce que ça
donne c'est joli et une fois qu'on a fait ça dans le uncilick en fait comme ça on oblige l'utilisateur
de cliquer deux fois c'est assz c'est ça peut être sympa donc si il a confirmé et
ben à ce momentl on appelle und delete et sinon on fait set is confirme à et
donc ici je peux faire delete citation button que je vais pouvoir afficher dans ma carde donc ici on avait le premier
élément qui se situe là on va venir faire une div comme ceci avec class name Flex Flex gap 2 ici on va faire Flex
item start gap 4 comme ceci là je vais pouvoir rajouter un flex 1 d'ailleurs
Supprimer un Client Component
ici pour que ça prennent bien toute la place disponible et ici dans ma div non
dans ma dans ma carte je vais pouvoir afficher Delite citation button à ce momentl tu peux voir qu'on a une erreur qui nous dit ouais c'est pas cool ce que
tu es en train de faire parce que tu utilises state dans un serveur component c'est pas possible donc ici on va dire
que ça c'est un entry point d'un client component et donc là on va voir un deuxième sujet très intéressant donc ici
par défaut en nextgs tout tes serveur components c'est-à-dire que tout ce que tu vois c'est toujours des serveurs
components et pour optin donc pour devenir un un un client component il
faut utiliser la directive donc c'est une sorte de string use clients et à ce moment-là tous les enfants de ce
composant seront aussi des client components donc ça vient vraiment spread ce client components sur le
comme ceci le composant et tous ces enfants qui vont aussi être des cent component on peut utiliser laun pour
éviter ça ce qu'on voit S c'est bon tu vois queand jeite aprs il devient rouge et
après ça me dit delete donc c'est bien c component parce que j'ai ce delete qui fonctionne pour delete ma citation je
pourrais aller dans citation action et ici cré export con delete citation
action avec ici une fonction encore une fois j'ai l'habitude d'écrire comme ça je m'en excuse ID number et iciit
Prisma citationdetew ID comme ceci là on va
passer E5 et on va retourner message
deled comme ceci donc delitation action là je peux faire delitation action avec
un ID l'ID je récupérer dans les props donc ici ID number ici je vais faire
props.id on va faire une fonction as synchrone qui va faire result est égal à
await if result point message et ben ici je peux afficher une alerte où je vais
pouvoir refresh la page donc pour refresh la page on utilise le hook use roue use router ça va nous permettre de
faire roueter. refresh qui va venir refres la page pour mettre à jour notre
interface donc ici ça vient appeler delititation action attention quand même parce que ici n'importe qui peut appeler
cette méthode avec l'ID qu'il veut il y a pas de sécurité hein donc toi si tu fais une application je t'invite vraiment à suivre la formation qui est
en description parce qu'on crée un projet full stack sécurisé qui s'appelle code où on va gérer les permissions les
tout ce que tu veux pour créer une application full stack sécuriisé en 2024 et 2025 et 2026 et 2027 toutes les
années que tu veux en fait bref ici on passe ça et donc là quand je clique sur ce test donc si je veux supprimer test
celui-là et donc ici ça marche pas pourquoi ça marche pas parce que j'ai pas passé de ID ici donc là faut faire
citation.id et à ce moment-là ça va mieux marcher parce que tu vois que ça l'a supprimé donc là je peux supprimer
toutes les citations qu'on a pas besoin donc ici tous les tests et comme ça je peux garder que les citations de JCVD
les deux citations que j'avais gardé et tu vois qu'on a géré le delete avec un client component comme ceci et des
serveur components et des serveur action pour nous simplifier le travail donc
comme ça maintenant on a une super application qui fonctionne qui fonctionne plutôt bien et il y a une
chose qu'il faut qu'on voit rapidement c'est l'édition donc comment est-ce qu'on pourrait éditer d'ailleurs pour
l'instant on peut pas cliquer sur ces trucs mais justement dans citation ID on va venir gérer l'édition et donc là on
va déplacer ce citation action directement dans citation comme ça c'est un peu plus juste et là on va faire
citation form citation form TSX et on va déplacer
toute la logique qu'on avait mis ici un peu près tout ce qu'on a mis ici dans ce citation form et ce citation form il va
Update d'une citation avec notre formulaire
prendre en props ici une citation qui sera égale à une a type de citation qui
est donné par Prisma ici on va juste importer correctement ça et en fait ce qu'on va faire ici c'est qu'on va venir
créer une autre méthode ici qui va s'appeler update citation on va prendre en premère en premier paramètre ID
number et ben si on update on va ici appeler update where
ID comme ceci comme ça on gère l'update une fois qu'on gère l'update ici en fait
on peut se dire bah tu sais quoi on va simplifier les chos on va faire unsubmit
ici et on va se dire en fait que si il y a une props point citation et ben en fait s'il y a déjà une citation on sait
qu'on va vouloir update s'il y a pas de citation on sait qu'on va vouloir edit
donc là on va stocker l'erreor qui va être par défaut nul on va dire par défaut que c'est nul ou string comme
ceci et ici on va faire error est égal à JSON error comme ceci et à à cet
endroit-là on va pouvoir faire bah si error et défini on affiche ça comme ça comme ça ici je peux copier ça mettre ça
là appeler update citation action et du coup ce qui se passe c'est que ici si
citation est définie dans les props on sait qu'on veut update si elle est pas définie on sait qu'on veut créer et du
coup si elle est définie et ben on aura évidemment accès à citation.id qui nous permet ici de venir
faire props.citation.it pour venir procéder à la modification d'une citation comme ça
ici on peut renommer ça citation form on peut enlever le export default et ici on
va faire la même chose donc ici propscitation create ou plutôt Update et sinon create
comme ça soit en update soit créer notre citation et du coup quand on a fait tout ça on peut maintenant venir ajouter un
petit bouton dans notre page ici on va faire un petit link donc ici je vais
pouvoir copier ça on va mettre un emoji ici edit ou comment on appelle ça Pen
voilà on met un petit emoji pen on met
admin/citation/citation.id comme ça on le met dans l'URL de manière dynamique on a le petit bouton edit on va venir
prendre une pe div comme ceci mettre la en sm faire cl Flex Flex G 2 modifier
celuici pour aussi lui mettre une SM et on lui mettre unoj trash comme ça c'est
plus joli et comme ça quandque surit tu vois que ça nous amène ici sur la page qu'on avait cré ensemble mais nous on
veut évidment avoir une meilleure page ici donc là ce qu'on va faire à l'intérieur c'est qu'on va retourner on
va récupérer la citation donc ici en fait vu qu'on a le params on
peut récupérer le citation ID const citation id est égal à params citation
ID ensuite je peux récupérer la citation avec
prismacitation.ff weare id depams.citation id et comme ça juste
pour tester ici on va pouvoir log la citation et doncci il faut juste passer ça en string sinon il est pas pourquoi
il est pas content Michel ah parce que c'est une string il faut transformer ça en number donc ici tu vois qu'on a la citation qui est bien donnée on peut
évidemment faire if bah s'il y a pas de citation on va plutôt retourner une carde bah qui informe C header car title
de citation ici citation id not existe
comme ça on a petite erreur et dans le cas où il y a une citation et ben on va retourner notre F citation form qu'on a
créé en ensemble avec la citation qui est égale à la citation comme ceci et tu vois qu'ici on a update citation alors
le seul truc qu'on a pas fait c'est qu'on a pas rajouté de default value et donc ici on fait default value
citation props.citation.texte et ici on va faire
la même chose pour le author comme ceci props.citationutor et comme ça tu vois
qu'ici ça nous autorempl tout ça on peut ensuite modifier JCVD 2022 par exemple
pour ajouter la date venir submit et à ce moment-là tu peux voir que ça vient modifier correctement notre citation
donc là maintenant ce qu'on peut faire c'est qu'on peut créer une citation donc je vais pouvoir retourner sur donc
évidemment le create ne marche plus pourquoi parce que si on va sur le new/ash page et ben maintenant on a tout
un bordel ce qu'il faut faire dans le New slash page hein c'est modifier tout ça et faire return citation form sauf
que ici on ne passe pas de citation donc ici on a le create citation je peux créer une nouvelle citation moi Adam et
Création d'une page pour VOIR les citations (avec un layout différent)
j'y crois plus tu vois parce que je suis pas un idiot la pomme ça peut pas être mauvais c'est plein de pectine ça c'est
très drôle JCVD submit hop on a bien la nouvelle
citation on peut la modifier pour par exemple rajouter la date 2021 submit et tu vois qu'ici on a bien
la date maintenant on pourrait faire un dernier truc c'est pouvoir cliquer sur la citation pour la partager rajouter un
dernier bouton qui serait un bouton partage afin d'avoir une page spéciale
où il y a que la citation comme on l'a vu avant donc on va faire un petit icône cher comme ça qui va aller en fait sur
slash citation/ citation et on aimerait bien qu'en fait qu' on aille sur cette page qui est pour l'instant une 404 on
va la créer/citation/ citation
id/ptsx ici tu vois qu'on est dans cette et là je va faire export default function page et ce qu'on va pouvoir
faire c'est reprendre à peu près le code qu'on a fait juste ici donc là je vais prendre tout ça je vais mettre tout ça
là et je vais pouvoir return ici une card qu'est-ce qu'on va dans cette carte
donc là j'ai juste repris le même code qu'on avait fait pour la l'Edit d'une
citation donc ici on va faire ça ça et donc là qu'est-ce qu'on va retourner on va retourner la même carte que dans la
page c'est-à-dire cette carte là avec ces données juste ici sauf qu'on va
enlever toutes les actions et comme ça ça va donner ça le truc qu'on aimerait bien faire c'est enlever le layout ici
pourquoi est-ce que on a ce citation master ça c'est plutôt pour les admins tu vois et donc dans layout ici on va
enlever la header comme ceci donc elle est plus là maintenant on a une page sans header et tu peux sans autre
rajouter un fichier layout pour admin export default function layout on va
prendre ici prop s child run comme ceci et là je vais pouvoir retourner une div
avec la fameuse header comme ceci et donc et le
fameux la fameuse div que j'avais juste ici check check che on supprime tout ça
on la met ici donc évidemment j'ai réussi à la perdre en gros c'était une div qui
faisait Flex col donc ça va pas être très dur à refaire class Flex flexc gap 4 donc ici on a notre head uniquement
pour la page ici donc pour la page admin/ citation donc ici ici ici pourquoi ça
marche plus parce que j'ai oublié d'afficher propschrun donc dès que tu ajoutes propsrun c'est beaucoup mieux ça
nous affiche les enfant on peut toujours edit on peut toujours supprimer et cetera sauf que maintenant quand on va sur la page full screen et ben on voit
que ça donc c'est super parce que nous ici on va pouvoir gérer notre propre
layout avec par exemple ici div qui vient faire class name min 8 full Flex
item Center justify donc évidemment j'ai mal écrit justify Center ça nous la met au milieu
et le seul truc qu'il faut qu'on fasse encore c'est de faire ici max with sm m
auto je sais pas pourquoi pourquoi tu te mets pas au centre Michel ah oui parce que parce que j'ai fait une erreur ici
dans dans dans dans la page dans le layout principal tu vois qu'ici j'aiais mis max SZ LG il faut aussi rajouter m
auto sinon ça centre pas au milieu et donc tu vois qu'ici on a notre page qui est centrée et comme ça avec l'url de
notre citation tu peux partager ça à tes amis et ils peuvent eux aussi voir les
citations que tu as créé et donc ici on a l'admin qui permet de créer de update de visionner une citation et on a
ensuite la page de partage de citation et c'est ça l'avantage de nextjs et ce nouveau système de layout c'est que tu
peux créer des layouts très précis encore une fois il y a beaucoup becoup beaucoup de concept à voir cette vidéo
dure déjà 1h20 sans montage donc elle durera bien 1 heure avec le montage mais je t'invite vraiment à aller sur
Outro
mlv.sh/nexjs ou le premier lien dans la description tout de suite afin de passer à l'action de monter en compétence avec
un projet de A à Z sur une plateforme de formation unique en son genre donc ici tu vas avoir tout un cours sur Next sur
prismas sur le setup du projet les serveur component le routing le client component les neutral components et react query avec les tests et cetera et
ca bref tu auras accès à tout ça et tu peux te rendre aussi directement sur cette url là ça te donnera à peu près la
même chose et je pense vraiment que tu vas pouvoir monter en compétence en pratiquant vraiment de cette manière
c'est vraiment le meilleur moyen de monter en compétence on a vu beaucoup de choses on a vraiment utilisé toute la
puissance ici de nextjs ça je peux t'en assurer ça nous a permis de créer une application complète et j'espère
vraiment que tu as pu suivre cette vidéo de A à Z tout de suite inscris-toi au cours dans la description like cette
vidéo et mets ton meilleur commentaire si tu veux aller plus loin avec nextjs j'ai plein de tutoriel Prisma next et C
je t'invite vraiment à regarder un cours sur Prisma par exemple c'est très intéressant et on se retrouve là-bas on se dit à très bientôt ciao ciao


