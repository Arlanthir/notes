# Angular

## Data binding

Make sure the App Module imports FormModule:

```typescript
import { FormsModule } from '@angular/forms';

@NgModule({
    imports: [
        FormsModule
    ]
})
export class AppModule { }
```

Add a string property to a component:

```typescript
import { Component } from '@angular/core';

@Component({
    selector: 'my',
    templateUrl: './my.component.html',
    styleUrls: ['./my.component.scss']
})
export class MyComponent {
    searchText = '';
}
```

Read and write to the property in the component's template:

```html
<input type="text" name="searchText" [(ngModel)]="searchText">
So far you have written: {{ searchText }}
```

## Event handling

Add a method to a component:

```typescript
export class MyComponent {
    search() {
    }
}
```

Bind the events in the template:

```html
<input type="text" name="searchText" [(ngModel)]="searchText" (input)="search()">
<button type="button" (click)="search()">Search</button>
```

# Angular-cli

## Usage

| Command                              | Use                                   | NPM Equivalent            |
| ------------------------------------ | ------------------------------------- | ------------------------- |
| `npm install -g @angular/cli`        | Install angular-cli (globally)        |                           |
| `ng new <projname> --style=scss`     | Create new project with SCSS styles   |                           |
| `ng set defaults.styleExt scss`      | Change project to SCSS styles         |                           |
| `ng set apps[0].prefix <prefix>`     | Change selector prefix                |                           |
| `ng g component my-component`        | Generate and include new component    |                           |
| `ng lint`                            | Lint the code                         | `npm run lint`            |
| `ng serve -o`                        | Code with livereload and open browser | `npm start -- -o`         |
| `ng build --prod`                    | Deploy for production                 | `npm run build -- --prod` |
| `ng test`                            | Run unit tests (karma)                | `npm test`                |
| `ng e2e`                             | Run end-to-end tests (protractor)     | `npm run e2e`             |


## Add typechecking and sass-lint to linting

Edit `package.json` scripts' lint to:
`"lint": "ng lint --type-check && sass-lint 'src/**/*.scss' -v -q"`

And use `npm run lint` instead of `ng lint`.

## Add global styles or scripts

Add the path to the files in the `apps[0].scripts` or `apps[0].styles` properties of `.angular-cli.json`.
