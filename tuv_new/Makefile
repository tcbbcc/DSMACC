##############################################################################
# -*- sh -*-
##############################################################################
#
#  DSMACC - The Dynamically Simple Model of Atmospheric Chemical Complexity
#        uses KPP to build simulation code for chemical kinetic systems
#
##############################################################################

# Basic compilers and compiler options are selected using configure

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

# 3. Linker flags to ensure access to the libraries including libfl.a
#    If libfl.a is in a standard location, this can be blank.
#    Otherwise LDFLAGS=-L<DIRPATH> where DIRPATH is the path that
#    contains libfl.a
LDFLAGS=

# 4. The name of the fortran compiler you want to use. Normaly this 
#    is either GNU Fortran compiler (gfortran), the Intel Fortran
#    compiler (ifort), or the Portland Group Compiler.
#    For GNU C compiler use:
#      FC=gfortran
#    For the Intel compiler use:
#      FC=ifort

FC=gfortran

# 6. Platform independent C compiler flags. By default "-O" is used which 
#    turns on optimisation. If you are experiencing problems you may try 
#    "-g" to include debuging informations.

F90FLAGS=-cpp -g -O2 -fno-automatic -fcheck=bounds -fimplicit-none


SRCS =	main.f90 functs.f grids.f la_srb.f numer.f odo3.f odrl.f orbit.f \
	qys.f rdetfl.f rdinp.f rdxs.f rtrans.f rxn.f savout.f setaer.f \
	setalb.f setcld.f setno2.f seto2.f setsnw.f setso2.f sphers.f \
	swbiol.f swchem.f swphys.f TUV.f vpair.f vpo3.f vptmp.f wshift.f

OBJS =	functs.o grids.o la_srb.o numer.o odo3.o odrl.o orbit.o qys.o \
	rdetfl.o rdinp.o rdxs.o rtrans.o rxn.o savout.o setaer.o setalb.o \
	setcld.o setno2.o seto2.o setsnw.o setso2.o sphers.o swbiol.o \
	swchem.o swphys.o TUV.o vpair.o vpo3.o vptmp.o wshift.o

LIBS =	


all: libtuv.a

libtuv.a: $(OBJS)
	ar -rcs $@ $(OBJS) $(LIBS)

clean:
	rm -f $(PROG) $(OBJS) *.mod

.SUFFIXES: $(SUFFIXES) .f90

.f.o:
	$(FC) $(F90FLAGS) -c $<

main.o: params
grids.o: params
la_srb.o: params
odo3.o: params
odrl.o: params
rdetfl.o: params
rdinp.o: params
rdxs.o: params
rtrans.o: params
rxn.o: params
savout.o: params
setaer.o: params
setalb.o: params
setcld.o: params
setno2.o: params
seto2.o: params
setsnw.o: params
setso2.o: params
sphers.o: params
swbiol.o: params
swchem.o: params
swphys.o: params
TUV.o: params
vpair.o: params
vpo3.o: params
vptmp.o: params
wshift.o: params
