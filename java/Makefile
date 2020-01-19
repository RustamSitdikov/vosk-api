KALDI_ROOT ?= $(HOME)/kaldi
CFLAGS := -g -O2 -DPIC -fPIC -Wno-unused-function
CPPFLAGS := -I$(JAVA_HOME)/include -I$(JAVA_HOME)/include/linux -I$(KALDI_ROOT)/src -I$(KALDI_ROOT)/tools/openfst/include -I../src

KALDI_LIBS = \
             ${KALDI_ROOT}/src/online2/kaldi-online2.a \
             ${KALDI_ROOT}/src/decoder/kaldi-decoder.a \
             ${KALDI_ROOT}/src/ivector/kaldi-ivector.a \
             ${KALDI_ROOT}/src/gmm/kaldi-gmm.a \
             ${KALDI_ROOT}/src/nnet3/kaldi-nnet3.a \
             ${KALDI_ROOT}/src/tree/kaldi-tree.a \
             ${KALDI_ROOT}/src/feat/kaldi-feat.a \
             ${KALDI_ROOT}/src/lat/kaldi-lat.a \
             ${KALDI_ROOT}/src/hmm/kaldi-hmm.a \
             ${KALDI_ROOT}/src/transform/kaldi-transform.a \
             ${KALDI_ROOT}/src/cudamatrix/kaldi-cudamatrix.a \
             ${KALDI_ROOT}/src/matrix/kaldi-matrix.a \
             ${KALDI_ROOT}/src/fstext/kaldi-fstext.a \
             ${KALDI_ROOT}/src/util/kaldi-util.a \
             ${KALDI_ROOT}/src/base/kaldi-base.a \
             ${KALDI_ROOT}/tools/openfst/lib/libfst.a \
             ${KALDI_ROOT}/tools/openfst/lib/libfstngram.a \
             ${KALDI_ROOT}/tools/OpenBLAS/libopenblas.a \
             -lgfortran

all: libvosk_jni.so

VOSK_SOURCES = vosk_wrap.cc ../src/kaldi_recognizer.cc ../src/kaldi_recognizer.h ../src/model.cc ../src/model.h

libvosk_jni.so: $(VOSK_SOURCES)
	$(CXX) -shared -o $@ $(CPPFLAGS) $(CFLAGS) $(VOSK_SOURCES) $(KALDI_LIBS)

vosk_wrap.cc: ../src/vosk.i
	mkdir -p org/kaldi
	swig -I../src -c++ \
		-java -package org.kaldi \
		-outdir org/kaldi -o $@ $<

clean:
	$(RM) *.so *_wrap.cc *_wrap.o test/*.class
	$(RM) -r org

run:
	javac test/*.java org/kaldi/*.java
	java -Djava.library.path=. -cp . test.DecoderTest