SCRIPTS = setup unstick status

SOURCES = iio_utils.h generic_buffer.c orientation.c light.c

EXECS = orientation generic_buffer light

LOGNAME := $(shell /usr/bin/logname)
GROUP := $(shell /usr/bin/id -gn `/usr/bin/logname`)

all: generic_buffer orientation light

generic_buffer: generic_buffer.c iio_utils.h
	cc generic_buffer.c $(CFLAGS) -o generic_buffer

orientation: orientation.c iio_utils.h
	cc orientation.c $(CFLAGS) -o orientation

hinge: hinge.c iio_utils.h
	cc hinge.c $(CFLAGS) -o hinge

light: light.c
	cc `pkg-config --cflags glib-2.0 --libs glib-2.0` $(CFLAGS) -o light light.c

permissions:
	@echo Making the control files writable by you
	chgrp $(GROUP) /sys/bus/iio/devices/iio:device*/scan_elements/in_*_en /sys/bus/iio/devices/iio:device*/buffer/* /sys/bus/iio/devices/iio:device*/trigger/current_trigger
	chmod g+w /sys/bus/iio/devices/iio:device*/scan_elements/in_*_en /sys/bus/iio/devices/iio:device*/buffer/* /sys/bus/iio/devices/iio:device*/trigger/current_trigger
	chgrp $(GROUP) /dev/iio:device*
	chmod g+rw /dev/iio:device*

install:	orientation permissions
	install --owner=root --group=root --mode=755 orientation /usr/bin
	install --owner=root --group=root --mode=755 light /usr/bin
	@echo NOTE: In most cases you also need to install drivers from the drivers directory

clean:
	rm -f *.o orientation light generic_buffer
