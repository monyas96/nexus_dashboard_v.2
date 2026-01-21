#  Nexus Dashboard v.2 (4 Pillars - Module Complexity)

![OSAA Logo](assets/logos/OSAA%20identifier%20color.png)

**A comprehensive, modular dashboard framework implementing the full Nexus Conceptual Framework across all four pillars, designed for expansion and reproducibility.**

---

##  Project Overview

The **Nexus Dashboard v.2** is the full implementation of the Nexus Conceptual Framework, featuring a complete 4-pillar architecture with modular components designed for systematic expansion across all themes and topics. This repository represents the **"module complexity"** version—a production-ready framework that can be extended to cover the entire Nexus framework.

### Key Characteristics

-  **Full 4-Pillar Architecture**: Complete implementation of all four Nexus pillars
- **Modular Design**: Reusable components and layouts for easy expansion
-  **Quadrant Logic**: Interactive 4-quadrant visualization system on the home page
-  **Scalable Structure**: Designed to accommodate all themes and topics across pillars
-  **Reproducible**: Clear documentation and standardized patterns for development

---

##  How This Differs from OSAA_DRM

### **Nexus Dashboard v.2** (This Repository)
- **Scope**: Full 4-pillar framework (all pillars, themes, and topics)
- **Purpose**: Development and expansion platform for the complete Nexus framework
- **Complexity**: Module complexity—full architecture with reusable components
- **Target Users**: Developers, researchers, and analysts expanding the dashboard
- **Status**: Framework ready for expansion across all pillars
- **Focus**: Structural completeness and extensibility

### **OSAA_DRM** ([Repository](https://github.com/monyas96/OSAA_DRM))
- **Scope**: Theme 4 (Domestic Resource Mobilization) only
- **Purpose**: Policy-ready dashboard for immediate DRM analysis
- **Complexity**: Hidden complexity—optimized for end-user simplicity
- **Target Users**: Policy analysts and decision-makers
- **Status**: Production-ready, focused implementation
- **Focus**: Immediate usability and policy insights

**Think of it this way:**
- **Nexus Dashboard v.2** = The full architectural blueprint (all pillars, expandable)
- **OSAA_DRM** = A specialized, polished room in that building (Theme 4, ready to use)

---

## Architecture Overview

### Four Pillars Structure

The dashboard implements the complete Nexus framework:

1. ** Pillar 1: Durable Peace Requires Sustainable Development**
   - Theme 1: Historical Root Causes of Instability
   - Theme 2: Africa's Three Geographies
   - Theme 3: The State-Building Imperative
   - Theme 4: Development as a Foundation for Peace

2. ** Pillar 2: Sustainable Development Requires Sustainable Financing**
   - Theme 1: Public Debt Management Quality
   - Theme 2: Domestic Institutions' Ability to Change Position in R/GVCs
   - Theme 3: Ownership Over Economic and Financial Flows
   - Theme 4: DRM Institutions and Systems *(Most developed)*
   - Theme 5: Derisking Strategies for Private Sector Engagement

3. ** Pillar 3: Sustainable Financing Requires Control Over Economic and Financial Flows**
   - Theme 1: Resource Sovereignty
   - Theme 2: Balancing Internal and External Dependence
   - Theme 3: Pathways to Sustainability
   - Theme 4: Control and Allocation of Resources

4. ** Pillar 4: Control Over Economic and Financial Flows Requires Strong Institutions**
   - Theme 1: Sustainable Finance as a Political Mindset
   - Theme 2: Institutional Strength
   - Theme 3: Domestic Resource Mobilization (DRM)

### Current Implementation Status

-  **Pillar Structure**: All 4 pillars configured and navigable
-  **Theme Pages**: All theme pages created (16 themes total)
-  **Topic Pages**: Topic 4.1-4.4 fully implemented (Pillar 2, Theme 4)
-  **Quadrant System**: Interactive 4-quadrant home page with flip cards
-  **Modular Components**: Reusable layouts, components, and visualization utilities
-  **Expansion**: Ready for adding topics and indicators to other themes

---

## 📂 Repository Structure

