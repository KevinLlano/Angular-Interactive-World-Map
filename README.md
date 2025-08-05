# WorldMap - Interactive World Map Application

This is an Angular 18 application that displays an interactive world map where users can hover over countries to retrieve detailed information about them from the World Bank API.

## Table of Contents
1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Components](#components)
4. [Services](#services)
5. [Configuration Files](#configuration-files)
6. [Styling](#styling)
7. [Development](#development)
8. [Project Structure](#project-structure)

## Overview

The WorldMap application is a standalone Angular application that presents an SVG-based world map. When users hover over a country, the application fetches and displays relevant data including capital city, income level, region, and geographical coordinates.

**Key Features:**
- Interactive SVG world map
- Real-time country data from World Bank API
- Hover effects for enhanced user experience
- Modern Angular 18 standalone architecture
- Tailwind CSS for styling

## Architecture

This application follows Angular 18's modern standalone component architecture:
- **Standalone Components**: No NgModules required
- **Reactive Programming**: Uses RxJS Observables for API calls
- **External API Integration**: World Bank API for country data
- **Modular Design**: Separation of concerns with dedicated service layer

## Components

### 1. AppComponent (`src/app/app.component.ts`)

**Purpose**: Root component that serves as the application's entry point.

**Key Details:**
- **Selector**: `app-root`
- **Type**: Standalone component
- **Template**: Contains only `<router-outlet></router-outlet>`
- **Styling**: No custom styles (empty CSS file)
- **Properties**:
  - `title`: Set to 'world-map'

**Functionality:**
- Acts as the shell for the entire application
- Uses Angular Router to render child components
- Minimal implementation focusing on routing delegation

**Template Structure:**
```html
<router-outlet></router-outlet>
```

### 2. MapComponent (`src/app/map/map.component.ts`)

**Purpose**: Core component that renders the interactive world map and handles user interactions.

**Key Details:**
- **Selector**: `app-map`
- **Type**: Standalone component
- **Imports**: `CommonModule` for Angular directives
- **Dependencies**: Injects `ApiService` for data fetching

**Properties:**
- `country`: Object to store selected country information
  - Contains: capital, income, region, place, latitude, longitude

**Methods:**

#### `setCountryData(event: any)`
- **Purpose**: Handles country selection events from map interaction
- **Parameters**: 
  - `event`: DOM event containing target country information
- **Functionality**:
  1. Extracts country ID from `event.target.id`
  2. Retrieves country title from `event.target.getAttribute('title')`
  3. Calls `ApiService.setCountryData()` with country ID
  4. Updates local `country` object with API response
  5. Adds the country's display name as `place` property

**Template Features:**
- Contains comprehensive SVG world map (1314 lines)
- Each country path has:
  - `(mouseenter)="setCountryData($event)"` event binding
  - Unique `id` attribute (country code)
  - `title` attribute (country name)
- Responsive design with proper viewport settings

**Styling:**
- Hover effects defined in CSS
- Orange highlight on country hover

### Services

### 3. ApiService (`src/app/api.service.ts`)

**Purpose**: Handles all external API communications with the World Bank API.

**Key Details:**
- **Type**: Injectable service with root-level provision
- **Dependencies**: Uses `HttpClient` for HTTP requests
- **API Base URL**: `http://api.worldbank.org/v2/country/`

**Methods:**

#### `fetchCountryData(code: string)`
- **Purpose**: Makes HTTP GET request to World Bank API
- **Parameters**: 
  - `code`: ISO country code (e.g., 'US', 'CA', 'GB')
- **Returns**: Observable containing raw API response
- **API URL Format**: `http://api.worldbank.org/v2/country/{code}?format=JSON&per_page=300`
- **Features**:
  - Console logging for debugging
  - JSON format specification
  - High page limit (300) to ensure complete data

#### `setCountryData(code: string)`
- **Purpose**: Processes and transforms raw API data into application-friendly format
- **Parameters**: 
  - `code`: ISO country code
- **Returns**: Observable containing processed country data
- **Data Transformation**:
  - Extracts first country object from API response (`data[1][0]`)
  - Maps to standardized format:
    ```typescript
    {
      capital: countryData.capitalCity,
      income: countryData.incomeLevel.value,
      region: countryData.region.value,
      countryLatitude: countryData.latitude,
      countryLongitude: countryData.longitude
    }
    ```
- **Pattern**: Uses Subject/Observable pattern for reactive data flow

## Configuration Files

### 4. App Configuration (`src/app/app.config.ts`)

**Purpose**: Central configuration for the Angular application.

**Key Details:**
- **Type**: ApplicationConfig object
- **Exports**: `appConfig` constant

**Providers Configuration:**
- **Router**: `provideRouter(routes)` - Enables routing functionality
- **HTTP Client**: `importProvidersFrom(HttpClientModule)` - Enables HTTP requests
- **Note**: HttpClientModule shows as deprecated in IDE but remains functional

### 5. Routing Configuration (`src/app/app.routes.ts`)

**Purpose**: Defines application routing structure.

**Route Configuration:**
- **Default Route**: `{ path: '', component: MapComponent }`
- **Behavior**: Empty path redirects to MapComponent
- **Type**: Simple single-page application routing

### 6. Main Bootstrap (`src/main.ts`)

**Purpose**: Application entry point and bootstrap configuration.

**Functionality:**
- Bootstraps `AppComponent` with `appConfig`
- Error handling for bootstrap failures
- Uses Angular's standalone bootstrapping approach

### 7. HTML Template (`src/index.html`)

**Purpose**: Main HTML document template.

**Key Features:**
- **Title**: "WorldMap"
- **Viewport**: Responsive design configuration
- **Base href**: Set to "/"
- **Favicon**: References favicon.ico
- **App Root**: `<app-root></app-root>` element for Angular app

## Styling

### 8. Global Styles (`src/styles.css`)

**Purpose**: Global stylesheet configuration.

**Framework Integration:**
- **Tailwind CSS**: Complete integration with all utilities
- **Directives**: `@tailwind base`, `@tailwind components`, `@tailwind utilities`

### 9. Component Styles (`src/app/map/map.component.css`)

**Purpose**: Map-specific styling.

**Hover Effects:**
```css
path:hover {
    fill: orange;
}
```

### 10. Tailwind Configuration (`tailwind.config.js`)

**Purpose**: Tailwind CSS customization.

**Configuration:**
- **Content**: Scans all HTML and TypeScript files in src
- **Theme**: Default theme with extension capability
- **Plugins**: Empty array (no additional plugins)

## Development

### Package Dependencies

**Runtime Dependencies:**
- Angular 18.1.0 (complete framework)
- RxJS 7.8.0 (reactive programming)
- Zone.js 0.14.3 (change detection)
- TypeScript 5.5.2 (type safety)

**Development Dependencies:**
- Angular CLI 18.1.4 (development tools)
- Tailwind CSS 3.4.10 (utility-first CSS)
- PostCSS & Autoprefixer (CSS processing)
- Karma & Jasmine (testing framework)

### Build Configuration (`angular.json`)

**Key Settings:**
- **Output Path**: `dist/world-map`
- **Source Root**: `src`
- **Prefix**: `app`
- **Build Tool**: Angular DevKit Application Builder
- **Assets**: Public folder contents
- **Styles**: Global styles.css only
- **Scripts**: No additional scripts

**Build Budgets:**
- **Initial Bundle**: 500kB warning, 1MB error
- **Component Styles**: 2kB warning, 4kB error

### Development Commands

```bash
# Development server
ng serve

# Production build
ng build

# Unit tests
ng test

# Component generation
ng generate component component-name

# Service generation
ng generate service service-name
```

## Project Structure

```
world-map/
├── src/
│   ├── app/
│   │   ├── map/                     # Map component
│   │   │   ├── map.component.ts     # Map logic & interaction
│   │   │   ├── map.component.html   # SVG world map (1314 lines)
│   │   │   ├── map.component.css    # Map-specific styles
│   │   │   └── map.component.spec.ts # Unit tests
│   │   ├── api.service.ts           # World Bank API integration
│   │   ├── api.service.spec.ts      # Service unit tests
│   │   ├── app.component.ts         # Root component
│   │   ├── app.component.html       # Router outlet template
│   │   ├── app.component.css        # Root component styles
│   │   ├── app.component.spec.ts    # Root component tests
│   │   ├── app.config.ts            # Application configuration
│   │   └── app.routes.ts            # Routing configuration
│   ├── index.html                   # Main HTML template
│   ├── main.ts                      # Application bootstrap
│   └── styles.css                   # Global Tailwind CSS
├── public/
│   └── favicon.ico                  # Application icon
├── angular.json                     # Angular CLI configuration
├── package.json                     # Dependencies & scripts
├── tailwind.config.js               # Tailwind CSS configuration
├── tsconfig.json                    # TypeScript configuration
├── tsconfig.app.json                # App-specific TypeScript config
└── README.md                        # This documentation
```

## Data Flow

1. **User Interaction**: User hovers over country in SVG map
2. **Event Handling**: `setCountryData()` method triggered with mouse event
3. **Data Extraction**: Country ID and name extracted from DOM event
4. **API Call**: `ApiService.setCountryData()` called with country code
5. **HTTP Request**: Service makes request to World Bank API
6. **Data Processing**: Raw API response transformed to application format
7. **UI Update**: Component updates with country information
8. **Display**: Country data displayed to user

## API Integration

**World Bank API Details:**
- **Base URL**: `http://api.worldbank.org/v2/country/`
- **Format**: JSON
- **Response Structure**: Array with metadata and country data
- **Data Fields Used**:
  - `capitalCity`: Capital city name
  - `incomeLevel.value`: Economic income classification
  - `region.value`: Geographic region
  - `latitude`: Geographic latitude
  - `longitude`: Geographic longitude

## Testing

**Test Files Included:**
- `app.component.spec.ts`: Root component tests
- `map.component.spec.ts`: Map component tests  
- `api.service.spec.ts`: API service tests

**Testing Framework:**
- **Jasmine**: Test framework
- **Karma**: Test runner
- **Angular Testing Utilities**: Component testing tools

## Future Enhancements

**Potential Improvements:**
1. **Error Handling**: Add comprehensive error handling for API failures
2. **Loading States**: Implement loading indicators during API calls
3. **Caching**: Add response caching for better performance
4. **Accessibility**: Enhance keyboard navigation and screen reader support
5. **Mobile Optimization**: Improve touch interactions for mobile devices
6. **Additional Data**: Integrate more country statistics and information
7. **Search Functionality**: Add country search capability
8. **Zoom Controls**: Implement map zoom and pan features


