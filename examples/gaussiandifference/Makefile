APPSOURCES =  xf_gaussian_diff_tb.cpp   xf_gaussian_diff_accel.cpp
EXECUTABLE = gaussiandiff.elf
VERBOSE = #-verbose
PLATFORM = <path-to-reVISION-platform>
SDSCC_FLAGS = -sds-pf ${PLATFORM} \
-sds-hw "xf::GaussianBlur<3, 0, 0, 2160, 3840, 1>"  xf_gaussian_diff_accel.cpp -files ../../include/imgproc/xf_gaussian_filter.hpp -clkid 2 -sds-end -dmclkid 2\
-sds-hw "xf::duplicateMat< 0, 2160, 3840, 1>"  xf_gaussian_diff_accel.cpp -files ../../include/imgproc/xf_duplicateimage.hpp -clkid 2 -sds-end -dmclkid 2\
-sds-hw "xf::delayMat< 15360,0, 2160, 3840, 1>"  xf_gaussian_diff_accel.cpp -files ../../include/imgproc/xf_delay.hpp -clkid 2 -sds-end -dmclkid 2\
-sds-hw "xf::subtract<0, 0, 2160, 3840, 1 >"  xf_gaussian_diff_accel.cpp -files ../../include/core/xf_arithm.hpp -clkid 2 -sds-end -dmclkid 2

CC = sds++ ${SDSCC_FLAGS}

IDIRS = -I. -I${SYSROOT}/usr/include -I ../../include
LDIRS = --sysroot=${SYSROOT} -L=/lib -L=/usr/lib -Wl,-rpath-link=${SYSROOT}/lib,-rpath-link=${SYSROOT}/usr/lib

LLIBS = -lopencv_imgcodecs -lopencv_core -llzma -ltiff -lpng16 -lz -ljpeg -lopencv_imgproc -ldl -lrt -lwebp -lopencv_features2d -lopencv_flann 
#LLIBS = -lopencv_core -lopencv_imgproc -lopencv_imgcodecs 
#OLEVEL = -ggdb
CFLAGS =  -O0 -Wall ${OLEVEL} -c ${IDIRS} -DHLS_NO_XIL_FPO_LIB -mstrict-align -hls-target 1
LFLAGS = ${OLEVEL} ${LDIRS} ${LLIBS}

SOURCES := $(patsubst %,./%,$(APPSOURCES))
OBJECTS := $(APPSOURCES:.cpp=.o)
DEPS := $(OBJECTS:.o=.d)
.PHONY: all
all: ${EXECUTABLE}
${EXECUTABLE}: ${OBJECTS}
	${CC} ${OBJECTS} ${LFLAGS} -o $@
-include ${DEPS}
%.o: %.cpp
	${CC} ${CFLAGS} $< -o $@
clean:
	${RM} ${EXECUTABLE} ${OBJECTS}
ultraclean: clean
	${RM} -rf _sds sd_card ${EXECUTABLE}.bit ${EXECUTABLE} ${OBJECTS}