```
nexus_dashboard_v.2/
├── app.py                          # Main Streamlit entry point
├── requirements.txt                # Python dependencies
├── README.md                       # This file
│
├── app_core/                       # Core modular components
│   ├── components/                 # Reusable UI components
│   │   ├── home_page_components.py    # Home page quadrant system
│   │   ├── navigation.py              # Navigation components
│   │   ├── quadrant_card.py          # Quadrant card component
│   │   ├── flip_card_component.py    # Flip card for pillars
│   │   └── ...
│   ├── config/                     # Configuration files
│   │   └── pillars_config.py        # Central pillar/theme/topic config
│   ├── layouts/                    # Layout templates
│   │   ├── pillar_layout.py        # Pillar page layouts
│   │   └── topic_layout.py         # Topic page layouts
│   └── styles/                     # Styling
│       └── style_osaa.css          # OSAA branding styles
│
├── pages/                          # Streamlit pages
│   ├── 0_home.py                   # Home page with quadrants
│   ├── pillars/                    # Pillar landing pages
│   │   ├── 1_pillar_1.py
│   │   ├── 1_pillar_2.py
│   │   ├── 1_pillar_3.py
│   │   └── 1_pillar_4.py
│   ├── themes/                     # Theme pages (16 themes)
│   │   ├── pillar1_theme1.py
│   │   ├── pillar2_theme4.py
│   │   └── ...
│   ├── 2_theme_4.py                # Legacy Theme 4 page
│   ├── 3_topic_4_1.py              # Topic 4.1: Public Expenditures
│   ├── 4_topic_4_2.py              # Topic 4.2: Budget and Tax Revenues
│   ├── 5_topic_4_3.py              # Topic 4.3: Capital Markets
│   ├── 6_topic_4_4.py              # Topic 4.4: Illicit Financial Flows
│   └── 99_indicator_explorer.py    # Indicator explorer tool
│
├── special_pages/                  # Specialized page components
│   └── tab_4_4_1.py                # Tab component for Topic 4.4.1
│
├── data/                           # Data files
│   ├── nexus.parquet               # Main indicator dataset
│   ├── iso3_country_reference.csv  # Country reference data
│   ├── countries_codes_and_coordinates.csv
│   └── ...
│
├── docs/                           # Documentation
│   ├── ARCHITECTURE.md             # Architecture documentation
│   ├── EXPANSION_GUIDE.md          # Guide for adding new content
│   ├── REPRODUCIBILITY.md          # Reproducibility guide
│   └── ...
│
├── scripts/                        # Utility and calculation scripts
│   ├── __init__.py                 # Package initialization
│   ├── utils.py                    # Helper utilities
│   ├── universal_viz.py            # Core visualization utilities
│   └── composite_indicator_methods.py  # Composite indicator calculations
│
├── scripts/                        # Utility and calculation scripts
│   ├── utils.py                    # Helper utilities
│   ├── universal_viz.py            # Core visualization utilities
│   └── composite_indicator_methods.py  # Composite indicator calculations
└── assets/                         # Static assets
    ├── logos/                      # Branding assets
    └── loop_arrow.svg              # SVG assets
```

---

##  Quick Start

### Prerequisites

- Python 3.9 or higher
- pip (Python package manager)
- Git

### Installation

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd nexus_dashboard_v.2
   ```

2. **Create and activate a virtual environment:**
   ```bash
   python -m venv nexusapp
   source nexusapp/bin/activate  # On Windows: nexusapp\Scripts\activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Run the application:**
   ```bash
   streamlit run app.py
   ```

The application will be available at `http://localhost:8501`

---

##  Documentation

### Core Documentation Files

- **[ARCHITECTURE.md](docs/ARCHITECTURE.md)**: Detailed architecture documentation
- **[EXPANSION_GUIDE.md](docs/EXPANSION_GUIDE.md)**: Step-by-step guide for adding new pillars, themes, topics, and indicators
- **[REPRODUCIBILITY.md](docs/REPRODUCIBILITY.md)**: Guide for reproducing and maintaining the dashboard

### Development Guides

- **[UNIFIED_DASHBOARD_TEMPLATE.md](docs/UNIFIED_DASHBOARD_TEMPLATE.md)**: Standardized structure for indicator pages
- **[LAYOUT_EXAMPLES.md](docs/LAYOUT_EXAMPLES.md)**: Layout examples for different numbers of indicators
- **[QUICK_REFERENCE.md](docs/QUICK_REFERENCE.md)**: Quick reference for dashboard patterns

