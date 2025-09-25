# Simple options
BINARY_NAME	=	my_program
SRC_FILES	=	src/main.c

OBJ_FILES	=	$(SRC_FILES:.c=.o)

LIBS	=	jpeg png
INCLUDES_FOLDERS	=	./src/includes/

# Advanced options
WARNING_FLAGS	=	-Wall -W -Werror

DEBUG_FLAGS	=	$(WARNING_FLAGS) $(addprefix -I,$(INCLUDES_FOLDERS)) $(addprefix -l,$(LIBS))
RELEASE_FLAGS	=	-O3 $(addprefix -I,$(INCLUDES_FOLDERS)) $(addprefix -l,$(LIBS))

OBJ_DESTINATION	=	./obj/

COMPILER	=	gcc
COMPILER_OBJECT_FLAG	=	-c
COMPILER_RES_FLAG	=	-o
LINKER	=	$(COMPILER)

OBJ_DEBUG	=	$(OBJ_FILES)


# Rules
all:
	make $(BINARY_NAME)
	$(LINKER) -o $(BINARY_NAME) $(OBJ_FILES)

dev:
	make $(OBJ_DEBUG)

$(BINARY_NAME):
	make $(OBJ)
	$(LINKER)

$(OBJ_FILES) : %.o : %.c
	$(COMPILER) -c $< -o $@ $(RELEASE_FLAGS)

$(OBJ_DEBUG) : %.o : %.c
	$(COMPILER) -c $< -o $@ $(DEBUG_FLAGS)

clean:
	rm -f $(OBJ_FILES)

fclean:
	make clean
	rm -f $(BINARY_NAME)

re:
	make fclean
	make
