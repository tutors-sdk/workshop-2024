Tutors Mono Repo Architecture

An illustrated guide to the Tutors monorepo architecture, rendered as Mermaid diagrams.

[[toc]]

## Layered Architecture

The monorepo follows a layered architecture with clear dependency boundaries. Lower layers never depend on higher layers.

```mermaid
block-beta
  columns 1
  block:apps["Applications Layer"]
    reader["Reader"]
    catalogue["Catalogue"]
    live["Live"]
  end
  block:svelte["Svelte Packages Layer"]
    ui["UI Components"]
    themes["Themes"]
    community["Community"]
    connect["Connect"]
    course["Course"]
    runes["Runes"]
  end
  block:jsr["JSR Packages Layer"]
    model["Model"]
    time["Time"]
    gen["Gen"]
    tutors["Tutors CLI"]
  end

  apps --> svelte
  svelte --> jsr
```

## Package Structure

The directory layout of the monorepo, showing the four subsystems: JSR packages, Svelte packages, Applications, and Services.

```mermaid
graph TD
  root["tutors-mono-repo"]

  root --> packages
  root --> apps
  root --> services

  packages --> jsr
  packages --> svelte_pkg["svelte"]

  jsr --> model["model<br/><i>@tutors/tutors-model-lib</i>"]
  jsr --> time["time<br/><i>@tutors/tutors-time-lib</i>"]
  jsr --> gen["gen<br/><i>@tutors/tutors-gen-lib</i>"]
  jsr --> tutors_cli["tutors<br/><i>@tutors/reader CLI</i>"]

  svelte_pkg --> runes["runes<br/><i>@tutors/runes</i>"]
  svelte_pkg --> course["course<br/><i>@tutors/course</i>"]
  svelte_pkg --> themes["themes<br/><i>@tutors/themes</i>"]
  svelte_pkg --> community["community<br/><i>@tutors/community</i>"]
  svelte_pkg --> connect["connect<br/><i>@tutors/connect</i>"]
  svelte_pkg --> ui["ui<br/><i>@tutors/ui</i>"]
  svelte_pkg --> utils
  utils --> logger["logger"]
  utils --> a11y["a11y"]
  utils --> i18n["i18n"]

  apps --> reader_app["reader"]
  apps --> catalogue_app["catalogue"]
  apps --> live_app["live"]

  services --> party["party<br/><i>PartyKit</i>"]
```

## JSR Subsystem

The JSR packages form the foundation layer — core data models, course generation tools, and CLI utilities that work across both Deno and Node.js runtimes.

```mermaid
graph LR
  subgraph JSR["JSR Subsystem (Deno-First, Node-Compatible)"]
    model["Model<br/>Core types & data structures"]
    time["Time<br/>Analytics & tracking"]
    gen["Gen<br/>Course generation"]
    tutors["Tutors CLI<br/>Entry point"]
  end

  tutors --> gen
  gen --> model
  time --> model

  jsr_registry[("jsr.io/@tutors/*")]
  model -.-> jsr_registry
  time -.-> jsr_registry
  gen -.-> jsr_registry
  tutors -.-> jsr_registry
```

## Content Generation Pipeline

How content authored as markdown is transformed into a deployable course, consumed by the Reader application.

```mermaid
flowchart TD
  author["Author Creates Content<br/>Markdown, YAML, Assets, Folder Structure"]

  author --> cli["tutors-cli<br/><i>deno run -A jsr:@tutors/reader</i>"]

  cli --> builder["Course Builder<br/>Scan folders & identify Lo types"]
  builder --> resource["Resource Builder<br/>Parse markdown frontmatter & content"]
  resource --> emitter{"Course Emitter"}

  emitter -->|JSON Mode| json["json/tutors.json<br/>+ markdown files"]
  emitter -->|HTML Mode| html["html/ folder<br/>Self-contained static site"]

  json --> cdn["Deploy to CDN<br/>Netlify / Vercel / GitHub Pages"]
  html --> cdn

  cdn --> reader_app["Reader Application<br/>Fetch & render course"]
```

