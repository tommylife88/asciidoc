# coding: utf-8

require "asciidoctor"
require "asciidoctor-diagram"
require "asciidoctor-pdf"
require "asciidoctor-pdf-cjk"
require "coderay"
require "launchy"
require "date"

OUTDIR_BASE = "output"

CHEAT_SOURCE = "asciidoc-cheatsheet.adoc"
OUTDIR_CHEAT = "#{OUTDIR_BASE}/cheatsheet"
OUTDIR_CHEAT_HTML = "#{OUTDIR_CHEAT}/html"
CHEAT_OUTPUT_HTML = "#{OUTDIR_CHEAT_HTML}/index.html"

OUTDIR_CHEAT_PDF = "#{OUTDIR_CHEAT}/pdf"
CHEAT_OUTPUT_PDF = "#{OUTDIR_CHEAT_PDF}/asciidoc-cheatsheet.pdf"

date_str = Date.today.strftime("%Y/%m/%d")

namespace :cheatsheet do

    # Build cheatsheet to Html
    namespace :html do
        desc "Convret asciidoc-cheatsheet.adoc to HTML file"
        task :build do
            mkdir_p(OUTDIR_CHEAT_HTML)
            cp_r("images", "#{OUTDIR_CHEAT_HTML}/")
            `bundle exec asciidoctor \
            -r asciidoctor-diagram \
            -a revdate=#{date_str} \
            #{CHEAT_SOURCE} \
            -o #{CHEAT_OUTPUT_HTML}`
            rm_r("#{OUTDIR_CHEAT_HTML}/.asciidoctor")
        end

        desc "Start index.html"
        task :launch => ["cheatsheet:html:build"] do
            if File.exist?(CHEAT_OUTPUT_HTML)
                Launchy.open CHEAT_OUTPUT_HTML
            else
                puts "#{CHEAT_OUTPUT_HTML} does not exist."
                puts "Please execute : bundle exec rake cheatsheet:html:build"
            end
        end
    end
    
    # Build cheatsheet to Pdf
    namespace :pdf do
        desc "Convret asciidoc-cheatsheet.adoc to PDF file"
        task :build do
            mkdir_p(OUTDIR_CHEAT_PDF)
            `bundle exec asciidoctor-pdf \
            -r asciidoctor-diagram \
            -r asciidoctor-pdf-cjk \
            -a skip-front-matter \
            -a allow-uri-read \
            -a source-highlighter=coderay \
            -a pdf-stylesdir=styles \
            -a pdf-style=default-with-fallback-font-theme.yml \
            -a revdate=#{date_str} \
            #{CHEAT_SOURCE} \
            -o #{CHEAT_OUTPUT_PDF}`
            rm_r("#{OUTDIR_CHEAT_PDF}/.asciidoctor")
            rm_r("#{OUTDIR_CHEAT_PDF}/images")
        end
    end

end

# Clean, Clobber
require "rake/clean"
CLEANDIR = "#{OUTDIR_BASE}/*"
CLEAN.include(CLEANDIR)
CLOBBER.include(CLEANDIR)

# Default
task :default => ["cheatsheet:html:build", "cheatsheet:pdf:build"]