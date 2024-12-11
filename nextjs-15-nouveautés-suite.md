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