## Svelte Package Ecosystem

The Svelte packages are organised in layers, with each layer building on the one below.

```mermaid
graph BT
  subgraph "Layer 0 — Foundation"
    model_lib["@tutors/tutors-model-lib"]
    time_lib["@tutors/tutors-time-lib"]
    logger["@tutors/logger"]
  end

  subgraph "Layer 1 — State"
    runes["@tutors/runes"]
  end

  subgraph "Layer 2 — Core"
    course["@tutors/course"]
    a11y["@tutors/a11y"]
    i18n["@tutors/i18n"]
  end

  subgraph "Layer 3 — Features"
    themes["@tutors/themes"]
    community["@tutors/community"]
  end

  subgraph "Layer 4 — Auth"
    connect["@tutors/connect"]
  end

  subgraph "Layer 5 — UI"
    ui["@tutors/ui"]
  end

  runes --> model_lib
  course --> runes
  course --> logger
  course --> model_lib
  a11y --> runes
  a11y --> course
  i18n --> runes
  i18n --> course
  themes --> course
  themes --> runes
  themes --> logger
  community --> course
  community --> runes
  community --> logger
  connect --> community
  connect --> course
  connect --> runes
  ui --> connect
  ui --> themes
  ui --> community
  ui --> i18n
  ui --> a11y
  ui --> model_lib
```

## Dependency Graph

Full package dependency hierarchy from applications down to the foundation layer.

```mermaid
graph TD
  subgraph Applications
    reader["reader"]
    catalogue["catalogue"]
    live["live"]
  end

  ui["@tutors/ui"]
  connect["@tutors/connect"]
  community["@tutors/community"]
  themes["@tutors/themes"]
  i18n["@tutors/i18n"]
  a11y["@tutors/a11y"]
  course["@tutors/course"]
  runes["@tutors/runes"]
  logger["@tutors/logger"]
  model["@tutors/tutors-model-lib"]
  time["@tutors/tutors-time-lib"]

  reader --> ui
  catalogue --> ui
  live --> ui
  reader --> time
  catalogue --> time
  live --> time

  ui --> connect
  ui --> themes
  ui --> community
  ui --> i18n
  ui --> a11y
  ui --> model

  connect --> community
  connect --> course
  connect --> runes

  community --> course
  community --> runes
  community --> logger

  themes --> course
  themes --> runes
  themes --> logger

  i18n --> runes
  i18n --> course

  a11y --> runes
  a11y --> course

  course --> runes
  course --> logger
  course --> model

  runes --> model
```

## Reader Application Flow

The sequence of events when a user navigates to a course in the Reader application.

```mermaid
sequenceDiagram
  actor User
  participant Browser
  participant SvelteKit as SvelteKit Router
  participant CourseService as courseService
  participant CDN as CDN (Static JSON)
  participant Runes as Runes (State)
  participant UI as UI Components

  User->>Browser: Navigate to /course/[id]
  Browser->>SvelteKit: Route match
  SvelteKit->>CourseService: readCourse(id, fetch)
  CourseService->>CDN: GET tutors.json
  CDN-->>CourseService: Course JSON
  CourseService->>CourseService: Decorate tree (lo-tree)
  CourseService->>CourseService: Cache in courses Map
  CourseService->>Runes: currentCourse.value = course
  Runes-->>UI: Reactive update ($effect)
  UI-->>Browser: Render cards & panels
  Browser-->>User: Course displayed
```

## Lab Viewing Sequence

How a lab is loaded, rendered step-by-step, and navigated by the learner.

```mermaid
sequenceDiagram
  actor User
  participant UI as Lab Component
  participant CourseService as courseService
  participant LiveLab
  participant Markdown as markdownService
  participant Runes

  User->>UI: Click lab card
  UI->>CourseService: readLab(course, labId, fetch)
  CourseService->>LiveLab: Create instance
  LiveLab->>LiveLab: Fetch step markdown files
  LiveLab->>Markdown: renderMarkdown(content)
  Markdown-->>LiveLab: HTML output
  CourseService->>CourseService: Cache in labs Map
  CourseService->>Runes: currentLo.value = lab
  Runes-->>UI: Reactive update
  UI-->>User: Display step 1

  User->>UI: Click next step
  UI->>LiveLab: loadStep(stepIndex)
  LiveLab->>Runes: currentLabStepIndex.value = n
  Runes-->>UI: Reactive update
  UI-->>User: Display step n
```

