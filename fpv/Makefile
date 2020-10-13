#############################################################################
#
# Filename    : Makefile
# Content     : Makefile for building linux OculusWorldDemo
# Created     : 2013
# Authors     : Simon Hallam and Peter Giokaris
# Copyright   : Copyright 2013 OculusVR, Inc. All Rights Reserved
# Instruction : The g++ compiler and stdndard lib packages need to be 
#               installed on the system.  Navigate in a shell to the 
#               directory where this Makefile is located and enter:
#
#               make                builds the release version for the 
#                                   current architechture
#               make clean          delete intermediate release object files 
#                                   and the executabe file
#               make DEBUG=1        builds the debug version for the current
#                                   architechture
#               make clean DEBUG=1  deletes intermediate debug object files 
#                                   and the executable file
#
# Output      : Relative to the directory this Makefile lives in, executable
#               files get built at the following locations depending upon the
#               architechture of the system you are running:
#
#               ./Release/OculusWorldDemo_i386_Release
#               ./Release/OculusWorldDemo_x86_64_Release
#               ./Release/OculusWorldDemo_i386_Debug
#               ./Release/OculusWorldDemo_x86_64_Debug
#
#############################################################################

####### Detect system architecture

SYSARCH       = i386
ifeq ($(shell uname -m),x86_64)
SYSARCH       = x86_64
endif

####### Compiler, tools and options

CXX           = g++
LINK          = g++
MAKE          = make
DELETEFILE    = rm -f
DEFINES       = -DQT_WEBKIT -DGL_GLEXT_PROTOTYPES -fpermissive

####### Detect debug or release

DEBUG         = 0
ifeq ($(DEBUG), 1)
	CXXFLAGS      = -pipe -DDEBUG -g $(DEFINES)
	LFLAGS        = 
	RELEASETYPE   = Debug
else
	CXXFLAGS      = -pipe -O2 $(DEFINES)
	LFLAGS        = -O1
	RELEASETYPE   = Release
endif

####### Paths

LIBOVRPATH    = ../LibOVR
COMMONSRCPATH = ../CommonSrc
3RDPARTYPATH  = ../../3rdParty
INCPATH       = -I. -I.. -I$(COMMONSRCPATH) -I$(LIBOVRPATH)/Include -I$(LIBOVRPATH)/Src -I/usr/include/opencv4/
#OBJPATH       = ./Obj/Linux/$(RELEASETYPE)/$(SYSARCH)
OBJPATH		= ./
CXX_BUILD     = $(CXX) -c $(CXXFLAGS) $(INCPATH) -o $(OBJPATH)/

####### Files

LIBS          = -L$(LIBOVRPATH)/Lib/Linux/$(RELEASETYPE)/$(SYSARCH) \
		-lovr \
		-ludev \
		-lpthread \
		-lGL \
		-lX11 \
		-lXinerama \
		-lopencv_stitching -lopencv_aruco -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_dnn_objdetect -lopencv_dnn_superres -lopencv_dpm -lopencv_highgui -lopencv_face -lopencv_freetype -lopencv_fuzzy -lopencv_hdf -lopencv_hfs -lopencv_img_hash -lopencv_line_descriptor -lopencv_quality -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_shape -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_superres -lopencv_optflow -lopencv_surface_matching -lopencv_tracking -lopencv_datasets -lopencv_text -lopencv_dnn -lopencv_plot -lopencv_ml -lopencv_videostab -lopencv_videoio -lopencv_viz -lopencv_ximgproc -lopencv_video -lopencv_xobjdetect -lopencv_objdetect -lopencv_calib3d -lopencv_imgcodecs -lopencv_features2d -lopencv_flann -lopencv_xphoto -lopencv_photo -lopencv_imgproc -lopencv_core \
		$(pkg-config --libs opencv4)

OBJECTS       = $(OBJPATH)/Fpv.o \
		$(OBJPATH)/Platform.o \
		$(OBJPATH)/Linux_Platform.o \
		$(OBJPATH)/Linux_Gamepad.o \
		$(OBJPATH)/Render_Device.o \
		$(OBJPATH)/Render_GL_Device.o \

TARGET        = ./FPV_$(SYSARCH)_$(RELEASETYPE)

####### Rules

all:    $(TARGET)

$(TARGET):  $(LIBOVRPATH)/Lib/Linux/$(RELEASETYPE)/$(SYSARCH)/libovr.a
	$(MAKE) -C $(LIBOVRPATH) DEBUG=$(DEBUG)

$(TARGET):  $(OBJECTS)
	$(LINK) $(LFLAGS) -o $(TARGET) $(OBJECTS) $(LIBS)

$(OBJPATH)/Fpv.o: Fpv.cpp 
	$(CXX_BUILD)Fpv.o Fpv.cpp

$(OBJPATH)/Platform.o: ../CommonSrc/Platform/Platform.cpp 
	$(CXX_BUILD)Platform.o ../CommonSrc/Platform/Platform.cpp

$(OBJPATH)/Linux_Platform.o: ../CommonSrc/Platform/Linux_Platform.cpp 
	$(CXX_BUILD)Linux_Platform.o ../CommonSrc/Platform/Linux_Platform.cpp

$(OBJPATH)/Linux_Gamepad.o: ../CommonSrc/Platform/Linux_Gamepad.cpp 
	$(CXX_BUILD)Linux_Gamepad.o ../CommonSrc/Platform/Linux_Gamepad.cpp

$(OBJPATH)/Render_Device.o: ../CommonSrc/Render/Render_Device.cpp 
	$(CXX_BUILD)Render_Device.o ../CommonSrc/Render/Render_Device.cpp

$(OBJPATH)/Render_GL_Device.o: ../CommonSrc/Render/Render_GL_Device.cpp 
	$(CXX_BUILD)Render_GL_Device.o ../CommonSrc/Render/Render_GL_Device.cpp

clean:
	-$(DELETEFILE) $(OBJECTS)
	-$(DELETEFILE) $(TARGET)

