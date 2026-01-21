# Architecture Documentation

## Overview

The Nexus Dashboard v.2 is built on a modular architecture designed for scalability and reproducibility. This document explains the core architectural decisions and how components interact.

## Core Principles

1. **Modularity**: Components are self-contained and reusable
2. **Separation of Concerns**: Clear boundaries between data, logic, and presentation
3. **Configuration-Driven**: Centralized configuration for easy expansion
4. **Standardization**: Consistent patterns across all pages and components

## Architecture Layers

### 1. Entry Point Layer

**File**: `app.py`

- Main Streamlit application entry point
- Sets up page navigation using Streamlit's `st.navigation()`
- Loads global styles and configuration
- Routes to appropriate pages based on user selection

### 2. Page Layer

**Location**: `pages/`

Pages are organized hierarchically:
- `0_home.py`: Home page with quadrant system
- `pillars/`: Pillar landing pages (1_pillar_1.py through 1_pillar_4.py)
- `themes/`: Theme pages (pillarX_themeY.py)
- Topic pages: Direct topic implementations (3_topic_4_1.py, etc.)

Each page follows a consistent structure:
1. Page configuration (`st.set_page_config`)
2. Style injection
3. Component imports
4. Content rendering

### 3. Component Layer

**Location**: `app_core/components/`

Reusable UI components:

- **`home_page_components.py`**: 
  - `render_introduction_section()`: Introduction text
  - `render_transition_block()`: Transition between sections
  - `render_systems_loop_grid()`: 4-quadrant grid visualization
  - `render_step2_section()`: Pillar cards section
  - `render_footer()`: Footer component

- **`quadrant_card.py`**: 
  - Quadrant card component with mini charts
  - Used in pillar pages

- **`flip_card_component.py`**: 
  - Interactive flip cards for pillars
  - Front/back content with animations

- **`navigation.py`**: 
  - Navigation components
  - Breadcrumbs and navigation helpers

### 4. Configuration Layer

**Location**: `app_core/config/`

- **`pillars_config.py`**: 
  - Central configuration for all pillars, themes, and topics
  - Defines structure, colors, descriptions, and routes
  - Single source of truth for navigation structure

### 5. Layout Layer

**Location**: `app_core/layouts/`

Standardized layout templates:

- **`pillar_layout.py`**: 
  - Standard layout for pillar pages
  - Theme listing and navigation

- **`topic_layout.py`**: 
  - Standard layout for topic pages
  - Indicator display patterns

### 6. Visualization Layer

**Location**: `universal_viz.py`

Core visualization utilities:
- `render_indicator_section()`: Standardized indicator rendering
- Chart type handlers (heatmap, line, bar, stacked_bar, map)
- Filter application and data processing
- Multi-view tab support (Graph, Map, Data Table)

### 7. Data Layer

**Location**: `data/`

- `nexus.parquet`: Main indicator dataset
- Reference data files (country codes, coordinates, etc.)
- Data is pre-processed and ready for visualization

### 8. Utility Layer

**Location**: `utils.py`

Helper functions:
- Data loading and processing
- Filter utilities
- Common data transformations

## Data Flow

```
User Interaction
    ↓
app.py (Navigation)
    ↓
Page (pages/*.py)
    ↓
Components (app_core/components/*)
    ↓
Layouts (app_core/layouts/*)
    ↓
Visualizations (universal_viz.py)
    ↓
Data (data/*.parquet, *.csv)
```

## Configuration System

### Pillars Configuration

All pillar, theme, and topic definitions are in `app_core/config/pillars_config.py`:

```python
PILLARS = {
    "pillar_1": {
        "number": 1,
        "title": "...",
        "description": "...",
        "color": "#1B75BB",
        "themes": {
            "theme_1": {
                "title": "...",
                "description": "...",
                "route": "pages/themes/pillar1_theme1.py"
            }
        }
    }
}
```

This centralized approach allows:
- Easy addition of new pillars/themes/topics
- Consistent structure across the dashboard
- Single point of maintenance

## Component Patterns

### Standard Indicator Module

Each indicator follows this structure:

1. **Header**: Title, info icon, analytical question
2. **Local Filters**: Year, Country, Region, Reset
3. **Multi-View Tabs**: Graph View, Map View, Data Table
4. **Supporting Information**: Expandable sections

### Standard Page Structure

1. **Topic Description** (if applicable)
2. **Orange Divider**
3. **"Key Indicators Overview"** heading
4. **Indicator Modules** (layout depends on count)
5. **Orange Divider**
6. **Data Availability Section**

## Styling System

**Location**: `app_core/styles/style_osaa.css`

- OSAA brand colors
- Consistent component styling
- Responsive design patterns
- Custom CSS for special components

## Navigation System

Streamlit's native navigation (`st.navigation()`) is used in `app.py`:

- All pages registered in a single list
- Automatic sidebar navigation
- Page routing handled by Streamlit

## Extension Points

### Adding a New Pillar

1. Add to `PILLARS` in `pillars_config.py`
2. Create pillar page in `pages/pillars/`
3. Add quadrant to home page configuration
4. Create theme pages

### Adding a New Theme

1. Add theme to pillar in `pillars_config.py`
2. Create theme page in `pages/themes/`
3. Update pillar page navigation

### Adding a New Topic

1. Create topic page following `UNIFIED_DASHBOARD_TEMPLATE.md`
2. Add to navigation in `app.py`
3. Implement indicators using standard patterns

### Adding a New Indicator

1. Follow indicator module template
2. Use `universal_viz.py` for rendering
3. Implement local filters
4. Add supporting information

## Performance Considerations

- **Data Loading**: Data is loaded once and cached
- **Lazy Loading**: Components load on demand
- **Session State**: Used for filter persistence
- **Caching**: Streamlit caching for expensive operations

## Security Considerations

- No user authentication (public dashboard)
- Data is read-only
- No external API calls from client
- All data processing server-side

## Testing Strategy

- Manual testing via Indicator Explorer
- Data validation scripts
- Visual regression testing (manual)
- Component isolation testing

## Future Enhancements

- Automated testing framework
- Performance monitoring
- User analytics
- Export functionality
- Advanced filtering
