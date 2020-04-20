# [`EAT-Jupyter`][1]

This Dockerfile builds a Jupyter environment on top of [Event Horizon
Telescope Analysis Toolkit (`EAT`)](https://github.com/sao-eht/eat)
for performing data calibration for the [Event Horizon Telescope
(EHT)](https://eventhorizontelescope.org/)'s observations.

Because this Docker image is based on AstroContainers, which use
Project Laniakea, one can simply run this `eat-jupyter` Docker image
as a Laniakea subcommand.
That is,

    l6a eat-jupyter [args]

Details on using AstroContainers and Laniakea images can be found in
https://astrocontainers.org/ and https://l6a.org, respectively.

## Releases

[`eat-jupyter`][1] tags | `eat` tag
--- | ---

In general, although a Docker image is immutable, building a new image
is not, even with the same Dockerfile.
Nevertheless, by following Project Laniakea's best practices and using
the Laniakea base image, building an EAT container using this
Dockerfile should be reproducible.
Non-reproducibility is considered a bug in Project Laniakea, please
report non-reproducibility issues to the maintainer.

[1]: https://hub.docker.com/repository/docker/ehtcon/eat-jupyter