# Use the Rocker Project's R version 4.3 image
FROM rocker/r-ver:4.3.0

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
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Set the default CRAN mirror
RUN echo 'options(repos = c(CRAN = "https://cloud.r-project.org"))' >>"${R_HOME}/etc/Rprofile.site"

# Install R packages from CRAN
RUN install2.r --error \
    --deps TRUE \
     keyring \
     readr

# Additional R package installation using Rscript
RUN Rscript -e "install.packages(c('shiny', 'ggplot2', 'mkde', 'R.utils', 'shinyjs', 'shinycssloaders', 'DT', 'stringr', 'shinydashboard', 'shinyBS', 'shinyFiles', 'tools', 'ggmap', 'terra', 'sf', 'move2'))"

# Set the working directory inside the Docker container
WORKDIR /srv/shiny-server/myapp

# Copy the Shiny app code into the Docker image
COPY app.R cleanUTMZones.R loadDataframe.R processDataframe.R compute.R util.R /srv/shiny-server/myapp/

# Expose port 3838 to run Shiny applications
EXPOSE 3838

# Command to run Shiny app when container starts
CMD ["R", "-e", "shiny::runApp('/srv/shiny-server/myapp', host='0.0.0.0', port=3838)"]