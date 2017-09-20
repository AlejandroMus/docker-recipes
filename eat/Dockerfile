# This Dockfiler builds the Event Horizon Telescope Analysis Toolkit
# (EAT) environment for performing data calibration for the Event
# Horizon Telescope (EHT)'s April 2017 observations.
#
# NOTE: Although a Docker image is immutable, building a new image is
# not, even when the same Dockfile is used.  Hence, the resulting
# images of `docker build .` may be different at different time.
#
#==============================================================================
# The builder image
FROM debian:stretch-slim AS builder

# Setup build environment
RUN apt-get update && apt-get install -y git &&\
	rm -rf /var/lib/apt/lists/*

# Get EAT source
WORKDIR /usr/local/src
RUN git clone https://github.com/sao-eht/eat.git &&\
	rm -rf eat/.git

#==============================================================================
# The EAT image
FROM chanchikwan/hops

# Setup runtime environment
RUN apt-get update && apt-get install -y --no-install-recommends \
		python-astropy python-future python-matplotlib python-pandas \
		python-scipy python-setuptools python-pip &&\
	rm -rf /var/lib/apt/lists/*

# Copy EAT source and install it
COPY --from=builder /usr/local/src/eat /usr/local/src/eat
RUN cd /usr/local/src/eat && pip install . &&\
	printf '\n# prepend the EAT script path to $PATH\n\
  export PATH="/usr/local/src/eat/bin:$PATH"\n\
' >> /etc/bash.bashrc