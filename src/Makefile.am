## Process this file with automake to produce Makefile.in

AUTOMAKE_OPTIONS = subdir-objects

AM_CPPFLAGS = $(EXTERNAL_XIPH_CFLAGS)

lib_LTLIBRARIES = libsndfile.la
include_HEADERS = sndfile.hh
nodist_include_HEADERS = sndfile.h

noinst_LTLIBRARIES =  GSM610/libgsm.la G72x/libg72x.la ALAC/libalac.la libcommon.la

SYMBOL_FILES = Symbols.gnu-binutils Symbols.darwin libsndfile-1.def Symbols.os2 Symbols.static

EXTRA_DIST = sndfile.h.in config.h.in test_endswap.c test_endswap.tpl test_endswap.def \
			$(SYMBOL_FILES) create_symbols_file binheader_writef_check.py \
			GSM610/README GSM610/COPYRIGHT GSM610/ChangeLog \
			G72x/README G72x/README.original G72x/ChangeLog \
			make-static-lib-hidden-privates.sh

noinst_HEADERS = common.h sfconfig.h sfendian.h wavlike.h sf_unistd.h ogg.h chanmap.h

noinst_PROGRAMS = create_symbols_file

check_PROGRAMS = test_main G72x/g72x_test

FILESPECIFIC = sndfile.c aiff.c au.c avr.c caf.c dwd.c flac.c g72x.c htk.c ircam.c \
		macos.c mat4.c mat5.c nist.c paf.c pvf.c raw.c rx2.c sd2.c \
		sds.c svx.c txw.c voc.c wve.c w64.c wavlike.c wav.c xi.c mpc2k.c rf64.c \
		ogg_vorbis.c ogg_speex.c ogg_pcm.c ogg_opus.c

CLEANFILES = *~ *.exe G72x/*.exe error.dat $(SYMBOL_FILES)

if USE_WIN_VERSION_FILE
WIN_VERSION_FILE = version-metadata.rc
else
WIN_VERSION_FILE =
endif

#===============================================================================
# MinGW requires -no-undefined if a DLL is to be built.
libsndfile_la_LDFLAGS = -no-undefined -version-info  $(SHARED_VERSION_INFO) $(SHLIB_VERSION_ARG)
libsndfile_la_SOURCES = $(FILESPECIFIC) $(noinst_HEADERS)
nodist_libsndfile_la_SOURCES = $(nodist_include_HEADERS)
libsndfile_la_LIBADD = GSM610/libgsm.la G72x/libg72x.la ALAC/libalac.la \
		libcommon.la $(EXTERNAL_XIPH_LIBS) -lm

EXTRA_libsndfile_la_DEPENDENCIES = $(SYMBOL_FILES)

libcommon_la_SOURCES = common.c file_io.c command.c pcm.c ulaw.c alaw.c \
		float32.c double64.c ima_adpcm.c ms_adpcm.c gsm610.c dwvw.c vox_adpcm.c \
		interleave.c strings.c dither.c cart.c broadcast.c audio_detect.c \
 		ima_oki_adpcm.c ima_oki_adpcm.h alac.c chunk.c ogg.c chanmap.c \
		windows.c id3.c $(WIN_VERSION_FILE)


#======================================================================
# Subdir libraries.

GSM610_libgsm_la_SOURCES = GSM610/config.h GSM610/gsm.h GSM610/gsm610_priv.h \
		GSM610/add.c GSM610/code.c GSM610/decode.c GSM610/gsm_create.c \
		GSM610/gsm_decode.c GSM610/gsm_destroy.c GSM610/gsm_encode.c \
		GSM610/gsm_option.c GSM610/long_term.c GSM610/lpc.c GSM610/preprocess.c \
		GSM610/rpe.c GSM610/short_term.c GSM610/table.c

G72x_libg72x_la_SOURCES = G72x/g72x.h G72x/g72x_priv.h \
		G72x/g721.c G72x/g723_16.c G72x/g723_24.c G72x/g723_40.c G72x/g72x.c

ALAC_libalac_la_SOURCES = ALAC/ALACAudioTypes.h ALAC/ALACBitUtilities.h \
		ALAC/EndianPortable.h ALAC/aglib.h ALAC/dplib.h ALAC/matrixlib.h \
		ALAC/alac_codec.h ALAC/shift.h \
		ALAC/ALACBitUtilities.c ALAC/ag_dec.c \
		ALAC/ag_enc.c ALAC/dp_dec.c ALAC/dp_enc.c ALAC/matrix_dec.c \
		ALAC/matrix_enc.c ALAC/alac_decoder.c ALAC/alac_encoder.c

#===============================================================================
# Test programs.

test_main_SOURCES = test_main.c test_main.h test_conversions.c test_float.c test_endswap.c \
					test_audio_detect.c test_log_printf.c test_file_io.c test_ima_oki_adpcm.c \
					test_strncpy_crlf.c test_broadcast_var.c test_cart_var.c \
					test_binheader_writef.c
test_main_LDADD = libcommon.la

G72x_g72x_test_SOURCES = G72x/g72x_test.c
G72x_g72x_test_LDADD = G72x/libg72x.la

SUFFIXES = .def .tpl

.def.c:
	autogen --writable $<

check :
	@if [ -x /usr/bin/python ]; then $(srcdir)/binheader_writef_check.py $(srcdir)/*.c ; fi
	G72x/g72x_test$(EXEEXT) all
	./test_main$(EXEEXT)

# Need this target to force building of test programs.
checkprograms : $(check_PROGRAMS)

#======================================================================
# Generate an OS specific Symbols files. This is done when the author
# builds the distribution tarball. There should be not need for the
# end user to create these files.

Symbols.gnu-binutils: create_symbols_file
	$(AM_V_GEN) ./create_symbols_file linux $(VERSION) $@

Symbols.darwin: create_symbols_file
	$(AM_V_GEN) ./create_symbols_file darwin $(VERSION) $@

libsndfile-1.def: create_symbols_file
	$(AM_V_GEN) ./create_symbols_file win32 $(VERSION) $@

Symbols.os2: create_symbols_file
	$(AM_V_GEN) ./create_symbols_file os2 $(VERSION) $@

Symbols.static: create_symbols_file
	$(AM_V_GEN) ./create_symbols_file static $(VERSION) $@

create_symbols_file_SOURCES = create_symbols_file.c

#======================================================================
# Building windows resource files (if needed).

.rc.lo:
	$(LIBTOOL) $(AM_LIBTOOLFLAGS) $(LIBTOOLFLAGS) --tag=RC --mode=compile $(RC) $(RCFLAGS) $< -o $@

#======================================================================
# Disable autoheader.
AUTOHEADER=echo



