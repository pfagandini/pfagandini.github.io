# ---------- CONFIG ----------
# List your top-level .tex mains here (no _handout files; just the real sources).
MAINS := clt.tex concavity.tex correspondences.tex diff_equations.tex differentiability.tex dynamic_optimization_discrete.tex functions.tex intro_prob_theory.tex markov.tex matrices.tex measure.tex optimal_control.tex rck.tex sets.tex topology.tex vectors.tex 

# clt.tex concavity.tex correspondences.tex diff_equations.tex differentiability.tex dynamic_optimization_discrete.tex functions.tex intro_prob_theory.tex markov.tex matrices.tex measure.tex optimal_control.tex rck.tex sets.tex topology.tex vectors.tex 
# This does not work static_optimization.tex

# Choose engine: pdflatex | xelatex | lualatex
ENGINE ?= lualatex

# Set to 1 if you need -shell-escape (e.g., minted, pygments)
SHELL_ESCAPE ?= 1

# Directory for PDFs (default: current directory)
OUT_DIR ?= latex_output

# Directory for auxiliary files (default: latex_output)
AUX_DIR ?= latex_aux

# Extra flags passed to the engine (e.g., -synctex=1)
EXTRA_TEX_FLAGS ?=

# ---------- DO NOT EDIT BELOW UNLESS YOU WANT TO ----------
# Create output and auxiliary directories if they don't exist
$(shell mkdir -p $(OUT_DIR) $(AUX_DIR))

# Pick the latexmk switch corresponding to the engine
ifeq ($(ENGINE),pdflatex)
  LMK_SWITCH := -pdf
else ifeq ($(ENGINE),xelatex)
  LMK_SWITCH := -xelatex
else ifeq ($(ENGINE),lualatex)
  LMK_SWITCH := -lualatex
else
  $(error ENGINE must be pdflatex, xelatex, or lualatex)
endif

# Build flags
ENGINE_FLAGS := -interaction=nonstopmode -halt-on-error -file-line-error $(EXTRA_TEX_FLAGS)
ifeq ($(SHELL_ESCAPE),1)
  ENGINE_FLAGS += -shell-escape
endif

# Derive basenames, normal PDFs, and handout PDFs
BASENAMES := $(MAINS:.tex=)
NORMAL_PDFS := $(addprefix $(OUT_DIR)/,$(BASENAMES:=.pdf))
HANDOUT_PDFS := $(addprefix $(OUT_DIR)/,$(BASENAMES:%=%_handout.pdf))

# Common auxiliary file extensions
AUX_EXTS := aux log out nav snm toc vrb fls fdb_latexmk

.PHONY: all normals handouts clean veryclean

# Build everything (normal + handout for each main)
all: normals handouts

# Only normal slide decks
normals: $(NORMAL_PDFS)

# Only handout slide decks
handouts: $(HANDOUT_PDFS)

# Pattern: normal PDF from .tex using latexmk multi-pass
$(OUT_DIR)/%.pdf: %.tex
	latexmk $(LMK_SWITCH) -outdir=$(OUT_DIR) -$(ENGINE)="$(ENGINE) $(ENGINE_FLAGS) %O %S" "$<"
	@for ext in $(AUX_EXTS); do mv -f $(OUT_DIR)/*.$$ext $(AUX_DIR)/ 2>/dev/null || true; done

# Pattern: handout PDF from .tex
$(OUT_DIR)/%_handout.pdf: %.tex
	latexmk $(LMK_SWITCH) -outdir=$(OUT_DIR) \
	  -jobname="$*_handout" \
	  -$(ENGINE)='$(ENGINE) $(ENGINE_FLAGS) %O "\PassOptionsToClass{handout}{beamer}\input{%S}"' \
	  "$<"
	@for ext in $(AUX_EXTS); do mv -f $(OUT_DIR)/$*_handout.$$ext $(AUX_DIR)/ 2>/dev/null || true; done

# Clean aux files (keeps PDFs)
clean:
	latexmk -c -outdir=$(OUT_DIR)
	rm -f $(AUX_DIR)/*.aux $(AUX_DIR)/*.log $(AUX_DIR)/*.out $(AUX_DIR)/*.nav $(AUX_DIR)/*.snm $(AUX_DIR)/*.toc $(AUX_DIR)/*.vrb $(AUX_DIR)/*.fls $(AUX_DIR)/*.fdb_latexmk $(AUX_DIR)/*.dat $(AUX_DIR)/*.script $(AUX_DIR)/*.table

# Remove EVERYTHING latexmk created, including PDFs
veryclean:
	latexmk -C -outdir=$(OUT_DIR)
	rm -f $(NORMAL_PDFS) $(HANDOUT_PDFS)
	rm -rf $(AUX_DIR)