##############################################################################
# -*- sh -*-
##############################################################################

# In order to compile UCI you have to provide the following information:

# 1. The name of the compiler you want to use. Normaly this 
#    is either GNU C compiler (gcc) or the native compiler (cc)
#    You can use the complete pathname if the compiler is not in $PATH 
#    Note that for SUN machines is better to use gcc.
#    For GNU C compiler use:
#      CC=gcc
#    For the native compiler use:
#      CC=cc

CC=gcc

# 2. Platform independent C compiler flags. By default "-O" is used which 
#    turns on optimisation. If you are experiencing problems you may try 
#    "-g" to include debuging informations.

CFLAGS=-g -O2
CPPFLAGS=

# 3. Linker flags to ensure access to the libraries
LDFLAGS=

# 4. The name of the fortran compiler you want to use. Normaly this 
#    is either GNU Fortran compiler (gfortran), the Intel Fortran
#    compiler (ifort), or the Portland Group Compiler.
#    For GNU C compiler use:
#      FC=gfortran
#    For the Intel compiler use:
#      FC=ifort

FC=gfortran

# 5. Platform independent C compiler flags. By default "-O" is used which 
#    turns on optimisation. If you are experiencing problems you may try 
#    "-g" to include debuging informations.

F90FLAGS=-cpp -g -O2 -fno-automatic -fcheck=bounds -fimplicit-none

SRCS =	dsmacc_fjx72.F90 cld_sub_mod.F90 fjx_init_mod.F90 fjx_sub_mod.F90  \
	cmn_fjx_mod.F90 

OBJS =	cld_sub_mod.o cmn_fjx_mod.o fjx_dsmacc.o fjx_init_mod.o \
	fjx_sub_mod.o


all: libfastjx.a

libfastjx.a: $(OBJS)
	ar -rcs $@ $(OBJS) $(LIBS)

clean:
	rm -f $(PROG) $(OBJS) *.mod

.SUFFIXES: $(SUFFIXES) .F90

.F90.o:
	$(FC) $(F90FLAGS) -c $<

fjx_dsmacc.o:  cmn_fjx_mod.o fjx_sub_mod.o fjx_init_mod.o cld_sub_mod.o
cld_sub_mod.o:   cmn_fjx_mod.o fjx_sub_mod.o
fjx_init_mod.o:  cmn_fjx_mod.o fjx_sub_mod.o
fjx_sub_mod.o:   cmn_fjx_mod.o
