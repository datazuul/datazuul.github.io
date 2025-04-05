create an angular dashboard with navbar and sidebar using Nesting routes for layout

### 1. **Create a New Standalone App**
   Start by creating a new Angular project:
   ```bash
   ng new angular-dashboard --standalone
   cd angular-dashboard
   ng serve
   ```

### 2. **Create Required Components**
   Generate standalone components for layout, navbar, sidebar, and content sections:
   ```bash
   ng generate component layout --standalone
   ng generate component navbar --standalone
   ng generate component sidebar --standalone
   ng generate component dashboard --standalone
   ng generate component section1 --standalone
   ng generate component section2 --standalone
   ```

### 3. **Set Up Routes**
   Define the routes in `app.routes.ts`, specifying the nested routes:
   ```typescript
   import { Routes } from '@angular/router';
   import { LayoutComponent } from './layout/layout.component';
   import { DashboardComponent } from './dashboard/dashboard.component';
   import { Section1Component } from './section1/section1.component';
   import { Section2Component } from './section2/section2.component';

   export const routes: Routes = [
     {
       path: '',
       component: LayoutComponent,
       children: [
         { path: '', redirectTo: 'dashboard', pathMatch: 'full' },
         { path: 'dashboard', component: DashboardComponent },
         { path: 'section1', component: Section1Component },
         { path: 'section2', component: Section2Component },
       ],
     },
   ];
   ```

### 4. **Update the Layout Component**
   In the `layout.component.html`, arrange the navbar, sidebar, and router outlet:
   ```html
   <app-navbar></app-navbar>
   <div class="container">
     <app-sidebar></app-sidebar>
     <div class="content">
       <router-outlet></router-outlet>
     </div>
   </div>
   ```

   Add styles in `layout.component.css`:
   ```css
   .container {
     display: flex;
     height: 100vh;
   }
   .content {
     flex: 1;
     padding: 20px;
     overflow-y: auto;
   }
   ```

### 5. **Standalone Component Configuration**
   Ensure each standalone component is properly configured. For example:
   ```typescript
   import { Component } from '@angular/core';

   @Component({
     selector: 'app-navbar',
     standalone: true,
     template: `<nav><h1>Dashboard</h1></nav>`,
     styles: [`nav { padding: 1rem; background-color: #007bff; color: white; }`],
   })
   export class NavbarComponent {}
   ```

   Similarly, configure the sidebar with links:
   ```typescript
   import { Component } from '@angular/core';
   import { RouterModule } from '@angular/router';

   @Component({
     selector: 'app-sidebar',
     standalone: true,
     imports: [RouterModule],
     template: `
       <ul>
         <li><a routerLink="/dashboard">Dashboard</a></li>
         <li><a routerLink="/section1">Section 1</a></li>
         <li><a routerLink="/section2">Section 2</a></li>
       </ul>
     `,
     styles: [`ul { list-style-type: none; padding: 0; } li { margin: 10px 0; }`],
   })
   export class SidebarComponent {}
   ```

### 6. **Bootstrap the Application**
   Update the main file, `main.ts`, to bootstrap the application:
   ```typescript
   import { bootstrapApplication } from '@angular/platform-browser';
   import { AppComponent } from './app/app.component';
   import { provideRouter } from '@angular/router';
   import { routes } from './app.routes';

   bootstrapApplication(AppComponent, {
     providers: [provideRouter(routes)],
   }).catch(err => console.error(err));
   ```

### 7. **Test Your Application**
   Run the app:
   ```bash
   ng serve
   ```
   Verify the layout and navigation between nested routes.

This approach leverages standalone components and makes the application modular and scalable. Let me know if you'd like further customization!