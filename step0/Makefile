TARGET=test_bl_sgemm_step0  # 定义最终生成的可执行文件名称前缀 

CC  = gcc  # 定义C的编译器
CXX = g++  # 定义C++的编译器

COMPILER_OPT_LEVEL=O3  # 定义编译器优化级别 03表示最高优化级别


CFLAGS = -$(COMPILER_OPT_LEVEL) -march=core-avx2 -fPIC -static  # 定义编译选项 （优化级别、处理器架构、位置无关代码PIC、静态链接）
LDLIBS = -lm  # 数学库-lm

COMMONDIR = ../common
CFLAGS += -I$(COMMONDIR)  # 定义包含公共源代码的目录

ASM_SRC=  # 定义汇编源文件
CC_SRC= my_sgemm.c \
        $(COMMONDIR)/test_bl_sgemm.c \
        $(COMMONDIR)/bl_sgemm_ref.c \
        $(COMMONDIR)/bl_sgemm_util.c    # 定义C源文件

CPP_SRC=    # 定义C++源文件

ALL_ASM_OBJS := $(ASM_SRC:=.o)  # 将源文件列表转换为对象文件列表（添加.o后缀）
ALL_C_OBJS := $(CC_SRC:=.o)
ALL_CXX_OBJS := $(CPP_SRC:=.o)  # 包含了所有的依赖文件（.d后缀），这些文件在编译时由编译器生成，用于记录头文件的依赖关系。

MKDEP_OPT = -MMD -MF $@.d  # $(MKDEP_OPT)用于生成依赖文件。

ALL_OBJS += $(ALL_ASM_OBJS) $(ALL_C_OBJS) $(ALL_CXX_OBJS)
ALL_DEPS := $(ALL_OBJS:=.d)

BLISLAB_TEST_EXE=$(TARGET:=.x) # 定义最终生成的可执行文件名称为 $(TARGET) + .x

.PHONY: all clean  # 定义了all和clean为伪目标，即使存在同名的文件，这些目标也会被执行。  clean：用于清理构建过程中产生的所有文件，包括对象文件、可执行文件和依赖文件。

all: $(BLISLAB_TEST_EXE)  # 默认目标，依赖于$(BLISLAB_TEST_EXE)，即最终的可执行文件。

# include dependency files of application
ifneq ($(MAKECMDGOALS),clean)       # 如果MAKECMDGOALS不是clean，则包含所有依赖文件（-include $(ALL_DEPS)）。
-include $(ALL_DEPS)
endif           

# 基本语法规则 格式如下 
#
#target: dependencies
#    command
#
# target：目标文件，可以是一个目标文件或一个动作名称（例如：all, clean）。
# dependencies：生成目标所依赖的文件或其他目标。
# command：构建目标的命令，必须以 Tab 键开头。

$(BLISLAB_TEST_EXE): $(ALL_OBJS)
	$(CC) $(CFLAGS) $^ -o $@ $(LDFLAGS) $(LDLIBS)  
# $^：表示所有的依赖文件。
# $@：表示目标文件。

# ---------------------------------------------------------------------------
# Object files compiling rules
# ---------------------------------------------------------------------------
#  %.c.o: %.c和%.cpp.o: %.cpp：定义了如何从.c和.cpp源文件编译出.o对象文件。
%.c.o: %.c             # 这是一个通用规则（模式匹配规则），表示如何将任意 .c 文件编译为对应的 .c.o 文件。% 是一个通配符，表示任意文件的前缀
	$(CC) $(CFLAGS) -c $< -o $@ $(LDFLAGS) $(MKDEP_OPT)

%.cpp.o: %.cpp
	$(CXX) $(CFLAGS) -c $< -o $@ $(LDFLAGS) $(MKDEP_OPT)
# ---------------------------------------------------------------------------

clean:
	@rm -rf $(ALL_OBJS) $(SHAREDLIBBLISLAB) $(BLISLAB_TEST_EXE) $(ALL_DEPS)


