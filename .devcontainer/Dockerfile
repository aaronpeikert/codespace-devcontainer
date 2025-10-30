FROM julia:1.12

# Set working directory
WORKDIR /workspace

# Copy Julia project files
COPY Project.toml Manifest.toml ./

# Install Julia packages using the manifest
RUN julia --project=. -e 'using Pkg; Pkg.instantiate(); Pkg.precompile()'

# Set the default project to the workspace
ENV JULIA_PROJECT=/workspace

# Keep the container running
CMD ["bash"]