## Data Sources & State Management

The Reader application draws data from four sources: static JSON on a CDN, Supabase for analytics, PartyKit for real-time presence, and Auth.js for authentication.

```mermaid
flowchart LR
  subgraph Sources
    cdn[("CDN<br/>tutors.json<br/>Markdown, PDFs, Images")]
    supabase[("Supabase<br/>Learning events<br/>User sessions<br/>Analytics")]
    partykit[("PartyKit<br/>Real-time presence<br/>Live student count")]
    auth[("Auth.js<br/>GitHub OAuth<br/>Session tokens")]
  end

  subgraph Services
    cs["courseService"]
    as["analyticsService"]
    ps["presenceService"]
    co["connectService"]
  end

  subgraph State
    runes["Runes<br/>currentCourse<br/>currentLo<br/>currentLabStepIndex"]
  end

  subgraph UI
    components["Svelte Components<br/>$effect() reactive rendering"]
  end

  cdn --> cs
  supabase --> as
  partykit --> ps
  auth --> co

  cs --> runes
  as --> runes
  ps --> runes
  co --> runes

  runes --> components
```

## PartyKit Real-Time Service

Room-based WebSocket broadcasting — one room per course — enabling live student presence across the platform.

```mermaid
flowchart TD
  subgraph Clients
    b1["Browser 1"]
    b2["Browser 2"]
    b3["Browser 3"]
  end

  subgraph PartyKit["PartyKit Server"]
    server["TutorsServer"]
    subgraph Rooms
      r1["Room: course-a"]
      r2["Room: course-b"]
    end
  end

  b1 <-->|WebSocket| r1
  b2 <-->|WebSocket| r1
  b3 <-->|WebSocket| r2

  r1 --> server
  r2 --> server

  server -->|"onConnect / onMessage / onClose"| r1
  server -->|"onConnect / onMessage / onClose"| r2
```

## UI Component Architecture

The UI package organises components into four categories: generic components, learning object components (content, layout, structure), navigators, and time/analytics views.

```mermaid
graph TD
  shell["TutorsShell.svelte"]

  shell --> components["Generic Components<br/>Icon, Image, Menu, Sidebar"]
  shell --> nav["Navigators"]
  shell --> lo["Learning Objects"]
  shell --> time_comp["Time Components"]

  nav --> buttons["Buttons<br/>Breadcrumbs, Search, ToC"]
  nav --> footers["Footers"]
  nav --> titles["Titles<br/>CourseTitle, TutorsTitle"]
  nav --> tutors_connect["Tutors Connect<br/>AnonProfile, ConnectedProfile"]
  nav --> layout_nav["Layout<br/>ThemeSwitcher, LanguageSwitcher"]

  lo --> content["Content<br/>Lab, Notebook, Talk<br/>Video, Calendar, Note"]
  lo --> layout_lo["Layout<br/>Cards, Panels, Wall, Units"]
  lo --> structure["Structure<br/>Context, LoContext<br/>CourseContext"]

  time_comp --> catalogue_view["Catalogue"]
  time_comp --> students["Students"]
  time_comp --> courses["Courses"]
```

## Technology Stack

The major technologies used across the platform.

```mermaid
mindmap
  root((Tutors Platform))
    Build & Package
      pnpm 8+
      Vite 8
      TypeScript 5
    Runtimes
      Deno
      Node.js 18+
      Browser
    Frontend
      Svelte 5
      SvelteKit 2
      Tailwind CSS 4
      Skeleton UI 5
    Content
      markdown-it
      Shiki
      Marp
      Mermaid
      PDF.js
    Backend
      Supabase
      PartyKit
      Auth.js
