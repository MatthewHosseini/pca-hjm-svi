An end-to-end workflow used in the accompanying code and figures:
  (1) convert daily U.S.\ Treasury \emph{par yields} from Excel into an \emph{instantaneous forward} surface,
  (2) run PCA on \emph{changes} in forwards to obtain data-driven ``level / slope / curvature'' factors,
  (3) map those PCA objects into a simple Heath--Jarrow--Morton (HJM) model to simulate curve dynamics
