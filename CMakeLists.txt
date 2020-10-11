cmake_minimum_required(VERSION 3.0)
project(bovidtrk)

find_library(HAVE_SDL SDL)

add_executable(dat2inc src/bme/dat2inc.c)
add_executable(datafile src/bme/datafile.c src/bme/bme_end.c)

set(GT2DAT_SOURCES
player.s altplayer.s chargen.bin palette.bin cursor.bin bcursor.bin goattrk2.bmp goattrk2.seq)

add_custom_command(OUTPUT ${CMAKE_BINARY_DIR}/goattrk2.dat
  COMMAND datafile ${CMAKE_BINARY_DIR}/goattrk2.dat goattrk2.seq
  WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}/src")

add_custom_command(OUTPUT ${CMAKE_BINARY_DIR}/goatdata.c
  COMMAND dat2inc ${CMAKE_BINARY_DIR}/goattrk2.dat ${CMAKE_BINARY_DIR}/goatdata.c
  DEPENDS ${CMAKE_BINARY_DIR}/goattrk2.dat
  WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}")

include_directories(src/bme src/asm)

set(GOATTRK2_SOURCES
${CMAKE_BINARY_DIR}/goatdata.c
src/goattrk2.c
src/gsong.c
src/gorder.c
src/gpattern.c
src/ginstr.c
src/gtable.c
src/gplay.c
src/gdisplay.c
src/gfile.c
src/greloc.c
src/ghelp.c
src/gsound.c
src/gconsole.c
src/gsid.cpp

src/resid/envelope.cpp
src/resid/extfilt.cpp
src/resid/filter.cpp
src/resid/pot.cpp
src/resid/sid.cpp
src/resid/wave.cpp
src/resid/wave6581__ST.cpp
src/resid/wave6581_P_T.cpp
src/resid/wave6581_PS_.cpp
src/resid/wave6581_PST.cpp
src/resid/wave8580__ST.cpp
src/resid/wave8580_P_T.cpp
src/resid/wave8580_PS_.cpp
src/resid/wave8580_PST.cpp
src/resid/voice.cpp

src/resid-fp/envelopefp.cpp
src/resid-fp/extfiltfp.cpp
src/resid-fp/filterfp.cpp
src/resid-fp/potfp.cpp
src/resid-fp/sidfp.cpp
src/resid-fp/wavefp.cpp
src/resid-fp/voicefp.cpp

src/asm/asmtab.c
src/asm/chnkpool.c
src/asm/expr.c
src/asm/lexyy.c
src/asm/log.c
src/asm/membuf.c
src/asm/membufio.c
src/asm/namedbuf.c
src/asm/parse.c
src/asm/pc.c
src/asm/vec.c

src/bme/bme_gfx.c
src/bme/bme_snd.c
src/bme/bme_win.c
src/bme/bme_mou.c
src/bme/bme_kbd.c
src/bme/bme_io.c
src/bme/bme_end.c
src/bme/bme.c)

add_executable(goattrk2 ${GOATTRK2_SOURCES})
#add_executable(gt2reloc ${GT2RELOC_SOURCES})
add_executable(ins2snd2 src/ins2snd2.c src/bme/bme_end.c)
add_executable(sngspli2 src/sngspli2.c src/bme/bme_end.c)
add_executable(mod2sng src/mod2sng.c src/bme/bme_end.c)

target_link_libraries(goattrk2 SDL)
