
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Experimental Automated CRAN Submission on GitHub Prerelease

<!-- badges: start -->

![Experimental](https://img.shields.io/badge/Status-Experimental-blue)
[![R-CMD-check](https://github.com/coatless-r-n-d/r-pkg-submit-on-tag/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/coatless-r-n-d/r-pkg-submit-on-tag/actions/workflows/R-CMD-check.yaml)
<!-- badges: end -->

The goal of this repository is to demonstrate how to automate R package
submissions to CRAN when a GitHub tag (pre-release) is pushed. You can
read more about the motivation and implementation in the [blog
post](https://blog.thecoatlessprofessor.com/programming/github-actions/first-steps-toward-automating-cran-r-package-submissions-with-github-actions/).

## Background

The Python ecosystem has long enabled developers to automate package
publishing to the [Python Package Index (PyPI) on a GitHub tag
push](https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/).
This is accomplished through GitHub Actions that listen for tag events
and execute the necessary steps to build and submit the package to PyPI,
creating a seamless integration between version control and package
distribution.

Until now, the R community has lacked a similar mechanism for automating
CRAN submissions. The closest has been using
[`devtools::submit_cran()`](https://devtools.r-lib.org/reference/submit_cran.html)
locally in an interactive context, which still requires human
confirmation and local building of the package. While the CRAN
submission process isn’t fully automatable (you still need to manually
confirm via email and handle reviewer feedback), this repository
demonstrates how to automate significant portions of the workflow:

- Building the package tarball (which can be time-consuming, especially
  for packages with compiled code)
- Submitting the package to CRAN’s web form
- Tracking the submission status through GitHub issues

This automation connects GitHub’s release workflow to CRAN submissions,
bringing the R package ecosystem closer to the level of automation
enjoyed by Python developers.

## How It Works

1.  Create a pre-release tag for your R package
2.  GitHub Actions automatically:
    - Runs R CMD check with `--as-cran` flag
    - Builds the package tarball
    - Submits the package to CRAN
    - Creates a GitHub issue with the submission status and next steps
3.  You still need to manually click the confirmation link in the CRAN
    email

## Implementation

To implement this workflow in your own R package:

### Using R

You can add the necessary files directly from R:

``` r
# Create directories if they don't exist
dir.create(path = ".github/workflows", recursive = TRUE, showWarnings = FALSE)
dir.create(path = ".github/scripts", recursive = TRUE, showWarnings = FALSE)

# Download the workflow yaml file
download.file(
  url = "https://raw.githubusercontent.com/coatless-r-n-d/r-pkg-submit-on-tag/main/.github/workflows/submit-cran.yaml",
  destfile = ".github/workflows/submit-cran.yaml"
)

# Download the submission script
download.file(
  url = "https://raw.githubusercontent.com/coatless-r-n-d/r-pkg-submit-on-tag/main/.github/scripts/submit-to-cran.R",
  destfile = ".github/scripts/submit-to-cran.R"
)
```

### Manual Steps

Alternatively, you can copy the files manually:

1.  Create `.github/workflows` and `.github/scripts` directories in your
    repository
2.  Copy the
    [submit-cran.yaml](https://github.com/coatless-r-n-d/r-pkg-submit-on-tag/blob/main/.github/workflows/submit-cran.yaml)
    file to `.github/workflows/`
3.  Copy the
    [submit-to-cran.R](https://github.com/coatless-r-n-d/r-pkg-submit-on-tag/blob/main/.github/scripts/submit-to-cran.R)
    script to `.github/scripts/` and run
    `chmod +x .github/scripts/submit-to-cran.R` to make it executable.

### After Implementation

Once implemented:

1.  Create a [GitHub
    pre-release](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository#creating-a-release)
    when you’re ready to submit to CRAN
2.  Monitor the GitHub Action logs and the created issue
3.  Check your email for the CRAN confirmation link (you still need to
    click this!)

## Limitations

- Cannot fully automate the process (manual email confirmation still
  required)
- Doesn’t handle communication with CRAN reviewers
- Still an experimental workflow, not yet a proper GitHub Action

## Related Resources

- [Blog post explaining the workflow in
  detail](https://blog.thecoatlessprofessor.com/programming/github-actions/first-steps-toward-automating-cran-r-package-submissions-with-github-actions/)
- [PyPI’s automated publishing
  workflow](https://packaging.python.org/en/latest/guides/publishing-package-distribution-releases-using-github-actions-ci-cd-workflows/)

## License

AGPL (\>= 3)
