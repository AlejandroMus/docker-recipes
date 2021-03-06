# Copyright 2017,2020 Chi-kwan Chan
# Copyright 2017 Center for Astrophysics | Harvard & Smithsonian
# Copyright 2020 Steward Observatory
#
# Licensed under the Apache License, Version 2.0 (the "License"); you
# may not use this file except in compliance with the License.  You
# may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
# implied.  See the License for the specific language governing
# permissions and limitations under the License.

ARG	dsttag=1.6,20200130,1,latest
ARG	srctag=20200130
ARG	hopstag=20200130
ARG	eattag=0cb085aa
ARG	ehtimtag=491dd805
ARG	ehtplottag=7a056749

#==============================================================================
# The base image: the EHT python2 environment/platform

FROM	ehtcon/ehtpython:${srctag} AS base

RUN	sed -n 's/^deb \(.*\) buster main$/deb \1 jessie main non-free/p' \
		/etc/apt/sources.list > /etc/apt/sources.list.d/jessie.list
COPY	jessie.pref /etc/apt/preferences.d/

RUN	laniapt	libgfortran5 libquadmath0 ghostscript-x \
		libfftw3-double3 zlib1g libx11-6 libpng12-0 pgplot5 \
		csh less
RUN	laniapt time liblist-moreutils-perl parallel

#==============================================================================
# The HOPS image

ARG	hopstag
FROM	astcon/hops:${hopstag} AS builder

#==============================================================================
# The EAT runtime image

FROM	base

# Copy HOPS from the builder image
COPY	--from=builder /usr/local/hops /usr/local/hops
COPY	entrypoint /usr/local/sbin/

# Install EHT python packages directly from GitHub
ARG	ehtplottag
RUN	lanisrc https://github.com/liamedeiros/ehtplot/archive/${ehtplottag}.tar.gz \
		/usr/local/src/ehtplot &&\
	cd /usr/local/src/ehtplot &&\
	pip install -e .

ARG	ehtimtag
RUN	lanisrc https://github.com/achael/eht-imaging/archive/${ehtimtag}.tar.gz \
		/usr/local/src/eht-imaging &&\
	cd /usr/local/src/eht-imaging &&\
	pip install -e .

ARG	eattag
RUN	lanisrc https://github.com/sao-eht/eat/archive/${eattag}.tar.gz \
		/usr/local/src/eat &&\
	cd /usr/local/src/eat &&\
	pip install -e .

# Download IERS data package at build time
# WARNING: this breaks reproducibility at build time!  But once this
# image is built, pipelines that use this image can use the cached
# IERS data package all the time, making pipeline results
# reproducible.
RUN	python -c 'from astropy.time import Time; t = Time("2016:001"); print(t.ut1)'