---

##  Key Features

### 1. Quadrant System

The home page features an interactive 4-quadrant visualization system:
- **Flip Cards**: Each pillar has a flip card showing description and themes
- **Visual Navigation**: Color-coded quadrants for each pillar
- **Systems Loop**: Visual representation of the interconnected pillars

### 2. Modular Components

Reusable components in `app_core/components/`:
- `home_page_components.py`: Quadrant rendering and home page sections
- `quadrant_card.py`: Quadrant card component
- `flip_card_component.py`: Interactive flip cards
- `navigation.py`: Navigation components

### 3. Centralized Configuration

All pillar, theme, and topic definitions in `app_core/config/pillars_config.py`:
- Easy to add new pillars, themes, or topics
- Centralized routing and metadata
- Consistent structure across the dashboard

### 4. Standardized Layouts

Layout templates in `app_core/layouts/`:
- `pillar_layout.py`: Standard pillar page layout
- `topic_layout.py`: Standard topic page layout
- Consistent styling and structure

### 5. Visualization Utilities

`universal_viz.py` provides:
- Standardized chart rendering
- Consistent visualization patterns
- Reusable visualization functions

---

##  Design Principles

### Modularity
- Each component is self-contained and reusable
- Clear separation between data, logic, and presentation
- Easy to extend without modifying existing code

### Consistency
- Standardized layouts and components
- Unified styling via CSS
- Consistent navigation patterns

### Reproducibility
- Clear documentation
- Standardized patterns
- Version-controlled configuration

### Scalability
- Designed to accommodate all 4 pillars
- Ready for expansion to all themes and topics
- Efficient data handling

---

##  Data Pipeline

The dashboard uses pre-processed data stored in `data/nexus.parquet`, which integrates data from multiple sources:

- World Bank PEFA Assessments
- IMF ISORA Database
- Global Financial Integrity (GFI)
- UNODC Crime Data
- World Justice Project
- And more...

Data processing scripts are available in the `scripts/` directory.

---

##  Development

### Adding a New Pillar

1. Update `app_core/config/pillars_config.py` with pillar definition
2. Create pillar page in `pages/pillars/`
3. Add quadrant configuration in `pages/0_home.py`
4. Create theme pages in `pages/themes/`

See [EXPANSION_GUIDE.md](docs/EXPANSION_GUIDE.md) for detailed instructions.

### Adding a New Theme

1. Add theme to pillar in `app_core/config/pillars_config.py`
2. Create theme page in `pages/themes/`
3. Update pillar page to link to new theme

### Adding a New Topic

1. Add topic configuration
2. Create topic page following [UNIFIED_DASHBOARD_TEMPLATE.md](docs/UNIFIED_DASHBOARD_TEMPLATE.md)
3. Add indicators following standardized patterns

### Adding a New Indicator

1. Follow the indicator module template
2. Use standardized visualization utilities
3. Implement local filters and multi-view tabs
4. Add supporting information expanders

---

##  Testing

The dashboard includes:
- Indicator Explorer (`pages/99_indicator_explorer.py`) for data exploration
- Data gap visualization utilities
- Standardized error handling

---

##  Code Style

- Follow PEP 8 Python style guidelines
- Use type hints where appropriate
- Document functions and classes
- Follow existing patterns for consistency

---

##  Contributing

When contributing:
1. Follow the existing architecture and patterns
2. Update documentation as needed
3. Test your changes thoroughly
4. Ensure reproducibility

---

##  License

This project is part of the OSAA (Office of the Special Adviser on Africa) initiative.

---

##  Contact

For questions or contributions, please contact the OSAA team or open an issue on GitHub.

---

##  Related Repositories

- **[OSAA_DRM](https://github.com/monyas96/OSAA_DRM)**: Theme 4 (DRM) focused implementation
- **[nexus-pipeline](https://github.com/UN-OSAA/nexus-pipeline)**: Data processing pipeline

---

##  Roadmap

- [ ] Complete indicator implementation for all themes
- [ ] Enhanced data visualization features
- [ ] Export functionality (PDF reports, CSV downloads)
- [ ] Advanced filtering and comparison tools
- [ ] User customization features

---

**Last Updated**: Nov 2025
