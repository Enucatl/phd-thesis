require "rake/clean"

LATEX_TEXT = FileList["*.tex, FrontBackmatter/*.tex, Chapters/*.tex"]
PICTURES = FileList["gfx/*"]
CLEAN.include(FileList["*.aux", "*.bbl", "*.blg", "*.brf", "*.idx", "*.ilg", "*.ind", "*.log"])
CLOBBER.include(FileList["*.pdf"])

namespace :main do

  desc "main pdf"
  file "ClassicThesis.pdf" => ["ClassicThesis.tex", "Bibliography.bib", "gfx:all"] + PICTURES + LATEX_TEXT do |f|
    sh "pdflatex ClassicThesis"
    sh "bibtex ClassicThesis"
    sh "pdflatex ClassicThesis"
    sh "pdflatex ClassicThesis"
  end

end

namespace :gfx do

  EEPIC = FileList["gfx/*.xp"]
  for picture_filename in EEPIC do
    file picture_filename.ext(".eepic") => picture_filename do |t|
      Dir.chdir("gfx") do
        sh "epix --tikz -o #{File.basename(t.name)} #{File.basename(t.source)}"
      end
    end
  end

  file "gfx/visibility_visibility_100kev.pgf" => ["gfx/visibility_100kev.hdf5", "gfx/plot_visibility_pgf.py"] do |f|
    sh "python #{f.prerequisites[1]} --steps 25 --pixel 510 #{f.source}"
  end

  file "gfx/visibility_S00618.pgf" => ["gfx/S00618.hdf5", "gfx/plot_visibility_pgf.py"] do |f|
    sh "python #{f.prerequisites[1]} --steps 25 --pixel 510 #{f.source}"
  end

  file "gfx/images_S00052.pgf" => ["gfx/S00052.hdf5", "gfx/plot_images.py"] do |f|
    sh "python #{f.prerequisites[1]} #{f.source} #{f.name} 6"
  end

  file "gfx/images_S00075_S00071.pgf" => ["gfx/S00075_S00071.hdf5", "gfx/plot_images.py"] do |f|
    sh "python #{f.prerequisites[1]} #{f.source} #{f.name} 7"
  end

  file "gfx/images_S00613.pgf" => ["gfx/S00613.hdf5", "gfx/plot_images.py"] do |f|
    sh "python #{f.prerequisites[1]} #{f.source} #{f.name} 2.5"
  end

  desc "all images"
  task :all => EEPIC.ext(".eepic") + ["gfx/visibility_visibility_100kev.pgf", "gfx/visibility_S00618.pgf", "gfx/images_S00052.pgf", "gfx/images_S00075_S00071.pgf", "gfx/images_S00613.pgf"]

end