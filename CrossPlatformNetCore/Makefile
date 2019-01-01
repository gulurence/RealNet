CC:= g++

CCOPT:= -g -Wall -Wno-unknown-pragmas -Wno-deprecated --std=c++0x #-Werror 

CURRENT_PATH:=$(shell pwd)

SRC_PATH:=$(CURRENT_PATH)/.

myinclude=$(CURRENT_PATH)/.

#MYINCLUDE = -I$(myinclude)/test -I$(myinclude)/xNet -I$(myinclude)/xCmd -I$(myinclude)/xServer -I$(myinclude)/xTool -I$(myinclude)/xTool/database -I$(myinclude)/xTool/zlib -I$(myinclude)/xTool/mySQL5.5 -I$(myinclude)/xTool/des

#MYLIB = -L$(lib)/XConfig/ -lXConfig -L$(lib)/XGameData/ -lXGameData -L/usr/local/mysql5.6/lib -L$(lib)/libXGame -lXGame -L$(lib)/libXFindPath/ -lXFindPath -L$(lib)/libXServer -lXServer -L$(lib)/libXNet -lXNet  -L$(lib)/recastnavigation/Detour/ -L$(lib)/recastnavigation/Recast/ -lDetour -lRecast -L$(lib)/util/ -lUtil  -L$(lib)/libXBase/ -lXBase

INC_PATH:= -I/usr/include/ -I/usr/local/include/libxml2/ -I$(myinclude)/test -I$(myinclude)/xNet -I$(myinclude)/xCmd -I$(myinclude)/xServer -I$(myinclude)/xTool -I$(myinclude)/xTool/database -I$(myinclude)/xTool/zlib -I$(myinclude)/xTool/mySQL5.5 -I$(myinclude)/xTool/des $(DEBUG_FLAG) 

LIB_PATH:= ${MYLIB} -L/usr/lib64/ -L/lib64/ -L$(myinclude)/xTool/des -L/usr/local/mysql5.6/lib -lxml2 -lpthread -lmysqlclient -llog4cxx  -lrt -ldl -lz -ld3des

OBJS_PATH:=$(CURRENT_PATH)/obj

SRC_FILE_DIR:=$(wildcard $(SRC_PATH)/*.cpp)

SRC_FILE:=$(notdir $(SRC_FILE_DIR))

OBJS_FILE:=$(patsubst %.cpp,%.o,$(SRC_FILE))

OBJS:=$(patsubst %.o,$(OBJS_PATH)/%.o,$(OBJS_FILE))

define walk
	$(wildcard $(1)) $(foreach e, $(wildcard $(1)/*), $(call walk, $(e)))
endef

OUTOBJS= $(SRC_PATH)/User/MsgHandle/%.cpp

BINOBJS =$(patsubst %.cpp,$(OBJS_PATH)/%.o,$(notdir $(filter-out $(OUTOBJS),$(filter %.cpp, $(call walk, $(SRC_PATH))))))

SUBDIRS= test xNet xCmd xServer xTool xTool/database

TARGET:=SceneServerBin

.PHONY : all

all: handle debug	
	@echo "--------- 警告: 当前为调试版本 ---------"
debug: $(TARGET)	

handle:
	@for dir in $(SUBDIRS);\
		do \
			CCOPT='$(CCOPT)' SRCPATH=$(SRC_PATH)/$$dir OBJPATH=$(OBJS_PATH) CXX=$(CC) DIR=$$dir CXXLFLAGS='$(LIB_PATH)' CXXFLAGS='$(INC_PATH)' $(MAKE) -C $(SRC_PATH)/$$dir || exit 1; \
		done
		
$(TARGET) : $(OBJS)	
	$(CC) -rdynamic -o $(TARGET) $(BINOBJS) $(LIB_PATH)
$(OBJS_PATH)/%.o:$(SRC_PATH)/%.cpp
	$(CC) $(CCOPT) $(INC_PATH) -c $< -o $@
	
.PHONY:clean

clean: 
	rm -rf $(TARGET)
	rm -rf $(OBJS)
	@for dir in $(SUBDIRS);\
		do \
			SRCPATH=$(SRC_PATH)/$$dir OBJPATH=$(OBJS_PATH) CXX=$(CC) DIR=$$dir $(MAKE) clean -C $(SRC_PATH)/$$dir; \
		done
