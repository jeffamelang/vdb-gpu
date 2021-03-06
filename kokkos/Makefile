#
# Makefile for vdb-gpu
#

KOKKOS_PATH = /research/jeff/kokkos
KOKKOS_DEVICES = "OpenMP"
EXE_NAME = "testing"

SRC = $(wildcard *.cpp)

default: build
	echo "Start Build"


ifneq (,$(findstring Cuda,$(KOKKOS_DEVICES)))
CXX = ${KOKKOS_PATH}/config/nvcc_wrapper
EXE = ${EXE_NAME}.cuda
KOKKOS_ARCH = "SNB,Kepler35"
KOKKOS_CUDA_OPTIONS = "enable_lambda"
else
CXX = g++
EXE = ${EXE_NAME}.host
KOKKOS_ARCH = "SNB"
endif

CXXFLAGS = -O0 -g -std=c++11 -Wall -Wextra -pedantic
LINK = ${CXX}
LINKFLAGS =

DEPFLAGS = -M

HEADERS = $(wildcard *.hpp)

OBJ = $(SRC:.cpp=.o)
LIB =

include $(KOKKOS_PATH)/Makefile.kokkos

build: $(EXE)

$(EXE): $(OBJ) $(KOKKOS_LINK_DEPENDS)
	$(LINK) $(KOKKOS_LDFLAGS) $(LINKFLAGS) $(EXTRA_PATH) $(OBJ) $(KOKKOS_LIBS) $(LIB) -o $(EXE)

clean: kokkos-clean
	rm -f *.o *.cuda *.host

# Compilation rules

%.o:%.cpp $(KOKKOS_CPP_DEPENDS) $(HEADERS)
	$(CXX) $(KOKKOS_CPPFLAGS) $(KOKKOS_CXXFLAGS) $(CXXFLAGS) $(EXTRA_INC) -c $<
