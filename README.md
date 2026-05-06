# younghanstark.github.io

Personal website built with [Jekyll](https://github.com/jekyll/jekyll). The design is adapted from [Jon Barron's website](https://jonbarron.info/) via [Leonid Keselman's fork](https://leonidk.com/).

## Adding content

All content shown on the home page lives in YAML files under `_data/`. To add or edit anything, append/edit an entry in the relevant file — `index.html` does not need to be touched.

| Section on `index.html` | Data file |
| --- | --- |
| News                    | `_data/news.yml` |
| Education               | `_data/education.yml` |
| Honors and Awards       | `_data/honors.yml` |
| Publications (Preprints + Conferences) | `_data/publications.yml` |
| Patents                 | `_data/patents.yml` |
| Experience              | `_data/experience.yml` |
| Selected Projects       | `_data/projects.yml` |

The order of entries in each file is the order rendered on the page (newest first). Blog posts under `_posts/` are still authored as Markdown and listed automatically in the "Misc." section.

## Local development

```sh
bundle install
bundle exec jekyll serve --baseurl=""
bundle exec jekyll clean
```
