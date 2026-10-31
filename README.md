# Allo Bank Frontend Technical Assignment — SpaceX Rockets

[![Vue 3](https://img.shields.io/badge/Vue-3.4.31-42b883.svg)](https://vuejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.6-3178c6.svg)](https://www.typescriptlang.org/)
[![Vuetify](https://img.shields.io/badge/Vuetify-3.6.14-1867c0.svg)](https://vuetifyjs.com/)
[![Pinia](https://img.shields.io/badge/Pinia-4.1.0-ffd859.svg)](https://pinia.vuejs.org/)
[![Vite](https://img.shields.io/badge/Vite-5.4.10-646cff.svg)](https://vitejs.dev/)

A clean, responsive, and type-safe SpaceX Rocket Fleet web application developed for the **Allo Bank Frontend Technical Assignment**. The application allows users to browse SpaceX rockets, filter catalog entries in real-time, inspect granular vehicle specifications, add new custom rockets client-side, and gracefully recover from API network failures.

---

## Candidate Information
* **Candidate Name:** Abu Dujanah Siregar
* **GitHub Profile:** [`DujanahSr`](https://github.com/DujanahSr)
* **Feature Branch:** `feat/allo-spacex`

---

## Key Technical Highlights & Architecture

### 1. Robust State Management with Pinia
* Centralized state in `src/stores/rocket.ts` managing API caching, local custom rockets, search query filtering, and asynchronous loading/error flags.
* **Smart Cache & Rate Limit Resilience:** The Space Devs API limits anonymous requests to 15 per hour. The store implements in-memory caching to avoid redundant API hits when navigating between the list and detail views.
* **Local Persistence:** Client-added rockets are persisted in browser `localStorage`, ensuring data survives page reloads.

### 2. Component-Based Architecture
* `RocketCard.vue`: Modular rocket card featuring image error handling, country/custom badges, and responsive typography.
* `RocketSkeleton.vue`: Realistic skeleton loader card maintaining visual continuity during data retrieval.
* `AddRocketDialog.vue`: Modal dialog with reactive form validation for adding custom rockets client-side.
* `ErrorState.vue`: Dedicated error recovery UI displaying informative error messages with an interactive **Retry** action.

### 3. Graceful Data Degradation
* Handles rockets with missing `image_url` values using an aesthetic fallback render (`FALLBACK_ROCKET_IMAGE`).
* Formats `launch_cost` using `Intl.NumberFormat` (e.g., `$67,000,000`), defaulting to `"N/A"` if undefined.
* Formats `maiden_flight` dates with `Intl.DateTimeFormat`, defaulting to `"N/A"` if missing.

### 4. Comprehensive UI States
* **Loading State:** Responsive grid of skeleton cards while awaiting response.
* **Error / Retry State:** Graceful alert banner and retry button if the network or API request fails.
* **Success State:** Interactive grid with filter indicators and empty search state.

---

## How to Build and Run Locally

### Prerequisites
* Node.js v20+ or v22+ (tested on Node v23.3.0)
* npm 10+

### Setup Instructions
```bash
# Clone the repository
git clone https://github.com/DujanahSr/allo-frontend-test.git
cd allo-frontend-test

# Switch to the feature branch
git checkout feat/allo-spacex

# Install dependencies
npm install

# Start the Vite development server
npm run dev
```
Open `http://localhost:3000` in your web browser.

### Quality Verification Commands
```bash
# Run TypeScript type check and build production bundle
npm run build

# Run ESLint validation
npm run lint

# Preview production build
npm run preview
```

---

## Original Challenge Specification

In this assignment, you’re assigned to create a website that displays rockets. This website only has two screens: rocket list screen and rocket detail screen. Here are the requirements:

### Functional Requirements
- As a user, I want to see a list of rockets in the rocket list screen (Show each rocket image, rocket name, and rocket description)
- As a user, I want to be able to filter the rockets in the rocket list screen
- As a user, I want to be able to add the new rocket in the rocket list screen (the API is read-only, so the new rocket only needs to appear in the running app)
- As a user, I want to be able to see the rocket detail by clicking a rocket in the rocket list screen (Show rocket image, rocket name, rocket description, cost per launch, country, first flight)
- As a user, I want both screens to still display correctly when some rocket data is missing

### API Specification
* **Rocket List:** `GET https://lldev.thespacedevs.com/2.2.0/config/launcher/?manufacturer__name=SpaceX&mode=detailed&limit=20`
* **Single Rocket:** `GET https://lldev.thespacedevs.com/2.2.0/config/launcher/:id/`

| Requirement | API Field |
| :--- | :--- |
| rocket image | `image_url` |
| rocket name | `full_name` |
| description | `description` |
| cost per launch | `launch_cost` |
| country | `manufacturer.country_code` |
| first flight | `maiden_flight` |

---

## Submission Checklist

- [x] Forked repository under `DujanahSr/allo-frontend-test`
- [x] Dedicated feature branch: `feat/allo-spacex`
- [x] List and Detail screens implemented with Vue 3, TypeScript & Vuetify
- [x] State management implemented with Pinia
- [x] 3 UI states implemented: Loading (Skeleton), Fail/Retry, and Success
- [x] Client-side Add Rocket feature implemented with validation
- [x] Search filter implemented
- [x] Missing and null values handled gracefully with fallbacks
- [x] 100% clean production build (`npm run build` and `npm run lint` pass with 0 errors)
