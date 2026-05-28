# Awesome openEO [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of resources for openEO — the open API standard for cloud-based Earth Observation processing.

Curated by [George Papadakis](https://github.com/GP-80) &nbsp;·&nbsp; <a href="https://ko-fi.com/D1D8T02WH"><img height="24" src="https://ko-fi.com/img/githubbutton_sm.svg" alt="Support me on Ko-fi" style="vertical-align:-6px"></a>

---

## What is openEO?

**openEO** is an open standard for processing satellite imagery in the cloud. The idea will feel familiar if you have used tools like Google Earth Engine: pick a dataset, filter by date and area, chain together operations (NDVI, composites, classifications), and get results back — all running remotely, on large data, without managing infrastructure.

What sets openEO apart is that it is not tied to any single provider. The same Python code runs on the Copernicus Data Space, VITO/Terrascope, Google Earth Engine (via a community driver, not officially supported by Google), EODC, or any other platform that implements the standard. Switching platforms is a one-line change. openEO became an official OGC international standard in 2026.

### What a workflow looks like

```python
import openeo

# Connect to a platform
conn = openeo.connect("https://openeo.dataspace.copernicus.eu").authenticate_oidc()

# Load Sentinel-2 imagery
cube = conn.load_collection(
    "SENTINEL2_L2A",
    spatial_extent={"west": 4.0, "south": 51.0, "east": 4.5, "north": 51.5},
    temporal_extent=["2023-06-01", "2023-08-31"],
    bands=["B04", "B08"],
)

# Compute NDVI and download
cube.ndvi(nir="B08", red="B04").download("ndvi.tiff")
```

The platform handles data access and computation at scale. You get a file back.

### Key concepts

| Term | What it means |
|---|---|
| **Collection** | A named dataset on the platform (e.g. `SENTINEL2_L2A`). |
| **Data Cube** | Your loaded imagery as a 3D block of pixels — space, time, and bands. |
| **Process** | A built-in operation you chain together — `ndvi`, `filter_bbox`, `aggregate_temporal`, etc. |
| **Backend** | The cloud platform that runs your code (Copernicus, VITO, EODC, and others). |
| **UDP** | A User-Defined Process — a reusable, shareable workflow you author once and run anywhere. |
| **UDF** | A User-Defined Function — custom Python or R code embedded in a workflow for bespoke processing. |

### How it fits alongside other platforms

openEO is not a replacement for any particular platform — it is a common language that several platforms speak. You can use it alongside or instead of other tools depending on what data you need and where it lives.

| | **openEO** | **Google Earth Engine** | **Sentinel Hub** | **OpenDataCube** |
|---|---|---|---|---|
| **Open standard** | Yes (OGC, 2026) | No | No | No |
| **Multi-platform** | Yes — same code, multiple backends | No | No | No |
| **Python support** | Yes | Yes | Yes | Yes |
| **No-code option** | Web Editor, QGIS plugin | Code Editor | Dashboard | No |
| **Free access** | Yes, via Copernicus Data Space | Yes, for research | Freemium | Self-hosted |
| **Custom code** | Python / R UDFs | JavaScript | Python | Python |

---

## Contents

- [Begin Here](#begin-here)
- [Official Resources](#official-resources)
- [Clients & SDKs](#clients--sdks)
- [Backends](#backends)
- [Platforms & Services](#platforms--services)
- [Tools & Ecosystem](#tools--ecosystem)
- [Tutorials & Notebooks](#tutorials--notebooks)
- [Videos](#videos)
- [Courses & MOOCs](#courses--moocs)
- [Slides & Presentations](#slides--presentations)
- [Podcasts](#podcasts)
- [Papers & Research](#papers--research)
- [Blog Posts & Articles](#blog-posts--articles)
- [Comparison & Positioning](#comparison--positioning)
- [Community & Support](#community--support)
- [Conferences & Workshops](#conferences--workshops)
- [Projects & Funding](#projects--funding)
- [Standards](#standards)
- [Data Catalogs & Collections](#data-catalogs--collections)
- [Historical](#historical)

---

## Begin Here

*New to openEO? Start with these.*

- [openEO Official Website](https://openeo.org/) - The project homepage: what it is, why it exists, and links to all official resources.
- [openEO About Page](https://openeo.org/about.html) - Plain-language explanation of the project's mission and rationale. No prior EO knowledge required.
- [openEO Glossary](https://openeo.org/documentation/1.0/glossary.html) - Definitions of every core concept: data cube, process graph, UDF, UDP, collection, backend.
- [openEO Web Editor (live)](https://editor.openeo.org/) - No-code browser-based editor. Connect to a demo backend and run your first workflow without installing anything.
- [Cubes & Clouds MOOC](https://eo-college.org/courses/cubes-and-clouds/) - Free 30-hour course on EO data cubes, cloud platforms, and open science, built around openEO. The single best structured introduction.
- [openEO: What it is and how it relates to Pangeo](https://www.youtube.com/watch?v=S_soiazvOmQ) - 30-minute video talk by core team members Matthias Mohr and Alexander Jacob. Clearest video explainer of the concept.
- [Introduction to openEO within the Copernicus Data Space](https://www.youtube.com/watch?v=Z3iingFH6T8) - Beginner-oriented video introduction from the Copernicus Data Space Ecosystem (2025).
- [openEO Cookbook](https://openeo.org/documentation/1.0/cookbook/) - Recipe-style guide showing common tasks (compute NDVI, create a composite, export results) in Python, R, and JavaScript side-by-side.
- [openEO Platform Basic Training Slides – Riga 2022 (PDF)](https://eo4society.esa.int/wp-content/uploads/2022/07/2022-Riga_openEO_Platform_Basic_Training_Introduction.pdf) - Structured slide deck from ESA's training course. Good offline reading for newcomers.
- [An Introduction to Spatial Datacubes & the openEO Python Client](https://medium.com/@thukupeter487/an-introduction-to-spatial-datacubes-openeo-python-client-c786ffe8c918) - Accessible community blog post explaining the datacube concept, then walking through the Python client.
- [Introduction to the openEO Platform](https://medium.com/openeo-platform/introduction-to-the-openeo-platform-ad346271c335) - First post in the openEO Platform Medium series; explains the "why" of a federated platform in plain language.
- [Galaxy Training – NDVI Time Series with openEO](https://training.galaxyproject.org/training-material/topics/ecology/tutorials/ndvi_openeo/tutorial.html) - Step-by-step guided tutorial: compute an NDVI time series from Sentinel-2 data and visualise results. No prior openEO knowledge needed.
- [Python Client – Getting Started](https://open-eo.github.io/openeo-python-client/basics.html) - Official guide: connect to a backend, load a collection, apply a process, and download results in Python.
- [CDSE – Basics Notebook](https://documentation.dataspace.copernicus.eu/notebook-samples/openeo/basics.html) - Jupyter notebook: discover collections and processes on the Copernicus Data Space backend.
- [openEO Hub](https://hub.openeo.org/) - Interactive registry of all available backends. Browse what data and processes each one offers before writing a single line of code.

---

## Official Resources

- [openEO Documentation](https://openeo.org/documentation/1.0/) - Main documentation portal covering concepts, client guides, and developer references.
- [openEO API Reference](https://api.openeo.org/) - Full OpenAPI-based HTTP API reference.
- [openEO Processes](https://github.com/Open-EO/openeo-processes) - The canonical JSON definitions of all interoperable predefined processes.
- [openEO API Profiles](https://openeo.org/documentation/1.0/developers/profiles/api.html) - Compliance tiers: L1 Minimal, L2 Recommended, L3 Advanced.
- [openEO Architecture](https://openeo.org/documentation/1.0/developers/arch.html) - Technical overview of the client–API–backend architecture.
- [openEO UDFs Documentation](https://openeo.org/documentation/1.0/udfs.html) - Guide to writing and deploying User-Defined Functions in Python and R.
- [openEO Software Page](https://openeo.org/software.html) - Comprehensive listing of all official clients, backends, and ecosystem tools.
- [openEO News](https://openeo.org/news/) - Official project announcements and release notes.
- [openEO Project Steering Committee](https://openeo.org/psc.html) - Governance information and PSC membership.
- [api.openeo.org](https://api.openeo.org/) - Machine-readable API specification endpoint.

---

## Clients & SDKs

### Python
- [openeo-python-client](https://github.com/Open-EO/openeo-python-client) - Official Python client library. The most mature and widely used client.
- [Python Client Documentation](https://open-eo.github.io/openeo-python-client/) - Full API reference, cookbook, and guides.
- [openeo on PyPI](https://pypi.org/project/openeo/) - Install with `pip install openeo`.
- [openeo on conda-forge](https://anaconda.org/conda-forge/openeo) - Install with `conda install -c conda-forge openeo`.
- [Python Client – Batch Jobs](https://open-eo.github.io/openeo-python-client/batch_jobs.html) - How to create, submit, monitor, and retrieve batch jobs.
- [Python Client – UDFs](https://open-eo.github.io/openeo-python-client/udf.html) - Writing and running User-Defined Functions.
- [Python Client – Machine Learning](https://open-eo.github.io/openeo-python-client/machine_learning.html) - Training and applying ML models (e.g. Random Forest) within openEO.
- [Python Client – Client-Side Processing](https://open-eo.github.io/openeo-python-client/cookbook/localprocessing.html) - Run process graphs locally without a remote backend.
- [Python Client – Job Manager](https://open-eo.github.io/openeo-python-client/cookbook/job_manager.html) - Orchestrate large numbers of batch jobs across backends.

### R
- [openeo-r-client](https://github.com/Open-EO/openeo-r-client) - Official R client package.
- [R Client Documentation](https://open-eo.github.io/openeo-r-client/) - Full reference and vignettes.
- [openeo on CRAN](https://cran.r-project.org/web/packages/openeo/index.html) - Install with `install.packages("openeo")`.
- [R Client – Getting Started Vignette](https://open-eo.github.io/openeo-r-client/articles/openeo-01-getting_started.html) - Main introductory vignette.
- [r4openeo-usecases](https://github.com/Open-EO/r4openeo-usecases) - Use-case notebooks for the R4openEO project (R analytics + openEO).

### JavaScript / TypeScript
- [openeo-js-client](https://github.com/Open-EO/openeo-js-client) - Official JavaScript/TypeScript client.
- [JS Client Documentation](https://open-eo.github.io/openeo-js-client/latest/) - Full API reference.
- [@openeo/js-client on npm](https://www.npmjs.com/package/@openeo/js-client) - Install with `npm install @openeo/js-client`.

### Julia
- [openeo-julia-client](https://github.com/Open-EO/openeo-julia-client) - Julia client for openEO.

### Visual / No-code
- [openEO Web Editor (live)](https://editor.openeo.org/) - Browser-based visual workflow editor.
- [openeo-web-editor](https://github.com/Open-EO/openeo-web-editor) - Source code for the web editor.
- [openEO QGIS Plugin](https://plugins.qgis.org/plugins/openeo_plugin/) - Connect and process data from any openEO backend directly in QGIS.
- [openeo-qgis-plugin](https://github.com/Open-EO/openeo-qgis-plugin) - Source code for the QGIS plugin.
- [openEO Studio (EOPF live)](https://studio.explorer.eopf.copernicus.eu/) - Visual workflow tool designed for EOPF Zarr data products.
- [openeo-studio (Development Seed)](https://github.com/developmentseed/openeo-studio) - Source code for the openEO Studio visual tool.

---

## Backends

*Server implementations of the openEO API.*

- [openeo-geopyspark-driver](https://github.com/Open-EO/openeo-geopyspark-driver) - GeoPySpark/GeoTrellis-based backend used in production by VITO/Terrascope and openEO Platform.
- [openeo-earthengine-driver](https://github.com/Open-EO/openeo-earthengine-driver) - Backend driver for Google Earth Engine (Node.js).
- [openeo-grassgis-driver](https://github.com/Open-EO/openeo-grassgis-driver) - Backend driver for GRASS GIS / actinia (by mundialis).
- [openeo-sentinelhub-python-driver](https://github.com/Open-EO/openeo-sentinelhub-python-driver) - Backend driver for Sentinel Hub.
- [openeo-wcps-driver](https://github.com/Open-EO/openeo-wcps-driver) - Prototype driver for WCPS/rasdaman backends.
- [openeo-spring-driver](https://github.com/Open-EO/openeo-spring-driver) - Java/Spring-based driver using WCPS and ODC storage.
- [openeo-eodc-driver](https://github.com/Open-EO/openeo-eodc-driver) - Apache Airflow/eoDataReaders-based backend.
- [openeo_odc_driver](https://github.com/Open-EO/openeo_odc_driver) - Processing engine based on OpenDataCube, Xarray and Dask.
- [openeo-odc](https://github.com/Open-EO/openeo-odc) - Functions to map openEO process graphs to ODC/Xarray jobs.
- [openeo-r-backend](https://github.com/Open-EO/openeo-r-backend) - R-based backend implementation.
- [titiler-openeo](https://github.com/sentinel-hub/titiler-openeo) - Lightweight TiTiler-based backend for fast synchronous processing (Sinergise + Development Seed).
- [tensorlakehouse-openeo-driver](https://github.com/IBM/tensorlakehouse-openeo-driver) - IBM's open-source reference backend driver implementation.
- [openeo-aggregator](https://github.com/Open-EO/openeo-aggregator) - Federation proxy combining multiple backends into a single unified endpoint.
- [openeo-python-driver](https://github.com/Open-EO/openeo-python-driver) - Python commons framework for building new backend drivers.
- [openeo-backend-validator](https://github.com/Open-EO/openeo-backend-validator) - Compliance validation tool to test whether a backend correctly implements the openEO API.
- [rasdaman openEO Demo](https://standards.rasdaman.com/demo_openEO.html) - Live rasdaman instance implementing the openEO API.
- [Getting Started for Backend Implementers](https://openeo.org/documentation/1.0/developers/backends/getting-started.html) - Official guide for building a new backend.

---

## Platforms & Services

*Live deployments of openEO-compatible backends.*

- [Copernicus Data Space Ecosystem (CDSE)](https://dataspace.copernicus.eu/analyse/openeo) - The primary publicly available openEO backend; access Sentinel-1, -2, -3, Landsat, and more for free.
- [CDSE openEO Web Editor](https://openeo.dataspace.copernicus.eu/) - CDSE-specific web editor, pre-connected to the Copernicus backend.
- [CDSE openEO Documentation](https://documentation.dataspace.copernicus.eu/APIs/openEO/openEO.html) - Full documentation for using openEO on CDSE.
- [openEO Platform](https://openeo.cloud/) - ESA-funded federated platform combining VITO/Terrascope, EODC, and Creodias backends.
- [openEO Platform Documentation](https://docs.openeo.cloud/) - Full documentation for the openEO Platform.
- [openEO Platform JupyterLab](https://docs.openeo.cloud/getting-started/jupyterlab/) - Hosted JupyterLab environment on openEO Platform.
- [Terrascope](https://docs.terrascope.be/Developers/WebServices/OpenEO/OpenEO.html) - VITO's Terrascope platform with hosted Jupyter notebooks and openEO access.
- [Terrascope Notebooks](https://notebooks.terrascope.be) - Hosted JupyterLab with openEO pre-installed.
- [openEO Hub](https://hub.openeo.org/) - Registry of all publicly available openEO backends with their collections and supported processes.
- [openEO Algorithm Plaza](https://marketplace-portal.dataspace.copernicus.eu/) - Marketplace for sharing and discovering openEO-based algorithms on CDSE.
- [APEx (ESA)](https://apex.esa.int/) - ESA's Application Propagation Environments platform; uses openEO as the core algorithm standard.

---

## Tools & Ecosystem

### Core Infrastructure
- [openeo-processes-dask](https://github.com/Open-EO/openeo-processes-dask) - Xarray/Dask implementations of openEO processes; the engine powering most modern backends.
- [openeo-pg-parser-networkx](https://github.com/Open-EO/openeo-pg-parser-networkx) - Parses openEO process graphs from JSON into traversable NetworkX graphs.
- [openeo-processes-python](https://github.com/Open-EO/openeo-processes-python) - Python representations of the openEO predefined process specifications.
- [openeo-js-processgraphs](https://github.com/Open-EO/openeo-js-processgraphs) - JavaScript library to parse, validate, and execute openEO process graphs.
- [openeo-processes-dask-ml](https://github.com/Open-EO/openeo-processes-dask-ml) - openEO ML process specification implemented in Python.

### Applications & Frameworks
- [openeo-gfmap](https://github.com/Open-EO/openeo-gfmap) - Generic Framework for EO Mapping — crop type, land cover, and other applications built on openEO.
- [FuseTS](https://github.com/Open-EO/FuseTS) - Time series fusion toolbox (MOGPR, Whittaker smoothing) integrated with openEO.
- [FuseTS Documentation](https://open-eo.github.io/FuseTS/) - Full documentation for the FuseTS toolbox.
- [FuseTS on PyPI](https://pypi.org/project/fusets/) - Install with `pip install fusets`.

### Examples & Community Content
- [openeo-community-examples](https://github.com/Open-EO/openeo-community-examples) - Community-contributed examples, User-Defined Processes, and code snippets in Python, R, and JavaScript.
- [eu-cdse/notebook-samples](https://github.com/eu-cdse/notebook-samples) - Copernicus CDSE Jupyter notebook samples including many openEO examples.
- [pangeo-data/pangeo-openeo-BiDS-2023](https://github.com/pangeo-data/pangeo-openeo-BiDS-2023) - Tutorial notebooks from the Pangeo+openEO joint session at BiDS 2023.

### Supporting Tools
- [openeo-vue-components](https://github.com/Open-EO/openeo-vue-components) - Vue.js UI components for building openEO-powered web applications.
- [openeo-processes-docgen](https://github.com/Open-EO/openeo-processes-docgen) - Documentation generator for openEO process specifications.
- [WorkflowHub – NDVI with openEO](https://workflowhub.eu/workflows/1712) - FAIR workflow registry entry for an openEO NDVI workflow.
- [openeo GitHub Topics](https://github.com/topics/openeo) - All public GitHub repositories tagged with the "openeo" topic.

---

## Tutorials & Notebooks

- [openEO Cookbook](https://openeo.org/documentation/1.0/cookbook/) - Official multi-language recipe guide (Python/R/JS) for common EO tasks.
- [Galaxy Training – NDVI Time Series with openEO](https://training.galaxyproject.org/training-material/topics/ecology/tutorials/ndvi_openeo/tutorial.html) - Full self-contained tutorial: NDVI time series from Sentinel-2 with visualisation using Holoviews.
- [BiDS 2023 Pangeo+openEO Tutorial Book](https://pangeo-data.github.io/pangeo-openeo-BiDS-2023/intro.html) - Complete tutorial book from the Pangeo+openEO joint workshop at BiDS 2023, Vienna.
- [BiDS 2023 – openEO Data Exploitation](https://pangeo-data.github.io/pangeo-openeo-BiDS-2023/part3/data_exploitability_openEO.html) - openEO-specific section of the BiDS 2023 tutorial.
- [BiDS 2023 – Advanced openEO Workflows](https://pangeo-data.github.io/pangeo-openeo-BiDS-2023/part2/advanced_workflows.html) - Advanced workflow examples from BiDS 2023.
- [CDSE – Getting Started Notebook](https://documentation.dataspace.copernicus.eu/APIs/openEO/openeo-community-examples/python/1.%20GettingStarted/GettingStarted.html) - First-steps notebook for the Copernicus Data Space backend.
- [CDSE – NDVI Timeseries Notebook](https://documentation.dataspace.copernicus.eu/notebook-samples/openeo/NDVI_Timeseries.html) - NDVI time-series computation for agricultural fields.
- [CDSE – Batch Job Notebook](https://documentation.dataspace.copernicus.eu/notebook-samples/openeo/Batch_job.html) - Running large-scale workflows with batch jobs.
- [CDSE – UDF Notebook](https://documentation.dataspace.copernicus.eu/notebook-samples/openeo/UDF.html) - Writing and running User-Defined Functions.
- [CDSE – UDP Notebook](https://documentation.dataspace.copernicus.eu/notebook-samples/openeo/UDP.html) - Creating and sharing User-Defined Processes.
- [CDSE – Sentinel-3 Notebook](https://documentation.dataspace.copernicus.eu/notebook-samples/openeo/Sentinel_3.html) - Working with Sentinel-3 OLCI/SLSTR data.
- [CDSE – Radar Vegetation Index (RVI) Notebook](https://documentation.dataspace.copernicus.eu/APIs/openEO/openeo-community-examples/python/RVI/RVI.html) - Computing RVI from Sentinel-1 SAR data.
- [CDSE – Parcel Delineation Notebook](https://documentation.dataspace.copernicus.eu/APIs/openEO/openeo-community-examples/python/ParcelDelineation/Parcel%20delineation.html) - Crop parcel delineation using deep learning.
- [WorldCereal Notebook](https://github.com/Open-EO/openeo-community-examples/blob/main/python/WorldCereal/WorldCereal.ipynb) - ESA WorldCereal global crop mapping workflow with openEO.
- [Python Client – Spectral Indices Cookbook](https://open-eo.github.io/openeo-python-client/cookbook/spectral_indices.html) - Computing NDVI, EVI, and other spectral indices.
- [APEx – UDP Writer Guide](https://esa-apex.github.io/apex_documentation/guides/udp_writer_guide.html) - Guide to writing portable User-Defined Processes for the APEx platform.
- [EOPF Zarr – openEO + STAC Webinar](https://zarr.eopf.copernicus.eu/webinars/webinar-7-eopf-zarr-data-access-with-openeo-and-stac/) - Accessing next-generation EOPF Zarr products via openEO and STAC.

---

## Videos

- [openEO Official YouTube Channel](https://www.youtube.com/channel/UCMJQil8j9sHBQkcSlSaEsvQ) - Official channel with demos, tutorials, and presentation recordings.
- [openEO Official Playlist](https://www.youtube.com/playlist?list=PLj4KwRQTBlPJPxyTezVW18v3RxZUUNcah) - Curated playlist on the official channel.
- [Copernicus CDSE YouTube Channel](https://www.youtube.com/@copernicusdataspaceecosystem) - CDSE channel with openEO webinars, demos, and how-to videos.
- [openEO: What it is and how it relates to Pangeo](https://www.youtube.com/watch?v=S_soiazvOmQ) - The clearest conceptual "what is openEO" explainer. Pangeo Showcase 2022.
- [Introduction to openEO within the Copernicus Data Space](https://www.youtube.com/watch?v=Z3iingFH6T8) - Beginner-focused introduction video from CDSE (2025).
- [C-SCALE Tutorial: openEO Platform](https://www.youtube.com/watch?v=pMiaBMh8VCQ) - Introduction to the openEO Platform for newcomers (2022).
- [ESA openEO Platform – Practical Session (Schumacher)](https://www.youtube.com/watch?v=YoF2bl5I1TE) - Practical ESA training session including a "what is the openEO Platform?" section.
- [IT&I: openEO – API for EO Workflows](https://www.youtube.com/watch?v=hGPKwslUGzM) - Conceptual overview talk on openEO as an API for EO workflows.
- [openEO @ Open Data Cube Conference 2021](https://www.youtube.com/watch?v=HR_dmDcaU30) - openEO presentation at the ODC community conference; explains the openEO/ODC relationship.
- [Webinar – Unlocking ML Potential Using openEO](https://www.youtube.com/watch?v=3_Ik-TyKWvw) - Machine learning capabilities within openEO (2025).
- [openEO Algorithm Plaza Playlist](https://www.youtube.com/playlist?list=PLj4KwRQTBlPIXadsw-bM2noKCdNreFOdb) - How-to videos for the openEO Algorithm Plaza.
- [openEO Web Editor Wizard Demo](https://www.youtube.com/watch?v=wagZdIt85O0) - Short demo of the Web Editor Wizard for building workflows visually.
- [Authenticate with the openEO API (Python)](https://www.youtube.com/watch?v=UL6om8Jxh9w) - Short tutorial on authentication using the Python client.
- [SAR2Cube Tutorial](https://www.youtube.com/watch?v=uR3HcmW1eFs) - Using Sentinel-1 SAR data with the SAR2CUBE framework via openEO.
- [Machine Learning on Data Cubes with openEO (TIB)](https://av.tib.eu/media/62890) - Lecture on ML applied to data cubes: concept and motivation.
- [Snow Monitoring with openEO (TIB)](https://av.tib.eu/media/69558) - High-resolution snow monitoring using satellite data and openEO.
- [OpenEO: Earth Observation Data Cubes (CCC, 2019)](https://media.ccc.de/v/bucharest-286-openeo-earth-observation-data-cubes) - Early H2020 project overview from 2019; useful historical context.

---

## Courses & MOOCs

- [Cubes & Clouds MOOC (EO College)](https://eo-college.org/courses/cubes-and-clouds/) - Free 30-hour MOOC covering EO data cubes, cloud platforms, and open science using openEO. ESA-funded and beginner-friendly.
- [ITC – Introduction to openEO for Cloud-Based Geoprocessing (2025)](https://www.utwente.nl/en/itc/about-itc/centres-of-expertise/big-geodata/capacity-development/introduction-to-openeo-2025-06/) - Instructor-led training course at University of Twente ITC (June 2025).
- [ITC – Introduction to openEO (2024 edition)](https://www.itc.nl/about-itc/centres-of-expertise/big-geodata/capacity-development/introduction-to-openeo-2024-06/) - Previous year's edition of the ITC course for reference.
- [EO4GEO – Sentinel-2 & Vegetation Indices](https://eo4geocourses.github.io/IGIK_Sentinel2-Data-and-Vegetation-Indices/) - Online course covering Sentinel-2 data and vegetation indices using openEO concepts.
- [Terrascope Application Development Training (EO4GEO)](https://eo4geocourses.github.io/VITO_TerraScope_TrainingPack_Application_Example/) - VITO/EO4GEO training pack with openEO-based examples.

---

## Slides & Presentations

- [Open EO Introduction by Pratichhya Sharma (SlideShare)](https://www.slideshare.net/slideshow/open-eo-introduction-by-pratichhya-sharma/270786313) - Accessible overview deck covering the openEO API, its benefits, and available resources.
- [SFScon19 – Alexander Jacob – openEO (SlideShare)](https://www.slideshare.net/slideshow/sfscon19-alexander-jacob-openeo/201901143) - Slides from the South Tyrol Free Software Conference 2019 by openEO core team member Alexander Jacob.
- [openEO Platform Basic Training – Riga 2022 (PDF)](https://eo4society.esa.int/wp-content/uploads/2022/07/2022-Riga_openEO_Platform_Basic_Training_Introduction.pdf) - Slide deck from the 12th ESA Training Course on Earth Observation.
- [openEO Platform Overview (ESA eo4society PDF)](https://eo4society.esa.int/wp-content/uploads/2022/11/openeo-platform.pdf) - Standalone openEO Platform overview PDF from ESA eo4society.
- [FOSS4G 2022 – openEO: Open Science for EO Research](https://talks.osgeo.org/foss4g-2022/talk/ELXNEB/) - Conference talk covering openEO's data model, processes, and open science principles.

---

## Podcasts

- Scene From Above – Matthias Mohr on openEO & STAC - EO-focused podcast episode with openEO lead developer Matthias Mohr discussing openEO and STAC. *(site currently offline)*
- [The MapScaping Podcast](https://mapscaping.com/podcasts/) - Geospatial industry podcast that has featured openEO as one of the transformative open-source projects in EO.

---

## Papers & Research

- [The openEO API – Harmonising EO Cloud Services (Remote Sensing, 2021)](https://www.mdpi.com/2072-4292/13/6/1125) - The foundational paper: Schramm et al. introduce the openEO API concept, the virtual data cube model, and the cross-platform benchmarking study.
- [Federated and Reusable EO Processing with openEO (Scientific Data, 2025)](https://www.nature.com/articles/s41597-025-04513-y) - Mohr, Pebesma et al. on openEO Platform federation, FAIR workflows, and reproducibility at scale.
- [Towards Provenance-Aware EO Workflows: the openEO Case Study (arXiv, 2025)](https://arxiv.org/abs/2506.08597) - Integrating the yProv4WFs data provenance library with openEO process graphs.
- [An Overview of Platforms for Big EO Data Management and Analysis (Remote Sensing, 2020)](https://www.mdpi.com/2072-4292/12/8/1253) - Comparative study of seven platforms (GEE, Sentinel Hub, ODC, SEPAL, openEO, JEODPP, pipsCloud).
- [Hands-On openEO: Access Cloud Platforms Using Your Preferred Programming Language (IGARSS, 2021)](https://research.wur.nl/en/publications/hands-on-openeo-access-cloud-platforms-using-your-preferred-progr/) - Conference paper by Kempeneers, Dries, Masiliunas (WUR) from the IGARSS 2021 tutorial.
- [OpenEO: an API that Unifies Access to EO Data (52North, 2018)](https://52north.org/wp-content/uploads/2018/09/GSW18_OpenEO_pebesma.pdf) - Early paper by Pebesma et al. presenting the openEO concept before the API was finalised.
- [How the openEO Project Unifies Access to Big EO Data (52North, 2019)](https://52north.org/wp-content/uploads/2019/09/GS2019-openEO_final.pdf) - Follow-up paper describing the evolving architecture and H2020 project outcomes.
- [ISPRS – MOOC Cubes and Clouds (ISPRS Archives, 2024)](https://isprs-archives.copernicus.org/articles/XLVIII-4-W12-2024/157/2024/) - Paper describing the design and delivery of the Cubes & Clouds MOOC.
- [openEO for GEO.INFORMED](https://www.academia.edu/112181064/openEO_for_GEO_INFORMED) - openEO applied to deep-learning-based EO within the GEO.INFORMED project.
- [GEO-OPEN-HACK: Big Geospatial Data Processing (WUR, 2025)](https://research.wur.nl/en/publications/geo-open-hack-an-initiative-on-big-geospatial-data-processing-wit/) - Paper on a hackathon using openEO, Pangeo, and OEMC infrastructure.
- [openEO D17: Full API Implementation Report (Zenodo)](https://zenodo.org/records/3733193) - H2020 deliverable documenting the first full implementation of the openEO API.

---

## Blog Posts & Articles

### openEO Platform Blog
- [openEO Platform Medium Publication](https://medium.com/openeo-platform) - Official blog with case studies, feature announcements, and tutorials.
- [Continental-Scale Crop Type Mapping in openEO Platform](https://medium.com/openeo-platform/crop-type-mapping-in-openeo-platform-2ade43aab662) - Producing a European-scale crop type map using openEO.
- [openEO Client Side Processing](https://medium.com/openeo-platform/openeo-client-side-processing-e984b4e0478c) - Announcement and explanation of the client-side processing feature.

### r-spatial.org
- [OpenEO: a GDAL for Earth Observation Analytics (2016)](https://r-spatial.org/2016/11/29/openeo.html) - The original blog post that sparked the openEO project, by Edzer Pebesma.
- [First openEO Hackathon Report (2017)](https://r-spatial.org/r/2017/04/19/first-openeo-hackaton.html) - Report from the inaugural openEO hackathon in Münster, January 2017.
- [Processing Large-Scale Satellite Imagery with openEO Platform and R (2022)](https://r-spatial.org/r/2022/11/24/openeo.html) - Practical tutorial using the R client on the openEO Platform.

### R-bloggers
- [Copernicus openEO NDVI Time Series](https://www.r-bloggers.com/2023/08/copernicus-openeo-ndvi-time-series/) - Creating NDVI time series on agricultural land using the openEO R client.

### VITO Blog
- [VITO openEO Topic Page](https://remotesensing.vito.be/hubspot-topics/openeo) - All VITO Remote Sensing posts tagged with openEO.
- [Simplify and Scale EO Workflows with CDSE and openEO](https://blog.vito.be/remotesensing/cdse-and-openeo-for-eoworkflows) - VITO blog on using CDSE and openEO together for scalable workflows.
- [AI4FOOD FuseTS: Sentinel Data Fusion Toolbox](https://blog.vito.be/remotesensing/ai4food-fusets-toolbox) - The FuseTS toolbox integrated with openEO.

### Copernicus Data Space Ecosystem
- [openEO Is Now an Official OGC Community Standard](https://dataspace.copernicus.eu/news/2026-5-19-openeo-now-official-ogc-community-standard) - Announcement of openEO becoming an OGC Community Standard (May 2026).
- [Extending the Reach of openEO – Interactive Processing](https://dataspace.copernicus.eu/news/2025-1-28-extending-reach-openeo-interactive-processing-and-path-forward) - CDSE article on interactive (synchronous) processing capabilities.
- [Unlocking ML Potential Using openEO](https://dataspace.copernicus.eu/news/2025-2-25-unlocking-machine-learning-potential-using-openeo) - Machine learning capabilities in openEO on CDSE.
- [openEO Introduces load_stac](https://dataspace.copernicus.eu/news/2024-6-3-openeo-introduces-loadstac) - Announcement of the `load_stac` process enabling access to any STAC-indexed data.
- [Disaster Monitoring Use Cases Using openEO](https://dataspace.copernicus.eu/news/2024-6-14-disaster-monitoring-use-cases-using-openeo) - Oil spill and forest fire monitoring with openEO.
- [ESA WorldCereal: Unlocking Crop Insights with openEO](https://dataspace.copernicus.eu/news/2024-11-27-esa-worldcereal-unlocking-crop-insights-openeo-and-cdse) - How WorldCereal uses openEO for global crop mapping.
- [From Land Deformation to Change Detection: InSAR Coherence with openEO](https://dataspace.copernicus.eu/news/2026-4-9-land-deformation-change-detection-interferometric-sar-coherence-openeo) - Interferometric SAR coherence analysis tutorial.
- [openEO TiTiler: Open-Source Light Backend](https://dataspace.copernicus.eu/news/2025-6-4-openeo-titiler-open-source-light-and-fast-processing-earth-observation) - Announcement of the TiTiler-based synchronous openEO backend.
- [Revolutionizing EO Data Access: openEO Federation](https://dataspace.copernicus.eu/news/2025-5-23-revolutionizing-eo-data-access-power-openeo-federation) - The power and design of the openEO federation architecture.

### Other
- [52North – Monitoring Ecosystems with openEO and OGC API Processes](https://blog.52north.org/2025/02/17/monitoring-ecosystems-and-habitats-with-openeo-and-ogc-api-processes/) - Combining openEO with OGC API Processes for habitat monitoring.
- [interTwin – openEO as a Core Digital Twin Engine Module](https://www.intertwin.eu/article/core-dte-module-openeo) - How openEO is used within the interTwin Digital Twin Engine.
- [Development Seed – openEO + TiTiler: Earth Data Value Journey](https://developmentseed.org/blog/2025-01-27-eodata-journey/) - Development Seed on building lightweight EO services with openEO and TiTiler.
- [Spatial Source – openEO Becomes a New OGC Community Standard](https://www.spatialsource.com.au/openeo-becomes-a-new-ogc-community-standard/) - Coverage of the OGC standardisation milestone.

---

## Comparison & Positioning

- [Comparing openEO and Sentinel Hub APIs (CDSE, 2024)](https://dataspace.copernicus.eu/news/2024-10-9-comparing-openeo-and-sentinel-hub-apis) - Head-to-head comparison: user interfaces, capabilities, and when to choose each.
- [An Overview of Platforms for Big EO Data (Remote Sensing, 2020)](https://www.mdpi.com/2072-4292/12/8/1253) - Peer-reviewed comparison of seven major EO platforms including openEO, GEE, Sentinel Hub, and OpenDataCube.
- [Sentinel-1 GRD: openEO vs GEE vs Sentinel Hub (CDSE Forum)](https://forum.dataspace.copernicus.eu/t/sentinel-1-grd-openeo-vs-gee-sentinel-hub/4371) - Practical community thread comparing the three platforms for SAR processing.
- [Best Alternatives to Google Earth Engine (FlyPix AI)](https://flypix.ai/google-earth-engine-alternatives/) - Roundup article listing openEO as a GEE alternative with brief positioning notes.

---

## Community & Support

- [CDSE Forum – openEO Category](https://forum.dataspace.copernicus.eu/c/openeo/28) - Primary Q&A and discussion forum for openEO on the Copernicus Data Space Ecosystem.
- [CDSE Forum – Algorithm Plaza](https://forum.dataspace.copernicus.eu/c/openeo/openeo-algorithm-plaza/46) - Forum category for Algorithm Plaza discussions.
- [openEO Platform Forums](https://forums.openeo.cloud/) - User discussion forum for the openEO Platform.
- [EODC Discussions – openEO Platform](https://discuss.eodc.eu/c/openeo-platform/5) - EODC-hosted forum for backend-specific discussions.
- [openEO GitHub Organization](https://github.com/Open-EO) - All source code, issue trackers, and GitHub Discussions.
- [openEO LinkedIn](https://www.linkedin.com/company/openeo/) - Official LinkedIn page for announcements.
- [Pangeo Discourse – openEO Backend](https://discourse.pangeo.io/t/pangeo-openeo-backend/815) - Pangeo community thread on building an openEO-compatible backend.

---

## Conferences & Workshops

- [FOSDEM 2026 – Build Reusable EO Workflows with openEO](https://fosdem.org/2026/schedule/event/MAKYCY-openeo-for-eo-data-analysis/) - Talk on building and sharing reusable openEO workflows on CDSE.
- [FOSDEM 2025 – Unlocking Open-Source Capabilities in CDSE](https://fosdem.org/2025/schedule/event/fosdem-2025-5112-unlocking-open-source-capabilities-in-the-copernicus-data-space-ecosystem/) - Open-source tools including openEO in the Copernicus ecosystem.
- [FOSS4G Europe 2024 – Cubes and Clouds MOOC](https://talks.osgeo.org/foss4g-europe-2024/talk/VE9UWX/) - Presentation of the Cubes & Clouds MOOC at FOSS4G Europe 2024.
- [FOSS4G 2023 – MOOC EO Open Data Science](https://talks.osgeo.org/foss4g-2023/talk/RFCAYS/) - Presentation of the openEO-powered MOOC.
- [FOSS4G 2023 – FuseTS Toolbox Workshop](https://talks.osgeo.org/foss4g-2023-workshop/talk/KACC8S/) - Hands-on workshop on the FuseTS time-series fusion toolbox for openEO.
- [FOSS4G 2022 – openEO: Open Science for EO Research](https://talks.osgeo.org/foss4g-2022/talk/ELXNEB/) - Talk on openEO and open science principles.
- [BiDS 2023 – Pangeo+openEO Joint Tutorial](https://openeo.cloud/2023/11/28/recap-tutorial-with-pangeo-at-bids23-in-vienna/) - Recap of the joint Pangeo+openEO tutorial session at BiDS 2023 (Vienna).
- [IGARSS 2021 – Hands-on openEO Tutorial](https://openeo.org/events/igarrs2021.html) - openEO tutorial session at the IEEE International Geoscience and Remote Sensing Symposium.
- [openEO Virtual User Workshop 2020](https://openeo.org/news/2020-09-02-openeo-user-workshop.html) - The first virtual openEO user workshop.
- [Open-Earth-Monitor Global Workshop 2023](https://earthmonitor.org/events/open-earth-monitor-global-workshop-2023/) - OEMC annual workshop; openEO features throughout.

---

## Projects & Funding

- [openEO H2020 Project (CORDIS)](https://cordis.europa.eu/project/id/776242) - The EU-funded H2020 project (2017–2020, grant #776242, €1.99M) that created openEO.
- [openEO Platform (ESA eo4society)](https://eo4society.esa.int/projects/openeo/) - ESA-funded project building the federated openEO Platform infrastructure.
- [R4openEO (ESA eo4society)](https://eo4society.esa.int/projects/r4openeo/) - ESA project integrating R analytics capabilities with openEO.
- [AI4FOOD (ESA eo4society)](https://eo4society.esa.int/projects/ai4food/) - ESA project that developed the FuseTS toolbox for openEO-based Sentinel data fusion.
- [WorldCereal (ESA)](https://esa-worldcereal.org/en/products/worldcereal-system) - Global crop mapping system powered by openEO.
- [Open-Earth-Monitor Cyberinfrastructure (OEMC)](https://earthmonitor.org/) - EU Horizon project (2022–2026) using openEO as a core processing component.
- [interTwin](https://www.intertwin.eu/intertwin-digital-twin-engine) - EU project whose Digital Twin Engine uses openEO as a core module.
- [APEx (ESA)](https://apex.esa.int/) - ESA Application Propagation Environments using openEO as the algorithm portability standard.
- [Eurac Research – openEO Projects](https://www.eurac.edu/en/projects/openeo-platform) - Eurac Research's role in openEO Platform and related projects.
- [OpenAIRE – openEO H2020 Record](https://explore.openaire.eu/search/project?projectId=corda__h2020::4a8a6c0a161d7c16a2823d39803d202a) - OpenAIRE project record with links to related open-access publications.

---

## Standards

- [OGC Community Standard Announcement (May 2026)](https://openeo.org/news/2026-05-14-openeo-is-an-ogc-community-standard.html) - openEO adopted as an official OGC Community Standard.
- [OGC – openEO Standard Page](https://www.ogc.org/requests/ogc-looks-to-adopt-openeo-as-community-standard-seeking-public-comment-before-moving-to-vote/) - OGC's official page covering the standardisation process.
- [OGC – openEO Standard Page](https://www.ogc.org/requests/ogc-looks-to-adopt-openeo-as-community-standard-seeking-public-comment-before-moving-to-vote/) - OGC's official page covering the standardisation process and submission.
- [openEO Submitted to OGC (Dec 2023)](https://openeo.org/news/2023-12-06-openeo-submitted-to-ogc.html) - PSC announcement of OGC submission.
- [openEO API Federation Extension](https://github.com/Open-EO/openeo-api/tree/master/extensions/federation) - Extension defining how multiple backends interoperate as a federation.

---

## Data Catalogs & Collections

- [openEO Hub – All Backends](https://hub.openeo.org/) - Explore collections and supported processes across every registered backend.
- [CDSE – Collections via openEO](https://documentation.dataspace.copernicus.eu/APIs/openEO/Collections.html) - Full list of Sentinel-1, -2, -3, Landsat, and DEM collections accessible on CDSE.
- [openEO Platform – Collections](https://docs.openeo.cloud/federation/backends/collections.html) - Collections available on the federated openEO Platform.
- [STAC Index – CDSE openEO](https://stacindex.org/catalogs/cdse-openeo) - STAC Index listing for the CDSE openEO API.
- [STAC Index – openEO Platform](https://stacindex.org/catalogs/openeo-platform) - STAC Index listing for the openEO Platform.
- [OEMC on Zenodo](https://zenodo.org/communities/oemc-project/about) - Open-Earth-Monitor Community datasets and publications on Zenodo.

---

## Historical

*The origin and early evolution of openEO.*

- [OpenEO: a GDAL for Earth Observation Analytics (2016)](https://r-spatial.org/2016/11/29/openeo.html) - The blog post by Edzer Pebesma that first articulated the openEO vision.
- [First openEO Hackathon Report (2017)](https://r-spatial.org/r/2017/04/19/first-openeo-hackaton.html) - Report from the inaugural hackathon in Münster, January 2017.
- [OpenEO: an API that Unifies Access to EO Data (2018)](https://52north.org/wp-content/uploads/2018/09/GSW18_OpenEO_pebesma.pdf) - First formal paper describing the openEO API concept.
- [openEO Proves Its Concept (2018)](https://openeo.org/news/2018-03-15-poc.html) - Milestone announcement: proof-of-concept demonstrated across multiple backends.
- [Stable Release of openEO API v1.0.0 (2020)](https://openeo.org/news/2020-07-31-openeo-api-released.html) - The first stable, production-ready API release.
- [New openEO Releases: API v1.2.0 / Processes 2.0.0 RC1 (2023)](https://openeo.org/news/2023-05-25-new-openeo-versions-120-200rc1.html) - Major release adding vector data support.
- [openEO Is an OGC Community Standard (2026)](https://openeo.org/news/2026-05-14-openeo-is-an-ogc-community-standard.html) - The most recent major milestone: adoption as an international standard.
