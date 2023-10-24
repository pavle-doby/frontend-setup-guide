# How to Setup React App

> React + SWC, TypeScript, TailwindCSS, ESlint + Prettier + Airbnb Setup, React Router + lazy loading, Redux

**// Content**

0. Pre-requirements
1. Setup base of the project (React + SWX, TypeScript)
2. Setup style (TailwindCSS)
3. VSCode - ESLint, Prettier & Airbnb Setup
4. Folder Structure
5. Setup React Router
6. Setup Redux (Redux-Toolkit)

## 0. Pre-requirements

- Install Node.js and npm
  - https://nodejs.org/en/download/
- Install Vite
  - `npm install -g vite`

## 1. Setup base of the project (React + SWX, TypeScript)

- Create app with vite
  - `npm create vite@latest`
    // Follow the wizard
    - Select Framework: `React`
    - Select: `TypeScript + SWC`

> Commit your changes

## 2. Setup style (TailwindCSS)

- Follow tailwindcss setup guide (skip the step 1 since we already created app with vite)
  - https://tailwindcss.com/docs/guides/vite
- _(Recommended)_ If you are using VSCode, install Tailwind CSS IntelliSense extension
  - https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss
- _(Recommended)_ Install A [Prettier v3+ plugin for Tailwind CSS v3.0+](https://www.npmjs.com/package/prettier-plugin-tailwindcss) that automatically sorts classes based on recommended class order.
  - `npm install -D prettier prettier-plugin-tailwindcss`

> Commit your changes

## 3. VSCode - ESLint, Prettier & Airbnb Setup // _Optional - But Highly Recommended_

- Install ESLint & Prettier extensions for VSCode

  > Optional - Set format on save and any global prettier options

  - [Prettier Extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
  - [ESLint Extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

- Install Packages

  - `npm i -D eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-node eslint-config-node`
  - `npx install-peerdeps --dev eslint-config-airbnb`

- Create .prettierrc for any prettier rules (semicolons, quotes, etc)

  - `node --eval "fs.writeFileSync('.prettierrc','{}\n')"`

- Create `.eslintrc.js` file (You can generate with `eslint --init` if you install eslint globally)
  ```js
  module.exports = {
    root: true,
    env: { browser: true, es2020: true },
    extends: [
      "eslint:recommended",
      "plugin:react-hooks/recommended",
      "airbnb",
      "prettier",
      "plugin:node/recommended",
      "plugin:@typescript-eslint/recommended",
    ],
    ignorePatterns: ["dist", ".eslintrc.cjs"],
    parser: "@typescript-eslint/parser",
    plugins: [
      "react-refresh",
      "prettier",
      "prettier-plugin-tailwindcss", // if you installed it for tailwind
    ],
    rules: {
      "react-refresh/only-export-components": [
        "warn",
        { allowConstantExport: true },
      ],
      "prettier/prettier": "error",
      "no-unused-vars": "warn",
      "no-console": "off",
      "func-names": "off",
      "no-process-exit": "off",
      "object-shorthand": "off",
      "class-methods-use-this": "off",
      "react/jsx-filename-extension": "off",
    },
  };
  ```

### Reference

- ESLint Rules - https://eslint.org/docs/rules/
- Prettier Options - https://prettier.io/docs/en/options.html
- Airbnb Style Guide - https://github.com/airbnb/javascript

> // TODO: Add guide how to set up Git hooks

> Commit your changes

## 4. Setup Folder Structure

> This maybe looks like structure for complex projects and I recommend you using it for not so complex projects, since if your projects grows, you will be ready. <br>
> Also, using the same folder structure in multiple projects can be beneficial, since switching between multiple projects is going to be easier. <br>
> This folder structure was inspired by mostly standardize Angular structure developed by ex. SAP Software Architect, modified to fit with React.

```c
src
├── api // Folder containing Services with api calls
├── assets // Static assets (logos, images, backgrounds...)
│   └── react.svg
├── core
│   ├── guards // Guards for routes
│   ├── interceptors // Interceptors for api calls
│   ├── layout // Layout components
│   │   ├── footer
│   │   ├── header
│   │   └── sidebar
│   └── store
├── models // models/types folder for your app
├── modules // Different modules/features (pages) for your app
│   ├── home // Home module (page)
│   │   ├── api // api calls in a home page
│   │   ├── components // components shared between home pages
│   │   ├── models // models/types shared between home pages
│   │   └── routes
│   │   └── pages // different pages in home module
│   │       └── dashboard
│   │           ├── Dashboard.page.tsx
│   │           ├── Dashboard.style.css
│   │           └── Dashboard.test.ts
│   │   ├── Home.page.tsx
│   │   ├── Home.style.css
│   │   ├── Home.test.ts
│   └── settings // Same structure as for Home module (page)
│       ├── api
│       ├── components
│       ├── models
│       └── routes
│       └── pages
│       ├── Settings.page.tsx
│       ├── Settings.style.css
│       ├── Settings.test.ts
├── shared // shared components & utils in the whole app
│   ├── components
│   └── utils
├── styles // base style for the whole app
    └── index.css // Main css file with imports
├── App.tsx
├── main.tsx
├── router.ts // different routes for an app
└── vite-env.d.ts
```

> Commit your changes

## 5. Setup React Router

- Install react-router requirements
  - `npm install react-router-dom localforage match-sorter sort-by`
- Setup lazy loading router

  - Create `src/routes/router.ts`
  - Create few modules/pages
    - Create `src/modules` directory
    - Create `src/modules/home/Home.page.tsx` component
      ```tsx
      export function Home(): JSX.Element {
        return (
          <p>
            Hello from <b>Home Page!</b>
          </p>
        );
      }
      ```
    - Create `src/modules/about/About.page.tsx` component
      ```tsx
      export function About(): JSX.Element {
        return (
          <p>
            Hello from <b>About Page!</b>
          </p>
        );
      }
      ```
  - Create routers for pages

    - Create `src/modules/home/Home.router.ts` file

      ```tsx
      export const homeRouter = {
        path: "/",
        async lazy() {
          let { Home } = await import("./Home.page.tsx");
          return { Component: Home };
        },
        children: [],
      };
      ```

    - Create `src/modules/about/About.router.ts` file

      ```tsx
      export const aboutRouter = {
        path: "/about",
        async lazy() {
          let { About } = await import("./About.page.tsx");
          return { Component: About };
        },
        children: [],
      };
      ```

  - Add routers to `src/router.ts`

    ```tsx
    import { createBrowserRouter } from "react-router-dom";
    import { homeRouter } from "./modules/home/Home.router";
    import { aboutRouter } from "./modules/about/About.router";

    export const router = createBrowserRouter([homeRoutes, aboutRoutes]);
    ```

  - Add router to `src/App.tsx`

    ```tsx
    import "./styles/index.css";

    import { RouterProvider } from "react-router-dom";
    import { router } from "./routes/router";

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

> Commit your changes

## 6. Setup Redux (Redux-Toolkit)

> If you have a smaller app, that is not complex, and you want to have stateless modules (pages), you can skip this step

> [Redux Toolkit](https://redux-toolkit.js.org/) is official recommended approach for writing Redux logic and will reduce boilerplate code.

- Install Redux Toolkit required packages
  - `npm i @reduxjs/toolkit react-redux`
- Create proper folder structure

  ```
  store
  ├── home
  │ ├── home.actions.ts
  │ └── home.slice.ts
  ├── hooks.store.ts
  └── index.store.ts
  ```

  - Create `src/core/store` directory
  - Create `src/core/store/hooks.store.ts` file
    - Here you will define hooks for your components
  - Create `src/core/store/index.store.ts`
    - Here you will define app's store
  - Create `src/core/store/home` directory
    - This will represent your store for the `Home` page
    - Create `src/core/store/home/home.slice.ts` file
      - Here you will define your reducer & export actions
    - Create `src/core/store/home/home.actions.ts` file
      - Here you will write your actions, that will change the state

- Implement `src/core/store/home/home.slice.ts` file

  - Reducer and actions export
  - I recommend not copy-pasting this, but re-typing it.
  - P.S. Don't worry about an error for `_updateHomeStateAction`, that will be resolved in the next step

  ```ts
  import { createSlice } from "@reduxjs/toolkit";
  import { _updateHomeStateAction } from "./home.actions";

  export interface HomeState {
    title: string;
    userName: string;
  }

  const initialState: HomeState = {
    title: "Naslov aplikacije",
    userName: "",
  };

  const homeSlice = createSlice({
    name: "home",
    initialState,
    reducers: {
      updateHomeState: _updateHomeStateAction,
    },
  });

  export const { updateHomeState } = homeSlice.actions;

  export const homeReducer = homeSlice.reducer;
  ```

- Implement `src/core/store/home/home.actions.ts` file

  - Implementation of actions that update state

  ```ts
  import { PayloadAction } from "@reduxjs/toolkit";
  import { HomeState } from "./home.slice";

  export interface HomeStateOptions extends Partial<HomeState> {}

  export function _updateHomeStateAction(
    state: HomeState,
    action: PayloadAction<HomeStateOptions>
  ) {
    /**
     *  Here you can actually "mutate" state and not return a new state...
     *  Because React is using immer, and if something is changed it will return a new state...
     *
     * @see {@link https://redux.js.org/tutorials/typescript-quick-start#application-usage}
     */
    return {
      ...state,
      ...action.payload,
    };
  }
  ```

- Implement your store in `src/core/store/index.store.ts`

  ```ts
  import { configureStore } from "@reduxjs/toolkit";
  import { homeReducer } from "./home/home.slice";

  export const store = configureStore({
    reducer: {
      home: homeReducer,
    },
  });

  export type RootState = ReturnType<typeof store.getState>;
  export type AppDispatch = typeof store.dispatch;
  ```

- Implement your hooks in `src/core/store/hooks.store.ts`

  ```ts
  import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
  import { AppDispatch, RootState } from "./index.store";

  export const useAppDispatch = () => useDispatch<AppDispatch>();
  export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
  ```

- Provide `store` to your app

  - Wrap your `App` component with `Provider` component

    ```tsx
    import { Provider } from "react-redux";
    import { store } from "./core/store/index.store";

    ReactDOM.createRoot(document.getElementById("root")!).render(
      <React.StrictMode>
        <Provider store={store}>
          <App />
        </Provider>
      </React.StrictMode>
    );
    ```

- Test your implementation by using the hooks in components

  - Update your Home page (`src/modules/home/Home.page.tsx`) file

    - Eyes on hooks (`useAppDispatch`, `useAppSelector`) and how can you use them in your app
    - P.S. Don't worry about styles, they are here, so you can easily see what is going on when you go to your page in browser

    ```tsx
    import { updateHomeState } from "../../core/store/home/home.slice";
    import {
      useAppDispatch,
      useAppSelector,
    } from "../../core/store/hooks.store";

    export function Home(): JSX.Element {
      const dispatch = useAppDispatch();
      const userName = useAppSelector((state) => state.home.userName);

      function updateUserName(userName: string): void {
        dispatch(updateHomeState({ userName }));
      }
      return (
        <div className="flex-col p-4 border border-solid border-yellow-600 rounded">
          <h2 className="text-2xl my-2 font-medium">Home.page.tsx</h2>
          <label className="flex flex-col max-w-xs">
            <span>Guest</span>
            <input
              className="border border-solid border-gray-600 rounded p-1"
              type="text"
              placeholder="Username"
              value={userName}
              onChange={(event) => updateUserName(event.target.value)}
            />
          </label>
        </div>
      );
    }
    ```

  - Update your App component (`src/App.tsx`) file

    - Eyes on hooks (`useAppDispatch`, `useAppSelector`) and how can you use them in your app
    - P.S. Don't worry about styles, they are here, so you can easily see what is going on when you go to your page in browser

    ```tsx
    import { useAppSelector } from "./core/store/hooks.store";

    export function App(): JSX.Element {
      const userName = useAppSelector((state) => state.home.userName);
      const title = useAppSelector((state) => state.home.title);

      return (
        <div className="flex flex-col justify-start p-4 items-center border border-solid border-yellow-600 rounded">
          <h1 className="text-3xl font-medium">App.tsx</h1>
          <h2 className="text-2xl font-bold my-4">{title}</h2>
          <p className="my-1">Guest: {userName}</p>
          <RouterProvider router={router} />
        </div>
      );
    }

    export default App;
    ```

  - Go to your page and write something in your input

- If something is not working properly try

  - Re-running your app
  - Check [Redux-Toolkit documentation](https://redux-toolkit.js.org/)
  - Watch a full YT video on setting up and using [Redux-Tools](https://www.youtube.com/watch?v=9zySeP5vH9c)

- _(Recommended)_ You can install [Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd/related) extension in your browser, so you can easily debug your web app.

> Commit your changes
