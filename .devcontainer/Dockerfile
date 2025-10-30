ARG JULIA_VERSION=1.12
FROM julia:${JULIA_VERSION}

# Set up Julia depot path to persist across workspace mounts
ENV JULIA_DEPOT_PATH=/opt/julia/depot

# Create depot directory
RUN mkdir -p ${JULIA_DEPOT_PATH}

# Set working directory
WORKDIR /tmp/build

# Copy Julia project files
COPY Project.toml Manifest.toml ./

# Install and precompile Julia packages with CPU-specific optimizations
RUN dpkgArch="$(dpkg --print-architecture)" \
  && case "${dpkgArch}" in \
    amd64) export JULIA_CPU_TARGET="generic;sandybridge,-xsaveopt,clone_all;haswell,-rdrnd,base(1);x86-64-v4,-rdrnd,base(1)" ;; \
    arm64) export JULIA_CPU_TARGET="generic;cortex-a57;thunderx2t99;carmel,clone_all;apple-m1,base(3);neoverse-512tvb,base(3)" ;; \
    *) echo "Unknown target processor architecture '${dpkgArch}'" >&2; exit 1 ;; \
  esac \
  && julia --project=. -e 'using Pkg; Pkg.instantiate(); Pkg.precompile()' \
  && rm -rf /tmp/*

# Set the workspace
WORKDIR /workspace

# Set the default project to the workspace
ENV JULIA_PROJECT=/workspace

# Keep the container running
CMD ["bash"]
