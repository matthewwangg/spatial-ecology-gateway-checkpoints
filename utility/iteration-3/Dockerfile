# Use the Rocker Project's Geospatial image
FROM rocker/geospatial:4.3.0

# Set the default CRAN mirror to use binary packages
RUN echo 'options(repos = c(CRAN = "https://packagemanager.rstudio.com/cran/__linux__/focal/latest"))' >> "${R_HOME}/etc/Rprofile.site"

# Install system dependencies
RUN apt-get update && apt-get install -y \
    libudunits2-dev \
    libgdal-dev \
    libgeos-dev \
    libproj-dev \
    libssl-dev \
    libcurl4-openssl-dev \
    libxml2-dev \
    libsodium-dev \
    libpng-dev \
    libsecret-1-dev \
    libjpeg-dev \
    libtiff-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install R packages individually to isolate failures and optimize build speed
RUN install2.r --error --skipinstalled -n -1 shiny
RUN install2.r --error --skipinstalled -n -1 ggplot2
RUN install2.r --error --skipinstalled -n -1 readr
RUN install2.r --error --skipinstalled -n -1 stringr
RUN install2.r --error --skipinstalled -n -1 DT
RUN install2.r --error --skipinstalled -n -1 tools
RUN install2.r --error --skipinstalled -n -1 R.utils
RUN install2.r --error --skipinstalled -n -1 shinyjs
RUN install2.r --error --skipinstalled -n -1 shinycssloaders
RUN install2.r --error --skipinstalled -n -1 shinydashboard
RUN install2.r --error --skipinstalled -n -1 shinyBS
RUN install2.r --error --skipinstalled -n -1 shinyFiles
RUN install2.r --error --skipinstalled -n -1 mkde
RUN install2.r --error --skipinstalled -n -1 move2
RUN install2.r --error --skipinstalled -n -1 keyring
RUN install2.r --error --skipinstalled -n -1 stars
RUN install2.r --error --skipinstalled -n -1 ggmap

# Copy the Shiny app code into the Docker image
COPY app.R cleanUTMZones.R loadDataframe.R processDataframe.R compute.R util.R /srv/shiny-server/myapp/

# Set the working directory to the app directory
WORKDIR /srv/shiny-server/myapp/

# Expose port 3838 for the Shiny app
EXPOSE 3838

# Command to run the Shiny app when the container starts
CMD ["R", "-e", "shiny::runApp('.', host='0.0.0.0', port=3838)"]
