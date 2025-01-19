# Feature Implementation Tree

## Context for AI Agent
This visualization shows the complete implementation tree for a feature in the Next.js/Supabase/Zustand stack, following a backend-first approach. It illustrates all dependencies, connections, and data flows between different templates and components.

## Implementation Tree
```mermaid
graph TD
    %% Data Foundation
    DM[_NEW_DATA_MODEL.md] -->|Creates| DB[Database Tables]
    DM -->|Defines| RLS[RLS Policies]
    DM -->|Creates| ZOD[Zod Schemas]
    DB -->|Generates| ST[Supabase Types]

    %% Data Access
    SL[_SERVICES_LAYER.md] -->|Uses| DB
    SL -->|Uses| ST
    SL -->|Enforces| RLS

    %% Server Integration
    SA[_SERVER_ACTION.md] -->|Uses| SL
    SA -->|Validates with| ZOD
    SA -->|Manages| CACHE[Cache]

    %% Client State
    BSS[_STORE_SLICE_BASIC.md] -->|Uses| SA
    ASS[_STORE_SLICE_ADVANCED.md] -->|Extends| BSS
    ASS -->|Provides| OU[Optimistic Updates]

    %% UI Layer
    H[_HOOKS.md] -->|Uses| BSS
    H -->|Uses| ASS
    H -->|Uses| SA
    UI[_UI_COMPONENT.md] -->|Uses| H

    %% Styling
    style DM fill:#e1f5fe,stroke:#01579b
    style SL fill:#e8f5e9,stroke:#1b5e20
    style SA fill:#f3e5f5,stroke:#4a148c
    style BSS fill:#fff3e0,stroke:#e65100
    style ASS fill:#fff3e0,stroke:#e65100
    style H fill:#fce4ec,stroke:#880e4f
    style UI fill:#fce4ec,stroke:#880e4f

    %% Connections
    ZOD -->|Validates| SA
    ZOD -->|Validates| UI
    ST -->|Types| SL
    ST -->|Types| SA
    ST -->|Types| BSS
    CACHE -->|Invalidates| BSS
    OU -->|Syncs with| SA
```

## Layer Color Guide
- 🔵 Blue: Data Foundation Layer
  - Database schema
  - RLS policies
  - Type generation
  - Validation schemas

- 🟢 Green: Data Access Layer
  - Database operations
  - Query optimization
  - Error handling

- 🟣 Purple: Server Integration Layer
  - Server actions
  - Cache management
  - API endpoints

- 🟡 Orange: Client State Layer
  - Basic state management
  - Advanced state patterns
  - Optimistic updates

- 🔴 Pink: UI Layer
  - Custom hooks
  - Components
  - User interactions

## Key Flows

### Type Flow
1. Database schema generates Supabase types
2. Types are used by services and actions
3. Actions provide types to stores
4. Stores provide types to UI

### Data Flow
1. UI triggers actions
2. Actions call services
3. Services interact with database
4. Results flow back up through layers

### Validation Flow
1. Zod schemas defined at data layer
2. Used by server actions for input
3. Used by UI for form validation
4. Ensures consistent validation
