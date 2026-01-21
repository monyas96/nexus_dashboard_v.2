# Expansion Guide

This guide provides step-by-step instructions for expanding the Nexus Dashboard to include new pillars, themes, topics, and indicators.

## Table of Contents

1. [Adding a New Pillar](#adding-a-new-pillar)
2. [Adding a New Theme](#adding-a-new-theme)
3. [Adding a New Topic](#adding-a-new-topic)
4. [Adding a New Indicator](#adding-a-new-indicator)
5. [Best Practices](#best-practices)

---

## Adding a New Pillar

### Step 1: Update Configuration

Edit `app_core/config/pillars_config.py` and add your pillar to the `PILLARS` dictionary:

```python
PILLARS = {
    # ... existing pillars ...
    "pillar_5": {
        "number": 5,
        "title": "Your Pillar Title",
        "description": "Description of your pillar",
        "color": "#HEXCOLOR",  # Choose a unique color
        "themes": {
            "theme_1": {
                "title": "Theme 1 Title",
                "description": "Theme 1 description",
                "route": "pages/themes/pillar5_theme1.py"
            },
            # Add more themes...
        }
    }
}
```

### Step 2: Create Pillar Page

Create a new file `pages/pillars/1_pillar_5.py`:

```python
import streamlit as st
from app_core.config.pillars_config import PILLARS
from app_core.layouts.pillar_layout import render_pillar_page

st.set_page_config(
    page_title=f"Pillar 5: {PILLARS['pillar_5']['title']}",
    layout="wide"
)

# Load styles
with open("app_core/styles/style_osaa.css") as f:
    st.markdown(f"<style>{f.read()}</style>", unsafe_allow_html=True)

# Render pillar page
render_pillar_page("pillar_5")
```

### Step 3: Add to Home Page

Edit `pages/0_home.py` and add your pillar to the `quadrants` list:

```python
quadrants = [
    # ... existing quadrants ...
    {
        "id": "p5",
        "title": "Pillar 5: Your Pillar Title",
        "tag": "Your tagline",
        "color": "#HEXCOLOR",
        "link": "pages/pillars/1_pillar_5.py",
        "back_content": """<div class="pillar-section">
  <p><strong>Description:</strong></p>
  <p>Your pillar description</p>
</div>
<hr class="pillar-divider">
<div class="pillar-section">
  <p><strong>Themes:</strong></p>
  <ul class="theme-list">
    <li><strong>Theme 1:</strong> Description</li>
    <!-- Add more themes -->
  </ul>
</div>""",
    },
]
```

### Step 4: Add to Navigation

Edit `app.py` and add your pillar page to the `pages` list:

```python
pages = [
    # ... existing pages ...
    st.Page("pages/pillars/1_pillar_5.py", title="Pillar 5: Your Title"),
]
```

### Step 5: Create Theme Pages

Follow the "Adding a New Theme" section below for each theme.

---

## Adding a New Theme

### Step 1: Update Configuration

Edit `app_core/config/pillars_config.py` and add your theme to the appropriate pillar:

```python
PILLARS = {
    "pillar_X": {
        # ... existing config ...
        "themes": {
            # ... existing themes ...
            "theme_Y": {
                "title": "Your Theme Title",
                "description": "Your theme description",
                "route": "pages/themes/pillarX_themeY.py"
            }
        }
    }
}
```

### Step 2: Create Theme Page

Create `pages/themes/pillarX_themeY.py`:

```python
import streamlit as st
from app_core.config.pillars_config import PILLARS

st.set_page_config(
    page_title="Theme Y: Your Theme Title",
    layout="wide"
)

# Load styles
with open("app_core/styles/style_osaa.css") as f:
    st.markdown(f"<style>{f.read()}</style>", unsafe_allow_html=True)

# Get theme config
pillar_key = "pillar_X"
theme_key = "theme_Y"
theme = PILLARS[pillar_key]["themes"][theme_key]

# Render theme page
st.title(f"Theme Y: {theme['title']}")
st.markdown(theme['description'])

# Add topic navigation or content here
# See existing theme pages for examples
```

### Step 3: Add to Navigation

Edit `app.py` and add your theme page:

```python
pages = [
    # ... existing pages ...
    st.Page("pages/themes/pillarX_themeY.py", title="Theme Y: Your Title"),
]
```

### Step 4: Update Pillar Page

Edit the corresponding pillar page to link to your new theme.

---

## Adding a New Topic

### Step 1: Create Topic Page

Create a new file following the naming convention: `pages/X_topic_Y_Z.py` where:
- X = sequential number
- Y = theme number
- Z = topic number

Example: `pages/7_topic_2_1.py` for Topic 2.1

### Step 2: Follow Template Structure

Use `UNIFIED_DASHBOARD_TEMPLATE.md` as a guide. Basic structure:

```python
import streamlit as st
import pandas as pd
import universal_viz as uv

st.set_page_config(
    page_title="Topic Y.Z: Your Topic Title",
    layout="wide"
)

# Load styles
with open("app_core/styles/style_osaa.css") as f:
    st.markdown(f"<style>{f.read()}</style>", unsafe_allow_html=True)

# Topic Description
st.title("Topic Y.Z: Your Topic Title")
st.markdown("Topic description here...")

# Orange divider
st.markdown("""
<div style="border-top: 2px solid #F26C2B; margin: 1.5rem 0; width: 100%;"></div>
""", unsafe_allow_html=True)

st.markdown("### Key Indicators Overview")

# Add indicator modules here
# See UNIFIED_DASHBOARD_TEMPLATE.md for full structure
```

### Step 3: Add Indicators

Follow the "Adding a New Indicator" section below.

### Step 4: Add to Navigation

Edit `app.py`:

```python
pages = [
    # ... existing pages ...
    st.Page("pages/X_topic_Y_Z.py", title="Topic Y.Z: Your Title"),
]
```

---

## Adding a New Indicator

### Step 1: Determine Indicator Details

- Indicator number (e.g., 4.1.1)
- Indicator name
- Data source and indicator label
- Chart type (from visualization specification)
- Analytical question

### Step 2: Create Indicator Module

Follow the structure in `UNIFIED_DASHBOARD_TEMPLATE.md`. Key components:

#### A. Indicator Header

```python
st.markdown(f"""
<div class='indicator-card'>
    <h4>
        Indicator {indicator_num}: {indicator_name}
        <button type="button" class="info-icon-btn" 
                data-tooltip="{tooltip_text}" 
                style="background: none; border: none; cursor: help; 
                       font-size: 0.8em; color: #666; margin-left: 0.5rem; padding: 0;">ℹ️</button>
    </h4>
    <p style="color: #555; line-height: 1.6; margin-bottom: 1rem;">
        <strong>Analytical Focus Question:</strong> {analytical_question}
    </p>
</div>
""", unsafe_allow_html=True)
```

#### B. Local Filter Row

```python
# Initialize session state
if 'ind_X_X_X_year' not in st.session_state:
    st.session_state.ind_X_X_X_year = None
# ... more session state ...

# Filter row
filter_col1, filter_col2, filter_col3, filter_col4 = st.columns([1.5, 1.5, 1.5, 0.7])

with filter_col1:
    selected_year = st.selectbox(
        "Select Year(s)",
        options=["All Years"] + available_years,
        index=0,
        key="ind_X_X_X_year_filter"
    )
# ... more filters ...
```

#### C. Multi-View Tabs

```python
tab_graph, tab_map, tab_data = st.tabs(["Graph View", "Map View", "Data Table"])

with tab_graph:
    # "How to Read This Graph" button
    # Chart rendering using universal_viz
    uv.render_indicator_section(
        df=filtered_data,
        indicator_label=indicator_label,
        title="",
        description="",
        chart_type="heatmap",  # or "line", "bar", "stacked_bar", "map"
        selected_countries=selected_countries if selected_countries else None,
        year_range=(selected_year, selected_year) if selected_year != "All Years" else None,
        chart_options={},
        show_data_table=False,
        container_key="topic_X_X_ind_X_chart"
    )

with tab_map:
    # Map view implementation

with tab_data:
    # Data table with CSV download
```

#### D. Supporting Information

```python
# Learn more about this indicator
with st.expander("Learn more about this indicator", expanded=False):
    tab_def, tab_rel, tab_proxy, tab_pillar = st.tabs(
        ["Definition", "Relevance", "Proxy Justification", "Pillar Connection"]
    )
    # Add content to each tab

# Analytical Lens
with st.expander("Analytical Lens (Efficiency and Effectiveness)", expanded=False):
    st.markdown("**Efficiency:** ...")
    st.markdown("**Effectiveness:** ...")
```

### Step 3: Data Preparation

Ensure your indicator data is in `data/nexus.parquet` with:
- `indicator_label`: Unique identifier for the indicator
- `country_or_area`: Country name
- `year`: Year
- `value`: Indicator value
- Other required columns

### Step 4: Test the Indicator

- Test all filter combinations
- Verify chart rendering
- Check map view (if applicable)
- Test CSV download
- Verify tooltips and expanders

---

## Best Practices

### Code Organization

1. **Follow Existing Patterns**: Use existing code as templates
2. **Reuse Components**: Use components from `app_core/components/`
3. **Centralize Configuration**: Add to `pillars_config.py`
4. **Consistent Naming**: Follow existing naming conventions

### Documentation

1. **Update README**: Document new features
2. **Add Comments**: Comment complex logic
3. **Update Config**: Keep configuration files up to date

### Testing

1. **Test Locally**: Test all functionality before committing
2. **Check Navigation**: Verify all links work
3. **Validate Data**: Ensure data displays correctly
4. **Test Filters**: Test all filter combinations

### Styling

1. **Use OSAA Colors**: Follow brand guidelines
2. **Consistent Spacing**: Use standard margins and padding
3. **Responsive Design**: Test on different screen sizes

### Performance

1. **Efficient Queries**: Filter data early
2. **Cache Expensive Operations**: Use Streamlit caching
3. **Lazy Loading**: Load components on demand

### Data

1. **Validate Data**: Ensure data quality
2. **Handle Missing Data**: Provide appropriate messages
3. **Consistent Formatting**: Use standard data formats

---

## Common Issues and Solutions

### Issue: Indicator not displaying

**Solution**: 
- Check indicator_label matches data
- Verify data filtering (Africa only)
- Check chart_type is correct

### Issue: Navigation not working

**Solution**:
- Verify page is in `app.py` pages list
- Check file path is correct
- Ensure page has `st.set_page_config`

### Issue: Styling not applied

**Solution**:
- Verify CSS file path
- Check CSS is loaded in page
- Ensure style classes match

### Issue: Filters not working

**Solution**:
- Check session state initialization
- Verify filter keys are unique
- Ensure data filtering logic is correct

---

## Resources

- **[UNIFIED_DASHBOARD_TEMPLATE.md](UNIFIED_DASHBOARD_TEMPLATE.md)**: Complete indicator template
- **[LAYOUT_EXAMPLES.md](LAYOUT_EXAMPLES.md)**: Layout examples
- **[QUICK_REFERENCE.md](QUICK_REFERENCE.md)**: Quick reference guide
- **[ARCHITECTURE.md](ARCHITECTURE.md)**: Architecture documentation

---

## Getting Help

If you encounter issues:
1. Check existing documentation
2. Review similar implementations
3. Check error messages carefully
4. Ask for help from the team
