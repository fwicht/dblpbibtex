# DBLP BibTeX

BibTeX wrapper for automatic DBLP &amp; IACR ePrint downloads

## What is DBLP BibTeX?

DBLP BibTeX is a BibTeX aid program that can automatically download citations and cross references from the [DBLP Computer Science Bibliography](http://www.dblp.org) and the [Cryptology ePrint Archive](http://eprint.iacr.org) and add them to your BIB file. Furthermore, when explicitely enabled it also allows you to search these archives and automatically insert up to 5 matched citations into your .tex file. DBLP BibTeX acts as a BibTeX replacement that needs to be called from your TeX environment instead of the original BibTeX, after it has finished its work it will call the original BibTeX to proceed as usual.

## Building DBLP BibTeX

DBLP BibTex depends on autotools, a C++17 compatible compiler and libcurl.

*Only* if you don't have a C++17 compatible compiler with the filesystem module then
instead you can also use a C++11 compatible compiler together with the Boost C++ library.
For instance, on many linux machines you can install the boost with `sudo apt-get install libboost-dev`, and on Mac OS X you can install boost with `brew install boost`.

In either case (C++17 filesystem or Boost filesystem module), you can then compile dblpbibtex by simply running:
```
   autoreconf --install
   ./configure
   make
```


## Installing DBLP BibTeX

1.  Set your TeX environment to call the DBLP BibTeX executable instead of the original BibTeX executable. Usually this can be achieved by setting the environment variable `BIBTEX` with the full path to your `dblpbibtex` executable. Otherwise, search inside your TeX editor. As a last resort, you can try to rename `dblpbibtex` to `bibtex` and setting the DBLP BibTeX directory as the first entry in your `PATH` environment variable.
2.  DBLP BibTeX needs to find your original BibTeX executable. If `bibtex` is found directly by the command line shell then you're all set. Otherwise set the environment variable `BIBTEXORG` with the full path to your original `bibtex` executable.

## Using DBLP BibTeX

Example excerpt from a TeX file:

<pre>\cite{DBLP:conf/crypto/StevensSALMOW09,cryptoeprint:2009:111}
\cite{search:chosen+prefix+collisions}
\cite{search-dblp:chosen+prefix+collisions}
\cite{search-cryptoeprint:chosen+prefix+collisions}
\cite{search-bib:chosen+prefix+collisions}

\nocite{dblpbibtex:mainbibfile:dblpbibtex}
\nocite{dblpbibtex:enablesearch}
\nocite{dblpbibtex:dblpformat:compact}
\nocite{dblpbibtex:addbibtexoption:--min-crossrefs=20}
\bibliography{thesis,dblpbibtex}
</pre>

The above nocites can also be achieved using a dblpbibtex.cfg file with the following contents:

<pre>mainbibfile=dblpbibtex.bib
enablesearch=1
dblpformat=compact
addbibtexoption=--min-crossrefs=20
</pre>

## Usage

### Main bib file

The main bib file is the bib file DBLP BibTeX will write its downloaded citations to. It defaults to the first mentioned bib file in your TeX file (in the example this would be `thesis.bib`). This can be overriden using a `\nocite{dblpbibtex:mainbibfile:bibfilename}` command in your TeX file where bibfilename is replaced with the desired filename (in the example this is `dblpbibtex.bib`). It is advised to use a seperate bib file for DBLP BibTeX as shown in the example.

### Citations

DBLP BibTeX will automatically read all citations from your AUX file (generated by (La)TeX) and all entries in your BIB files. It will try to download all missing `DBLP:conf/crypto/StevensSALMOW09` style and `cryptoeprint:2009:111` style bibtex entries from DBLP and Crypto ePrint Archive, respectively. These citations will be prepended to the main bib file. Any missing cross reference citations will also be downloaded and appended to the main bib file.

### Searching for citations

DBLP BibTeX is capable of searching the DBLP and Crypto ePrint archives and your included BIB files through `search:keyword1+keyword2+keyword3+etc` style citation keys. It will replace this citation key in your TeX file with up to 5 found results. Since it edits your TeX file, searches must be explicitly enabled in your TeX file through a `\nocite{dblpbibtex:enablesearch}` command. Using `search-dblp:`, `search-cryptoeprint:` or `search-bib:` instead of `search:` will search only DBLP, Crypto ePrint Archive or processed BIB files, respectively. The general `search:` command will first perform a `search-dblp` search and when unsuccesful will do a `search-cryptoeprint` search. If both are unsuccesful it will search your BIB files.

### DBLP format

You can choose between DBLP bib formats: `compact`, `standard`, and `crossref` using the `dblpformat` option.

### Note on multiple bib files

It is recommended to use one bib file solely for DBLP BibTeX, and another for manual additions. Avoid placing `DBLP:*` and `cryptoeprint:YYYY:NNN` style entries in bib files other than the main dblpbibtex bib file.  This allows to easily start from scratch and switch between DBLP formats. Furthermore, this avoids issues with the ordering of crossref entries: i.e., BibTeX requires crossref entries to be placed later than referring entries.

### Adding BibTeX options

Passing extra command parameters to BibTeX can be difficult depending on your TeX environment. You can let DBLP BibTeX pass extra parameters to BibTeX specified in your TeX file through a `\nocite{dblpbibtex:addbibtexoption:--min-crossrefs=20}` command. In this example, it will pass the `--min-crossrefs=20` parameter that tells BibTeX to seperately citate crossrefs in your bibliography when it has been crossref'd at least 20 times (instead of the default 2 times).

### Cleaning up the main bib file

As an extra feature, DBLP BibTeX can clean up the main bib file from all entries that are not citated or crossreferenced using the `\nocite{dblpbibtex:cleanupmainbibfile}` command. Make sure you do this only when the main bib file is only used for DBLP BibTeX, so it doesn't delete any manually added bibtex entries.

## Copyright and Licence

Copyright Marc Stevens 2010 - 2019. Distributed under the Boost Software License, Version 1.0, copy available [here](LICENSE.txt).

## Feedback

Any comments, requests, improvements, etc. can be send to: marc(replacebyAT)marc(replacebyDASH)stevens(replacebyDOT)nl.

## Thanks

Thanks go out to Peter van Liesdonk for providing patches and feedback.
Christian Cachin and Bart Mennink for telling me how useful this program can be.
