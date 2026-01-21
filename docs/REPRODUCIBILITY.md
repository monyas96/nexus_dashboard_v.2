# Reproducibility Guide

This guide ensures that the Nexus Dashboard v.2 can be reliably reproduced, maintained, and extended by different developers and researchers.

## Table of Contents

1. [Environment Setup](#environment-setup)
2. [Data Requirements](#data-requirements)
3. [Dependencies](#dependencies)
4. [Configuration](#configuration)
5. [Running the Dashboard](#running-the-dashboard)
6. [Troubleshooting](#troubleshooting)
7. [Maintenance](#maintenance)

---

## Environment Setup

### Prerequisites

- **Python**: 3.9 or higher
- **Operating System**: macOS, Linux, or Windows
- **Package Manager**: pip (comes with Python)
- **Git**: For version control

### Step-by-Step Setup

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd nexus_dashboard_v.2
   ```

2. **Create Virtual Environment**
   ```bash
   python -m venv nexusapp
   ```

3. **Activate Virtual Environment**
   
   On macOS/Linux:
   ```bash
   source nexusapp/bin/activate
   ```
   
   On Windows:
   ```bash
   nexusapp\Scripts\activate
   ```

4. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

5. **Verify Installation**
   ```bash
   python -c "import streamlit; print(streamlit.__version__)"
   ```

---

## Data Requirements

### Required Data Files

The dashboard requires the following data files in the `data/` directory:

1. **`nexus.parquet`** (Required)
   - Main indicator dataset
   - Contains all indicator data with columns:
     - `indicator_label`: Unique indicator identifier
     - `country_or_area`: Country name
     - `year`: Year
     - `value`: Indicator value
     - Additional metadata columns

2. **`iso3_country_reference.csv`** (Required)
   - Country reference data with ISO3 codes
   - Used for country filtering and mapping

3. **`countries_codes_and_coordinates.csv`** (Required)
   - Country codes and geographic coordinates
   - Used for map visualizations

### Data Structure

The `nexus.parquet` file should have the following structure:

```
indicator_label | country_or_area | year | value | [other columns]
----------------|-----------------|------|-------|------------------
4.1.1           | Kenya          | 2020 | 0.75  | ...
4.1.1           | Kenya          | 2021 | 0.78  | ...
...
```

### Data Sources

Data is typically generated from the [nexus-pipeline](https://github.com/UN-OSAA/nexus-pipeline) repository. If you need to regenerate data:

1. Follow the nexus-pipeline documentation
2. Export data to `nexus.parquet` format
3. Place in `data/` directory

### Data Validation

To validate your data:

```python
import pandas as pd

# Load data
df = pd.read_parquet('data/nexus.parquet')

# Check structure
print(df.columns)
print(df.head())
print(df.info())

# Check for required columns
required_cols = ['indicator_label', 'country_or_area', 'year', 'value']
assert all(col in df.columns for col in required_cols), "Missing required columns"
```

---

## Dependencies

### Core Dependencies

All dependencies are listed in `requirements.txt`:

- **streamlit** (>=1.32.0): Web framework
- **pandas** (>=2.0.0): Data manipulation
- **numpy** (>=1.24.0): Numerical operations
- **plotly** (>=5.20.0): Interactive visualizations
- **altair** (>=5.0.0): Statistical visualizations
- **pydeck** (>=0.8.0): Map visualizations
- **pyarrow** (>=14.0.0): Parquet file support

### Installing Dependencies

```bash
pip install -r requirements.txt
```

### Updating Dependencies

To update dependencies:

1. Update version numbers in `requirements.txt`
2. Test the application
3. Update this documentation if needed

### Dependency Conflicts

If you encounter dependency conflicts:

1. Check Python version (must be 3.9+)
2. Try upgrading pip: `pip install --upgrade pip`
3. Create a fresh virtual environment
4. Install dependencies one by one to identify conflicts

---

## Configuration

### Configuration Files

1. **`app_core/config/pillars_config.py`**
   - Central configuration for pillars, themes, and topics
   - Defines structure, colors, descriptions, routes

2. **`app_core/styles/style_osaa.css`**
   - Styling and branding
   - OSAA color scheme

3. **`app.py`**
   - Main application entry point
   - Page navigation configuration

### Customization

To customize the dashboard:

1. **Change Colors**: Edit `pillars_config.py` and `style_osaa.css`
2. **Add Pages**: Follow EXPANSION_GUIDE.md
3. **Modify Layouts**: Edit files in `app_core/layouts/`

---

## Running the Dashboard

### Basic Run

```bash
streamlit run app.py
```

The dashboard will be available at `http://localhost:8501`

### Custom Port

```bash
streamlit run app.py --server.port 8502
```

### Custom Configuration

Create `.streamlit/config.toml`:

```toml
[server]
port = 8501
address = "localhost"

[theme]
primaryColor = "#072D92"
backgroundColor = "#FDF4EC"
secondaryBackgroundColor = "#FFFFFF"
textColor = "#000000"
```

### Production Deployment

For production deployment:

1. Use a process manager (e.g., systemd, supervisor)
2. Set up reverse proxy (e.g., nginx)
3. Configure SSL/TLS
4. Set environment variables
5. Use production-grade data storage

See Streamlit deployment documentation for details.

---

## Troubleshooting

### Common Issues

#### Issue: ModuleNotFoundError

**Symptoms**: `ModuleNotFoundError: No module named 'X'`

**Solution**:
```bash
# Ensure virtual environment is activated
source nexusapp/bin/activate  # or nexusapp\Scripts\activate on Windows

# Reinstall dependencies
pip install -r requirements.txt
```

#### Issue: Data File Not Found

**Symptoms**: `FileNotFoundError: data/nexus.parquet`

**Solution**:
1. Verify data files are in `data/` directory
2. Check file paths in code
3. Ensure you're running from project root

#### Issue: Port Already in Use

**Symptoms**: `Port 8501 is already in use`

**Solution**:
```bash
# Use a different port
streamlit run app.py --server.port 8502

# Or kill the process using port 8501
# On macOS/Linux:
lsof -ti:8501 | xargs kill
# On Windows:
netstat -ano | findstr :8501
taskkill /PID <PID> /F
```

#### Issue: Styling Not Applied

**Symptoms**: Dashboard looks unstyled

**Solution**:
1. Check CSS file path in pages
2. Verify `app_core/styles/style_osaa.css` exists
3. Clear browser cache
4. Check browser console for errors

#### Issue: Navigation Not Working

**Symptoms**: Pages not accessible

**Solution**:
1. Verify pages are in `app.py` pages list
2. Check file paths are correct
3. Ensure page files exist
4. Check for syntax errors in page files

### Debug Mode

Run with debug information:

```bash
streamlit run app.py --logger.level=debug
```

### Check Logs

Streamlit logs are typically in:
- macOS/Linux: `~/.streamlit/logs/`
- Windows: `%USERPROFILE%\.streamlit\logs\`

---

## Maintenance

### Regular Maintenance Tasks

1. **Update Dependencies**
   ```bash
   pip list --outdated
   pip install --upgrade <package>
   ```

2. **Check Data Freshness**
   - Verify data files are up to date
   - Check data pipeline is running

3. **Review Logs**
   - Check for errors
   - Monitor performance

4. **Test Functionality**
   - Test all pages
   - Verify visualizations
   - Check filters

### Backup

Regular backups should include:

1. **Code Repository**
   - Git repository
   - Configuration files

2. **Data Files**
   - `data/nexus.parquet`
   - Reference data files

3. **Documentation**
   - README files
   - Documentation in `docs/`

### Version Control

Best practices:

1. **Commit Frequently**: Small, logical commits
2. **Write Clear Messages**: Descriptive commit messages
3. **Use Branches**: Feature branches for new work
4. **Review Changes**: Code review before merging

### Documentation Updates

When making changes:

1. Update relevant documentation
2. Update README if needed
3. Add comments to code
4. Update configuration documentation

---

## Performance Optimization

### Data Loading

- Data is loaded once and cached
- Use appropriate data types
- Filter data early in processing

### Caching

Use Streamlit caching for expensive operations:

```python
@st.cache_data
def load_data():
    return pd.read_parquet('data/nexus.parquet')
```

### Memory Management

- Release large objects when done
- Use generators for large datasets
- Monitor memory usage

---

## Testing

### Manual Testing Checklist

- [ ] All pages load correctly
- [ ] Navigation works
- [ ] Filters function properly
- [ ] Visualizations render
- [ ] Data tables display
- [ ] CSV downloads work
- [ ] Responsive design works

### Automated Testing

Consider adding:
- Unit tests for utilities
- Integration tests for components
- Data validation tests

---

## Getting Help

If you encounter issues:

1. **Check Documentation**: Review this guide and other docs
2. **Check Issues**: Look for similar issues in repository
3. **Debug**: Use debug mode and check logs
4. **Ask for Help**: Contact the team or open an issue

---

## Additional Resources

- **Streamlit Documentation**: https://docs.streamlit.io/
- **Pandas Documentation**: https://pandas.pydata.org/docs/
- **Plotly Documentation**: https://plotly.com/python/
- **Altair Documentation**: https://altair-viz.github.io/

---

**Last Updated**: 2025
