# How to Setup React App

> React + SWC, TypeScript, TailwindCSS, ESlint + Prettier + Airbnb Setup, React Router + lazy loading, Redux, i18next

**// Content**

0. Pre-requirements
1. Setup base of the project (React + SWX, TypeScript)
2. Setup style (TailwindCSS)
3. VSCode - ESLint, Prettier & Airbnb Setup
4. Folder Structure
5. Setup React Router
6. Setup Redux (Redux-Toolkit)
7. Setup daisyUI
8. Setup i18next - Multi-language support / Internationalization
9. Setup path aliases
10. Setup PWA

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
- Setup typescript checking

  - Install Vite Plugin Checker
    - `npm install -D vite-plugin-checker`
    - [More Info](https://vite-plugin-checker.netlify.app/introduction/introduction.html)
  - Update `vite.config.ts`

    ```ts
    import { defineConfig } from "vite";
    import react from "@vitejs/plugin-react-swc";
    import checker from "vite-plugin-checker";

    // https://vitejs.dev/config/
    export default defineConfig({
      plugins: [
        react(),
        checker({
          typescript: true,
        }),
      ],
    });
    ```

> Commit your changes

## 2. Setup style (TailwindCSS) // _Commit previous changes_

- Follow tailwindcss setup guide (skip the step 1 since we already created app with vite)
  - https://tailwindcss.com/docs/guides/vite
- _(Recommended)_ If you are using VSCode, install Tailwind CSS IntelliSense extension
  - https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss

> Commit your changes

## 3. VSCode - ESLint, Prettier & Airbnb Setup // _Optional - But Highly Recommended_ // _Commit previous changes_

- Install ESLint & Prettier extensions for VSCode

  > Optional - Set format on save and any global prettier options

  - [Prettier Extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
  - [ESLint Extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

- Install Packages

  - `npm i -D eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-node eslint-config-node`
  - `npx install-peerdeps --dev eslint-config-airbnb`
  - _(Recommended for Tailwind)_ `npm install -D prettier prettier-plugin-tailwindcss`
    - [prettier-plugin-tailwindcss](https://www.npmjs.com/package/prettier-plugin-tailwindcss) automatically sorts classes based on recommended class order.

- Create .prettierrc for any prettier rules (semicolons, quotes, etc)

  - `node --eval "fs.writeFileSync('.prettierrc','{}\n')"`

- Create `.eslintrc.js`
  - edit
    ```js
    module.exports = {
      root: true,
      env: { browser: true, es2020: true },
      extends: [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:react-hooks/recommended",
        "airbnb",
        "prettier",
        "plugin:node/recommended",
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

## 4. Setup Folder Structure // _Commit previous changes_

> This maybe looks like structure for complex projects and I recommend you using it for not so complex projects, since if your projects grows, you will be ready. <br>
> Also, using the same folder structure in multiple projects can be beneficial, since switching between multiple projects is going to be easier. <br>
> This folder structure was inspired by mostly standardize Angular structure developed by ex. SAP Software Architect, modified to fit with React.

```c
public
└── locales // Keeps your `translation` files separated in folders
    ├── en // English language
    └── sr // Serbian language
        └── translation.ts
src
├── api // Folder containing Services with api calls
├── assets // Static assets (logos, images, backgrounds...)
│   └── react.svg
├── core
│   ├── guards // Guards for routes
│   ├── interceptors // Interceptors for api calls
│   └── layout // Layout components
│       ├── footer
│       ├── header
│       └── sidebar
├── localization // Localization
│   └── i18next.local.ts // Init your localization here
├── types // types folder for your app
├── modules // Different modules/features (pages) for your app
│   ├── home // Home module (page)
│   │   ├── api // api calls in a home page
│   │   ├── components // components shared between home pages
│   │   ├── types // types shared between home pages
│   │   ├── store // keeps and manages modules state
│   │   ├── locals // translation files
│   │   │   └── en
│   │   │   │   └── translation.ts
│   │   │   └── sr
│   │   │       └── translation.ts
│   │   ├── pages // different pages in home module
│   │   │   └── dashboard
│   │   │       ├── Dashboard.page.tsx
│   │   │       ├── Dashboard.style.css
│   │   │       └── Dashboard.test.ts
│   │   ├── Home.page.tsx
│   │   ├── Home.style.css
│   │   ├── Home.test.ts
│   │   └── Home.router.tsx
│   └── settings // Same structure as for Home module (page)
│       ├── api
│       ├── components
│       ├── types
│       ├── store // keeps and manages modules state
│       ├── pages
│       ├── Settings.page.tsx
│       ├── Settings.style.css
│       ├── Settings.test.ts
│       └── Settings.router.tsx
├── shared // shared components & utils in the whole app
│   ├── components
│   └── utils
├── store // If you are adding some state management
├── styles // base style for the whole app
│   └── index.css // Main css file with imports
├── App.tsx
├── App.css
├── main.tsx
├── router.tsx // different routes for an app
└── vite-env.d.ts
```

> Commit your changes

## 5. Setup React Router // _Commit previous changes_

// TODO: Update this so it is correct!!!

- Install react-router requirements
  - `npm install react-router-dom localforage match-sorter sort-by`
- Setup lazy loading router

  - Create `src/router.ts`
  - Add basic code to your pages so you can check if routing is working
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

    export const router = createBrowserRouter([
      {
        path: "/",
        element: <Outlet />, // or Some custom Layout that contains <Outlet />
        children: [homeRouter, aboutRouter],
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

## 6. Setup Redux (Redux-Toolkit) // _Commit previous changes_

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

  - Create `src/store` directory
  - Create `src/store/hooks.store.ts` file
    - Here you will define hooks for your components
  - Create `src/store/index.store.ts`
    - Here you will define app's store
  - Create `src/store/home` directory
    - This will represent your store for the `Home` page
    - Create `src/modules/home/store/homeSlice.store.ts` file
      - Here you will define your reducer & export actions
    - Create `src/modules/home/store/homeAction.store.ts` file
      - Here you will write your actions, that will change the state

- Implement `src/modules/home/store/homeSlice.store.ts` file

  - Reducer and actions export
  - I recommend not copy-pasting this, but re-typing it.
  - P.S. Don't worry about an error for `_updateHomeStateAction`, that will be resolved in the next step

  ```ts
  import { createSlice } from "@reduxjs/toolkit";
  import { _updateHomeStateAction } from "./homeActions.store.ts";

  export interface HomeState {
    title: string;
    username: string;
  }

  const initialState: HomeState = {
    title: "Naslov aplikacije",
    username: "",
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

- Implement `src/modules/home/store/homeActions.store.ts` file

  - Implementation of actions that update state

  ```ts
  import { PayloadAction } from "@reduxjs/toolkit";
  import { HomeState } from "./homeSlice.store";

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

- Implement your store in `src/store/index.store.ts`

  ```ts
  import { configureStore } from "@reduxjs/toolkit";
  import { homeReducer } from "../modules/home/store/homeSlice.store";

  export const store = configureStore({
    reducer: {
      home: homeReducer,
    },
  });

  export type RootState = ReturnType<typeof store.getState>;
  export type AppDispatch = typeof store.dispatch;
  ```

- Implement your hooks in `src/store/hooks.store.ts`

  ```ts
  import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
  import { AppDispatch, RootState } from "./index.store";

  export const useAppDispatch = () => useDispatch<AppDispatch>();
  export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
  ```

- Provide `store` to your app

  - Wrap your `App` component with `Provider` component

    ```tsx
    // Other imports
    import { store } from "./store/index.store.ts";
    import { Provider } from "react-redux";

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
      const username = useAppSelector((state) => state.home.username);

      function updateUserName(username: string): void {
        dispatch(updateHomeState({ username }));
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
              value={username}
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
      const username = useAppSelector((state) => state.home.username);
      const title = useAppSelector((state) => state.home.title);

      return (
        <div className="flex flex-col justify-start p-4 items-center border border-solid border-yellow-600 rounded">
          <h1 className="text-3xl font-medium">App.tsx</h1>
          <h2 className="text-2xl font-bold my-4">{title}</h2>
          <p className="my-1">Guest: {username}</p>
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

## 7. Setup daisyUI // _Commit previous changes_

> The most popular component library component library for Tailwind CSS

[More Info](https://daisyui.com/)

- Install daisyUI
  - `npm i -D daisyui@latest`
- Then add daisyUI to your `tailwind.config.js` files:
  ```js
  module.exports = {
    //...
    plugins: [require("daisyui")],
  };
  ```

> Commit your changes

## 8. Setup i18next - Multi-language support / Internationalization // _Commit previous changes_

> i18next is an internationalization-framework written in and for JavaScript. But it's much more than that!

[More Info](https://react.i18next.com)`

- Install dependencies

  - `npm install i18next react-i18next i18next-browser-languagedetector i18next-resources-to-backend vite-plugin-checker`

  - Why so many dependencies?
    - `i18next` - core library
    - `react-i18next` - react friendly
    - `i18next-browser-languagedetector` - helpful for detecting chosen langue
    - `i18next-resources-to-backend` - will enable us to lazy-load translation resources for every module and language
    - `vite-plugin-checker` - will help us set type checking for translation resources, so every languages has the same props filled out.

- Create `src/localization` folder

- Init Localization

  - Create `i18next.local.ts` and add this code, setup your `fallbackLng` and have a look at configuration.

    ```ts
    import i18n from "i18next";
    import { initReactI18next } from "react-i18next";

    import LanguageDetector from "i18next-browser-languagedetector";
    import resourcesToBackend from "i18next-resources-to-backend";

    const lazyLoadRecourse = resourcesToBackend(
      async (language: string, namespace: string) => {
        return await import(
          `../../public/locales/${language}/${namespace || "translation"}.ts`
        );
      }
    );

    i18n
      // learn more: https://github.com/i18next/i18next-browser-languageDetector
      .use(LanguageDetector)
      .use(lazyLoadRecourse)
      .use(initReactI18next)
      // init i18next
      // for all options read: https://www.i18next.com/overview/configuration-options
      .init({
        partialBundledLanguages: true,
        fallbackLng: "sr",
        debug: true,
        interpolation: {
          escapeValue: false, // not needed for react as it escapes by default
        },
        react: {
          bindI18nStore: "added",
        },
      });

    export default i18n;
    ```

  - Import localization in `main.ts` (_Required_)
    // And remove strict mode (_Recommended_)

    ```ts
    //...
    import "./localization/i18next.local.ts";
    //...

    ReactDOM.createRoot(document.getElementById("root")!).render(
      <Provider store={store}>
        <App />
      </Provider>
    );
    ```

- Add Recourses (translation files)

  - Setup general(App) recourses

    - Create `public/locales` folder

    - Create App's Recourse type `src/types/locales.type.ts`

      We are creating types, so if someone adds something to one language recourse, error will appear in other files. So, this way we ensure that all recourses are up-to-date.

      ```ts
      export interface AppRecourse {
        title: string;
        description: {
          part1: string;
          part2: string;
        };
        uncommon: {
          guest: string;
        };
      }
      ```

    - Create multiple App translation files

      - Create `public/locales/en/translation.ts`

        ```ts
        import { AppRecourse } from "../../../src/types/locales.type";

        const enRecourse: AppRecourse = {
          title: "Welcome Trainerr",
          description: {
            part1: "The best Trainerr app!",
            part2:
              "Switch language between english and german using buttons above.",
          },
          uncommon: {
            guest: "Guest",
          },
        };

        export default enRecourse;
        ```

      - Create `public/locales/sr/translation.ts`

        ```ts
        import { AppRecourse } from "../../../src/types/locales.type";

        const srRecourse: AppRecourse = {
          title: "Dobrodosao Trenerru",
          description: {
            part1: "Najdobarata aplikacija za Trenerri",
            part2:
              "Switch language between english and german using buttons above.",
          },
          uncommon: {
            guest: "Gost",
          },
        };

        export default srRecourse;
        ```

  - Create local(Module's) recourses

    - Create `src/modules/home/types/HomeLocal.type.ts`
      ```ts
      export interface HomeLocal {
        title: string;
        usernameLabel: string;
        langButtonEng: string;
        langButtonSrb: string;
        langButton: {
          en: string;
          sr: string;
        };
      }
      ```
    - Create `src/modules/home/locales`

      - Create `src/modules/home/locales/en/translation.ts`

        ```ts
        import { HomeLocal } from "../../types/HomeLocal.type";

        const enRecourse: HomeLocal = {
          title: "Home.page.tsx",
          usernameLabel: "Trainerr",
          langButtonEng: "English",
          langButtonSrb: "Serbian",
          langButton: {
            en: "English",
            sr: "Serbian",
          },
        };

        export default enRecourse;
        ```

      - Create `src/modules/home/locales/sr/translation.ts`

        ```ts
        import { HomeLocal } from "../../types/HomeLocal.type";

        const srRecourse: HomeLocal = {
          title: "Pocetna.page.tsx",
          usernameLabel: "Trenerr",
          langButtonEng: "Engleski",
          langButtonSrb: "Srpski",
          langButton: {
            en: "Engleski",
            sr: "Srpski",
          },
        };

        export default srRecourse;
        ```

- Create hook for loading recourses `src/shared/hooks/lazyLoadRecourse.hook.ts`

  ```ts
  import { useEffect } from "react";

  import i18n from "../../localization/i18next.local";

  export async function lazyLoadRecourse({
    folderName,
    namespace,
  }: {
    folderName: string;
    namespace: string;
  }) {
    const module = await import(
      `../../modules/${folderName}/locales/${i18n.language}/translation.ts`
    );
    const resource = module.default;

    i18n.addResourceBundle(i18n.language, namespace, resource, true);
  }

  /**
   * Lazy Loads recourse
   * @param folderName - Folder name containing locales folder // eg. Home
   * @param namespace - Use this to access translation for this module
   */
  export function lazyLoadRecourseHook({
    folderName,
    namespace,
  }: {
    folderName: string;
    namespace: string;
  }) {
    useEffect(() => {
      // Load Recourses first time component is loaded
      (async () => {
        await lazyLoadRecourse({ folderName, namespace });
      })();

      // Wrap the function so it can be passed by reference with existing params
      function wrappedLazyLoadRecourse() {
        return lazyLoadRecourse({ folderName, namespace });
      }

      // Lazy load resources on language change
      i18n.on("languageChanged", wrappedLazyLoadRecourse);

      return () => {
        // Cleanup listener for wrapped function
        i18n.off("languageChanged", wrappedLazyLoadRecourse);
      };
    }, []);
  }
  ```

- Load recourses in your module & show translations

  - Go to: `src/modules/home/Home.page.tsx`

    ```tsx
    // Imports...

    export function Home(): JSX.Element {
      // Load Recourses
      lazyLoadRecourseHook({ folderName: "Home", namespace: "home" });
      const { t, i18n } = useTranslation();

      // Add functions for changing languages
      function setEnglish() {
        i18n.changeLanguage("en");
      }

      function setSerbian() {
        i18n.changeLanguage("sr");
      }

      // Use Recourses in template
      return (
        <div className="flex flex-col items-center gap-4 p-4 border border-solid border-yellow-600 rounded">
          {
            {
              /** Using "local" (modules) translation with "home" `namespace` */
            }
          }
          <h2 className="text-2xl my-2 font-medium">{t("home:title")}</h2>
          <label className="flex flex-col max-w-xs">
            {
              {
                /** Using 'global (apps)' translation without `namespace` */
              }
            }
            <span>{t("uncommon.guest")}</span>
            <input
              className="input input-bordered w-full max-w-xs"
              type="text"
              placeholder="Username"
              value={username}
              onChange={(event) => updateUserName(event.target.value)}
            />
          </label>
          <div className="join">
            <button
              className="btn btn-neutral join-item"
              onClick={() => setEnglish()}
            >
              {t("home:langButton.en")}
            </button>
            <button
              className="btn btn-neutral join-item"
              onClick={() => setSerbian()}
            >
              {t("home:langButton.sr")}
            </button>
          </div>
        </div>
      );
    }
    ```

- ## Final touches

> Commit your changes
