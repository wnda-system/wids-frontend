# wids-frontend

Operator dashboard for [WNDA System](https://github.com/wnda-system). Network staff use it to watch live alerts, review history and check the capture device.

> 🚧 **Status:** project scaffolding in progress.

## Stack

| Concern | Choice |
|---|---|
| Framework | React + Vite + TypeScript |
| Data fetching | TanStack Query |
| UI | Tailwind CSS + shadcn/ui |
| Charts | ECharts |
| API types | Generated from the backend's OpenAPI spec with [openapi-typescript](https://openapi-ts.dev/) |

## Pages

| Page | Shows |
|---|---|
| Login | Staff sign-in (no self sign-up; accounts are created by an administrator) |
| Overview | Open alerts, device status and key numbers at a glance |
| Alerts | Attack and performance alerts with filters, details and acknowledgement |
| History | Charts of past network metrics and detections |
| Devices | Capture device health: online status, CPU, disk, clock offset |

New alerts appear without refreshing, through Server-Sent Events.

## Authentication

The backend issues an `httpOnly` session cookie. The dashboard is served from the same origin as the API, so no tokens are stored in JavaScript.

## Getting started

```bash
npm install
npm run gen:api   # regenerate API types from wids-backend/api/openapi.yaml
npm run dev
```

During early development the app can run against a mock API generated from the OpenAPI examples.

## Contributing

- Branch from `dev`, open a pull request back to `dev`. `main` receives releases only.
- Pull request titles follow [Conventional Commits](https://www.conventionalcommits.org/). Pull requests are squash-merged.
- Do not edit generated API types by hand.

## Team

- Naran Wongvuttisaroj
- Boonyapon Boontub
