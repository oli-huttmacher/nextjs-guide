
# Système de Gestion de Contenu sur Next.js 15

Ce document détaille un tutoriel complet pour apprendre à utiliser **Next.js 15** en une heure, en abordant les principales fonctionnalités et concepts de ce framework. 

## Objectifs du tutoriel

- Comprendre et maîtriser les bases et fonctionnalités avancées de Next.js 15.
- Créer une application complète avec du routage, des Server Components, une base de données, et une gestion des données via API Routes.
- Découvrir comment structurer une application moderne avec des outils comme TailwindCSS et ShadCN/UI.

---

## Plan du tutoriel

### 1. Introduction à Next.js 15

Next.js est un **framework** qui ajoute des fonctionnalités spécifiques au web autour de React. Contrairement à React qui est une bibliothèque permettant de gérer l'interface utilisateur, Next.js introduit :

- **Routing** (basé sur des fichiers).
- **Server Components** pour gérer des actions côté serveur.
- **Gestion des cookies**, headers, middleware.
- **Gestion des états de chargement** et des erreurs.
- Des surcouches backend avec API Routes et interactions avec une base de données.

**Commandes essentielles pour démarrer un projet :**

```bash
npx create-next-app@latest
```

### 2. Configuration du projet

#### Options de configuration recommandées :

- TypeScript : Oui.
- ESLint : Oui.
- Tailwind CSS : Oui.
- App directory (Proposed default) : Oui.
- Turbopack (pour le développement local rapide) : Oui.

Structure initiale :

```
- app/
  - page.tsx
  - layout.tsx
- public/
- styles/
- tsconfig.json
- next.config.js
```

---

### 3. Mise en place de TailwindCSS et ShadCN/UI

- **TailwindCSS** permet d'ajouter rapidement des styles sans écrire de CSS classique.
- **ShadCN/UI** est une bibliothèque de composants pré-stylés.

#### Installation TailwindCSS

```bash
npm install tailwindcss postcss autoprefixer
npx tailwindcss init
```

Configurer `tailwind.config.js` pour inclure les fichiers du projet :

```javascript
content: ["./app/**/*.{js,ts,jsx,tsx}", "./components/**/*.{js,ts,jsx,tsx}"]
```

#### Installation ShadCN/UI

```bash
npx shadcn-ui@latest init
```

Ajouter des composants (exemple avec Button, Card, Input) :

```bash
npx shadcn-ui add button input card
```

---

### 4. Gestion des Pages et Layouts

#### Routing

- **Routing basé sur des fichiers** : chaque fichier dans `app/` représente une route.
  - `app/page.tsx` : Page racine.
  - `app/admin/page.tsx` : Page `/admin`.

#### Layouts

- Le fichier `layout.tsx` dans un dossier s'applique à toutes les pages enfants.
- Exemple :

```javascript
export default function Layout({ children }) {
  return (
    <div className="max-w-lg mx-auto p-4">
      <Header />
      {children}
    </div>
  );
}
```

---

### 5. Création et Gestion de Données avec Prisma

#### Setup Prisma

```bash
npm install prisma --save-dev
npx prisma init
```

Configurer `prisma/schema.prisma` :

```prisma
model Citation {
  id        Int      @id @default(autoincrement())
  text      String
  author    String
  createdAt DateTime @default(now())
}
```

Appliquer la migration :

```bash
npx prisma migrate dev --name init
```

Utilisation dans Next.js avec un client Prisma :

```javascript
import { PrismaClient } from "@prisma/client";
const prisma = new PrismaClient();

export async function createCitation(data) {
  return await prisma.citation.create({ data });
}
```

---

### 6. Server Components et API Routes

#### Server Components

- Directement intégrés dans Next.js 15.
- Utilisation d'`await` pour effectuer des requêtes serveur directement dans les composants.

```javascript
export default async function Page() {
  const citations = await prisma.citation.findMany();
  return (
    <div>
      {citations.map(citation => (
        <p key={citation.id}>{citation.text} - {citation.author}</p>
      ))}
    </div>
  );
}
```

#### API Routes

- Fichier `app/api/citation/route.ts` pour gérer des requêtes POST :

```javascript
import { NextResponse } from "next/server";

export async function POST(req) {
  const body = await req.json();
  const newCitation = await prisma.citation.create({ data: body });
  return NextResponse.json(newCitation);
}
```

---

### 7. Interaction Client-Serveur

#### Gestion des états avec React

Utilisation d'un composant client pour gérer des actions interactives :

```javascript
"use client";
import { useState } from "react";

export default function CitationForm() {
  const [isLoading, setIsLoading] = useState(false);

  const handleSubmit = async (e) => {
    e.preventDefault();
    setIsLoading(true);
    // Envoyer les données au serveur
    setIsLoading(false);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="text" required />
      <button type="submit" disabled={isLoading}>
        {isLoading ? "Loading..." : "Submit"}
      </button>
    </form>
  );
}
```

---

### 8. Optimisation et Fonctionnalités Supplémentaires

- **Gestion des erreurs** avec `error.tsx`.
- **Loading states** avec `loading.tsx`.
- **Layouts avancés** pour des zones spécifiques (par exemple, `/admin`).
- **Support des Server Actions** pour centraliser les actions backend dans Next.js.

---

## Conclusion

Ce guide couvre les principales fonctionnalités de **Next.js 15**. Il vous permet de construire une application complète avec des outils modernes et des pratiques recommandées. Pour aller plus loin, explorez des fonctionnalités avancées comme les tests unitaires, React Query, ou encore la sécurisation des routes.

**Liens utiles :**
- [Documentation Next.js](https://nextjs.org/docs)
- [Formation complète sur Next.js et Prisma](https://mlv.sh/nextjs)
