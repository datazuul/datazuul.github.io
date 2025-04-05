---
layout: post
title: Dashboard Layout (sticky top nav and sidebar) with Boostrap 5 and Angular
author: Ralf Eichinger
toc: true
---

When it comes to implement a webapp with many structured functionality a dashboard layout is often a good choice.
With a sticky top navigation bar and a sticky navigation sidebar you provide a well known usability experience to the users.

Bootstrap 5 provides the needed CSS framework for the layout. In combination with Angular a dynamic SPA (Single Page Application) using this layout is coming to life.

# Bootstrap 5 dashboard layout

1. Basic structure with LayoutComponent

    The LayoutComponent includes:

        A navbar at the top (using Bootstrap .sticky-top for fixing it)

        A sidebar on the left (using Bootstrap .position-fixed for a permanently visible menu)

        A router-outlet where the dynamic content is loaded

2. Example HTML layout

```
<div class="container-fluid">
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark sticky-top">
    <!-- Navigation elements -->
  </nav>
  
  <div class="d-flex">
    <aside class="bg-light position-fixed vh-100 p-3">
      <!-- Sidebar links -->
    </aside>
    
    <main class="flex-grow-1 ms-3">
      <router-outlet></router-outlet>
    </main>
  </div>
</div>
```

3. Routing structure

    The AppComponent loads LayoutComponent, which holds the general structure.

    All subpages are defined under LayoutComponent as child routes.
    
```
const routes: Routes = [
  {
    path: '',
    component: LayoutComponent,
    children: [
      { path: 'dashboard', component: DashboardComponent },
      { path: 'settings', component: SettingsComponent }
    ]
  }
];
```

Benefits of this approach

✅ Sticky navbar: Remains at the top for better UX ✅ Sidebar stays visible: Navigation is always accessible ✅ Only content updates: Performance optimization via Angular routing

If you have any specific needs or customizations in mind, I’d be happy to help further! 😊

how to structure project's directories containing layout separate from inner components?

To structure your Angular project so that the layout is separate from inner components, a clean directory organization will help maintain clarity and scalability. Here's a suggested structure:

```
/src
  /app
    /core               # Global services, guards, interceptors
    /shared             # Reusable components, pipes, directives
    /layout             # Contains layout-related components
      /navbar           # Top navbar component
      /sidebar          # Sidebar component
      /footer           # Footer component
      layout.component.ts  # Wrapper component holding layout structure
    /features           # Contains individual feature modules
      /dashboard        # Dashboard feature module
      /settings         # Settings feature module
    /pages             # Standalone pages (e.g., login, error pages)
    /app-routing.module.ts  # Defines main routes
    /app.component.ts      # Root component
```

Key Ideas

✅ Encapsulation of Layout → layout directory houses UI elements like navigation, sidebar, and footer, keeping them separate from feature modules. ✅ Feature Modules → Each core functionality (e.g., dashboard, settings) is under features for modular development. ✅ Shared Resources → Reusable UI elements (buttons, cards, modals) go into /shared. ✅ Global Services → Authentication, API handlers, and guards live in /core.

This structure makes maintenance easier and improves scalability as your application grows. Do you have a specific use case in mind where you'd like adjustments? 😊