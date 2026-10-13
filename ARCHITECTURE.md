# Architecture Overview

This diagram shows a high-level architecture for the "Just The Wright Interiors" project and how the delivered ZIP, SPA assets, and static hosting fit together.

```mermaid
flowchart LR
  subgraph Repo[GitHub Repository - jorwrght-png/JTWI2]
    RepoZip["stitch_just_the_wright_interiors.zip\n(artifact in repo)"]
    SrcFiles["src / static files\nunified_spa_index.html, services_packages/"]
  end

  Developer["Developer\n(local dev)"]
  CI["CI / Build (optional)"]
  StaticHost["Static Host / CDN\n(e.g. GitHub Pages, Netlify)"]
  Browser["User Browser / Client"]

  Repo --> RepoZip
  Repo --> SrcFiles
  Developer --> Repo
  Developer --> CI
  CI --> StaticHost
  SrcFiles --> StaticHost
  RepoZip --> StaticHost

  subgraph Client[SPA Client]
    SPAIndex["unified_spa_index.html\n(client-side routing)"]
    Router["Client Router"]
    Services["services_packages/\n(code.html, images, etc.)"]
  end

  StaticHost --> Browser
  Browser -->|requests| SPAIndex
  SPAIndex --> Router
  Router --> Services
  Browser -->|fetch assets| Services

  note right of Services: "Package assets (HTML fragments, images) are loaded
on demand by the SPA. The ZIP contains a distributable snapshot."

  style RepoZip fill:#f9f,stroke:#333,stroke-width:1px
  style StaticHost fill:#efe,stroke:#333,stroke-width:1px
  style Browser fill:#eef,stroke:#333,stroke-width:1px
```

Summary

- The repository contains a stitched ZIP artifact and a SPA-style set of static files.
- The SPA (unified_spa_index.html) uses client-side routing to load packaged assets from services_packages/.
- Deploy the static files (or the extracted ZIP) to a static host or CDN for users to access with a browser.

Notes / Next steps

- If you want, I can add this file to the repository (at the root or under docs/) and open a PR.
- I can also expand the diagram with CI/CD, cache/CDN details, or component-level breakdowns.
