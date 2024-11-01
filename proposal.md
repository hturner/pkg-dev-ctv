# Package Development and Maintenance

Contributors: Roger Bivand, Heather Turner, Lluís Revilla 

## Introduction

Packages extend R by providing additional code and/or data. Package
development is typically an iterative process and maintenance involves
responding to changes in R and package dependencies.

The essential tools for building and installing packages are provided as
`R CMD` command line tools. Base R provides many relevant functions in 
`r pkg("utils")` alongside the `r pkg("tools")` package, which is dedicated to 
package development and maintenance. Contributed packages provide alternative 
or supplementary helper functions.

The definitive reference for R package development is the [Writing R
Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html)
(WRE) manual. The [CRAN Repository Policy](https://cran.r-project.org/web/packages/policies.html) sets standards on top of
this - see [Links](#links) for policies of other repositories.

Contributed packages designed to facilitate package development are not 
guaranteed to be consistent with WRE or repository policies. Particular caution 
should be taken when a helper package must be added as a forward dependency: 
convenience during development can make maintenance harder, both for package 
authors and for authors of any reverse dependencies.

To assist package developers, this task view collates contributed packages with
reference to relevant sections of WRE, highlighting relevant functionality from 
base/recommended packages before alternative/supplementary tools.

If you think that some package is missing from the Task View, please file an issue in the GitHub repository or contact the maintainer(s).

## First steps

### Checking the package landscape

Before starting a new package it's worth searching for already available
packages, both from a developer's standpoint ("do not reinvent the
wheel") and from a user's one (many packages implementing same/similar
procedures can be confusing). If a package addressing the same
functionality already exists, you may consider contributing to it
instead of starting a new one.

If you are looking for opportunities to contribute, you may find that an 
existing package needs a new maintainer or there is an open wish for a 
package that does not yet exist.

The CRAN Team occasionally use the [R-package-devel mailing list](https://stat.ethz.ch/mailman/listinfo/r-package-devel) to ask for maintainers to take on orphaned packages.

The [CRAN Search](https://cran.r-project.org/search.html) page links to R-focused search tools maintained by the community.

`utils::RSiteSearch()` facilitates search for keywords/phrases in help
pages (all the CRAN packages except those for Windows only and some from
Bioconductor), vignettes or task views, using the search engine at
<http://search.r-project.org/>.

- rOpenSci maintain a [Help Wanted](https://ropensci.org/help-wanted/) page that can be used to search for rOpenSci packages looking for (co-)maintainers. The [Wishlist](https://discuss.ropensci.org/c/wishlist/6) channel of the rOpenSci forum is used to discuss ideas for new packages/features.
- [cran GitHub mirror](https://github.com/cran), an unofficial read-only mirror of all CRAN packages (including archived packages), enables search across the full package source files using [GitHub Code Search syntax](https://docs.github.com/en/search-github/github-code-search/understanding-github-code-search-syntax).

### Intializing a package

`utils::package.skeleton()` automates some of the set-up for a new
source package. It creates directories, saves functions, data, and R
code files provided to appropriate places, and creates skeleton help
files and a Read-and-delete-me file describing further steps in
packaging.

WRE reference: [Package Structure](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Package-structure).

 - `r pkg("usethis", priority = "core")` has many utilities to setup the structure of the package, including a NEWS file and adding dependencies in the README.
 - `r pkg("pkgKitten")` provides an almost empty package skeleton and some utilities to create help pages.
 - `r bioc("biocthis")` prepares for Bioconductor repository.

Some packages like `r pkg("fusen")` and `r github("jacobbien/litr-project")` prepare a project from an R markdown file. 
`r pkg("noweb")` allows to use literate programming to create the package.
There are others that help packaging several R files, `r pkg("rfold")`, or create a package from a dataset `r pkg("DataPackageR")`.
Others, like `r pkg("cookiecutter")` can create a new package from a template stored elsewhere. 

SEE ALSO:  
<https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#initializing-an-r-package>,
<https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#package-templates->, <https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#naming-things->

## Package development

### Development workflow

Some contributed packages are analogous to `r pkg("tools")` in that
they provide tools that cut across package development tasks.

[Writing R Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html)
(WRE) describes the fundamental tasks of package development.

See the [Links](#links) section for other guides, including those from
[Bioconductor](https://www.bioconductor.org/) and
[rOpenSci](https://ropensci.org/).

CONSIDER: gpttools, command line tools like ComputationalProteomicsUnit/maker, rdatsci/rtcl

SEE ALSO:  
<https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#swiss-army-knives->

### Package documentation

Help pages must be written for exported R objects and a help page may also be written for the package as a whole. Packages may also include vignettes to give an overview of package functionality or discuss more complex uses. Some contributed packages provide tools to create other types of documentation such 
as package websites or tutorials.

SEE ALSO (map to relevant subsections): 
<https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#documentation>, <https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#documentation->

#### Help pages

Source files for help pages use the "R documentation" (Rd) format. 
`utils::prompt` and `utils::promptData` may be used to create an Rd template for a function or data set, respectively. `tools::Rdcheck` may be used to validate Rd files, e.g., detecting syntax errors.

WRE reference: [Writing R documentation files](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Writing-R-documentation-files)

#### Vignettes

The default format for vignettes is Sweave format with special metadata described in WRE. `R CMD build` will run `utils::Sweave` to build the vignettes as part of the package.

WRE reference: [Writing package vignettes](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Writing-package-vignettes)

CONSIDER: rmarkdown::html_vignette, markdown::html_format as more lightweight alternative c.f. https://github.com/Rdatatable/data.table/pull/5773

#### Other forms of documentation

CONSIDER: pkgdown, learnr

### Package metadata and information files

`tools::CRAN_package_db` returns a data frame with character columns containing most `DESCRIPTION` metadata for the current packages in the CRAN package repository.

`utils::NEWS` can be used to extract the NEWS for a package and display it in a browser.

CONSIDER: desc, thankr, badger

SEE ALSO:   https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#package-metadata-

### Package logos

To help promote packages, it has become popular to create hexagon-shaped logos 
that may be used in package documentation or information files and may be 
used to create promotional material such as stickers.

SEE ALSO:   https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#badges-and-stickers

### Packages tests

Unit tests or regression tests can be added in a `tests` directory. Tests can 
be written in R scripts without depending on additional packages, using 
`base::stopifnot` or similar to throw errors when expectations fail. Test 
scripts are run by [`R CMD check`](#standard-check), with output saved to a 
corresponding `.Rout` file. If a corresponding `.Rout.save` exists in the 
`tests` directory, the two output files are compared, with differences being 
reported but not causing an error.

WRE reference: [Package subdirectories](https://cran.r-project.org/doc/manuals/R-exts.html#Package-subdirectories)

These packages provide some automation and helpers to test code:
 - `r pkg("tinytest")` allows to add tests to a package with no further dependencies and includes the tests in the installed package, making them available to users.
 - `r pkg("testthat")` helpers for tests including snapshot tests. `r pkg("patrick")` allows to parameterize testing with testthat. `r pkg("hySpc.testthat")` attaches the tests to functions.
 - `r pkg("RUnit")` and `r pkg("svUnit")` provides alternative unit testing frameworks, similar to `r pkg("testthat")`
 - `r pkg("testit")` provides two convenience functions `assert()` and `test_pkg()` for a simple unit testing interface with minimal dependencies.
 - `r pkg("roxytest")` and `r pkg("roxut")` provide `r pkg("roxygen2")` roclets for testing with testthat and tinytest.
 - `r pkg("realtest")` testing with distinct behaviours: expected, acceptable, current, fallback, ideal, or regressive.
 - `r pkg("unitizer")` provides a testing framework for interactive regression testing, making it simpler to review and debug tests.
 - `r pkg("unittest")` testing using the Test Anything Protocol, producing test output in a standard text format.
 - `r pkg("exampletestr")` and `r pkg("doctest")` convert examples into tests to be run by testthat. `r pkg("testex")` converts documentation by roxygen2 into test to be run by testthat.
 - `r pkg("cucumber")` write specifications in feature files using 'Gherkin' language.
 - `r pkg("quickcheck")` checks against randomly generated input and compatible with testthat.
 - `r pkg("xpectr")` provides tools for generating expectations in a systematic way.
 

Testing internet requests can be difficult to do it reliably. 
One can use [this book "HTTP testing in R"](https://books.ropensci.org/http-testing/) to take inspiration:
 - `r pkg("vcr")` records requests and replays them during future runs. 
 - `r pkg("webmockr")` stubbing and setting expectations on 'HTTP' requests. 
 - `r pkg("httptest2")` works for recording and saving requests made by the httr2 package without requiring access to the remote service.

Other packages focused on specific areas:
 - `r pkg("vdiffr")` provides helpers for graphical unit tests, testing visual differences.
 - `r pkg("shinytest2")` provides a testing framework for Shiny applications.
 - `r pkg("validate")` declares data validation rules and data quality indicators.

#### Code coverage

SEE ALSO:   https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#code-coverage

### Working with package options

CONSIDER: options, withr::with_options

SEE ALSO: <https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#using-options-in-packages>

### Creating graphical user interfaces

For simple interactive interfaces, `base::readline()` can be used to create a simple prompt. `utils::menu()`, `utils::select.list()` can provide graphical and console-based selection of items from a list, and `utils::txtProgressBar()` provides a simple text progress bar.

`tcltk` is a base package that provides a large set of tools for creating interfaces uses Tcl/tk (most functions are thin wrappers around corresponding Tcl and tk functions).

SEE ALSO:   https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#creating-graphical-interfaces

### Localisation

Packages might be addressed to people using a different language and locale. 
Localization in R uses GNU `gettext` as described in the notes on [Translating R Messages](https://developer.r-project.org/Translations30.html) which uses translations stored in PO files.

`tools:update_pkg_po` creates or updates the PO template files for a package, 
and updates corresponding PO files as required. `tools::checkPoFile` can be 
used to check translation files for inconsistent format strings.

CONSIDER: potools, https://github.com/eliocamp/rhelpi18n

### Building and installing a source package

The standard tools to build and install a package are `R CMD build` and 
`R CMD INSTALL`.

CONSIDER: pkgload, remotes::install_github(), R universe

### Checking a package

#### Standard check

The standard package check is implemented in `R CMD check` which should
be run on a package built with `R CMD build`. The check runs examples
and tests in the package and the contents of the package are tested in
various ways for consistency and portability.

WRE reference: [Checking and building packages](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Checking-and-building-packages).
Listings of environment variables used in `R CMD check` may be found the
the [Tools
chapter](https://cran.r-project.org/doc/manuals/r-devel/R-ints.html#Tools)
of the R Internals manual. In the [Suggested packages section](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Suggested-packages) WRE recommends to run `R CMD check` both with `_R_CHECK_DEPENDS_ONLY_=true` and `_R_CHECK_SUGGESTS_ONLY_=true`, as well as with both of these set to `false`.

CRAN provide the [WinBuilder](https://win-builder.r-project.org/) and [macOS builder](https://mac.r-project.org/macbuilder/submit.html) services for checking on Windows and M1 macOS machines, respectively.

-   [R-hub](https://builder.r-hub.io/) enables checking on multiple
    platforms via the web interface or the `r pkg("rhub")` package.
    `rhub::check_for_cran()` will automatically select the available
    platforms closest to those used by CRAN. `rhub::platforms` returns
    the available platforms - it is worth checking how up-to-date these
    are. Further platforms are available via Docker containers
- `r bioc("BiocCheck")` guides maintainers through Bioconductor best practices.
    
See the [Continuous Integration](#Continuous-Integration) section for
information on running package checks automatically.

Package authors may supplement the standard package check with additional 
checks of code or text quality, using tools in the following sub-sections.

#### Code quality checks

-   The [rchk](https://github.com/kalibera/rchk) GitHub repository
    provides Docker and Singularity containers that may be used to check
    for memory protection errors in the C code of a package. The
    [r-lib/actions](https://github.com/r-lib/actions) GitHub repository
    provides a GitHub action to run the rchk tests.
* `codetools::checkUsagePackage` will check all functions in a package for 
possible problems, e.g., calls not consistent with visible function definitions.

CONSIDER: lintr::lint_package, PaRe, badger::badge_codefactor using codefactor.io

SEE ALSO:  
<https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#code-analysis-and-formatting>, https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#codedocument-formatting-, https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#code-analysis-  except code coverage

#### Text quality checks

`utils` provides multiple functions for spell-checking portions of packages, including .Rd files ( `utils::aspell_package_Rd_files`) and vignettes (`utils::aspell_package_vignettes`) via the general purpose aspell function, which requires a system spell checking library, such as [aspell](http://aspell.net/), [hunspell](http://hunspell.github.io/), or [ispell](https://www.cs.hmc.edu/~geoff/ispell.html).

CONSIDER: urlchecker.

SEE ALSO:  
<https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#spell-checking>,
https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#documentation-quality-%EF%B8%8F

### Debugging issues found in package checks

Using a check service such as WinBuilder or R-hub may reveal a bug in
your package that is specific to the computational environment, e.g.,
the version of R, or the compiler used. The following tools support
debugging in such cases.

-   R-Hub provide Docker containers in the
    [rhub-linux-builders](https://github.com/r-hub/rhub-linux-builders)
    GitHub repository. The `r pkg("rhub")` package has [helpers to use
    the images locally from
    R](https://r-hub.github.io/rhub/articles/local-debugging.html).
-   The [Rocker](https://rocker-project.org/images/) project offers
    stacks of system images. Probably most helpful for package
    development is the base stack, where R-devel is installed alongside
    R-base, with variants using different compilers/compiler settings.

## Maintenance

Most packages are developed with long-term use in mind. This requires
package developers to keep pace with changes in R, other packages,
system tools and environments (e.g. compilers, operating systems) that
affect the affect the behaviour of their package. Repositories like CRAN
or Bioconductor require packages to meet their latest policies to avoid
being archived/deprecated. Issues are typically picked up by a package
failing the regular checks run by the repository maintainers and package
authors will be contacted when action is required. It is best if package
authors take a proactive approach to maintenance to ensure their package
remains useful and available.

### Continuous integration {#continuous-integration}

GitHub actions is widely used to automate package development tasks.
Typically these are triggered on committing to the main branch of the
repository, though other triggers such as a pull request comment can be
used as a trigger. The [chron
feature](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule)
may be used to schedule actions, which can be useful to run
`R CMD check` regularly, to check for issues when a package is not under
regular development.

-   The [r-lib/actions](https://github.com/r-lib/actions) GitHub
    repository provides a GitHub action to run `R CMD check`.
-   `usethis::use_github_action` facilitates setting up an action to run
    `R CMD check` with R-release on Linux, Mac, and Windows, as well as
    R-devel and R-oldrel on Linux. Other supported actions include
    computing test coverage; building and publishing a
    `r pkg("pkgdown")` site; creating documentation with
    `r pkg("roxygen2")` and enforcing style conventions with
    `styler::style_pkg()`.
    
SEE ALSO:   https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#cicd-

### Dependency management

Most packages have forward dependencies that are declared in the
`Imports` or `Suggests` fields of the package `DESCRIPTION`. Packages
also typically depend on a minimum version of R, due to explicit or
implicit use of base R functions (e.g. for compressing data objects). 
Package authors must be vigilant to need to changes in the functionality, 
behaviour, or API of these forward dependencies.

In time, a package may in turn be imported or suggested by another package, 
creating a reverse dependency. For CRAN packages, reverse dependencies are 
listed on the landing pages (of the form `⁠https://cran.r-project.org/package={PACKAGE}`).
Package authors should be aware of these packages that may be impacted as 
their package evolves. Note that CRAN permits dependencies on Bioconductor
packages, as well as other repositories specified in the 
`Additional_repositories` field of the `DESCRIPTION` file.

Reverse dependencies may be identified with `tools::dependsOnPkgs`. Dependency 
graphs may be created using `tools::package_dependencies`,
based on a package database like that returned by `utils::available.packages`. Reverse dependencies back to CRAN packages from Bioconductor packages may be found by updating the package database repository list from `utils::available.packages` before running `tools::package_dependencies`.

`utils::update.packages` is useful for updating dependencies when trying out changes to a package. `utils::sessionInfo` is helpful in tracking changes caused by upstream updates.

`tools::check_packages_in_dir` can be used to check the reverse dependencies of a package (or set of packages).

CONSIDER: `sessioninfo::session_info`, revdepcheck, prefixer, rcheology, tools like drat and miniCRAN

WRE reference: [Package Dependencies](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Package-Dependencies).



SEE ALSO:  
<https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#dependency-management>, https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#dependency-management-%EF%B8%8F

### Managing changes

CONSIDER: lifecycle, TODOr, maybe diffify

SEE ALSO:   https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#change-log-and-versioning

### Tracking usage

CONSIDER: dlstats

SEE ALSO:   https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#usage-

### Keeping up to date

WRE is versioned, addressing
[R-release](https://cran.r-project.org/doc/manuals/r-release/R-exts.html),
[R-patched](https://cran.r-project.org/doc/manuals/r-patched/R-exts.html),
and
[R-devel](https://cran.r-project.org/doc/manuals/r-devel/R-exts.html).
Changes do occur as R develops and as the software components on which R
is built evolve.

`tools::testInstalledPackage` can be used to check if an installed 
package still passes check, while `tools::summarize_CRAN_check_status` will 
summarize the CRAN check status for one or more packages. 

The [R-package-devel mailing list](https://stat.ethz.ch/mailman/listinfo/r-package-devel) 
provides help on package development and can help keep up-to-date with best practices.
    
The [R Blog](https://blog.r-project.org/) and the [R-devel mailing list](https://stat.ethz.ch/mailman/listinfo/r-devel) can help keep track of 
developments in R that may affect your package. 

The [R-announce mailing list](https://stat.ethz.ch/pipermail/r-announce/) 
announces planned R releases, indicating when it is a good time to [test release candidates](https://blog.r-project.org/2021/04/28/r-can-use-your-help-testing-r-before-release/) on critical workflows.

-   Changes in the CRAN Repository Policy are tracked in the [crp GitHub
    repository](https://github.com/eddelbuettel/crp).
-  The [r-mailing-list-archive GitHub repository](https://github.com/MichaelChirico/r-mailing-list-archive)
    can be used to search past discussions on the R-devel and R-package-devel 
    mailing lists.
-   `usethis::browse_package` lets you select from URLs in the package
    `DESCRIPTION`, which can be a convenient way to find source code
    repositories of crucial upstream dependencies, to track their
    development.
    
CONSIDER: foghorn, badger::badge_cran_checks(), wurli/updateme

## Links {#links}

-   [Bioconductor Packages: Development, Maintenance, and Peer
    Review](https://contributions.bioconductor.org/)
-   [rOpenSci Packages: Development, Maintenance, and Peer
    Review](https://devguide.ropensci.org/)
    
CONSIDER: other repositories

SEE ALSO: 
<https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#related-links>
