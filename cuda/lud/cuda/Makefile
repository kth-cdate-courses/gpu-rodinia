include ../../../common/make.config
CC = gcc
NVCC = nvcc
INCLUDE := $(CUDA_DIR)/include

DEFS += \
		-DGPU_TIMER \
		$(SPACE)

NVCCFLAGS += -I../common \
			 -I../../util \
			 -I$(INCLUDE) \
			 -O3 \
			 -use_fast_math \
			 -arch=sm_75 \
			 -lm \
			 -DTIMING \
			 $(SPACE)

CFLAGS += -I../common \
					-I/usr/include/cuda \
		  -O3 \
		  -Wall \
		  $(SPACE)

# Add source files here
EXECUTABLE  := lud_cuda
# Cuda source files (compiled with cudacc)
CUFILES     := lud_kernel.cu
# C/C++ source files (compiled with gcc / c++)
CCFILES     := lud.c lud_cuda.c ../common/common.c
# C/CUDA source files (compiled with nvcc / c++)
NVCCFILES	:= lud.cu lud_kernel.cu ../common/common.c

OBJS = ../common/common.o lud.o lud_kernel.o 

.PHONY: all clean 
all : $(EXECUTABLE)

#release: $(SRC)
#	$(NVCC) $(NVCCFILES) $(NVCCFLAGS) -o $(EXECUTABLE)

.c.o : 
	$(NVCC) $(KERNEL_DIM) $(NVCCFLAGS) $(DEFS) -o $@ -c $<

%.o:	%.cu 
	$(NVCC) $(KERNEL_DIM) $(NVCCFLAGS) $(DEFS) -o $@ -c $<

clang: $(SRC)
	clang++ lud.cu lud_kernel.cu ../common/common.c -o $(EXECUTABLE) \
		-I../common -I../../util --cuda-gpu-arch=sm_75 \
		-L/usr/local/cuda-12.1/lib64 -lcudart_static -ldl -lrt -pthread -DTIMING

$(EXECUTABLE) : $(OBJS)
	$(NVCC) $(NVCCFLAGS) -o $@  $?

clean:
	rm -f $(EXECUTABLE) $(OBJS) *.linkinfo
