## 
## Makefile.am 
##

AM_CFLAGS=-I$(top_srcdir)/include

bin_PROGRAMS = mri_vol2vol
mri_vol2vol_SOURCES=mri_vol2vol.c
mri_vol2vol_LDADD= $(addprefix $(top_builddir)/, $(LIBS_MGH))
mri_vol2vol_LDFLAGS=$(OS_LDFLAGS)

# mri_vol2vol is called by qdec. On Mac OSX
# systems 10.11 (El Capitan) and greater implemented SIP
# which necessitates a wrapper script to pass the DYLD_LIBRARY_PATH env var.

install-exec-hook:
	cp $(top_builddir)/mri_vol2vol/mri_vol2vol $(DESTDIR)$(bindir)/mri_vol2vol.bin
	echo "#!/bin/bash" > $(DESTDIR)$(bindir)/mri_vol2vol
	echo "source \$$FREESURFER_HOME/sources.sh" >> $(DESTDIR)$(bindir)/mri_vol2vol
	echo "mri_vol2vol.bin \"\$$@\"" >> $(DESTDIR)$(bindir)/mri_vol2vol

# Our release target. Include files to be excluded here. They will be
# found and removed after 'make install' is run during the 'make
# release' target.
EXCLUDE_FILES=""
include $(top_srcdir)/Makefile.extra

## ----
## CUDA
## ----

# BUILDCUDA is defined if configure.in finds CUDA
if BUILDCUDA

# rules for building cuda files
.cu.o:
	$(NVCC) -o $@ -c $< $(NVCCFLAGS) $(CUDA_CFLAGS) $(AM_CFLAGS) $(MNI_CFLAGS)

bin_PROGRAMS += mri_vol2vol_cuda

mri_vol2vol_cuda_SOURCES = mri_vol2vol.c

mri_vol2vol_cuda_CFLAGS = $(CUDA_CFLAGS) $(AM_CFLAGS) -DFS_CUDA
mri_vol2vol_cuda_CXXFLAGS = $(CUDA_CFLAGS) $(AM_CFLAGS) -DFS_CUDA
mri_vol2vol_cuda_LDADD = $(addprefix $(top_builddir)/, $(LIBS_CUDA_MGH)) $(CUDA_LIBS)
mri_vol2vol_cuda_LDFLAGS = $(OS_LDFLAGS) 
mri_vol2vol_cuda_LINK = $(LIBTOOL) --tag=CC $(AM_LIBTOOLFLAGS) \
        $(LIBTOOLFLAGS) --mode=link $(CCLD) $(mri_em_register_cuda_CFLAGS) \
        $(CFLAGS) $(mri_em_register_cuda_LDFLAGS) $(LDFLAGS) -o $@

endif
