# How to Setup React App

> React + SWC, TypeScript, Tailwind, React Router + lazy loading, Redux

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

- Follow tailwindcss setup guide (skip the step 1 since we already created app with vite)
  - https://tailwindcss.com/docs/guides/vite
- _(Recommended)_ If you are using VSCode, install Tailwind CSS IntelliSense extension
  - https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss

> Commit your changes

## Setup React Router

- Install react-router requirements
  - `npm install react-router-dom localforage match-sorter sort-by`
- Setup lazy loading router

  - Create `src/routes/router.ts`
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
  - Create routes for pages

    - Create `src/routes/Home.routes.ts` file

      ```tsx
      export const homeRoutes = {
        path: "/",
        async lazy() {
          let { Home } = await import("../pages/Home/Home.page.tsx");
          return { Component: Home };
        },
        children: [],
      };
      ```

    - Create `src/routes/About.routes.ts` file

      ```tsx
      export const aboutRoutes = {
        path: "/about",
        async lazy() {
          let { About } = await import("../pages/About/About.page.tsx");
          return { Component: About };
        },
        children: [],
      };
      ```

  - Add routes to `src/router.ts`

    ```tsx
    import { createBrowserRouter } from "react-router-dom";
    import { homeRoutes } from "./Home.routes.ts";
    import { aboutRoutes } from "./About.routes.ts";

    export const router = createBrowserRouter([homeRoutes, aboutRoutes]);
    ```

  - Add router to `src/App.tsx`

    ```tsx
    import "./App.css";
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

## Setup Redux (Redux-Toolkit)

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
    - Create `src/store/home/home.slice.ts` file
      - Here you will define your reducer & export actions
    - Create `src/store/home/home.actions.ts` file
      - Here you will write your actions, that will change the state

- Implement `src/store/home/home.slice.ts` file

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

- Implement `src/store/home/home.actions.ts` file

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

- Implement your store in `src/store/index.store.ts`

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
    import { Provider } from "react-redux";
    import { store } from "./store/index.store";

    ReactDOM.createRoot(document.getElementById("root")!).render(
      <React.StrictMode>
        <Provider store={store}>
          <App />
        </Provider>
      </React.StrictMode>
    );
    ```

- Test your implementation by using the hooks in components

  - Update your Home page (`src/pages/home.page.tsx`) file

    - Eyes on hooks (`useAppDispatch`, `useAppSelector`) and how can you use them in your app
    - P.S. Don't worry about styles, they are here, so you can easily see what is going on when you go to your page in browser

    ```tsx
    import { updateHomeState } from "../../store/home/home.slice";
    import { useAppDispatch, useAppSelector } from "../../store/hooks.store";

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
    import { useAppSelector } from "./store/hooks.store";

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
  - Checking [Redux-Toolkit documentation](https://redux-toolkit.js.org/)
  - Watch a full YT video on setting up and using [Redux-Tools](https://www.youtube.com/watch?v=9zySeP5vH9c)

- _(Recommended)_ You can install [Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd/related) extension in your browser, so you can easily debug your web app.

> Commit your changes
