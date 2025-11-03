# PHTN AI Platform - Complete System Documentation

**Version:** 1.0  
**Last Updated:** November 3, 2025  
**Documentation Status:** Complete & Production Ready

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [System Architecture Overview](#system-architecture-overview)
3. [Technology Stack](#technology-stack)
4. [Repository Structure](#repository-structure)
5. [Frontend Layer (PUI)](#frontend-layer-pui)
6. [Middleware Layer (pmiddleware)](#middleware-layer-pmiddleware)
7. [Agent Layer (pagents)](#agent-layer-pagents)
8. [Authentication & Authorization](#authentication--authorization)
9. [Database Architecture](#database-architecture)
10. [API Documentation](#api-documentation)
11. [WebSocket Communication](#websocket-communication)
12. [Deployment & Infrastructure](#deployment--infrastructure)
13. [Data Flow & Integration](#data-flow--integration)
14. [Security Implementation](#security-implementation)
15. [Environment Configuration](#environment-configuration)
16. [Development Workflow](#development-workflow)
17. [Troubleshooting Guide](#troubleshooting-guide)

---

## Executive Summary

The PHTN AI Platform is a sophisticated **three-tier full-stack application** that provides an enterprise-grade AI agent management and conversation platform. The system consists of:

1. **PUI (Frontend)**: React-based TypeScript UI with Redux state management
2. **pmiddleware (Backend)**: FastAPI-based Python middleware with comprehensive RBAC
3. **pagents (Agent Layer)**: Google ADK-powered AI agent system with hierarchical agent architecture

### Key Capabilities

- **Multi-tenant RBAC System**: 8-level hierarchical tenant management (Platform → Tenant Group → Tenant → Domain → Team → User → Project → AI Agent)
- **Real-time Communication**: WebSocket-based bidirectional communication between frontend, middleware, and agents
- **AI Agent Orchestration**: Hierarchical agent system with super agents and sub-agents
- **Comprehensive Authentication**: JWT-based auth with role-based permissions (61 permissions across 13 roles)
- **Enterprise Features**: Full observability with OpenTelemetry, Azure integration, CI/CD pipelines
- **Catalog Management**: Agent, Tool, Model, and Ontology catalogs with version control

---

## System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER INTERFACE                          │
│                    (Browser / Web Client)                       │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         │ HTTPS / WebSocket
                         │
┌────────────────────────▼────────────────────────────────────────┐
│                    FRONTEND LAYER (PUI)                         │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  React 19 + TypeScript + Redux                           │  │
│  │  - Authentication & Session Management                   │  │
│  │  - Redux State Management (Saga middleware)              │  │
│  │  - WebSocket Client for Real-time Updates               │  │
│  │  - Monaco Editor for Code Display                       │  │
│  │  - React Flow for Agent Visualization                   │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         │ HTTP/HTTPS + WebSocket
                         │ (axios + socket.io-client)
                         │
┌────────────────────────▼────────────────────────────────────────┐
│                 MIDDLEWARE LAYER (pmiddleware)                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  FastAPI 0.115.2 + Python 3.x                           │  │
│  │  - REST API Endpoints (100+ routes)                     │  │
│  │  - WebSocket Bridge (LangFlow <-> Frontend)             │  │
│  │  - RBAC Engine (8-level hierarchy)                      │  │
│  │  - JWT Authentication & Authorization                   │  │
│  │  - PostgreSQL Database Layer                            │  │
│  │  - OpenTelemetry Observability                          │  │
│  │  - Redis Caching & Rate Limiting                        │  │
│  │  - Catalog Management APIs                              │  │
│  └──────────────────────────────────────────────────────────┘  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  PostgreSQL Database                                     │  │
│  │  - 19+ Tables (Users, Roles, Permissions, Tenants...)   │  │
│  │  - RBAC Schema (320 role-permission mappings)           │  │
│  │  - Agent Catalogs, Conversations, Sessions              │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         │ HTTP API Calls
                         │ (Google ADK SDK)
                         │
┌────────────────────────▼────────────────────────────────────────┐
│                   AGENT LAYER (pagents)                         │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  Google ADK (Agent Development Kit) 1.4.2                │  │
│  │  - FastAPI Server (port 8080)                           │  │
│  │  - Super Flow Agent (Orchestrator)                      │  │
│  │  - Sub-Agents (HR, MCP Toolset)                         │  │
│  │  - LLM Integration (OpenAI, Anthropic, Vertex AI)       │  │
│  │  - Cloud Logging & Tracing                              │  │
│  │  - Session Management                                    │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────────────────┬────────────────────────────────────────┘
                         │
                         │
                         ▼
              External AI Services
         (OpenAI, Anthropic, Google Vertex AI)
```

### Communication Flow

1. **User → Frontend**: User interacts with React UI
2. **Frontend → Middleware**: HTTP requests (REST API) via axios + WebSocket connections
3. **Middleware → Database**: PostgreSQL queries for data persistence
4. **Middleware → Agents**: HTTP calls to Google ADK agent endpoints
5. **Agents → LLMs**: API calls to OpenAI, Anthropic, Vertex AI
6. **Real-time Updates**: WebSocket bidirectional flow (Agent → Middleware → Frontend)

---

## Technology Stack

### Frontend (PUI)

| Technology | Version | Purpose |
|------------|---------|---------|
| React | 19.1.0 | UI Framework |
| TypeScript | 4.9.5 | Type Safety |
| Redux | 5.0.1 | State Management |
| Redux Saga | 1.3.0 | Side Effect Management |
| @reduxjs/toolkit | 2.8.2 | Redux Utilities |
| React Router DOM | 7.6.2 | Routing |
| Axios | 1.9.0 | HTTP Client |
| Socket.io Client | 3.1.3 | WebSocket Client |
| Monaco Editor | 0.52.2 | Code Editor |
| React Flow | 11.11.4 | Node-based UI |
| Bootstrap | 5.3.6 | CSS Framework |
| Lucide React | 0.511.0 | Icons |
| js-cookie | 3.0.5 | Cookie Management |

### Middleware (pmiddleware)

| Technology | Version | Purpose |
|------------|---------|---------|
| FastAPI | 0.115.2 | Web Framework |
| Uvicorn | 0.24.0 | ASGI Server |
| Gunicorn | 21.2.0 | Production Server |
| Pydantic | 2.5.0 | Data Validation |
| SQLAlchemy | 2.0.23 | ORM |
| Asyncpg | 0.29.0 | Async PostgreSQL Driver |
| Psycopg2-binary | 2.9.9 | PostgreSQL Adapter |
| Alembic | 1.13.0 | Database Migrations |
| Redis | 5.0.1 | Caching |
| PyJWT | 2.8.0 | JWT Authentication |
| Passlib | 1.7.4 | Password Hashing |
| OpenTelemetry | 1.21.0 | Observability |
| Prometheus Client | 0.19.0 | Metrics |
| Azure SDK | Various | Azure Integration |
| OpenAI | 1.3.0+ | OpenAI API |
| HTTPX | 0.25.2 | Async HTTP Client |

### Agent Layer (pagents)

| Technology | Version | Purpose |
|------------|---------|---------|
| Google ADK | 1.4.2 | Agent Framework |
| FastAPI | Latest | API Server |
| Uvicorn | Latest | ASGI Server |
| Google Cloud AI Platform | 1.101.0 | Vertex AI |
| Google Generative AI | 0.7.2 | Gemini Models |
| OpenAI | 1.95.1 | GPT Models |
| Anthropic | Latest | Claude Models |
| LiteLLM | Latest | Multi-LLM Gateway |
| LangChain | 0.3.27 | LLM Orchestration |
| LangChain Core | 0.3.74 | Core Components |
| Google Cloud Logging | Latest | Cloud Logging |
| Google Cloud Storage | 2.10.0+ | File Storage |
| Psycopg2-binary | Latest | Database Connectivity |

### Infrastructure

| Technology | Purpose |
|------------|---------|
| PostgreSQL | Primary Database |
| Redis | Caching & Rate Limiting |
| Docker | Containerization |
| Jenkins | CI/CD Pipeline |
| Google Cloud Build | Cloud CI/CD |
| Google Cloud Run | Serverless Deployment |
| Nginx | Reverse Proxy (implicit) |

---

## Repository Structure

### 1. PUI (Frontend Repository)

```
pui/
├── public/
│   ├── index.html              # HTML entry point
│   ├── favicon.ico             # App icon
│   ├── manifest.json           # PWA manifest
│   └── robots.txt              # SEO configuration
├── src/
│   ├── App.tsx                 # Root component
│   ├── App.css                 # Global styles
│   ├── index.tsx               # React entry point
│   ├── core-api/
│   │   ├── api/                # API service layer
│   │   │   ├── apiService.ts           # Generic API service
│   │   │   ├── axiosInstance.ts        # Axios configuration with interceptors
│   │   │   ├── login.ts                # Login API
│   │   │   ├── getDashboardData.ts     # Dashboard API
│   │   │   ├── getGeneratedCode.ts     # Code generation API
│   │   │   ├── getSourceCode.ts        # Source code retrieval
│   │   │   ├── getSettings.ts          # Settings API
│   │   │   └── addSettings.ts          # Settings update API
│   │   └── redux/              # Redux state management
│   │       ├── actions/                # Action creators
│   │       │   ├── loginAction.ts
│   │       │   ├── getDashboardDataAction.ts
│   │       │   ├── getGeneratedCodeAction.ts
│   │       │   ├── getSourceCodeAction.ts
│   │       │   ├── getSettingsAction.ts
│   │       │   └── addSettingsAction.ts
│   │       ├── reducers/               # Reducers
│   │       │   ├── loginReducer.ts
│   │       │   ├── getDashboardDataReducer.ts
│   │       │   ├── getGeneratedCodeReducer.ts
│   │       │   ├── getSourceCodeReducer.ts
│   │       │   ├── getSettingsReducer.ts
│   │       │   └── addSettingsReducer.ts
│   │       ├── sagas-actions/          # Saga action types
│   │       ├── rootReducer.ts          # Combined reducers
│   │       └── rootSaga.ts             # Combined sagas
├── .env                        # Environment variables
├── package.json                # Dependencies
├── tsconfig.json               # TypeScript configuration
├── eslint.config.mjs           # ESLint configuration
└── generate-react-cli.json     # Component generator config
```

### 2. pmiddleware (Middleware Repository)

```
pmiddleware/
├── main.py                             # FastAPI application entry point
├── api/
│   ├── routes.py                       # Main REST API routes
│   ├── sse.py                          # Server-Sent Events
│   ├── ai_conversation.py              # AI conversation endpoints
│   ├── agent_catalog.py                # Legacy agent catalog
│   ├── langflow_integration/           # LangFlow integration
│   │   ├── bidirectional_bridge.py     # WebSocket bridge
│   │   └── routes.py                   # LangFlow routes
│   ├── ontology/                       # Ontological intelligence
│   ├── websockets/
│   │   └── langflow_bridge.py          # WebSocket manager
│   ├── services/
│   │   └── hot_modification/
│   │       └── live_updater.py         # Live update service
│   └── features/                       # Feature modules
│       ├── auth/                       # Authentication & RBAC
│       │   ├── __init__.py             # Module exports
│       │   ├── routes.py               # Auth endpoints (100+ routes)
│       │   ├── models.py               # Pydantic models
│       │   ├── schemas.py              # Request/Response schemas
│       │   ├── service.py              # Business logic
│       │   ├── database.py             # DB connection pool
│       │   ├── data_repository.py      # Data access layer
│       │   ├── auth_orchestrator.py    # Auth orchestration
│       │   ├── jwt_utils.py            # JWT utilities
│       │   ├── security.py             # Security utilities
│       │   ├── dependencies.py         # FastAPI dependencies
│       │   ├── rate_limiter.py         # Rate limiting
│       │   ├── guest_context_service.py    # Guest user service
│       │   ├── guest_role_service.py       # Guest role management
│       │   ├── user_creation_service.py    # User creation
│       │   ├── response_models.py      # API response models
│       │   └── docs/                   # Documentation
│       │       ├── AUTHENTICATION_COMPLETE_GUIDE.md
│       │       ├── API_ENDPOINTS.md
│       │       ├── PERMISSION_HIERARCHY.md
│       │       └── *.drawio            # Architecture diagrams
│       ├── tenant_management/          # Multi-tenant management
│       │   ├── main_routes.py          # Tenant CRUD routes
│       │   ├── roles/
│       │   │   └── routes.py           # Role management
│       │   └── docs/
│       │       ├── TENANT_MANAGEMENT_COMPLETE_GUIDE.md
│       │       └── TENANT_MANAGEMENT_VERIFICATION_CHECKLIST.md
│       ├── catalogs/                   # Catalog APIs
│       │   ├── simple_agent_catalog.py     # Simple agent catalog
│       │   ├── tool_catalog_api.py         # Tool catalog
│       │   ├── model_catalog_api.py        # Model catalog
│       │   ├── ontology_catalog_api.py     # Ontology catalog
│       │   ├── agent_catalog_api.py        # Advanced agent catalog
│       │   └── agent_catalog_tree_api.py   # Hierarchical catalog
│       ├── dashboard/                  # Dashboard features
│       │   ├── dashboard.py            # Main dashboard
│       │   ├── finops_usage.py         # FinOps usage tracking
│       │   ├── finops_dashboard.py     # FinOps dashboard
│       │   └── techops.py              # TechOps dashboard
│       ├── users/
│       │   └── routes.py               # User management
│       └── config/
│           └── routes.py               # Configuration management
├── core/                               # Core middleware
│   ├── loader.py                       # Framework loader
│   ├── middleware.py                   # Custom middleware
│   ├── auth.py                         # Auth middleware
│   ├── rate_limit.py                   # Rate limit middleware
│   ├── config.py                       # Configuration
│   └── health.py                       # Health checks
├── database/                           # Database layer
├── schemas/                            # Database schemas
├── services/                           # Business services
├── utils/                              # Utilities
│   └── telemetry.py                    # OpenTelemetry setup
├── plugins/                            # Plugin system
│   └── example_plugin.py
├── sql/                                # SQL scripts
│   └── new/
│       └── environments/
│           └── dev_seed.sql            # Database seed data
├── tests/                              # Test suite
├── docs/                               # Documentation
├── scripts/                            # Utility scripts
├── requirements.txt                    # Python dependencies
├── requirements-production.txt         # Production dependencies
├── requirements-test.txt               # Test dependencies
├── Dockerfile                          # Container image
├── Jenkinsfile                         # Jenkins CI/CD
└── pyproject.toml                      # Python project config
```

### 3. pagents (Agent Layer Repository)

```
pagents/
├── server.py                           # FastAPI agent server
├── tracing.py                          # Cloud tracing setup
├── deploy_agent.py                     # Deployment script
├── super_flow_agent/                   # Main agent directory
│   ├── agent.py                        # Super flow agent
│   ├── prompt.py                       # Agent prompts
│   ├── models.py                       # Data models
│   ├── http_mcp_toolset.py             # MCP HTTP tools
│   ├── agent_tree.json                 # Agent hierarchy config
│   ├── super_agent/                    # Super agent module
│   │   ├── agent.py                    # Super agent implementation
│   │   ├── prompt.py                   # Super agent prompts
│   │   └── sub_agents/                 # Sub-agent modules
│   │       └── HR_Agent/               # HR Agent example
│   │           ├── README.md
│   │           ├── agents/
│   │           │   ├── clean-agent/
│   │           │   │   └── clean_vtt_agent.py
│   │           │   ├── hr-agent/
│   │           │   │   └── hr_super_agent.py
│   │           │   └── mcp/
│   │           │       └── report-mcp-server/
│   │           │           └── report_mcp_server.py
│   │           └── hiring_super_agent/
│   │               ├── agent.py
│   │               ├── agent2.py
│   │               ├── agent_communication.py
│   │               └── agent_urls.py
├── .env                                # Environment variables
├── .env.backup                         # Environment backup
├── requirements.txt                    # Python dependencies
├── pyproject.toml                      # Python project config
├── Dockerfile                          # Container image
├── Jenkinsfile                         # Jenkins CI/CD
├── cloudbuild.yaml                     # Google Cloud Build
└── env-vars.yaml                       # Environment variables template
```

---

## Frontend Layer (PUI)

### Overview

The PUI (PHTN User Interface) is a modern React application built with TypeScript, providing a rich user interface for interacting with the PHTN AI Platform.

### Key Components

#### 1. **Redux State Management**

The frontend uses Redux with Redux Saga for managing application state and side effects.

**Store Structure:**
```typescript
{
  login: {
    loading: boolean,
    data: {
      access_token: string,
      token_type: string,
      user: {
        id: string,
        email: string,
        name: string,
        role: string,
        permissions: string[]
      }
    },
    error: string | null
  },
  dashboardData: {
    loading: boolean,
    data: any,
    error: string | null
  },
  generatedCode: {
    loading: boolean,
    data: string,
    error: string | null
  },
  sourceCode: {
    loading: boolean,
    data: string,
    error: string | null
  },
  settings: {
    loading: boolean,
    data: any,
    error: string | null
  }
}
```

#### 2. **Axios Instance Configuration**

**File:** `src/core-api/api/axiosInstance.ts`

```typescript
import axios from 'axios';
import Cookies from 'js-cookie';

const axiosInstance = axios.create({
  baseURL: process.env.REACT_APP_API_BASE_URL || 'http://localhost:8000',
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor - adds JWT token
axiosInstance.interceptors.request.use(
  (config) => {
    const token = Cookies.get('access_token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Response interceptor - handles errors and token refresh
axiosInstance.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Handle unauthorized - redirect to login
      Cookies.remove('access_token');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);
```

**Key Features:**
- Automatic JWT token injection in request headers
- 401 error handling with automatic redirect to login
- 30-second timeout for API calls
- Centralized error handling

#### 3. **Authentication Flow**

**Login Action Flow:**

```
User submits credentials
    ↓
loginAction.ts dispatches LOGIN_REQUEST
    ↓
Redux Saga intercepts LOGIN_REQUEST
    ↓
Saga calls api/login.ts
    ↓
POST /api/v1/auth/login to middleware
    ↓
Middleware validates credentials & returns JWT
    ↓
Saga stores token in cookies
    ↓
Saga dispatches LOGIN_SUCCESS with user data
    ↓
loginReducer.ts updates state
    ↓
UI updates with authenticated state
```

**Login Reducer Logic:**
```typescript
// src/core-api/redux/reducers/loginReducer.ts

interface LoginState {
  loading: boolean;
  data: {
    access_token: string;
    token_type: string;
    user: UserData;
  } | null;
  error: string | null;
}

const initialState: LoginState = {
  loading: false,
  data: null,
  error: null,
};

export const loginReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'LOGIN_REQUEST':
      return { ...state, loading: true, error: null };
    
    case 'LOGIN_SUCCESS':
      return {
        ...state,
        loading: false,
        data: action.payload,
        error: null,
      };
    
    case 'LOGIN_FAILURE':
      return {
        ...state,
        loading: false,
        data: null,
        error: action.payload,
      };
    
    case 'LOGOUT':
      return initialState;
    
    default:
      return state;
  }
};
```

#### 4. **API Service Layer**

**Generic API Service Pattern:**
```typescript
// src/core-api/api/apiService.ts

export const apiService = {
  get: async <T>(url: string): Promise<T> => {
    const response = await axiosInstance.get<T>(url);
    return response.data;
  },
  
  post: async <T>(url: string, data: any): Promise<T> => {
    const response = await axiosInstance.post<T>(url, data);
    return response.data;
  },
  
  put: async <T>(url: string, data: any): Promise<T> => {
    const response = await axiosInstance.put<T>(url, data);
    return response.data;
  },
  
  delete: async <T>(url: string): Promise<T> => {
    const response = await axiosInstance.delete<T>(url);
    return response.data;
  },
};
```

#### 5. **Real-time Communication (WebSocket)**

The frontend uses `socket.io-client` for WebSocket connections:

```typescript
import io from 'socket.io-client';

const socket = io(process.env.REACT_APP_WEBSOCKET_URL || 'ws://localhost:8000', {
  transports: ['websocket'],
  auth: {
    token: Cookies.get('access_token'),
  },
});

// Connect to agent workflow
socket.emit('join_workflow', { workflow_id: 'super_flow_agent' });

// Listen for agent responses
socket.on('agent_response', (data) => {
  console.log('Agent response:', data);
  // Update UI with agent response
});

// Send message to agent
socket.emit('send_message', {
  message: 'Hello, agent!',
  workflow_id: 'super_flow_agent',
});
```

#### 6. **Code Editor Integration**

**Monaco Editor Setup:**
```typescript
import { Editor } from '@monaco-editor/react';
import { editor } from 'monaco-editor';

const CodeEditor = ({ code, language, onChange }) => {
  const editorOptions: editor.IStandaloneEditorConstructionOptions = {
    minimap: { enabled: false },
    fontSize: 14,
    lineNumbers: 'on',
    scrollBeyondLastLine: false,
    automaticLayout: true,
    theme: 'vs-dark',
  };

  return (
    <Editor
      height="600px"
      language={language}
      value={code}
      options={editorOptions}
      onChange={onChange}
    />
  );
};
```

### Environment Variables

**`.env` Configuration:**
```bash
# API Base URL
REACT_APP_API_BASE_URL=http://localhost:8000

# WebSocket URL
REACT_APP_WEBSOCKET_URL=ws://localhost:8000

# Environment
REACT_APP_ENV=development
```

### Build & Deployment

**Development:**
```bash
npm install
npm start  # Runs on port 3000
```

**Production Build:**
```bash
npm run build
# Creates optimized production build in /build directory
```

---

## Middleware Layer (pmiddleware)

### Overview

The pmiddleware is a comprehensive FastAPI-based backend that serves as the central orchestration layer between the frontend and the agent layer. It handles authentication, authorization, database operations, and API routing.

### Application Entry Point

**File:** `main.py`

```python
# Key imports
from fastapi import FastAPI, Request, HTTPException, WebSocket
from fastapi.middleware.cors import CORSMiddleware
from contextlib import asynccontextmanager

# Application lifespan management
@asynccontextmanager
async def lifespan(app: FastAPI):
    """Startup and shutdown events"""
    logger.info("Starting PHTN AI Middleware")
    
    # Startup: Initialize database pool
    app.state.db_pool = await init_db_pool()
    app.state.health_checker = HealthChecker()
    await app.state.health_checker.initialize()
    
    # Start LangFlow live updater
    asyncio.create_task(live_updater.start_update_processor())
    
    yield
    
    # Shutdown: Cleanup
    await live_updater.stop_update_processor()
    await close_db_pool()
    await app.state.health_checker.cleanup()

# Create FastAPI application
app = FastAPI(
    title="PHTN AI Middleware Core",
    description="Enterprise-grade AI middleware",
    version="1.0.0",
    docs_url="/api/docs",
    redoc_url="/api/redoc",
    lifespan=lifespan
)

# CORS Configuration
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # Configure for production
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE", "OPTIONS"],
    allow_headers=["*"],
)

# Include routers
app.include_router(auth_router, prefix="/api/v1")
app.include_router(rest_router, prefix="/api/v1")
app.include_router(langflow_router)
app.include_router(tenant_management_router)
# ... more routers
```

### Authentication & RBAC System

#### JWT Authentication Flow

```
1. User Login Request
   POST /api/v1/auth/login
   Body: { email, password }
        ↓
2. auth_orchestrator.py validates credentials
        ↓
3. Password verification using passlib bcrypt
        ↓
4. Fetch user data from PostgreSQL
        ↓
5. Fetch user roles & permissions
        ↓
6. Generate JWT token with payload:
   {
     sub: user_id,
     email: user_email,
     tenant_id: tenant_id,
     domain_id: domain_id,
     team_id: team_id,
     roles: [role_ids],
     permissions: [permission_codes],
     exp: expiration_timestamp
   }
        ↓
7. Return JWT token to client
```

#### Permission Validation

**File:** `api/features/auth/dependencies.py`

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
import jwt

security = HTTPBearer()

async def get_current_user(
    credentials: HTTPAuthorizationCredentials = Depends(security)
) -> dict:
    """Extract and validate JWT token"""
    try:
        token = credentials.credentials
        payload = jwt.decode(
            token,
            settings.secret_key,
            algorithms=["HS256"]
        )
        return payload
    except jwt.ExpiredSignatureError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Token has expired"
        )
    except jwt.InvalidTokenError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid token"
        )

def require_permission(permission_code: str):
    """Dependency to check if user has specific permission"""
    async def permission_checker(
        current_user: dict = Depends(get_current_user)
    ) -> dict:
        user_permissions = current_user.get("permissions", [])
        if permission_code not in user_permissions:
            raise HTTPException(
                status_code=status.HTTP_403_FORBIDDEN,
                detail=f"Permission '{permission_code}' required"
            )
        return current_user
    return permission_checker

# Usage in routes:
@router.get("/protected-endpoint")
async def protected_route(
    current_user: dict = Depends(require_permission("read:data"))
):
    return {"message": "Access granted"}
```

#### RBAC Hierarchy (8 Levels)

```
1. Platform Level (Root)
   ↓
2. Tenant Group Level
   ↓
3. Tenant Level
   ↓
4. Domain Level (Product, Engineering, Sales, Finance)
   ↓
5. Team Level (Backend, Frontend, DevOps, AI/ML, etc.)
   ↓
6. User Level
   ↓
7. Project Level
   ↓
8. AI Agent Level
```

#### Role Definitions (13 Roles)

| Role ID | Role Name | Level | Permissions Count |
|---------|-----------|-------|-------------------|
| 1 | Platform Admin | Platform | 61 (all) |
| 2 | Tenant Platform Admin | Tenant Group | 57 |
| 3 | Domain Admin | Domain | 45 |
| 4 | Team Lead | Team | 35 |
| 5 | Architect | Team | 30 |
| 6 | Agent Builder | Team | 22 |
| 7 | Developer | Team | 18 |
| 8 | Viewer | Team | 8 |
| 9 | Guest User | Platform | 5 |
| 10 | FinOps Admin | Domain | 40 |
| 11 | TechOps Admin | Domain | 38 |
| 12 | Security Admin | Platform | 45 |
| 13 | Auditor | Platform | 15 |

#### Permission Categories (61 Permissions)

**1. Tenant Management (15 permissions)**
- `tenant:create`, `tenant:read`, `tenant:update`, `tenant:delete`
- `tenant:list`, `tenant:assign_users`
- `tenant_group:create`, `tenant_group:read`, etc.

**2. User Management (12 permissions)**
- `user:create`, `user:read`, `user:update`, `user:delete`
- `user:list`, `user:assign_roles`, `user:manage_permissions`

**3. Agent Management (18 permissions)**
- `agent:create`, `agent:read`, `agent:update`, `agent:delete`
- `agent:deploy`, `agent:execute`, `agent:configure`
- `agent:view_logs`, `agent:manage_versions`

**4. System Operations (16 permissions)**
- `system:view_metrics`, `system:manage_configs`
- `finops:view_usage`, `finops:manage_budgets`
- `techops:view_infrastructure`, `techops:deploy`

### Database Layer

#### Database Connection

**File:** `api/features/auth/database.py`

```python
import asyncpg
from typing import Optional

# Global connection pool
_pool: Optional[asyncpg.Pool] = None

async def init_db_pool() -> asyncpg.Pool:
    """Initialize PostgreSQL connection pool"""
    global _pool
    _pool = await asyncpg.create_pool(
        host=settings.db_host,
        port=settings.db_port,
        user=settings.db_user,
        password=settings.db_password,
        database=settings.db_name,
        min_size=10,
        max_size=100,
        command_timeout=60,
    )
    return _pool

async def get_db_pool() -> asyncpg.Pool:
    """Get the database pool"""
    if _pool is None:
        raise RuntimeError("Database pool not initialized")
    return _pool

async def close_db_pool():
    """Close database pool"""
    global _pool
    if _pool:
        await _pool.close()
        _pool = None
```

#### Data Repository Pattern

**File:** `api/features/auth/data_repository.py`

```python
class AuthDataRepository:
    """Data access layer for authentication"""
    
    def __init__(self, pool: asyncpg.Pool):
        self.pool = pool
    
    async def get_user_by_email(self, email: str) -> Optional[dict]:
        """Fetch user by email"""
        query = """
            SELECT 
                u.user_id, u.email, u.name, u.password_hash,
                u.tenant_id, u.domain_id, u.team_id,
                u.is_active, u.created_at, u.updated_at
            FROM phtnai_users u
            WHERE u.email = $1 AND u.is_deleted = FALSE
        """
        async with self.pool.acquire() as conn:
            row = await conn.fetchrow(query, email)
            return dict(row) if row else None
    
    async def get_user_roles(self, user_id: str) -> list[dict]:
        """Fetch all roles for a user"""
        query = """
            SELECT 
                r.role_id, r.role_name, r.role_code,
                r.role_level, r.description
            FROM phtnai_user_roles ur
            JOIN phtnai_roles r ON ur.role_id = r.role_id
            WHERE ur.user_id = $1 AND ur.is_active = TRUE
            AND r.is_deleted = FALSE
        """
        async with self.pool.acquire() as conn:
            rows = await conn.fetch(query, user_id)
            return [dict(row) for row in rows]
    
    async def get_user_permissions(self, user_id: str) -> list[str]:
        """Fetch all permission codes for a user"""
        query = """
            SELECT DISTINCT p.permission_code
            FROM phtnai_user_roles ur
            JOIN phtnai_role_permissions rp ON ur.role_id = rp.role_id
            JOIN phtnai_permissions p ON rp.permission_id = p.permission_id
            WHERE ur.user_id = $1 
            AND ur.is_active = TRUE
            AND p.is_deleted = FALSE
            
            UNION
            
            SELECT p.permission_code
            FROM phtnai_user_permissions up
            JOIN phtnai_permissions p ON up.permission_id = p.permission_id
            WHERE up.user_id = $1
            AND up.is_active = TRUE
            AND p.is_deleted = FALSE
        """
        async with self.pool.acquire() as conn:
            rows = await conn.fetch(query, user_id)
            return [row['permission_code'] for row in rows]
```

### API Endpoints

#### Authentication Endpoints

**Base Path:** `/api/v1/auth`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/login` | User login | No |
| POST | `/logout` | User logout | Yes |
| POST | `/refresh` | Refresh JWT token | Yes |
| GET | `/me` | Get current user info | Yes |
| PUT | `/me` | Update current user | Yes |
| POST | `/register` | User registration | No |
| POST | `/forgot-password` | Initiate password reset | No |
| POST | `/reset-password` | Complete password reset | No |
| POST | `/verify-email` | Verify email address | No |

#### Tenant Management Endpoints

**Base Path:** `/api/v1/tenants`

| Method | Endpoint | Description | Permission Required |
|--------|----------|-------------|---------------------|
| GET | `/` | List all tenants | `tenant:list` |
| POST | `/` | Create tenant | `tenant:create` |
| GET | `/{tenant_id}` | Get tenant details | `tenant:read` |
| PUT | `/{tenant_id}` | Update tenant | `tenant:update` |
| DELETE | `/{tenant_id}` | Delete tenant | `tenant:delete` |
| GET | `/{tenant_id}/users` | List tenant users | `tenant:read` |
| POST | `/{tenant_id}/users` | Assign user to tenant | `tenant:assign_users` |

#### Agent Catalog Endpoints

**Base Path:** `/api/v1/phtnai_agent_catalog`

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/agents` | List all agents |
| POST | `/agents` | Create new agent |
| GET | `/agents/{agent_id}` | Get agent details |
| PUT | `/agents/{agent_id}` | Update agent |
| DELETE | `/agents/{agent_id}` | Delete agent |
| POST | `/agents/{agent_id}/deploy` | Deploy agent |
| GET | `/agents/{agent_id}/versions` | List agent versions |
| POST | `/agents/{agent_id}/versions` | Create agent version |

#### Dashboard Endpoints

**Base Path:** `/api/v1/dashboard`

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/overview` | Get dashboard overview |
| GET | `/metrics` | Get system metrics |
| GET | `/finops/usage` | Get FinOps usage data |
| GET | `/finops/costs` | Get cost breakdown |
| GET | `/techops/infrastructure` | Get infrastructure status |
| GET | `/techops/deployments` | Get deployment history |

### WebSocket Implementation

#### LangFlow Bridge

**File:** `api/langflow_integration/bidirectional_bridge.py`

```python
from fastapi import WebSocket
from typing import Dict, Set
import json

class WebSocketBridge:
    """Bidirectional WebSocket bridge between frontend and LangFlow"""
    
    def __init__(self):
        # Frontend connections: {user_id: websocket}
        self.frontend_connections: Dict[str, WebSocket] = {}
        
        # LangFlow connections: {workflow_id: websocket}
        self.langflow_connections: Dict[str, WebSocket] = {}
        
        # Message queue for offline delivery
        self.message_queue: Dict[str, list] = {}
    
    async def connect_phtn_frontend(self, websocket: WebSocket, user_id: str):
        """Connect PHTN frontend client"""
        await websocket.accept()
        self.frontend_connections[user_id] = websocket
        
        # Deliver queued messages
        if user_id in self.message_queue:
            for message in self.message_queue[user_id]:
                await websocket.send_json(message)
            del self.message_queue[user_id]
    
    async def connect_langflow(self, websocket: WebSocket, workflow_id: str):
        """Connect LangFlow workflow"""
        await websocket.accept()
        self.langflow_connections[workflow_id] = websocket
    
    async def handle_phtn_message(self, websocket: WebSocket, message: dict):
        """Handle message from PHTN frontend"""
        workflow_id = message.get('workflow_id')
        
        if workflow_id and workflow_id in self.langflow_connections:
            # Forward to LangFlow
            langflow_ws = self.langflow_connections[workflow_id]
            await langflow_ws.send_json(message)
        else:
            # Queue message or return error
            await websocket.send_json({
                'error': 'Workflow not connected',
                'workflow_id': workflow_id
            })
    
    async def handle_langflow_message(self, websocket: WebSocket, message: dict):
        """Handle message from LangFlow"""
        user_id = message.get('user_id')
        
        if user_id and user_id in self.frontend_connections:
            # Forward to frontend
            frontend_ws = self.frontend_connections[user_id]
            await frontend_ws.send_json(message)
        else:
            # Queue for later delivery
            if user_id not in self.message_queue:
                self.message_queue[user_id] = []
            self.message_queue[user_id].append(message)
    
    def disconnect_phtn_frontend(self, websocket: WebSocket):
        """Disconnect PHTN frontend"""
        user_id = None
        for uid, ws in self.frontend_connections.items():
            if ws == websocket:
                user_id = uid
                break
        if user_id:
            del self.frontend_connections[user_id]
    
    def disconnect_langflow(self, websocket: WebSocket):
        """Disconnect LangFlow"""
        workflow_id = None
        for wid, ws in self.langflow_connections.items():
            if ws == websocket:
                workflow_id = wid
                break
        if workflow_id:
            del self.langflow_connections[workflow_id]

# Global bridge instance
bridge = WebSocketBridge()
```

#### WebSocket Endpoints in main.py

```python
@app.websocket("/ws/langflow-bridge/{workflow_id}")
async def langflow_websocket(websocket: WebSocket, workflow_id: str):
    """WebSocket endpoint for LangFlow connections"""
    await bridge.connect_langflow(websocket, workflow_id)
    try:
        while True:
            data = await websocket.receive_text()
            message = json.loads(data)
            await bridge.handle_langflow_message(websocket, message)
    except WebSocketDisconnect:
        bridge.disconnect_langflow(websocket)

@app.websocket("/ws/phtn-frontend/{user_id}")
async def phtn_frontend_websocket(websocket: WebSocket, user_id: str):
    """WebSocket endpoint for PHTN frontend connections"""
    await bridge.connect_phtn_frontend(websocket, user_id)
    try:
        while True:
            data = await websocket.receive_text()
            message = json.loads(data)
            await bridge.handle_phtn_message(websocket, message)
    except WebSocketDisconnect:
        bridge.disconnect_phtn_frontend(websocket)
```

### Configuration Management

**File:** `core/config.py`

```python
from pydantic_settings import BaseSettings
from typing import List

class Settings(BaseSettings):
    # Application
    service_name: str = "phtn-middleware"
    service_version: str = "1.0.0"
    environment: str = "development"
    debug: bool = True
    
    # Server
    host: str = "0.0.0.0"
    port: int = 8000
    
    # Database
    db_host: str = "localhost"
    db_port: int = 5432
    db_user: str = "postgres"
    db_password: str = "password"
    db_name: str = "phtnai_platform_db"
    
    # Redis
    redis_host: str = "localhost"
    redis_port: int = 6379
    redis_password: str = ""
    
    # JWT
    secret_key: str = "your-secret-key-change-in-production"
    algorithm: str = "HS256"
    access_token_expire_minutes: int = 30
    refresh_token_expire_days: int = 7
    
    # CORS
    cors_enabled: bool = True
    allowed_origins: List[str] = ["*"]
    allowed_hosts: List[str] = ["*"]
    
    # Security
    enable_auth: bool = True
    enable_rate_limiting: bool = True
    enable_compression: bool = True
    
    # Logging
    log_level: str = "INFO"
    
    # Agent Service
    agent_service_url: str = "http://localhost:8080"
    
    class Config:
        env_file = ".env"
        case_sensitive = False

def get_settings() -> Settings:
    return Settings()
```

---

## Agent Layer (pagents)

### Overview

The pagents repository contains the Google ADK-based agent system that handles AI-powered conversations and task execution. It uses the Google Agent Development Kit (ADK) to create hierarchical, multi-agent systems.

### Server Architecture

**File:** `server.py`

```python
import os
from dotenv import load_dotenv
from fastapi import FastAPI
from google.adk.cli.fast_api import get_fast_api_app
from pydantic import BaseModel
from typing import Literal
from google.cloud import logging as google_cloud_logging

# Load environment variables
load_dotenv()

# Setup Google Cloud Logging
logging_client = google_cloud_logging.Client()
logger = logging_client.logger(__name__)

AGENT_DIR = os.path.dirname(os.path.abspath(__file__))

# Get session service URI
session_uri = os.getenv("SESSION_SERVICE_URI", None)

# Prepare Google ADK app arguments
app_args = {"agents_dir": AGENT_DIR, "web": True}

if session_uri:
    app_args["session_service_uri"] = session_uri
else:
    logger.log_text(
        "SESSION_SERVICE_URI not provided. Using in-memory session service.",
        severity="WARNING",
    )

# Create FastAPI app with Google ADK
app: FastAPI = get_fast_api_app(**app_args)

app.title = "super_flow_agent"
app.description = "API for interacting with the Agent super_flow_agent"

# Feedback collection endpoint
class Feedback(BaseModel):
    score: int | float
    text: str | None = ""
    invocation_id: str
    log_type: Literal["feedback"] = "feedback"
    service_name: Literal["super_flow_agent"] = "super_flow_agent"
    user_id: str = ""

@app.post("/feedback")
def collect_feedback(feedback: Feedback) -> dict[str, str]:
    """Collect and log feedback"""
    logger.log_struct(feedback.model_dump(), severity="INFO")
    return {"status": "success"}

# Run server
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8080)
```

**Key Features:**
- Google ADK integration for agent framework
- In-memory or external session management
- Google Cloud Logging integration
- Feedback collection mechanism
- Runs on port 8080

### Agent Hierarchy

```
super_flow_agent (Root Agent)
    ↓
super_agent (Orchestrator)
    ↓
sub_agents/
    ├── HR_Agent/
    │   ├── agents/
    │   │   ├── clean-agent/
    │   │   │   └── clean_vtt_agent.py    # VTT file cleaning
    │   │   ├── hr-agent/
    │   │   │   └── hr_super_agent.py     # HR operations
    │   │   └── mcp/
    │   │       └── report-mcp-server/
    │   │           └── report_mcp_server.py  # Report generation
    │   └── hiring_super_agent/
    │       ├── agent.py                   # Hiring workflow
    │       ├── agent_communication.py     # Inter-agent communication
    │       └── agent_urls.py              # Agent endpoints
    └── (Other sub-agents can be added)
```

### Super Flow Agent

**File:** `super_flow_agent/agent.py`

```python
from google.adk import Agent, InferenceOptions
from .prompt import SYSTEM_PROMPT
from .models import ConversationRequest, ConversationResponse
import os

class SuperFlowAgent(Agent):
    """Main orchestrator agent"""
    
    def __init__(self):
        super().__init__(
            name="super_flow_agent",
            description="Enterprise AI agent orchestrator",
            system_prompt=SYSTEM_PROMPT,
        )
        
        # Configure LLM
        self.inference_options = InferenceOptions(
            model=os.getenv("DEFAULT_MODEL", "gemini-1.5-pro"),
            temperature=float(os.getenv("TEMPERATURE", "0.7")),
            max_tokens=int(os.getenv("MAX_TOKENS", "2048")),
        )
    
    async def process(self, request: ConversationRequest) -> ConversationResponse:
        """Process conversation request"""
        # Route to appropriate sub-agent or handle directly
        if self.should_delegate(request):
            sub_agent = self.get_sub_agent(request)
            return await sub_agent.process(request)
        else:
            return await self.generate_response(request)
    
    def should_delegate(self, request: ConversationRequest) -> bool:
        """Determine if request should be delegated to sub-agent"""
        # Logic to analyze request and decide delegation
        pass
    
    def get_sub_agent(self, request: ConversationRequest):
        """Get appropriate sub-agent for request"""
        # Logic to select sub-agent
        pass
    
    async def generate_response(self, request: ConversationRequest) -> ConversationResponse:
        """Generate response using LLM"""
        # Call LLM with context and return response
        pass
```

### Agent Prompts

**File:** `super_flow_agent/prompt.py`

```python
SYSTEM_PROMPT = """
You are the Super Flow Agent, an enterprise AI orchestrator for the PHTN AI Platform.

Your responsibilities:
1. Understand user intent and context
2. Route requests to appropriate sub-agents when necessary
3. Coordinate multi-agent workflows
4. Provide direct responses for general queries
5. Maintain conversation context and history

Available sub-agents:
- HR_Agent: Handles hiring, employee management, and HR workflows
- (More agents can be registered)

Always maintain professional, helpful, and accurate responses.
When delegating to sub-agents, provide clear context and instructions.
"""

# Agent tree configuration defines hierarchy
AGENT_TREE = {
    "super_flow_agent": {
        "type": "orchestrator",
        "sub_agents": [
            {
                "name": "super_agent",
                "type": "coordinator",
                "sub_agents": [
                    {
                        "name": "HR_Agent",
                        "type": "specialized",
                        "capabilities": ["hiring", "employee_management"]
                    }
                ]
            }
        ]
    }
}
```

### MCP (Model Context Protocol) Integration

**File:** `super_flow_agent/http_mcp_toolset.py`

```python
import httpx
from typing import Any, Dict, List
from google.adk.tools import Tool

class HttpMcpToolset:
    """HTTP-based MCP tools for agent interactions"""
    
    def __init__(self, base_url: str):
        self.base_url = base_url
        self.client = httpx.AsyncClient(timeout=30.0)
    
    @Tool(
        name="http_get",
        description="Make HTTP GET request to external service"
    )
    async def http_get(self, url: str, params: Dict = None) -> Dict:
        """Execute HTTP GET request"""
        full_url = f"{self.base_url}/{url}"
        response = await self.client.get(full_url, params=params)
        return response.json()
    
    @Tool(
        name="http_post",
        description="Make HTTP POST request to external service"
    )
    async def http_post(self, url: str, data: Dict) -> Dict:
        """Execute HTTP POST request"""
        full_url = f"{self.base_url}/{url}"
        response = await self.client.post(full_url, json=data)
        return response.json()
    
    @Tool(
        name="call_middleware_api",
        description="Call PHTN middleware API endpoint"
    )
    async def call_middleware_api(
        self, 
        endpoint: str, 
        method: str = "GET",
        data: Dict = None
    ) -> Dict:
        """Call middleware API with authentication"""
        middleware_url = os.getenv("MIDDLEWARE_URL", "http://localhost:8000")
        api_key = os.getenv("MIDDLEWARE_API_KEY")
        
        headers = {"Authorization": f"Bearer {api_key}"}
        full_url = f"{middleware_url}/api/v1/{endpoint}"
        
        if method == "GET":
            response = await self.client.get(full_url, headers=headers)
        elif method == "POST":
            response = await self.client.post(full_url, json=data, headers=headers)
        
        return response.json()
```

### Agent Models

**File:** `super_flow_agent/models.py`

```python
from pydantic import BaseModel, Field
from typing import Optional, List, Dict, Any
from datetime import datetime

class ConversationRequest(BaseModel):
    """Request model for agent conversation"""
    message: str = Field(..., description="User message")
    user_id: str = Field(..., description="User identifier")
    session_id: str = Field(..., description="Session identifier")
    context: Optional[Dict[str, Any]] = Field(default={}, description="Additional context")
    workflow_id: Optional[str] = Field(default=None, description="Workflow identifier")

class ConversationResponse(BaseModel):
    """Response model for agent conversation"""
    response: str = Field(..., description="Agent response")
    session_id: str = Field(..., description="Session identifier")
    agent_name: str = Field(..., description="Responding agent name")
    timestamp: datetime = Field(default_factory=datetime.utcnow)
    metadata: Optional[Dict[str, Any]] = Field(default={})
    
class AgentMetadata(BaseModel):
    """Metadata about agent capabilities"""
    name: str
    description: str
    version: str
    capabilities: List[str]
    sub_agents: Optional[List[str]] = []
```

### Environment Configuration

**File:** `.env`

```bash
# Google Cloud Project
PROJECT_ID=photon-phtn-ai
REGION=us-central1

# Session Service
SESSION_SERVICE_URI=  # Optional: external session service

# LLM Configuration
DEFAULT_MODEL=gemini-1.5-pro
TEMPERATURE=0.7
MAX_TOKENS=2048

# Middleware Integration
MIDDLEWARE_URL=http://localhost:8000
MIDDLEWARE_API_KEY=your-api-key-here

# Google Cloud Services
GOOGLE_APPLICATION_CREDENTIALS=./photon-phtn-ai-credentials.json

# OpenAI (if using OpenAI models)
OPENAI_API_KEY=sk-...

# Anthropic (if using Claude models)
ANTHROPIC_API_KEY=sk-ant-...

# Logging
LOG_LEVEL=INFO
ENABLE_CLOUD_LOGGING=true

# Server
PORT=8080
HOST=0.0.0.0
```

---

## Database Architecture

### Schema Overview

The PHTN AI Platform uses PostgreSQL with **19+ tables** implementing a comprehensive RBAC system with multi-tenant architecture.

### Core Tables

#### 1. **phtnai_users**
Stores user account information.

```sql
CREATE TABLE phtnai_users (
    user_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    tenant_id UUID REFERENCES phtnai_tenants(tenant_id),
    domain_id UUID REFERENCES phtnai_domains(domain_id),
    team_id UUID REFERENCES phtnai_teams(team_id),
    is_active BOOLEAN DEFAULT TRUE,
    email_verified BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE,
    deleted_at TIMESTAMP
);

CREATE INDEX idx_users_email ON phtnai_users(email);
CREATE INDEX idx_users_tenant ON phtnai_users(tenant_id);
CREATE INDEX idx_users_domain ON phtnai_users(domain_id);
CREATE INDEX idx_users_team ON phtnai_users(team_id);
```

**Key Fields:**
- `user_id`: Unique user identifier (UUID)
- `email`: User email (unique, used for login)
- `password_hash`: Bcrypt hashed password
- `tenant_id`, `domain_id`, `team_id`: Hierarchy references
- `is_active`: Account status
- `is_deleted`: Soft delete flag

#### 2. **phtnai_tenants**
Multi-tenant isolation.

```sql
CREATE TABLE phtnai_tenants (
    tenant_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    tenant_name VARCHAR(255) NOT NULL,
    tenant_code VARCHAR(50) UNIQUE NOT NULL,
    tenant_group_id UUID REFERENCES phtnai_tenant_groups(tenant_group_id),
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE
);
```

#### 3. **phtnai_tenant_groups**
Grouping of tenants.

```sql
CREATE TABLE phtnai_tenant_groups (
    tenant_group_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    group_name VARCHAR(255) NOT NULL,
    group_code VARCHAR(50) UNIQUE NOT NULL,
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE
);
```

#### 4. **phtnai_domains**
Business domain organization.

```sql
CREATE TABLE phtnai_domains (
    domain_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    domain_name VARCHAR(255) NOT NULL,
    domain_code VARCHAR(50) UNIQUE NOT NULL,
    tenant_id UUID REFERENCES phtnai_tenants(tenant_id),
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE
);
```

**Domains:** Product, Engineering, Sales, Finance

#### 5. **phtnai_teams**
Team organization within domains.

```sql
CREATE TABLE phtnai_teams (
    team_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    team_name VARCHAR(255) NOT NULL,
    team_code VARCHAR(50) UNIQUE NOT NULL,
    domain_id UUID REFERENCES phtnai_domains(domain_id),
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE
);
```

**Teams:** Backend, Frontend, DevOps, AI/ML, Enterprise Sales, SMB Sales, Accounting, FinOps, IT

#### 6. **phtnai_roles**
Role definitions.

```sql
CREATE TABLE phtnai_roles (
    role_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    role_name VARCHAR(255) NOT NULL,
    role_code VARCHAR(100) UNIQUE NOT NULL,
    role_level VARCHAR(50),  -- Platform, TenantGroup, Tenant, Domain, Team
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE
);
```

**13 Roles:**
1. Platform Admin
2. Tenant Platform Admin
3. Domain Admin
4. Team Lead
5. Architect
6. Agent Builder
7. Developer
8. Viewer
9. Guest User
10. FinOps Admin
11. TechOps Admin
12. Security Admin
13. Auditor

#### 7. **phtnai_permissions**
Permission definitions.

```sql
CREATE TABLE phtnai_permissions (
    permission_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    permission_code VARCHAR(100) UNIQUE NOT NULL,
    permission_name VARCHAR(255) NOT NULL,
    category VARCHAR(50),  -- tenant, user, agent, system
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE
);
```

**61 Permissions** across categories:
- Tenant Management (15)
- User Management (12)
- Agent Management (18)
- System Operations (16)

#### 8. **phtnai_role_permissions**
Role-permission mappings (many-to-many).

```sql
CREATE TABLE phtnai_role_permissions (
    role_permission_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    role_id UUID REFERENCES phtnai_roles(role_id),
    permission_id UUID REFERENCES phtnai_permissions(permission_id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(role_id, permission_id)
);

CREATE INDEX idx_role_permissions_role ON phtnai_role_permissions(role_id);
CREATE INDEX idx_role_permissions_permission ON phtnai_role_permissions(permission_id);
```

**320 Role-Permission Mappings** total

#### 9. **phtnai_user_roles**
User-role assignments (many-to-many).

```sql
CREATE TABLE phtnai_user_roles (
    user_role_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES phtnai_users(user_id),
    role_id UUID REFERENCES phtnai_roles(role_id),
    assigned_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    assigned_by UUID REFERENCES phtnai_users(user_id),
    is_active BOOLEAN DEFAULT TRUE,
    UNIQUE(user_id, role_id)
);
```

**Multi-role Support:** Users can have up to 3 roles

#### 10. **phtnai_user_permissions**
User-specific permissions (override or additional).

```sql
CREATE TABLE phtnai_user_permissions (
    user_permission_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES phtnai_users(user_id),
    permission_id UUID REFERENCES phtnai_permissions(permission_id),
    granted_by UUID REFERENCES phtnai_users(user_id),
    granted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE,
    UNIQUE(user_id, permission_id)
);
```

#### 11. **phtnai_projects**
Project management.

```sql
CREATE TABLE phtnai_projects (
    project_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_name VARCHAR(255) NOT NULL,
    project_code VARCHAR(100) UNIQUE NOT NULL,
    team_id UUID REFERENCES phtnai_teams(team_id),
    description TEXT,
    created_by UUID REFERENCES phtnai_users(user_id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE
);
```

#### 12. **phtnai_agent_catalog**
AI Agent catalog.

```sql
CREATE TABLE phtnai_agent_catalog (
    agent_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    agent_name VARCHAR(255) NOT NULL,
    agent_code VARCHAR(100) UNIQUE NOT NULL,
    agent_type VARCHAR(50),  -- super_agent, sub_agent, mcp_agent
    description TEXT,
    version VARCHAR(50),
    project_id UUID REFERENCES phtnai_projects(project_id),
    created_by UUID REFERENCES phtnai_users(user_id),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    is_deleted BOOLEAN DEFAULT FALSE
);
```

#### 13. **phtnai_conversations**
Conversation history.

```sql
CREATE TABLE phtnai_conversations (
    conversation_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES phtnai_users(user_id),
    agent_id UUID REFERENCES phtnai_agent_catalog(agent_id),
    session_id VARCHAR(255),
    started_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    ended_at TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE
);
```

#### 14. **phtnai_messages**
Individual messages in conversations.

```sql
CREATE TABLE phtnai_messages (
    message_id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    conversation_id UUID REFERENCES phtnai_conversations(conversation_id),
    role VARCHAR(20),  -- user, assistant, system
    content TEXT NOT NULL,
    metadata JSONB,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Entity Relationship Diagram

```
phtnai_tenant_groups
        ↓
phtnai_tenants
        ↓
phtnai_domains
        ↓
phtnai_teams
        ↓
phtnai_users ←→ phtnai_user_roles ←→ phtnai_roles ←→ phtnai_role_permissions ←→ phtnai_permissions
        ↓                                                         ↑
phtnai_user_permissions ────────────────────────────────────────┘
        ↓
phtnai_projects
        ↓
phtnai_agent_catalog
        ↓
phtnai_conversations
        ↓
phtnai_messages
```

### Sample Data

**Pre-populated with:**
- 4 Domains (Product, Engineering, Sales, Finance)
- 9 Teams across domains
- 16 Users at different hierarchy levels
- 13 Roles
- 61 Permissions
- 320 Role-Permission mappings

**Example Users:**

| Email | Password | Role | Permissions |
|-------|----------|------|-------------|
| superadmin@phtn.ai | SuperAdmin@2024 | Platform Admin | 61 (all) |
| tg.admin@phtn.ai | TgAdmin@123 | Tenant Platform Admin | 57 |
| eng.admin@phtn.ai | EngAdmin@123 | Domain Admin | 45 |
| aiml.lead@phtn.ai | AimlLead@123 | Team Lead + Architect | 40 |
| sophia.aiml@phtn.ai | AimlEng@123 | Architect + Builder | 22 |
| john.developer@phtn.ai | BackendDev@123 | Agent Builder | 14 |

### Database Initialization

**File:** `sql/new/environments/dev_seed.sql`

```bash
# Execute seed script
cd sql/new/environments
PGPASSWORD=root psql -h localhost -p 5432 -U postgres -d phtnai_platform_db -f dev_seed.sql
```

This creates:
- ✅ All 19+ database tables
- ✅ 4 domains, 9 teams, 16 users
- ✅ 13 roles, 61 permissions
- ✅ 320 role-permission mappings
- ✅ Complete RBAC system with zero errors

---

## API Documentation

### Authentication API

#### Login

**Endpoint:** `POST /api/v1/auth/login`

**Request:**
```json
{
  "email": "superadmin@phtn.ai",
  "password": "SuperAdmin@2024"
}
```

**Response:**
```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "token_type": "Bearer",
  "expires_in": 1800,
  "user": {
    "user_id": "123e4567-e89b-12d3-a456-426614174000",
    "email": "superadmin@phtn.ai",
    "name": "Super Admin",
    "roles": [
      {
        "role_id": "role-uuid",
        "role_name": "Platform Admin",
        "role_code": "platform_admin"
      }
    ],
    "permissions": [
      "tenant:create",
      "tenant:read",
      "user:create",
      "agent:deploy",
      ...
    ],
    "tenant_id": "tenant-uuid",
    "domain_id": "domain-uuid",
    "team_id": "team-uuid"
  }
}
```

#### Get Current User

**Endpoint:** `GET /api/v1/auth/me`

**Headers:**
```
Authorization: Bearer <access_token>
```

**Response:**
```json
{
  "user_id": "123e4567-e89b-12d3-a456-426614174000",
  "email": "superadmin@phtn.ai",
  "name": "Super Admin",
  "tenant_id": "tenant-uuid",
  "domain_id": "domain-uuid",
  "team_id": "team-uuid",
  "roles": [...],
  "permissions": [...],
  "is_active": true,
  "created_at": "2025-01-01T00:00:00Z"
}
```

### Tenant Management API

#### List Tenants

**Endpoint:** `GET /api/v1/tenants`

**Permission Required:** `tenant:list`

**Response:**
```json
{
  "tenants": [
    {
      "tenant_id": "uuid",
      "tenant_name": "PHTN Corporation",
      "tenant_code": "PHTN",
      "tenant_group_id": "group-uuid",
      "is_active": true,
      "created_at": "2025-01-01T00:00:00Z"
    }
  ],
  "total": 1
}
```

#### Create Tenant

**Endpoint:** `POST /api/v1/tenants`

**Permission Required:** `tenant:create`

**Request:**
```json
{
  "tenant_name": "New Customer Corp",
  "tenant_code": "NEWCUST",
  "tenant_group_id": "group-uuid",
  "description": "New customer tenant"
}
```

**Response:**
```json
{
  "tenant_id": "new-tenant-uuid",
  "tenant_name": "New Customer Corp",
  "tenant_code": "NEWCUST",
  "created_at": "2025-11-03T00:00:00Z"
}
```

### Agent Catalog API

#### List Agents

**Endpoint:** `GET /api/v1/phtnai_agent_catalog/agents`

**Permission Required:** `agent:read`

**Response:**
```json
{
  "agents": [
    {
      "agent_id": "uuid",
      "agent_name": "Super Flow Agent",
      "agent_code": "super_flow_agent",
      "agent_type": "super_agent",
      "version": "1.0.0",
      "description": "Main orchestrator agent",
      "is_active": true,
      "created_at": "2025-01-01T00:00:00Z"
    }
  ],
  "total": 1
}
```

#### Deploy Agent

**Endpoint:** `POST /api/v1/phtnai_agent_catalog/agents/{agent_id}/deploy`

**Permission Required:** `agent:deploy`

**Request:**
```json
{
  "environment": "production",
  "version": "1.0.0",
  "config": {
    "model": "gemini-1.5-pro",
    "temperature": 0.7
  }
}
```

**Response:**
```json
{
  "deployment_id": "deploy-uuid",
  "agent_id": "agent-uuid",
  "status": "deploying",
  "endpoint": "https://agent-service.phtn.ai/super_flow_agent",
  "deployed_at": "2025-11-03T00:00:00Z"
}
```

### Conversation API

#### Start Conversation

**Endpoint:** `POST /api/v1/conversations/start`

**Request:**
```json
{
  "agent_id": "agent-uuid",
  "message": "Hello, I need help with hiring",
  "context": {
    "user_role": "HR Manager",
    "department": "Engineering"
  }
}
```

**Response:**
```json
{
  "conversation_id": "conv-uuid",
  "session_id": "session-uuid",
  "response": "Hello! I'd be happy to help with your hiring needs. What position are you looking to fill?",
  "agent_name": "HR_Agent",
  "timestamp": "2025-11-03T00:00:00Z"
}
```

#### Continue Conversation

**Endpoint:** `POST /api/v1/conversations/{conversation_id}/message`

**Request:**
```json
{
  "message": "I need to hire a Senior Backend Developer"
}
```

**Response:**
```json
{
  "response": "Great! I'll help you with hiring a Senior Backend Developer. Let me gather some information...",
  "timestamp": "2025-11-03T00:00:01Z"
}
```

---

## WebSocket Communication

### Connection Flow

```
1. Frontend establishes WebSocket connection
   ws://localhost:8000/ws/phtn-frontend/{user_id}
        ↓
2. Middleware accepts connection and registers user
        ↓
3. Frontend sends message to agent
   {
     "workflow_id": "super_flow_agent",
     "message": "Hello",
     "context": {...}
   }
        ↓
4. Middleware forwards to agent via bridge
   ws://localhost:8080/ws/langflow-bridge/{workflow_id}
        ↓
5. Agent processes message and sends response
        ↓
6. Middleware forwards response to frontend
        ↓
7. Frontend receives and displays response
```

### Message Format

**Frontend → Middleware:**
```json
{
  "type": "agent_message",
  "workflow_id": "super_flow_agent",
  "user_id": "user-uuid",
  "message": "What's the weather?",
  "context": {
    "session_id": "session-uuid",
    "previous_messages": []
  },
  "timestamp": "2025-11-03T00:00:00Z"
}
```

**Middleware → Agent:**
```json
{
  "type": "process_request",
  "user_id": "user-uuid",
  "session_id": "session-uuid",
  "message": "What's the weather?",
  "context": {
    "user_roles": ["Team Lead"],
    "user_permissions": ["agent:execute"],
    "tenant_id": "tenant-uuid"
  }
}
```

**Agent → Middleware:**
```json
{
  "type": "agent_response",
  "user_id": "user-uuid",
  "session_id": "session-uuid",
  "response": "I don't have real-time weather data, but I can help you find that information.",
  "agent_name": "super_flow_agent",
  "metadata": {
    "model_used": "gemini-1.5-pro",
    "tokens_used": 150
  }
}
```

**Middleware → Frontend:**
```json
{
  "type": "agent_response",
  "response": "I don't have real-time weather data...",
  "agent_name": "super_flow_agent",
  "timestamp": "2025-11-03T00:00:01Z",
  "metadata": {
    "latency_ms": 250
  }
}
```

---

## Deployment & Infrastructure

### Docker Deployment

#### Frontend (PUI) Dockerfile

```dockerfile
# Production build not included in repo
# Standard React production build:

FROM node:18-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

#### Middleware Dockerfile

**File:** `pmiddleware/Dockerfile`

```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements
COPY requirements-production.txt .
RUN pip install --no-cache-dir -r requirements-production.txt

# Copy application code
COPY . .

# Create non-root user
RUN useradd -m -u 1000 appuser && chown -R appuser:appuser /app
USER appuser

# Expose port
EXPOSE 8000

# Run with gunicorn
CMD ["gunicorn", "main:app", \
     "--workers", "4", \
     "--worker-class", "uvicorn.workers.UvicornWorker", \
     "--bind", "0.0.0.0:8000", \
     "--timeout", "300", \
     "--access-logfile", "-", \
     "--error-logfile", "-"]
```

#### Agent Dockerfile

**File:** `pagents/Dockerfile`

```dockerfile
FROM python:3.11-slim

WORKDIR /app

# Install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy agent code
COPY . .

# Create non-root user
RUN useradd -m -u 1000 agentuser && chown -R agentuser:agentuser /app
USER agentuser

# Expose port
EXPOSE 8080

# Run server
CMD ["uvicorn", "server:app", "--host", "0.0.0.0", "--port", "8080"]
```

### Jenkins CI/CD Pipeline

#### Middleware Jenkinsfile

**File:** `pmiddleware/Jenkinsfile`

```groovy
pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = "phtn-middleware"
        DOCKER_REGISTRY = "gcr.io/photon-phtn-ai"
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Test') {
            steps {
                sh '''
                    python -m venv venv
                    . venv/bin/activate
                    pip install -r requirements-test.txt
                    pytest tests/ --cov=api --cov-report=xml
                '''
            }
        }
        
        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER} .
                    docker tag ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER} \
                               ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:latest
                """
            }
        }
        
        stage('Push to Registry') {
            steps {
                sh """
                    gcloud auth configure-docker
                    docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER}
                    docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:latest
                """
            }
        }
        
        stage('Deploy to Cloud Run') {
            steps {
                sh """
                    gcloud run deploy phtn-middleware \
                        --image ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER} \
                        --platform managed \
                        --region us-central1 \
                        --allow-unauthenticated \
                        --set-env-vars-file env-vars.yaml
                """
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

### Google Cloud Build

#### Agent cloudbuild.yaml

**File:** `pagents/cloudbuild.yaml`

```yaml
steps:
  # Build Docker image
  - name: 'gcr.io/cloud-builders/docker'
    args:
      - 'build'
      - '-t'
      - 'gcr.io/$PROJECT_ID/phtn-agents:$COMMIT_SHA'
      - '-t'
      - 'gcr.io/$PROJECT_ID/phtn-agents:latest'
      - '.'
  
  # Push Docker image
  - name: 'gcr.io/cloud-builders/docker'
    args:
      - 'push'
      - 'gcr.io/$PROJECT_ID/phtn-agents:$COMMIT_SHA'
  
  - name: 'gcr.io/cloud-builders/docker'
    args:
      - 'push'
      - 'gcr.io/$PROJECT_ID/phtn-agents:latest'
  
  # Deploy to Cloud Run
  - name: 'gcr.io/cloud-builders/gcloud'
    args:
      - 'run'
      - 'deploy'
      - 'phtn-agents'
      - '--image'
      - 'gcr.io/$PROJECT_ID/phtn-agents:$COMMIT_SHA'
      - '--region'
      - 'us-central1'
      - '--platform'
      - 'managed'
      - '--allow-unauthenticated'
      - '--set-env-vars-file'
      - 'env-vars.yaml'

images:
  - 'gcr.io/$PROJECT_ID/phtn-agents:$COMMIT_SHA'
  - 'gcr.io/$PROJECT_ID/phtn-agents:latest'

options:
  logging: CLOUD_LOGGING_ONLY
```

---

## Data Flow & Integration

### Complete Request Flow Example

**Scenario:** User logs in and starts a conversation with HR Agent

```
Step 1: User Login
┌─────────────┐
│   Browser   │
└──────┬──────┘
       │ POST /api/v1/auth/login
       │ { email, password }
       ▼
┌─────────────┐
│  Frontend   │ (React)
│  Component  │
└──────┬──────┘
       │ dispatch(loginAction(credentials))
       ▼
┌─────────────┐
│ Redux Saga  │
└──────┬──────┘
       │ axios.post('/api/v1/auth/login')
       ▼
┌─────────────┐
│ Middleware  │ (FastAPI)
│ Auth Router │
└──────┬──────┘
       │ auth_orchestrator.authenticate()
       ▼
┌─────────────┐
│ PostgreSQL  │
│  Database   │
└──────┬──────┘
       │ User data + roles + permissions
       ▼
┌─────────────┐
│ Middleware  │
│ JWT Utils   │
└──────┬──────┘
       │ Generate JWT with user context
       ▼
┌─────────────┐
│  Frontend   │
│   Redux     │
└──────┬──────┘
       │ Store token in cookies + Redux state
       ▼
┌─────────────┐
│   Browser   │ Authenticated ✓
└─────────────┘

Step 2: Start Conversation
┌─────────────┐
│   Browser   │
└──────┬──────┘
       │ User types message: "I need to hire a developer"
       ▼
┌─────────────┐
│  Frontend   │
│ Conversation│
│  Component  │
└──────┬──────┘
       │ WebSocket message
       │ ws.send({workflow_id, message, context})
       ▼
┌─────────────┐
│ Middleware  │
│  WebSocket  │
│   Bridge    │
└──────┬──────┘
       │ Validate JWT token
       │ Fetch user permissions from token payload
       ▼
┌─────────────┐
│ Middleware  │
│  Check Perm │
└──────┬──────┘
       │ user has 'agent:execute'? ✓
       ▼
┌─────────────┐
│ Middleware  │
│  Forward to │
│    Agent    │
└──────┬──────┘
       │ HTTP POST to agent service
       │ POST http://localhost:8080/super_flow_agent/process
       ▼
┌─────────────┐
│   Agent     │ (Google ADK)
│   Server    │
└──────┬──────┘
       │ super_flow_agent.process(request)
       ▼
┌─────────────┐
│   Agent     │
│   Router    │
└──────┬──────┘
       │ Analyze intent: "hiring" → route to HR_Agent
       ▼
┌─────────────┐
│  HR_Agent   │
│  Sub-Agent  │
└──────┬──────┘
       │ Call hiring_super_agent.process()
       ▼
┌─────────────┐
│    LLM      │ (Gemini/OpenAI/Claude)
│   Service   │
└──────┬──────┘
       │ Generate response
       ▼
┌─────────────┐
│  HR_Agent   │
│  Response   │
└──────┬──────┘
       │ "I can help you hire a developer. What role?"
       ▼
┌─────────────┐
│   Agent     │
│   Server    │
└──────┬──────┘
       │ Return response to middleware
       ▼
┌─────────────┐
│ Middleware  │
│  WebSocket  │
│   Bridge    │
└──────┬──────┘
       │ Log conversation to database
       ▼
┌─────────────┐
│ PostgreSQL  │
│ phtnai_     │
│ messages    │
└──────┬──────┘
       │ Message saved
       ▼
┌─────────────┐
│ Middleware  │
│  Forward to │
│  Frontend   │
└──────┬──────┘
       │ ws.send(response)
       ▼
┌─────────────┐
│  Frontend   │
│  WebSocket  │
│   Handler   │
└──────┬──────┘
       │ Update conversation state
       ▼
┌─────────────┐
│   Browser   │
│   Display   │
│   Response  │ "I can help you hire a developer..."
└─────────────┘
```

### Integration Points Summary

| Layer | Technology | Port | Protocol | Purpose |
|-------|------------|------|----------|---------|
| Frontend | React | 3000 | HTTP/WS | User Interface |
| Middleware | FastAPI | 8000 | HTTP/WS | API Gateway & RBAC |
| Database | PostgreSQL | 5432 | TCP | Data Persistence |
| Cache | Redis | 6379 | TCP | Session & Rate Limiting |
| Agent | Google ADK | 8080 | HTTP | AI Processing |
| LLM | OpenAI/Vertex | 443 | HTTPS | AI Inference |

---

## Security Implementation

### Password Security

**Hashing Algorithm:** Bcrypt with salt rounds = 12

```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def hash_password(password: str) -> str:
    """Hash password using bcrypt"""
    return pwd_context.hash(password)

def verify_password(plain_password: str, hashed_password: str) -> bool:
    """Verify password against hash"""
    return pwd_context.verify(plain_password, hashed_password)
```

### JWT Token Security

**Token Structure:**
```json
{
  "sub": "user-uuid",
  "email": "user@example.com",
  "tenant_id": "tenant-uuid",
  "domain_id": "domain-uuid",
  "team_id": "team-uuid",
  "roles": ["role-uuid-1", "role-uuid-2"],
  "permissions": ["tenant:read", "user:create", ...],
  "exp": 1730678400,
  "iat": 1730676600,
  "jti": "unique-token-id"
}
```

**Token Generation:**
```python
import jwt
from datetime import datetime, timedelta

def create_access_token(data: dict, expires_delta: timedelta = None):
    """Create JWT access token"""
    to_encode = data.copy()
    if expires_delta:
        expire = datetime.utcnow() + expires_delta
    else:
        expire = datetime.utcnow() + timedelta(minutes=30)
    
    to_encode.update({
        "exp": expire,
        "iat": datetime.utcnow(),
        "jti": str(uuid.uuid4())
    })
    
    encoded_jwt = jwt.encode(
        to_encode,
        settings.secret_key,
        algorithm=settings.algorithm
    )
    return encoded_jwt
```

### Rate Limiting

**Implementation:**
```python
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(
    key_func=get_remote_address,
    default_limits=["100/minute"]
)

# Apply to specific endpoint
@app.post("/api/v1/auth/login")
@limiter.limit("5/minute")  # 5 login attempts per minute
async def login(credentials: LoginRequest):
    ...
```

### CORS Configuration

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "https://app.phtn.ai",
        "https://admin.phtn.ai"
    ],
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE"],
    allow_headers=["Authorization", "Content-Type"],
    expose_headers=["X-Request-ID"],
    max_age=3600
)
```

### SQL Injection Prevention

**Using parameterized queries with asyncpg:**
```python
# SAFE - Parameterized query
query = "SELECT * FROM phtnai_users WHERE email = $1"
user = await conn.fetchrow(query, email)

# UNSAFE - Never do this
query = f"SELECT * FROM phtnai_users WHERE email = '{email}'"  # ❌
```

### XSS Prevention

**Frontend:** React automatically escapes content
**Backend:** Pydantic validates and sanitizes input

```python
from pydantic import BaseModel, validator

class UserInput(BaseModel):
    message: str
    
    @validator('message')
    def sanitize_message(cls, v):
        # Remove potentially dangerous characters
        return v.replace('<', '&lt;').replace('>', '&gt;')
```

---

## Environment Configuration

### Frontend (.env)

```bash
# API Configuration
REACT_APP_API_BASE_URL=http://localhost:8000
REACT_APP_WEBSOCKET_URL=ws://localhost:8000

# Environment
REACT_APP_ENV=development
REACT_APP_DEBUG=true

# Feature Flags
REACT_APP_ENABLE_VOICE=true
REACT_APP_ENABLE_CODE_EDITOR=true

# Analytics (optional)
REACT_APP_ANALYTICS_ID=
```

### Middleware (.env)

```bash
# Application
SERVICE_NAME=phtn-middleware
SERVICE_VERSION=1.0.0
ENVIRONMENT=development
DEBUG=true
HOST=0.0.0.0
PORT=8000

# Database
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your-db-password
DB_NAME=phtnai_platform_db

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_PASSWORD=

# JWT
SECRET_KEY=your-super-secret-key-change-this-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
REFRESH_TOKEN_EXPIRE_DAYS=7

# CORS
CORS_ENABLED=true
ALLOWED_ORIGINS=http://localhost:3000,http://localhost:3001
ALLOWED_HOSTS=*

# Security
ENABLE_AUTH=true
ENABLE_RATE_LIMITING=true

# Agent Service
AGENT_SERVICE_URL=http://localhost:8080

# Logging
LOG_LEVEL=INFO

# OpenTelemetry
OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4317
OTEL_SERVICE_NAME=phtn-middleware
```

### Agent Layer (.env)

```bash
# Google Cloud
PROJECT_ID=photon-phtn-ai
REGION=us-central1
GOOGLE_APPLICATION_CREDENTIALS=./credentials.json

# Session Service
SESSION_SERVICE_URI=

# LLM Configuration
DEFAULT_MODEL=gemini-1.5-pro
TEMPERATURE=0.7
MAX_TOKENS=2048

# Alternative Models
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...

# Middleware
MIDDLEWARE_URL=http://localhost:8000
MIDDLEWARE_API_KEY=your-api-key

# Logging
LOG_LEVEL=INFO
ENABLE_CLOUD_LOGGING=true

# Server
PORT=8080
HOST=0.0.0.0
```

---

## Development Workflow

### Local Development Setup

#### Prerequisites
- Node.js 18+
- Python 3.11+
- PostgreSQL 14+
- Redis 7+
- Docker (optional)

#### 1. Setup Frontend

```bash
cd pui
npm install
cp .env.example .env
# Edit .env with local settings
npm start
# Runs on http://localhost:3000
```

#### 2. Setup Middleware

```bash
cd pmiddleware

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Setup database
createdb phtnai_platform_db
psql -d phtnai_platform_db -f sql/new/environments/dev_seed.sql

# Copy environment file
cp .env.example .env
# Edit .env with local settings

# Run server
python main.py
# Or with auto-reload:
uvicorn main:app --reload --port 8000
```

#### 3. Setup Agent Layer

```bash
cd pagents

# Create virtual environment
python -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Copy environment file
cp .env.example .env
# Edit .env with credentials

# Run server
python server.py
# Runs on http://localhost:8080
```

### Testing

#### Frontend Tests
```bash
cd pui
npm test
npm run test:coverage
```

#### Middleware Tests
```bash
cd pmiddleware
pytest tests/ -v
pytest tests/ --cov=api --cov-report=html
```

#### Agent Tests
```bash
cd pagents
pytest tests/ -v
```

### Code Quality

#### Frontend
```bash
# Linting
npm run lint

# Format
npm run format

# Type check
npm run type-check
```

#### Backend (Middleware & Agents)
```bash
# Linting
flake8 .

# Format
black .
isort .

# Type check
mypy .
```

---

## Troubleshooting Guide

### Common Issues

#### 1. Frontend Cannot Connect to Middleware

**Symptoms:**
- API calls fail with CORS errors
- Network errors in browser console

**Solutions:**
```bash
# Check middleware is running
curl http://localhost:8000/health

# Verify CORS settings in middleware .env
ALLOWED_ORIGINS=http://localhost:3000

# Check frontend API URL
REACT_APP_API_BASE_URL=http://localhost:8000
```

#### 2. JWT Token Expired

**Symptoms:**
- 401 Unauthorized errors
- Automatic logout

**Solutions:**
```javascript
// Frontend: Implement token refresh
const refreshToken = async () => {
  const response = await axios.post('/api/v1/auth/refresh');
  Cookies.set('access_token', response.data.access_token);
};
```

#### 3. Database Connection Failed

**Symptoms:**
- Middleware crashes on startup
- Database-related errors in logs

**Solutions:**
```bash
# Verify PostgreSQL is running
pg_isready -h localhost -p 5432

# Check credentials
psql -h localhost -p 5432 -U postgres -d phtnai_platform_db

# Verify middleware .env settings
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=your-password
DB_NAME=phtnai_platform_db
```

#### 4. Agent Service Not Responding

**Symptoms:**
- Conversation requests timeout
- No response from agents

**Solutions:**
```bash
# Check agent service health
curl http://localhost:8080/health

# Verify middleware can reach agent
curl http://localhost:8080/super_flow_agent

# Check agent .env
MIDDLEWARE_URL=http://localhost:8000
```

#### 5. WebSocket Connection Fails

**Symptoms:**
- Real-time updates don't work
- WebSocket connection refused

**Solutions:**
```javascript
// Frontend: Check WebSocket URL
const socket = io('ws://localhost:8000', {
  transports: ['websocket'],
  reconnection: true,
  reconnectionAttempts: 5
});

socket.on('connect_error', (error) => {
  console.error('WebSocket connection error:', error);
});
```

#### 6. Permission Denied Errors

**Symptoms:**
- 403 Forbidden on API calls
- User cannot access features

**Solutions:**
```sql
-- Check user permissions
SELECT 
  u.email,
  r.role_name,
  array_agg(p.permission_code) as permissions
FROM phtnai_users u
JOIN phtnai_user_roles ur ON u.user_id = ur.user_id
JOIN phtnai_roles r ON ur.role_id = r.role_id
JOIN phtnai_role_permissions rp ON r.role_id = rp.role_id
JOIN phtnai_permissions p ON rp.permission_id = p.permission_id
WHERE u.email = 'user@example.com'
GROUP BY u.email, r.role_name;
```

### Performance Optimization

#### Database Query Optimization

```sql
-- Add indexes for frequently queried columns
CREATE INDEX idx_users_email ON phtnai_users(email);
CREATE INDEX idx_user_roles_user ON phtnai_user_roles(user_id);
CREATE INDEX idx_messages_conversation ON phtnai_messages(conversation_id);

-- Analyze query performance
EXPLAIN ANALYZE
SELECT * FROM phtnai_users WHERE email = 'user@example.com';
```

#### Redis Caching

```python
# Cache user permissions
async def get_user_permissions_cached(user_id: str) -> list:
    cache_key = f"user_permissions:{user_id}"
    
    # Check cache
    cached = await redis_client.get(cache_key)
    if cached:
        return json.loads(cached)
    
    # Fetch from database
    permissions = await get_user_permissions(user_id)
    
    # Cache for 5 minutes
    await redis_client.setex(cache_key, 300, json.dumps(permissions))
    
    return permissions
```

---

## Conclusion

This documentation provides a comprehensive, low-level analysis of the PHTN AI Platform's three-tier architecture:

1. **Frontend (PUI)**: React + TypeScript + Redux for rich user interface
2. **Middleware (pmiddleware)**: FastAPI + PostgreSQL for API gateway, RBAC, and data management
3. **Agent Layer (pagents)**: Google ADK for hierarchical AI agent system

**Key Features:**
- ✅ Complete RBAC with 8-level hierarchy
- ✅ 61 permissions across 13 roles
- ✅ JWT-based authentication
- ✅ Real-time WebSocket communication
- ✅ Multi-tenant architecture
- ✅ Enterprise observability (OpenTelemetry)
- ✅ CI/CD pipelines (Jenkins + Google Cloud Build)
- ✅ Docker containerization
- ✅ Comprehensive security implementation

**Repository Links:**
- Frontend: https://github.com/46surya/pui
- Middleware: https://github.com/46surya/pmiddleware
- Agents: https://github.com/46surya/pagents

**Support & Contact:**
For questions or issues, refer to the individual repository documentation or contact the development team.

---

**End of Documentation**
