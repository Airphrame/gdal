

include ../../../GDALmake.opt

OBJ	=	ogrodsdriver.o ogrodsdatasource.o ods_formula.o ods_formula_parser.o ods_formula_node.o

ifeq ($(HAVE_EXPAT),yes)
CPPFLAGS +=   -DHAVE_EXPAT
endif

CPPFLAGS	:=	-I.. -I../.. -I../mem  $(EXPAT_INCLUDE) $(CPPFLAGS)

default:	$(O_OBJ:.o=.$(OBJ_EXT))

clean:
	rm -f *.o $(O_OBJ) testparser$(EXE)

$(O_OBJ):	ogr_ods.h ods_formula.h ods_formula_parser.hpp ../mem/ogr_mem.h

# The sed substition below workarounds a bug with gcc 4.1 -O2 (checked on 64bit platforms)
# that produces buggy compiled code.
# Seen on gcc 4.1.2-27ubuntu1 (Ubuntu 10.04) (not the default compiler) and gcc-4.1.2-48.el5 (CentOS 5.5)
# (default compiler...)
# The memset isn't necessary at all with a non-buggy compiler, but I've found
# that it helps gcc 4.1 generating correct code here...
parser:
	bison -p ods_formula -d -oods_formula_parser.cpp ods_formula_parser.y
	sed "s/yytype_int16 yyssa\[YYINITDEPTH\];/yytype_int16 yyssa[YYINITDEPTH]; \/\* workaround bug with gcc 4.1 -O2 \*\/ memset(yyssa, 0, sizeof(yyssa));/" < ods_formula_parser.cpp > ods_formula_parser.cpp.tmp
	mv ods_formula_parser.cpp.tmp ods_formula_parser.cpp

testparser$(EXE):	testparser.$(OBJ_EXT)
	$(LD) $(LDFLAGS) testparser.$(OBJ_EXT) $(CONFIG_LIBS) -o testparser$(EXE)