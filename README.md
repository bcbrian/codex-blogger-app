# PRD: LocalStorage-Powered Blog Post Engine

## Overview

This is a single-page **Blog Post Engine** built with **React (TypeScript)** and tooled by **Vite** for rapid web development. Styling is handled with **Tailwind CSS**, and **React Router** enables client-side navigation. All data is stored in **localStorage**, providing persistence without a backend. The application supports multiple authors and basic blog functionality all within the browser.

**Key Features:**

* **Post Listing & Filtering**: Landing page displays all blog posts and allows filtering by author or by recency.
* **Author Management**: Users can create new authors by providing name, bio, and avatar URL.
* **Post Creation & Editing**: Users can create new posts and edit existing posts, all persisted to localStorage.
* **Author Preferences**: Each author has display preferences (theme and font size) set on a dedicated page.
* **No Backend Required**: Data is stored locally so the app works offline with no server required.

## Technical Architecture

**React & Vite** provide the SPA framework and tooling. The codebase is modular with components per page and utility modules for data handling.

**Tailwind CSS** is used for styling with utility classes. **React Router** manages client-side navigation. Routes include:

* `/` – Landing page
* `/authors/new` – Create Author page
* `/posts/new` – Create Post page
* `/posts/:postId/edit` – Edit Post page
* `/authors/:authorId/preferences` – Author Preferences page

**LocalStorage for Persistence**: Data is stored under the keys `"authors"` and `"posts"`. Objects are serialized with `JSON.stringify` and parsed with `JSON.parse`. Components fetch data from localStorage on mount via a dedicated service module so UI code does not access localStorage directly.

## Data Model

The app stores **authors** and **posts** collections. Each author includes a **preferences** object. TypeScript interfaces enforce data shape:

```typescript
interface Preferences {
  theme: string;
  fontSize: string;
}

interface Author {
  id: number;
  name: string;
  bio: string;
  avatarUrl: string;
  prefs: Preferences;
}

interface Post {
  id: number;
  title: string;
  body: string;
  authorId: number;
  createdAt: string;
  updatedAt: string;
}
```

These interfaces act as contracts ensuring consistent data throughout the app.

## LocalStorage Data Service (`dataService.ts`)

A module `dataService.ts` handles all CRUD operations on authors and posts. It initializes empty arrays in localStorage when needed and provides functions like `getAuthors`, `createAuthor`, `updateAuthor`, `getPosts`, `getPostById`, `createPost`, and `updatePost`. Components import this service to interact with storage instead of using the Web Storage API directly.

## View Specifications and Behavior

### Landing Page – *All Posts List & Filters*
Lists all posts and lets users filter by author or show only recent posts. Each post entry links to an edit page. The page also links to creating new posts or authors.

### Create Author Page – *New Author Form*
Form for adding an author (name, bio, avatar URL). Validates required fields and saves via `dataService.createAuthor`.

### Create Post Page – *New Blog Post Form*
Form for composing a new post. Requires a title, body, and author selection. Saves via `createPost` and redirects back to the landing page.

### Edit Post Page – *Edit Existing Post Form*
Similar to the create form but pre-populated with the post’s data. Saves changes via `updatePost`.

### Author Preferences Page – *Adjust Author Display Settings*
Allows editing an author’s theme and font size preferences using `updateAuthor`.

### Modularity and Inter-View Communication
UI components delegate all storage operations to `dataService`. Routing via React Router ensures pages can be accessed directly and data is fetched fresh on mount. Tailwind keeps styling modular. LocalStorage acts as the single source of truth for data across views.

The PRD above describes the app structure and behaviors.

## Task Tracking

Tasks for building the app are defined in `todo.json`. Each entry includes a `status` field that should be set to `todo` or `done` to track progress.
