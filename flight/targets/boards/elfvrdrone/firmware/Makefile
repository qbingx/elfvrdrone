#
# Copyright (c) 2009-2013, The OpenPilot Team, http://www.openpilot.org
# Copyright (c) 2012, PhoenixPilot, http://github.com/PhoenixPilot
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful, but
# WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
# or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License
# for more details.
#
# You should have received a copy of the GNU General Public License along
# with this program; if not, write to the Free Software Foundation, Inc.,
# 59 Temple Place, Suite 330, Boston, MA 02111-1307 USA
#

ifndef OPENPILOT_IS_COOL
    $(error Top level Makefile must be used to build this target)
endif

include ../board-info.mk
include $(ROOT_DIR)/make/firmware-defs.mk

# ARM DSP library
USE_DSP_LIB ?= NO

# Set to YES to build a FW version that will erase data flash memory
ERASE_FLASH ?= NO

# Set to yes to include Gcsreceiver module
GCSRECEIVER ?= NO

# Enable Diag tasks ?
DIAG_TASKS ?= NO

# List of mandatory modules to include
MODULES += Attitude
MODULES += Stabilization
MODULES += Actuator
MODULES += Receiver
MODULES += ManualControl
MODULES += FirmwareIAP
MODULES += Telemetry
MODULES += UartTrsrRcvr
#MODULES += Battery

# List of optional modules to include
OPTMODULES += CameraStab
#OPTMODULES += ComUsbBridge
OPTMODULES += GPS
OPTMODULES += TxPID
OPTMODULES += Osd/osdoutput
#OPTMODULES += Altitude
#OPTMODULES += Fault

SRC += $(FLIGHTLIB)/notification.c

# Include all camera options
CDEFS += -DUSE_INPUT_LPF -DUSE_GIMBAL_LPF -DUSE_GIMBAL_FF



# Erase flash firmware should be buildable from command line
ifeq ($(ERASE_FLASH), YES)
    CDEFS += -DERASE_FLASH
endif

# List C source files here (C dependencies are automatically generated).
# Use file-extension c for "c-only"-files
ifndef TESTAPP
    ## The standard CMSIS startup
    SRC += $(CMSIS_DEVICEDIR)/system_stm32f10x.c

    ## Application Core
    SRC += ../pios_usb_board_data.c
    SRC += $(OPMODULEDIR)/System/systemmod.c
    SRC += $(OPSYSTEM)/elfvrdrone.c
    SRC += $(OPSYSTEM)/pios_board.c
    SRC += $(FLIGHTLIB)/alarms.c
    SRC += $(FLIGHTLIB)/instrumentation.c
    SRC += $(OPUAVTALK)/uavtalk.c
    SRC += $(OPUAVOBJ)/uavobjectmanager.c
    SRC += $(OPUAVOBJ)/uavobjectpersistence.c
    SRC += $(OPUAVOBJ)/eventdispatcher.c
    
    ## UAVObjects
    SRC += $(OPUAVSYNTHDIR)/accessorydesired.c
    SRC += $(OPUAVSYNTHDIR)/objectpersistence.c
    SRC += $(OPUAVSYNTHDIR)/gcstelemetrystats.c
    SRC += $(OPUAVSYNTHDIR)/flightbatterysettings.c
    SRC += $(OPUAVSYNTHDIR)/flightbatterystate.c
    SRC += $(OPUAVSYNTHDIR)/flighttelemetrystats.c
    SRC += $(OPUAVSYNTHDIR)/faultsettings.c
    SRC += $(OPUAVSYNTHDIR)/flightstatus.c
    SRC += $(OPUAVSYNTHDIR)/systemstats.c
    SRC += $(OPUAVSYNTHDIR)/systemalarms.c
    SRC += $(OPUAVSYNTHDIR)/systemsettings.c
    SRC += $(OPUAVSYNTHDIR)/stabilizationdesired.c
    SRC += $(OPUAVSYNTHDIR)/stabilizationsettings.c
    SRC += $(OPUAVSYNTHDIR)/stabilizationsettingsbank1.c
    SRC += $(OPUAVSYNTHDIR)/stabilizationsettingsbank2.c
    SRC += $(OPUAVSYNTHDIR)/stabilizationsettingsbank3.c
    SRC += $(OPUAVSYNTHDIR)/stabilizationstatus.c
    SRC += $(OPUAVSYNTHDIR)/stabilizationbank.c
    SRC += $(OPUAVSYNTHDIR)/actuatorcommand.c
    SRC += $(OPUAVSYNTHDIR)/actuatordesired.c
    SRC += $(OPUAVSYNTHDIR)/actuatorsettings.c
    SRC += $(OPUAVSYNTHDIR)/accelstate.c
    SRC += $(OPUAVSYNTHDIR)/accelgyrosettings.c
    SRC += $(OPUAVSYNTHDIR)/gyrostate.c
    SRC += $(OPUAVSYNTHDIR)/attitudestate.c
    SRC += $(OPUAVSYNTHDIR)/manualcontrolcommand.c
    SRC += $(OPUAVSYNTHDIR)/watchdogstatus.c
    SRC += $(OPUAVSYNTHDIR)/manualcontrolsettings.c
    SRC += $(OPUAVSYNTHDIR)/flightmodesettings.c
    SRC += $(OPUAVSYNTHDIR)/mixersettings.c
    SRC += $(OPUAVSYNTHDIR)/firmwareiapobj.c
    SRC += $(OPUAVSYNTHDIR)/attitudesettings.c
    SRC += $(OPUAVSYNTHDIR)/camerastabsettings.c
    SRC += $(OPUAVSYNTHDIR)/cameradesired.c
    SRC += $(OPUAVSYNTHDIR)/gpspositionsensor.c
    SRC += $(OPUAVSYNTHDIR)/gpsvelocitysensor.c
    SRC += $(OPUAVSYNTHDIR)/gpssettings.c
    SRC += $(OPUAVSYNTHDIR)/hwsettings.c
    SRC += $(OPUAVSYNTHDIR)/receiveractivity.c
    SRC += $(OPUAVSYNTHDIR)/mixerstatus.c
    SRC += $(OPUAVSYNTHDIR)/ratedesired.c
    SRC += $(OPUAVSYNTHDIR)/txpidsettings.c
    SRC += $(OPUAVSYNTHDIR)/mpu6000settings.c
    # Command line option for Gcsreceiver module
    ifeq ($(GCSRECEIVER), YES)
        SRC += $(OPUAVSYNTHDIR)/gcsreceiver.c
    endif
    # Enable Diag tasks and UAVOs needed
    ifeq ($(DIAG_TASKS), YES)
        CDEFS += -DDIAG_TASKS
        SRC += $(OPUAVSYNTHDIR)/taskinfo.c
        SRC += $(OPUAVSYNTHDIR)/callbackinfo.c
        SRC += $(OPUAVSYNTHDIR)/perfcounter.c
        SRC += $(OPUAVSYNTHDIR)/i2cstats.c
    endif
else
    ## Test Code
    SRC += $(OPTESTS)/test_common.c
    SRC += $(OPTESTS)/$(TESTAPP).c
endif

include $(ROOT_DIR)/make/apps-defs.mk
include $(ROOT_DIR)/make/common-defs.mk
