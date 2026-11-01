## Project Context

This is the front-end of a personal tool that randomizes Dead by Daylight builds (character, 4 perks, add-ons) from the player's inventory and explains why the combination works. The back-end is a separate .NET API (Controllers, EF Core). Requirements are in the FRD (modules CAT, INV, SOR, EXP, HIS).

## Stack and Decisions

- Angular 22, Angular Material, SCSS for styling. No SSR/SSG.
- Do NOT add Tailwind or other UI libraries unless asked.
- Zod validates data at the boundaries (API responses). Form validation stays with Signal Forms.
- Ask before adding any new dependency.

## Architecture

- Organize by feature: `src/app/features/{catalog,inventory,draw,history}`; shared code in `src/app/shared`; API clients and interceptors in `src/app/core`.
- Business rules (draw, inventory filtering, explanation) live in the API. The front only sends filters and renders results.
- One service per API resource, using `HttpClient` with `inject()`. The API base URL comes from the environment config, never hardcoded.

## API (MVP)

- `GET /api/characters`, `/api/perks`, `/api/addons`, `/api/items`, `/api/tags`
- `GET /api/inventory`, `PUT /api/inventory/characters/{id}`, `PUT /api/inventory/perks/{id}`, `PUT /api/inventory/bulk`
- `POST /api/draws` (side, locks, exclusions, challenge tag) returns a build with its explanation
- `POST /api/builds`, `GET /api/builds`, `PATCH /api/builds/{id}/recorded`
- Errors come as `ProblemDetails`; show a friendly message in the UI.

## Domain Rules the UI Must Respect

- A build has 4 perks, all from the same side (killer or survivor).
- If the API returns a partial build, show how many perks are missing.
- Locked items stay in the build on re-roll; excluded items never appear.

## Conventions

- Code, identifiers, and comments in English. UI text in Portuguese (pt-BR).
- The MVP has no authentication and no AI explanation (phase 2).
