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

# 3. KPP requires a lexical analizer like FLEX to be used.
#    FLEX is a public domain lexical analizer and you can download it from
#    http://www.gnu.org/software/flex/ or any other mirror site. If flex
#    directory is not included in your path use the complete pathname.

FLEX=flex

# 4. Flex library. Can either be -lfl or the full path to a libfl.a
FLEX_LIB=-lfl


# 5. Linker flags to ensure access to the libraries including libfl.a
#    If libfl.a is in a standard location, this can be blank.
#    Otherwise LDFLAGS=-L<DIRPATH> where DIRPATH is the path that
#    contains libfl.a
LDFLAGS=

# 6. The name of the fortran compiler you want to use. Normaly this 
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

TOPDIR=..

MODEL=dsmacc

##############################################################################

PROG = $(TOPDIR)/bin/$(MODEL) 

all: libtuv.a libfastjx.a $(PROG)

params:
	ln -s $(TOPDIR)/tuv_new/params

libtuv.a:
	make -C $(TOPDIR)/tuv_new
	ln -s $(TOPDIR)/tuv_new/$@ $@

libfastjx.a:
	make -C $(TOPDIR)/UCI_fastJX72e/
	ln -s $(TOPDIR)/UCI_fastJX72e/$@ $@

# complete list of all f90 source files
SRCS1 = $(wildcard ./$(MODEL)_*.f90)
SRCS3 = constants.f90 

# the object files are the same as the source files but with suffix ".o"
OBJS1 := $(SRCS1:.f90=.o) 
OBJS := $(OBJS1)
LIBS = -L$(TOPDIR)/tuv_new/ -ltuv -L$(TOPDIR)/UCI_fastJX72e/ -lfastjx

# the dependencies depend on the link
# the executable depends on depend and also on all objects
# the executable is created by linking all objects
$(PROG): $(MODEL)_Main.f90 $(OBJS) $(MODEL)_Main.f90
	@mkdir -p $(TOPDIR)/bin
	$(FC) $(F90FLAGS) $(OBJS) $(LIBS) -o $@

# update file dependencies
depend depend.mk: $(SRCS1) $(SRCS2) $(SRCS3)
	$(TOPDIR)/sfmakedepend --file=depend.mk --srcdir=. --moddir=.

clean:
	rm -f $(OBJS) *.mod *.log *~ depend.mk.old

distclean: clean
	rm -f $(PROG)
	rm -f depend.mk* 
	rm -f *.nc
	rm -f *.dat
	rm -f $(SRCS1) Makefile_$(MODEL) $(MODEL).map tuvlog.txt $(TOPDIR)/bin/$(MODEL)
	rm -f fflags

# all object files *.o depend on their source files *.f90
# the object files are created with the "-c" compiler option
%.o: %.f90
	$(FC) $(F90FLAGS) $(LINCLUDES) -c $<

$(TOPDIR)/tuv_new/%.o: $(TOPDIR)/tuv_new/%.f
	$(FC) $(TUVF90FLAGS) $(LINCLUDES) -o $@ -c $<

$(TOPDIR)/UCI_fastJX72e/%.o: $(TOPDIR)/UCI_fastJX72e/%.F90
	$(FC) $(F90FLAGS) -J$(TOPDIR)/UCI_fastJX72e $(LINCLUDES) -o $@ -c $<

# list of dependencies (via USE statements)
#include depend.mk
./$(MODEL)_Function.o: ./$(MODEL)_Parameters.o
./$(MODEL)_Global.o: params
./$(MODEL)_Global.o: ./$(MODEL)_Parameters.o
./$(MODEL)_Initialize.o: ./$(MODEL)_Global.o ./$(MODEL)_Parameters.o ./$(MODEL)_Util.o
./$(MODEL)_Integrator.o: ./$(MODEL)_Function.o ./$(MODEL)_Global.o
./$(MODEL)_Integrator.o: ./$(MODEL)_Jacobian.o ./$(MODEL)_LinearAlgebra.o
./$(MODEL)_Integrator.o: ./$(MODEL)_Parameters.o ./$(MODEL)_Rates.o
./$(MODEL)_Jacobian.o: ./$(MODEL)_JacobianSP.o ./$(MODEL)_Parameters.o
./$(MODEL)_LinearAlgebra.o: ./$(MODEL)_JacobianSP.o ./$(MODEL)_Parameters.o
./$(MODEL)_Main.o: ./$(MODEL)_Global.o ./$(MODEL)_Integrator.o ./$(MODEL)_Monitor.o
./$(MODEL)_Main.o: ./$(MODEL)_Parameters.o ./$(MODEL)_Rates.o ./$(MODEL)_Util.o
./$(MODEL)_Model.o: ./$(MODEL)_Function.o ./$(MODEL)_Global.o ./$(MODEL)_Integrator.o
./$(MODEL)_Model.o: ./$(MODEL)_Jacobian.o ./$(MODEL)_LinearAlgebra.o
./$(MODEL)_Model.o: ./$(MODEL)_Monitor.o ./$(MODEL)_Parameters.o ./$(MODEL)_Precision.o
./$(MODEL)_Model.o: ./$(MODEL)_Rates.o ./$(MODEL)_Stoichiom.o ./$(MODEL)_Util.o
./$(MODEL)_Parameters.o: ./$(MODEL)_Precision.o
./$(MODEL)_Rates.o: ./$(MODEL)_Global.o ./$(MODEL)_Parameters.o
./$(MODEL)_Stoichiom.o: ./$(MODEL)_Parameters.o ./$(MODEL)_StoichiomSP.o
./$(MODEL)_StoichiomSP.o: ./$(MODEL)_Precision.o
./$(MODEL)_Util.o: ./$(MODEL)_Global.o ./$(MODEL)_Integrator.o ./$(MODEL)_Monitor.o
./$(MODEL)_Util.o: ./$(MODEL)_Parameters.o
