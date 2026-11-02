# Ancestor Fan

An interactive page that shows how many ancestors you have as you go back in time, and why the real number is smaller than the textbook one.

Everyone has 2 parents, 4 grandparents, 8 great-grandparents, so the number of places in your family tree doubles every generation (2<sup>n</sup>). Go back far enough and that number is larger than the number of people who were alive. The same people must fill many places. This overlap is called **pedigree collapse**. Ancestor Fan lets you pick a year and see where your tree runs out of people.

No build step, no dependencies. It is a single `index.html` file.

## Features

- **Radial fan chart** with one ring per generation. Father's side and mother's side are colored differently. Hover a segment to see who it is, for example "Your father's mother's father".
- **Collapse chart** (log scale) comparing places in your tree, people alive in your pool, and the estimated number of different ancestors. Hover or drag to move through the years.
- **Regional population data** for six regions (East Asia, South & Southeast Asia, Europe & Russia, Middle East & N. Africa, Sub-Saharan Africa, Americas & Oceania). Choose where your ancestors lived and how widely people married within those regions.
- **Travel back** button that animates the fan and counters as the year moves backward.
- **Family tree import** for GEDCOM (`.ged`) files. Known ancestors show in full color, gaps show as faint ghosts, and people who fill more than one place get a gold outline.
- **Generation table** with the numbers behind every ring. Select a row to jump to that generation.
- Light and dark themes that follow your system setting.

## Run it

Open `index.html` in a browser. That is all.

If you prefer a local server:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Use your own family tree

1. Export a GEDCOM file from Ancestry, FamilySearch, MyHeritage or any genealogy program that supports it.
2. Click **Choose GEDCOM file** and select the `.ged` file.
3. Pick a starting person if the default is not you.

The file is read inside your browser with the `FileReader` API. Nothing is uploaded.

To try it without your own data, click **Try a sample** or load [`sample/sample-tree.ged`](sample/sample-tree.ged), a made-up family with deliberate gaps and shared ancestors.

Notes on GEDCOM support:

- Reads people (`INDI`), families (`FAM`), names, birth years and parent links.
- If a person has more than one parent link, the first one is used.
- Files are read as UTF-8.

## How the estimate works

For a given generation `k`, the page calculates:

- `T = 2^k`, the number of places in your tree
- `N`, the pool of people who could fill them: the population of your chosen regions in that year, times the marriage reach (2%, 15% or 50% of the region)
- Estimated different ancestors `= N × (1 − e^(−T/N))`

This is the expected number of distinct people when `T` places are filled by random draws from `N` people. Real families are less random than that, so treat the output as a sense of scale.

Regional populations are rounded from broad published estimates (the kind compiled by McEvedy and Jones, the HYDE database and the UN) and are uncertain, often by a quarter or more before 1800. They live in the `REG` array near the top of the script, so you can replace them with a dataset you trust.

## Deploy with GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, then save.
4. Your site appears at `https://<your-username>.github.io/<repo-name>/` after a minute.

## Project layout

```
index.html              the whole app (HTML, CSS and JavaScript)
sample/sample-tree.ged  made-up GEDCOM file for testing the import
README.md
LICENSE
.gitignore
```

## Ideas for next steps

- Better population data by country, not just six regions
- Show the real birth years from a GEDCOM file on the fan instead of modeled years
- Support GEDCOM 7 and other encodings
- A Python version of the importer for batch analysis of tree files

## License

MIT. See [LICENSE](LICENSE).
