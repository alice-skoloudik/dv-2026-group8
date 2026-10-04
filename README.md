# Same test results, different advice?

**Group members:**

- Alice Školoudíková
- Robin Lux

**Research question:** Do schools with similar test results give similar secondary-school advice, regardless of how disadvantaged their student population is?

**Level:** Inference

## About this project

This project uses the DUO doorstroomtoets (transfer test) data for 2024-2025 to compare primary schools' secondary-school advice with their pupils' test results. We relate each school's advice (e.g. the share of pupils advised HAVO or higher) to the share of its pupils reaching the target reference levels in maths (1S) and reading (2F), and test whether this relationship depends on the school weighting (*schoolweging*), a measure of how disadvantaged the pupil population is. Our final visualization is aimed at secondary schools and should show whether two schools with the same test results give the same advice, regardless of who their pupils are.

## Cloning this project

To get a copy of this project on your own computer, as an RStudio project:

1. On this repository's GitHub page, click the green `<> Code` button, choose **HTTPS**, and copy the URL.
2. In RStudio, make sure no project is open (top right: `Project: (None)`).
3. Go to `File` > `New Project` > `Version Control` > `Git`, paste the URL into `Repository URL`, choose where the project should live on your computer, and click `Create Project`.

RStudio opens the project, with a `Git` tab next to your `Environment` pane. Full instructions (including how to set up Git and GitHub on your computer first) are in the course's [Working with Git](https://ann1ejohansson.github.io/data-visualization-2026/documents/git-workflow.html) tutorial.

## Reproducing this project

1. Open the project in RStudio (double-click its `.Rproj` file, or clone it as described above).
2. Run `scripts/00-packages.R` to install and load the packages this project uses.
3. Run `scripts/01-get-data.R` once to download the data into `data/raw/`.
4. Knit `report/DV-Assignment2-Part2-Group8.Rmd` (the final report). Knitting runs both scripts above for you.
