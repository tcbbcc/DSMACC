export KPP_HOME=$(PWD)/../kpp
all:
	make check

check:
	make mz4.check
	make cri.check
	make geos.check
	make mz4.show
	make cri.show
	make geos.show

%.show:
	@echo "$* Timeseries Check"
	@tail -n 3 $*.check.timeseries.log
	@echo "$* Diurnal Check"
	@tail -n 3 $*.check.diurnal.log

%.check:
	make $*.check.timeseries.log
	make $*.check.diurnal.log
	make $*.show

.SECONDARY: geos geos.Spec_1.dat.diurnal geos.Spec_1.dat.timeseries cri cri.Spec_1.dat.diurnal cri.Spec_1.dat.timeseries mz4 mz4.Spec_1.dat.diurnal mz4.Spec_1.dat.timeseries

%.check.diurnal.log: %.Spec_1.dat.diurnal check.py
	@python check.py check.$*.Spec_1.dat.diurnal $*.Spec_1.dat.diurnal > $@

%.check.timeseries.log: %.Spec_1.dat.timeseries check.py
	@python check.py check.$*.Spec_1.dat.timeseries $*.Spec_1.dat.timeseries > $@

%.Spec_1.dat.diurnal: % %.Init_cons.dat.diurnal
	ln -fs $*.Init_cons.dat.diurnal Init_cons.dat && \
	./$* >& $*.diurnal.log && mv Spec_1.dat $*.Spec_1.dat.diurnal && mv Rate_1.dat $*.Rate_1.dat.diurnal

%.Spec_1.dat.timeseries: % %.Init_cons.dat.timeseries
	ln -fs $*.Init_cons.dat.timeseries Init_cons.dat && \
	./$* >& $*.timeseries.log && mv Spec_1.dat $*.Spec_1.dat.timeseries && mv Rate_1.dat $*.Rate_1.dat.timeseries

cri: cri.kpp ../src/global.inc ../src/mcmratedef.inc ../src/io.inc ../driver.f90 ../src/tuv.inc ../kpp/bin/kpp
	cd ../src && rm -f depend.mk && ../kpp/bin/kpp ../test/cri.kpp dsmacc && make && mv ../bin/dsmacc ../test/cri

geos: geos.kpp ../src/global.inc ../src/mcmratedef.inc ../src/io.inc ../driver.f90 ../src/tuv.inc ../kpp/bin/kpp
	cd ../src && rm -f depend.mk && ../kpp/bin/kpp ../test/geos.kpp dsmacc && make && mv ../bin/dsmacc ../test/geos

geoschem-09-02: geoschem-09-02.kpp ../src/global.inc ../src/mcmratedef.inc ../src/io.inc ../driver.f90 ../src/tuv.inc ../kpp/bin/kpp
	cd ../src && rm -f depend.mk && ../kpp/bin/kpp ../test/geoschem-09-02.kpp dsmacc && make && mv ../bin/dsmacc ../test/geoschem-09-02

mz4: mz4.kpp ../src/global.inc ../src/mcmratedef.inc ../src/io.inc ../driver.f90 ../src/tuv.inc ../kpp/bin/kpp
	cd ../src && rm -f depend.mk && ../kpp/bin/kpp ../test/mz4.kpp dsmacc && make && mv ../bin/dsmacc ../test/mz4

clean:
	rm -f cri.Spec_1.dat.* cri.Rate_1.dat.* mz4.Spec_1.dat.* mz4.Rate_1.dat.* geos.Spec_1.dat.* geos.Rate_1.dat.* *.diurnal.log *.timeseries.log cri geos mz4
