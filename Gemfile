source "https://rubygems.org"

# Jekyll itself. GitHub Pages builds this site with the Actions workflow in
# .github/workflows/pages.yml, so we are free to use current Jekyll rather
# than being pinned to the github-pages gem's older release.
gem "jekyll", "~> 4.4"

group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.17"   # RSS/Atom feed at /feed.xml
  gem "jekyll-seo-tag", "~> 2.8" # <title>, OpenGraph and JSON-LD metadata
  gem "jekyll-sitemap", "~> 1.4" # /sitemap.xml for search engines
end

# Windows / JRuby helpers, harmless elsewhere
platforms :windows, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end
