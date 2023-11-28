# How to Setup Angular App

> Angular, Angular Material, Angular Router + Lazy Loading

## Setup base of the project

- `	ng new <name>`
  - select: `SCSS`
  - select: Router: `Yes`

## Setup style (Angular Material)

- `ng add @angular/material`

  - The package @angular/material@16.2.1 will be installed and executed.
    Would you like to proceed?
    - select: `Yes`
  - Choose a prebuilt theme name, or "custom" for a custom theme
    - select: `Indigo/Pink` // "Default Angular Material App Theme"
  - Set up global Angular Material typography styles?
    - select: `Yes`
  - Include the Angular animations module?
    - select: `Include and enable animations`

> Commit your changes

## Add Pages

- `ng g m modules/<page-name> --route <page-name> --module app.module`

> Commit your changes

## Update App Component

Delete all content and setup router outlet in `app.component.html`

```html
<router-outlet></router-outlet>
```

Now you can check if your app is working by running `ng serve` and opening `http://localhost:4200/` and going to routes you created. <br />
P.S. You can see auto-generated routes in `app-routing.module.ts`

## Improve your Router

```ts
const routes: Routes = [
  { path: "", redirectTo: "home", pathMatch: "full" },
  // Your other routes
];
```

> Commit your changes

Your all done! Happy coding!
