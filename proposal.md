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

If you think that some package is missing from the task view, please file an issue in the GitHub repository or contact the maintainer(s).

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

The [CRAN Search](https://cran.r-project.org/search.html) page links to R-focused search tools, facilitating search of package sources, books, task views, support lists, blogs and the internet at large.

`utils::RSiteSearch()` facilitates search for keywords/phrases in help
pages (all the CRAN packages except those for Windows only and some from
Bioconductor), package metadata, vignettes or task views, using the search engine at
<http://search.r-project.org/>.

- rOpenSci maintain a [Help Wanted](https://ropensci.org/help-wanted/) page that can be used to search for rOpenSci packages looking for (co-)maintainers. The [Wishlist](https://discuss.ropensci.org/c/wishlist/6) channel of the rOpenSci forum is used to discuss ideas for new packages/features.
- [Bioconductor Code Search](https://code.bioconductor.org/search/) is an official tool to search the source files of Bioconductor packages.
- [cran GitHub mirror](https://github.com/cran), an unofficial read-only mirror of all CRAN packages (including archived packages), enables search across the full package source files using [GitHub Code Search syntax](https://docs.github.com/en/search-github/github-code-search/understanding-github-code-search-syntax).

### Intializing a package

`utils::package.skeleton()` automates some of the set-up for a new
source package. It creates directories, saves functions, data, and R
code files provided to appropriate places, and creates skeleton help
files and a Read-and-delete-me file describing further steps in
packaging.

When initializing a package, it is worth considering how it should be licensed. The CRAN Repository Policy links to a [database of licenses acceptable for CRAN](https://svn.r-project.org/R/trunk/share/licenses/license.db).

WRE reference: [Package Structure](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Package-structure).

 - `r pkg("available")` checks whether a package name is valid and available, i.e., not already in use on CRAN, Bioconductor or GitHub. Also checks for unintended meanings of the name. `r pkg("collidr")` checks for collisions between a package or function name and existing names of packages and/or functions on CRAN.
 - `r pkg("usethis", priority = "core")` provides `create_package()` to set up a minimal package structure, along with many utilities to add components, including the `use_*_license()` functions, where `*` is replaced by the license name.
 - `r pkg("pkgKitten")` provides an almost empty package skeleton and some utilities to create help pages.
 - `Rcpp.package.skeleton()` from `r pkg("Rcpp")` extends `package.skeleton()` to add the components required to use `r pkg("Rcpp")` for interfacing C or C++ code in R packages. `r pkg("usethis")` provide similar functionality with the `use_c()` and `use_rcpp()` functions.
 - `r bioc("biocthis")` automates setup for Bioconductor packages.
 - `r github("insightsengineering/r.pkg.template")` initializes a GitHub repository for an R package, with the standard files and directories, along with CI/CD configurations and pre-commit git hooks to identify and resolve common issues.
 - `r pkg("fusen")` and `r github("jacobbien/litr-project")` create a package from a R markdown file. `r pkg("noweb")` creates a package via literate programming with noweb syntax (as used by Sweave).
 - `r pkg("DataPackageR")` creates a package from a dataset. `r pkg("rcompendium")` creates the structure for a Research Compendium: an R package structure to support reproducible research including raw data, analysis scripts, outputs and a make script.
 - `r pkg("leprechaun")` adds templating code to a package skeleton for creating a Shiny app as a package, without adding to the dependencies. `r pkg("golem")` creates a package template for developing and deploying a Shiny app using the `r pkg("golem")` framework.
 - `r pkg("pkgverse")` creates a meta package that bundles several related packages that can be installed and loaded together.

## Package development

### Development workflow

Some contributed packages are analogous to `r pkg("tools")` in that
they provide tools that cut across package development tasks.

[Writing R Extensions](https://cran.r-project.org/doc/manuals/r-release/R-exts.html)
(WRE) describes the fundamental tasks of package development.

See the [Links](#links) section for other guides, including those from
[Bioconductor](https://www.bioconductor.org/) and
[rOpenSci](https://ropensci.org/).

 - `r pkg("devtools", priority = "core")` facilitates interactive development of R and compiled code via the `load_all()` function to simulate installing and reloading the package. Additional functions support generating documentation, testing, checking a package and submitting to CRAN.
 - `r pkg("usethis", priority = "core")` provides helpers to add new components such as `use_r()`, `use_data()`, `use_vignette()`, or `use_news_md()`, along with functions to support specific packages or workflows, such as `use_testthat()` or `use_git()`.
 - `r pkg("pkgmaker")` provides utilities for working with package-specific options, registry objects (as defined by `r pkg("registry")`), vignettes, unit tests and BibTeX. Serves as an incubator for tools that may later be packaged separately.
 - `r pkg("packager")` performs package development tasks (document, build, check, etc) with `r pkg("fakemake")` or GNU make, so that make targets are only regenerated when files in the make chain have been updated. Provides a `create()` function to initialize a package with the required structure and an `infect()` function to work with a package initialized another way. The `r github("ComputationalProteomicsUnit/maker")` repository provides an external Makefile to perform development tasks.
 - `r github("rdatsci/rtcl")` provides command line utilities for development tasks, which are also provided as regular R functions.
 - `r github("unDocUMeantIt/roxyPackage")` provides the `roxy.package()` function to generate help files, vignettes and package-level documentation (e.g., NEWS and README) in both PDF and HTML; check and build packages, and manage a local package repository. Tasks can be performed individually or in combination.
 - `r github("JamesHWade/gpttools")` facilitates using large language models (from an AI service provider or a local model) for package development, e.g. converting code to a function; adding documentation or tests, or identifying improvements.

### Package documentation

Help pages must be written for exported R objects and a help page may also be written for the package as a whole. Packages may also include vignettes to give an overview of package functionality or discuss more complex uses. Some contributed packages provide tools to create other types of documentation such 
as package websites or tutorials.

SEE ALSO (map to relevant subsections): 
<https://github.com/ropensci-archive/PackageDevelopment/blob/master/README-NOT.md#documentation>, <https://github.com/IndrajeetPatil/awesome-r-pkgtools?tab=readme-ov-file#documentation->

#### Help pages

Source files for help pages use the "R documentation" (Rd) format. 
`utils::prompt()` and `utils::promptData()` may be used to create an Rd template for a function or data set, respectively. `tools::checkRd()` may be used to validate Rd files, e.g., detecting syntax errors.

WRE reference: [Writing R documentation files](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Writing-R-documentation-files)

- `r pkg("roxygen2", priority = "core")` provides the `roxygenise()` function to generate Rd files from comments with specific markup in R source files. When used with Markdown, `r pkg("roxygen2")` can greatly reduce the markup required in the documentation source. Several [development workflow](#development-workflow) packages support creating documentation with `r pkg("roxygen2")`. 
- `r pkg("sinew")` generates roxygen skeletons and updates the NAMESPACE and DESCRIPTION file as required; can be used to update as well as create royxgen documentation.  
- `r pkg("Rd2roxygen")` provides functions to convert Rd files to `r pkg("roxygen2")` comments.
- `r pkg("roxygen2md")` replaces Rd syntax in `r pkg("roxygen2")` comments with the Markdown equivalent.
- `r pkg("roclang")` facilitates extracting components of documentation from an Rd file (in the same or another package) for reuse, by insertion in a roxygen comment.
- `r pkg("pasteAsComment")` provides an RStudio addin to paste clipboard content as a roxygen block - useful for inserting examples; `r github("csgillespie/roxygen2Comment")` provides an RStudio addin to convert regular code to roxygen commented code and vice versa.
- `r pkg("roxyglobals")` adds additional roxygen tags to generate R code defining global variables with `utils::globalVariables()`. This can be used to avoid false positives in the package check when functions in the package call functions that use non-standard evaluation.
- `r github("moodymudskipper/devtag")` provides the `@dev` tag for creating developer documentation for unexported functions; `r github("ropensci-review-tools/srr")` adds tags to document adherence to rOpenSci's standards for software review.
- `r pkg("Rdpack")` provides functions and Rd macros for developing documentation, e.g. adding template documentation for new arguments; importing references from BibTeX files, and evaluating R code then inserting the resulting output or graphic.
- `r pkg("Rd2md")` converts Rd files to Markdown and creates a combined Markdown reference manual; `r github("Genentech/rd2markdown")` generates Markdown from a source Rd file or the help page of an installed package.
- `r github("coolbutuseless/rd2list")` converts Rd files to an R list.

#### Vignettes

The default format for vignettes is Sweave format with special metadata described in WRE. `R CMD build` will use `utils::Sweave()` as the default *vignette engine* to build a vignette. Alternative vignette engines can be specified in the metadata in the form `<package>::<engine>`.

WRE reference: [Writing package vignettes](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Writing-package-vignettes), especially [Non-Sweave vignettes](https://cran.r-project.org/doc/manuals/R-exts.html#Non_002dSweave-vignettes-1)

- `r pkg("knitr", priority = "core")` provides vignette engines to compile HTML and PDF vignettes. The `knitr::rmarkdown` engine can be used with the `rmarkdown::html_vignette()` format, which is a lightweight alternative to `knitr::html_document()`. For mathematics to render offline, the `math_method` argument of `rmarkdown::html_vignette()` should be set to `"katex"` or `"r-katex"`.
- `r pkg("litedown", priority = "core")` provides a vignette engine that can be used with its own output formats for HTML and PDF. It is designed to have minimal dependencies and produce lightweight HTML files. It has sufficient features for most vignettes (e.g., table of contents, cross-references, citations) and is recommended unless richer features are required.
- `r pkg("quarto")` provides a vignette engine that fixes configurations of the HTML format to produce a lightweight file. This may be preferred for richer features (equivalent to Sweave) or re-using material already using Quarto. Since `r pkg("quarto")` depends on `r pkg("rmarkdown")`, and adds Quarto as a system dependency, this option is dependency-heavy.
- `r pkg("prettydoc")` provides `html_pretty()` as an alternative to `rmarkdown::html_vignette` that produces lightweight files with fancier themes.
- `r pkg("R.rsp")` provides facilities to include static PDF or HTML vignettes; compile vignettes from plain LaTeX files, and render PDF or HTML vignettes with the `R.rsp::rsp` engine to use RSP pre-processing directives (e.g., include an external file or use document metadata) and code expressions that allow looping over text with code snippets.

#### Other forms of documentation

- `r pkg("pkgdown", priority = "core")` generates a package website based on standard documentation files with the option to complement this with additional content, such as externally hosted `r pkg("learnr")` tutorials. There are helpers for deploying the site on GitHub Pages. 
- `r pkg("altdoc")` is designed as a lightweight alternative to `r pkg("pkgdown")` with support for many documentation generators, including Quarto, Docute, Docsify, and MkDocs.
- `r pkg("ropenscilabs/r2readthedocs")` converts package documentation to a Read the Docs website.
- `r pkg("rdoxygen")` creates Doxygen documentation for C++ code in packages, optionally made available as a vignette.

### Package metadata and information files

`tools::CRAN_package_db()` returns a data frame with character columns containing most `DESCRIPTION` metadata for the current packages in the CRAN package repository.

`utils::NEWS()` can be used to extract the NEWS for a package and display it in a browser.

- `r pkg("desc")` provides tools to read, write, create, and manipulate DESCRIPTION files.
- `r pkg("semver")` provides tools for operating on [semantic version strings](http://semver.org).
- `r pkg("newsmd")` provides functions to create or update a `NEWS.md` file. `r pkg("fledge")` and `r pkg("autonewsmd")` generate `NEWS.md` from git commit messages following conventions specific to each package.
- `r pkg("codemetar")`, or the leaner `r pkg("codemeta")`, convert metadata from packages sources such as DESCRIPTION and CITATION to the CodeMeta `JSON-LD` format. This is a cross-language metadata standard used by search engines, software repositories etc. 
- `r pkg("cffr")` and `r pkg("citation")` generate a `CITATION.cff` file from package metadata and provide utilities to work with such files, e.g. converting to/from `"bibentry"` objects (see `utils::bibentry()`). `r pkg("cffr")` provides helpers for maintenance via git. `CITATION.cff` is a cross-language citation file format recognized by software repositories and citation managers.
- `r pkg("badger")` generates URLs for customised badges from providers such as [shields.io](https://shields.io/), commonly added to README files and package websites to display metadata such as current CRAN version.
- `r pkg("allcontributors")` facilitates acknowledging all contributors to code and repository issues in the README.

### Package logos

To help promote packages, it has become popular to create hexagon-shaped logos 
that may be used in package documentation or information files and may be 
used to create promotional material such as stickers.

- `r pkg("hexSticker")` creates hex sticker designs based on R plots or image files.
- `r github("mitchelloharawild/hexwall")` generates an image of tesselated hex sticker designs.

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
 - `r pkg("tinytest", priority = "core")` allows to add tests to a package with no further dependencies and includes the tests in the installed package, making them available to users.
 - `r pkg("testthat", priority = "core")` helpers for tests including snapshot tests. `r pkg("patrick")` allows to parameterize testing with testthat. `r pkg("hySpc.testthat")` attaches the tests to functions.
 - `r pkg("RUnit")` and `r pkg("svUnit")` provides alternative unit testing frameworks, similar to `r pkg("testthat")`
 - `r pkg("testit")` provides two convenience functions `assert()` and `test_pkg()` for a simple unit testing interface with minimal dependencies.
 - `r pkg("roxytest")` and `r pkg("roxut")` provide `r pkg("roxygen2")` roclets for testing with testthat and tinytest.
 - `r pkg("realtest")` testing with distinct behaviours: expected, acceptable, current, fallback, ideal, or regressive.
 - `r pkg("unitizer")` provides a testing framework for interactive regression testing, making it simpler to review and debug tests.
 - `r pkg("unittest")` testing using the Test Anything Protocol, producing test output in a standard text format.
 - `r pkg("exampletestr")` and `r pkg("doctest")` convert examples into tests to be run by testthat. `r pkg("testex")` converts documentation by roxygen2 into tests to be run by testthat.
 - `r pkg("cucumber")` integrates with testthat to run tests specified using the 'Gherkin' language to describe high level scenarios, e.g. when \<I do this\>, then \<this should happen\>.
 - `r pkg("quickcheck")` and `r github("ropensci-review-tools/autotest")` check against randomly generated inputs and are compatible with testthat. 
 - `r pkg("xpectr")` provides tools for generating expectations for testthat tests in a systematic way.
 

Testing internet requests can be difficult to do it reliably. 
One can use [this book "HTTP testing in R"](https://books.ropensci.org/http-testing/) to take inspiration:
 - `r pkg("vcr")` records HTTP requests and replays them during future runs. 
 - `r pkg("webmockr")` stubbing (generating dummy results) and setting expectations on 'HTTP' requests. 
 - `r pkg("httptest2")` works for recording and saving requests made by the httr2 package without requiring access to the remote service.
 - `r pkg("webfakes")` allows to create and launch fake apps in test files, for testing complex behaviour.

Other packages focused on specific areas:
 - `r pkg("gdiff")` and `r pkg("vdiffr")` provide helpers for visual tests on graphical output. vdiffr integrates with testthat and provides a Shiny app to manage test cases.
 - `r pkg("shinytest2")` provides a testing framework for Shiny applications.
 - `r pkg("mcunit")` helps with unit testing MCMC methods.
 - `r pkg("checkmate")` and `r pkg("testdat")` package which extend testthat for data structures unit testing.

#### Code coverage

- `r pkg(covr, priority = "core")` track and reports code coverage of package tests and optionally uploads the results to a service like [Codecov](https://about.codecov.io) or [Coveralls](https://coveralls.io).
- `r pkg(covtracer)` links tested code to documentation, to evaluate coverage of documented behaviours.

### Package-specific options

A simple way to implement package-specific options is to set global options with 
a package-specific prefix, but there are several packages that provide more 
refined and robust management of options.

- `r pkg("options")` provides helpers to define and document package-specific options, managed via `base::options()`, with corresponding environment variables. The options can be set globally or locally (e.g., within a function).
- `r pkg("GlobalOption")` provides similar functionality to `r pkg("options")`, with extra features such as option validation, setting options as functions of other options, and secret or read-only options.
- `r pkg("potions")` provides the `brew()` and `pour()` functions, that can be used to store and retrieve package-specific global options, without over-writing any global options of the same name.
- `r pkg("settings")` allows to set up a package-specific options manager, for setting global or local options, with optional validation rules.
- `r pkg("futile.options")` allows to set up a package-specific options manager for global options.
- `r pkg("pkgconfig")` allows packages to set package-specific values of global options, that can be queried by other packages.
- `r pkg("withr")` provides the `local_options()` function to temporarily change global options within the scope of a function.

### Creating user interfaces

For simple interactive interfaces, `base::readline()` can be used to create a basic prompt, while `utils::askYesNo()` prompts for a set response, by default "Yes" or "No". 
`utils::menu()` and `utils::select.list()` can provide graphical and console-based selection of items from a list, respectively. `utils::txtProgressBar()` provides a text progress bar.

`tcltk` is a base package (not loaded by default) that provides a large set of tools for creating graphical interfaces using Tcl/Tk. most functions are thin wrappers around the corresponding Tcl and Tk functions.

  * `r pkg(tcltk2)` provides additional Tcl commands and Tk widgets to supplement tcltk. 
  * `r pkg(fgui)` facilitates rapid generation of a Tcl/Tk interface to one or multiple functions.
  * `r pkg(getPass)` provides interfaces for securely requesting a passphrase, masking the characters typed in by the user. A GUI is used where possible, with fallback to a terminal interface.
  * `r pkg(progress)` provides configurable text progress bars, for R and C++.
  * `r pkg(shiny)` provides a framework to create browser-based interfaces, from function dialogues to more complex interactive web applications, that can be run locally with `runApp()` or deployed as static web or dynamic websites. See the [Web Technologies and Services](https://cran.r-project.org/web/views/WebTechnologies.html#frameworks) task view for other frameworks for building R-based web applications.

### Localization

Packages might be addressed to people using a different language and locale. 
Localization in R uses GNU `gettext` as described in the notes on [Translating R Messages](https://developer.r-project.org/Translations30.html) which uses translations stored in PO files. 

`tools::update_pkg_po()` creates or updates the PO template (`.pot`) files for a package, 
and updates corresponding PO (`.po`) files as required. `tools::checkPoFile()` can be 
used to check translation files for inconsistently formatted strings.

* `r pkg(potools, priority = "core")` provides helpers to create/update `.pot` and `.po` files, compile the `.po` files for distribution in a package, and run diagnostics to detect issues, e.g., untranslated messages due to inappropriate R/C code.
* Alternative mechanisms for localization of R messages are provided by `r pkg(stranslate)` 
and `r pkg(translated)`, using plain text and JSON files respectively. 
* `r github("eliocamp/rhelpi18n")` provides experimental support for localization of help pages, based on YAML files provided by companion packages.

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

- `r pkg("rhub", priority = "core")` (R-hub v2) enables you to run 
`R CMD check` on multiple platforms, including Linux, macOS and Windows, via 
GitHub Actions. The rhub package helps you set up the GitHub Actions on your 
own GitHub repository, or if you don’t have a GitHub account, you can submit 
your package to the r-hub2 GitHub organization to use their shared pool of 
runners.
- `r pkg("rcmdcheck")` runs `R CMD check` from R, returning an `"rcmdcheck"` 
object, which you can query and manipulate. The package also provides functions 
to parse check results from a file or from a url, including official CRAN check 
results.
- `r bioc("BiocCheck")` guides maintainers through Bioconductor best practices.
    
See the [Continuous Integration](#Continuous-Integration) section for
information on running package checks automatically.

Package authors may supplement the standard package check with additional 
checks of code or text quality, using tools in the following sub-sections.

#### Code quality checks

- The recommended package `r pkg("codetools", priority = "core")` provides 
`checkUsagePackage()` to check all R code in a package for 
possible problems, e.g., calls not consistent with visible function definitions. 
`r pkg("checkglobals")` provides a lightweight alternative to 
`codetools::findGlobals()` to check for missing function imports and/or variable 
definitions without the need for package installation or code execution.
- `r pkg("lintr", priority = "core")` checks code for adherence to a given 
style, syntax errors and violations of best practices. `lintr::lint_package()` 
can be used to lint R code in a package, including in supplementary files 
such as tests and vignettes. lintr is combined with linters 
for other languages by tools including [MegaLinter](https://megalinter.io), 
[super-linter](https://github.com/github/super-linter) and 
[CodeFactor](https://www.codefactor.io/). 
`r pkg("adaptalint")` infers the coding style from a package, for linting 
the same package or a different one. 
`r pkg("styler")` automatically reformats code to adhere to a given style guide, 
elminating some of the problems `r pkg("lintr")` can detect.
- `r pkg("goodpractice")` checks a package for good practices, incoporating 
checks from `R CMD check` and `lintr`, as well as further checks such a using 
`r pkg("cyclocomp")` to check code complexity. Checks can be run individually, 
e.g., `goodpractice::gp(pkg_path, checks = "rcmdcheck_portable_file_names")`.

#### Compiled code checks

WRE reference: [Checking memory access](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Checking-memory-access) 
documents how memory violations and other issues in compiled code such as 
undefined behaviour can be detected with tools including Valgrind, 
Address Sanitizer (ASAN) and Undefined Behaviour Sanitizer (UBSAN). 
These tools required appropriately configured builds of R. 

- [rchk](https://github.com/kalibera/rchk) is a tool for finding 
memory protection errors in code that uses R's C API and it is run by 
CRAN as an additional check on R packages using C. The rchk repository provides 
the tool in a pre-built Docker container. 
-  [R-hub](https://r-hub.github.io/rhub/) v2 enables you to run 
`R CMD check` with R-devel built with Valgrind, sanitizers, or rchk, along with 
many other specific configurations of R. 
- `r pkg("cppcheckR")` allows to run [Cppcheck](https://cppcheck.sourceforge.io/) 
on C and C++ files to detect errors and recommend good practices.

#### Text quality checks

`utils` provides multiple functions for spell-checking portions of packages, 
including .Rd files ( `utils::aspell_package_Rd_files`) and 
vignettes (`utils::aspell_package_vignettes`) via the general purpose `aspell` 
function, which requires a system spell checking library, such as 
[aspell](http://aspell.net/), [hunspell](http://hunspell.github.io/), or 
[ispell](https://www.cs.hmc.edu/~geoff/ispell.html).

- `r pkg("spelling")` spell checks documents in common formats, including 
LaTeX and Markdown, as well as R documentation and DESCRIPTION files. Packages 
may define a wordlist to allow custom terminology.

### Debugging issues found in package checks

Using a check service such as WinBuilder or R-hub may reveal a bug in
your package that is specific to the computational environment, e.g.,
the version of R, or the compiler used. The following tools support
debugging in such cases.

- The [Rocker](https://rocker-project.org/images/) project provides several 
Dockers containers with R installed. The 
[versioned stack](https://rocker-project.org/images/#the-versioned-stack) can 
be used to work with specific version of R, while the 
[base stack](https://rocker-project.org/images/#the-base-stack) provides R-devel 
installed alongside R-release, optionally configured with ASAN and USAN 
sanitizers.
- The Docker containers used by [R-hub](https://r-hub.github.io/rhub/) are 
documented on the [R-hub containers](https://r-hub.github.io/containers/) site, 
with instructions for how to use them for local debugging.
Available containers include builds of R-release, R-patched and R-devel, with 
the latter available in multiple configurations, including several 
configured for debugging memory issues.
- [r-debug](https://github.com/wch/r-debug) provides a single Docker image 
containing various builds of R for debugging memory problems. These include 
images with the debugging tools Valgrind or gdb, as well as images with R 
compiled for use with ASAN, UBSAN or [gctorture](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Using-gctorture-1).

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

CONSIDER: checked, `sessioninfo::session_info`, revdepcheck, prefixer, rcheology, tools like drat and miniCRAN

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
