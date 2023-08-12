# How to Setup React.js App

## Pre-requisites

- Install Node.js and npm
  - https://nodejs.org/en/download/
- Install Vite
  - `npm install -g vite`

## Setup base of the project

- Create app with vite
  - `npm create vite@latest <project-name>`
    - Select Framework: `React`
    - Select: `TypeScript + SWC`

> Commit your changes

## Setup style (TailwindCSS)

- Follow tailwindcss setup guide (pass step 1 since we already created app with vite)
  - https://tailwindcss.com/docs/guides/vite
- (Recommended) If you are using VSCode, install Tailwind CSS IntelliSense extension
  - https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss

> Commit your changes

## Setup React Router

- Install react-router requirements
  - `npm install react-router-dom localforage match-sorter sort-by`
- Setup lazy loading router

  - Create `src/router.ts`
  - Create few pages
    - Create `src/pages` directory
    - Create `src/pages/Home/Home.page.tsx` component
      ```tsx
      export function Home(): JSX.Element {
        return (
          <p>
            Hello from <b>Home Page!</b>
          </p>
        );
      }
      ```
    - Create `src/pages/About/About.page.tsx` component
      ```tsx
      export function About(): JSX.Element {
        return (
          <p>
            Hello from <b>About Page!</b>
          </p>
        );
      }
      ```
  - Add routes to `src/router.ts`

    ```tsx
    import { createBrowserRouter } from "react-router-dom";

    export const router = createBrowserRouter([
      {
        path: "/",
        async lazy() {
          let { Home } = await import("./pages/Home/Home.page.tsx");
          return { Component: Home };
        },
      },
      {
        path: "/about",
        async lazy() {
          let { About } = await import("./pages/About/About.page.tsx");
          return { Component: About };
        },
      },
    ]);
    ```

  - Add router to `src/App.tsx`

    ```tsx
    import "./App.css";
    import { RouterProvider } from "react-router-dom";
    import { router } from "./router";

    export function App(): JSX.Element {
      return (
        <>
          <h1 className="text-3xl font-bold my-4">Hello world!</h1>
          <RouterProvider router={router} />
        </>
      );
    }

    export default App;
    ```

- Check in browser are your routes working properly
  - If something is not working, check react-router documentation
    - https://reactrouter.com/en/main/route/lazy

## Setup Redux

> TODO
