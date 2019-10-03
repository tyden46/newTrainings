# RProfile

`R` allows you to create an **RProfile**, which defines standard settings when you are using `R`. (Similar in concept to a `.bashrc` file). Your **RProfile** is written in `R` and is sourced whenever `R` is started up.

## R user library

Users are not allowed to install R packages into the system library. However, it is easy to create a personal library for installing any packages you choose. Here are instructions on how to set this up:

**Step 1:** In your home directory, create a directory where R packages will be installed. `R` will create subdirectories within this directory depending on the version of `R` you are running.

```bash
mkdir .Rlibs
```

Now you want `R` to add this directory to the path on startup. You do this by adding a few lines your your **RProfile** that determine the version of `R` you are running and selects (or creates) the appropriate directory.

**Step 2:** If needed, create a file called `.Rprofile` in your home directory using a text editor of your choice. Add the following lines to this file:

```R
# Add user path to .libPaths
libdir <- paste0("~/.Rlibs/",R.Version()$major,".",R.Version()$minor)
if (!file.exists(libdir)) dir.create(libdir)
.libPaths(libdir)
rm(libdir)
```

## Set default CRAN mirror

To avoid selecting CRAN mirror, add this to your .Rprofile:

```R
# Set default repository
options("repos" = c(CRAN = "http://cran.r-project.org/"))
```
