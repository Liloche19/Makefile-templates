################################################################################
#                            SIMPLE/EASY OPTIONS                               #
################################################################################

# Name of the final executable binary
BINARY_NAME	=	my_program

# List of source files to compile (space-separated)
SRC_FILES	=	src/main.c

# Libraries to link against (without -l prefix, e.g., "pthread m")
LIBS	=

# Additional include directories (without -I prefix, e.g., "include/ lib/headers/")
INCLUDES_FOLDERS	=

# Other project directories to build before this one (space-separated paths)
OTHER_PROJECTS_TO_BUILD	=

################################################################################
#                           ADVANCED OPTIONS                                   #
################################################################################

# Destination directory for release build object files
OBJ_RELEASE_DESTINATION	=	./obj/release/

# Destination directory for debug build object files
OBJ_DEBUG_DESTINATION	=	./obj/dev/

# Automatically generated list of object files for release build
OBJ_RELEASE	=	$(SRC_FILES:%.c=$(OBJ_RELEASE_DESTINATION)%.o)

# Automatically generated list of object files for debug build
OBJ_DEBUG	=	$(SRC_FILES:%.c=$(OBJ_DEBUG_DESTINATION)%.o)

# Compiler warning flags
WARNING_FLAGS	=	-Wall -Wextra

# Optimization level for release builds
OPTIMISATION_FLAG	=	-O3

# Memory debugging flags (address and undefined behavior sanitizers)
MEMORY_SANITIZER_FLAGS	=	-fsanitize=address -fsanitize=undefined

# Compilation flags for release builds
COMPILATION_RELEASE_FLAGS	=	$(addprefix -I,$(INCLUDES_FOLDERS)) $(addprefix -l,$(LIBS)) $(OPTIMISATION_FLAG) $(WARNING_FLAGS) -DNDEBUG -march=native -flto

# Compilation flags for debug builds
COMPILATION_DEBUG_FLAGS	=	$(addprefix -I,$(INCLUDES_FOLDERS)) $(addprefix -l,$(LIBS)) -g $(WARNING_FLAGS) $(MEMORY_SANITIZER_FLAGS) -fno-omit-frame-pointer

# Linking flags for release builds
LINKING_RELEASE_FLAGS	=	$(addprefix -I,$(INCLUDES_FOLDERS)) $(addprefix -l,$(LIBS)) $(OPTIMISATION_FLAG) -flto -s

# Linking flags for debug builds
LINKING_DEBUG_FLAGS	=	$(addprefix -I,$(INCLUDES_FOLDERS)) $(addprefix -l,$(LIBS)) -g $(MEMORY_SANITIZER_FLAGS)

# Directory where the final binary will be placed
FINAL_BINARY_DESTINATION	=	./

# C compiler to use
COMPILER	=	gcc

# Linker to use (typically same as compiler)
LINKER	=	$(COMPILER)

# Full path to the final binary (destination + name)
FINAL_BINARY_PATH	=	$(addprefix $(FINAL_BINARY_DESTINATION),$(BINARY_NAME))

################################################################################
#                               COMMANDS                                       #
################################################################################

# Command to compile a source file in release mode
RELEASE_COMPILATION	=	$(COMPILER) -c $< -o $@ $(COMPILATION_RELEASE_FLAGS)

# Command to compile a source file in debug mode
DEBUG_COMPILATION	=	$(COMPILER) -c $< -o $@ $(COMPILATION_DEBUG_FLAGS)

# Command to link object files in release mode
RELEASE_LINKING	=	$(LINKER) -o $(FINAL_BINARY_PATH) $(OBJ_RELEASE) $(LINKING_RELEASE_FLAGS)

# Command to link object files in debug mode
DEBUG_LINKING	=	$(LINKER) -o $(FINAL_BINARY_PATH) $(OBJ_DEBUG) $(LINKING_DEBUG_FLAGS)

################################################################################
#                                 RULES                                        #
################################################################################

# Default target: builds the project in release mode
all:
	make $(FINAL_BINARY_PATH)

# Builds the final binary in release mode (optimized)
$(FINAL_BINARY_PATH):
	$(foreach PROJECT,$(OTHER_PROJECTS_TO_BUILD),make -C $(PROJECT))
	make $(OBJ_RELEASE)
	$(RELEASE_LINKING)

# Builds the project in debug mode (with debug symbols and sanitizers)
dev:
	$(foreach PROJECT,$(OTHER_PROJECTS_TO_BUILD),make -C $(PROJECT) dev)
	make $(OBJ_DEBUG)
	$(DEBUG_LINKING)

# Pattern rule to compile source files into release object files
$(OBJ_RELEASE) : $(OBJ_RELEASE_DESTINATION)%.o : %.c
	@mkdir -p $(dir $@)
	$(RELEASE_COMPILATION)

# Pattern rule to compile source files into debug object files
$(OBJ_DEBUG) : $(OBJ_DEBUG_DESTINATION)%.o : %.c
	@mkdir -p $(dir $@)
	$(DEBUG_COMPILATION)

# Removes all object files (both release and debug)
clean:
	rm -f $(OBJ_RELEASE)
	rm -f $(OBJ_DEBUG)

# Removes all object files and the final binary
fclean:
	make clean
	rm -f $(FINAL_BINARY_PATH)

# Performs a full rebuild: clean everything and rebuild in debug mode
re:
	make fclean
	make dev
