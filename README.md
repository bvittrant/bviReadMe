# ![Alt text](src/img/bvireadme_img.svg)

## General informations : bviReadMe

Usual functions things for everythings ! It is just a kind of storage for what I found useful through my personnal or professionnal iterations.

## Licences

Feel free to use or modify and reshare it but cite the github page if you do and you're honnestly honnest.


# Table of content

- [Not So Random Things](#nsrt)
- [Markdown](#markdown)
- [Integrated Development Environnement (IDE)](#ide)
- [Python](#python)
- [R](#r)
- [Sql](#sql)
- [Git](#git)
- [Vpn](#vpn)
- [Datascience](#datascience)
- [End](#end)

# ![](src/img/nsrt_img.svg) <a name="nsrt"></a>

## Work organisation

- [Data team handbook (jobs and organisation)](https://about.gitlab.com/handbook/business-technology/data-team/)

# ![](src/img/markdown_img.svg) <a name="markdown"></a>

## Tips

- [10 awesomes r markdown tricks](https://towardsdatascience.com/ten-awesome-r-markdown-tricks-56ef6d41098)
- [Bookdown](https://bookdown.org/)
- [Blogdown](https://bookdown.org/yihui/blogdown/)

# ![](src/img/ide_img.svg) <a name="ide"></a>

## VCS

### shortcuts

- collapse all chunks possible : ctrl+k then ctrl+0

## RStudio

### Creating section

*To insert a new code section you can use the Code -> Insert Section command. Alternatively, any comment line which includes at least four trailing dashes (-), equal signs (=), or pound signs (#) automatically creates a code section. For example, all of the following lines create code sections*

 \# Section One ---------------------------------
 
 \# Section Two =================================
 
 \#\#\# Section Three ############################# 

### Shortcuts

Collapsing sections :

- Collapse — Alt+L
- Expand — Shift+Alt+L
- Collapse All — Alt+O
- Expand All — Shift+Alt+O

## Quarto

### Chunck

[Chunck options](https://quarto.org/docs/computations/execution-options.html) :

- eval : Evaluate the code chunk (if false, just echos the code into the output).
- echo : Include the source code in output
- output: Include the results of executing the code in the output (true, false, or asis to indicate that the output is raw markdown and should not have any of Quarto’s standard enclosing markdown).
- warning : Include warnings in the output.
- error : Include errors in the output (note that this implies that errors executing code will not halt processing of the document).
- include : Catch all for preventing any output (code or results) from being included (e.g. include: false suppresses all output from the code block).

### Figures

test 1

# ![Alt text](src/img/python_img.svg) <a name="python"></a>

## Manage paths

[How to import with direct path](https://docs.python.org/3/library/importlib.html#importing-a-source-file-directly)

```
file_path_module_functions = "path/to/your/file.py"
module_name = "name"
spec = importlib.util.spec_from_file_location(module_name, file_path_module_functions)
module = importlib.util.module_from_spec(spec)
sys.modules[module_name] = module
spec.loader.exec_module(module)
from name import *
```

Just check if a file exist.

```
import os
os.path.isfile(path)
os.path.exists(path)
```

## Pyenv

Command line :

- ```pyenv versions```
- ```pyenv install 3.8.0```
- ```pyenv virtualenv x.x.x my_data_env```
- ```pyenv virtualenv 3.8.0 my_data_env```
- ```pyenv local my_data_env```
- ```pyenv uninstall my_data_env```


## DB interaction

Download data from clickhouse with a specific url ocnnection

```
def download_data_clickhouse(path_to_file, url, query, silence=False):
    
    """
    Download data from your database from your specified query and format it to panda dataframe. 
    
    If the file already exist it just read it isntead of re-downloading it.

    Arguments:
        path_to_file -- Provide the path where to download the data
        url -- Provide the URL connection to your database
        query -- Provide the query you want to send to your database

    Keyword Arguments:
        silence -- No verbose function (default: {False})

    Returns:
        return a panda dataframe with the data from your query
    """

    if os.path.isfile(path_to_file):
        if(silence == False):
            print("File already exists")
        df = pd.read_csv(path_to_file, sep=';')
    else:
        if(silence == False):
            print("I'm connecting & downloading the data from your database")

        # Connect to the client
        client = Client.from_url(url)

        # Download the data from CH client
        data = client.execute_iter(query, with_column_types=True)

        # Build columns name
        columns = [column[0] for column in next(data)]

        # Convert data to pandas DF format
        df = pd.DataFrame.from_records(data, columns=columns)
        
        # save the results
        df.to_csv(path_to_file, index=False, sep=';')

    return df
```

# ![Alt text](src/img/r_img.svg) <a name="r"></a>

## Plot

### Double axis

Plot examples - add double x axis

```
ggplot(d_eda_summary, aes(x=minute_floor)) +
  geom_ribbon(aes(ymin=q1, ymax=q3), fill="grey80") +
  geom_line(aes(y=median), color="#69b3a2") +
  geom_vline(xintercept = Xmin, linetype=2, color = "red", linewidth=0.5) +
  geom_vline(xintercept = Xmax, linetype=2, color = "red", linewidth=0.5) +
  scale_x_continuous(breaks=c(0,Xmin, Xmax, 720, 1080,1440), labels = ~paste(., (" min ("), round(./60), "h)", sep = "")) +
  ylab("ESC") + xlab("") + ylim(0,100) +
  theme(legend.position = "top") +
  discrete_ggsci_npg
```

### Complexe heatmap

- [Offcial website](https://jokergoo.github.io/ComplexHeatmap-reference/book/)
- [interactive complexe heatmap !](https://github.com/jokergoo/InteractiveComplexHeatmap)

## Actions

Split string and keep specific element

```
sapply(strsplit(colnames(d), "_"), getElement, 1)
```

Check duplicated value in dataframe colum.

```
# Check if 'name' column contains duplicates
df_duplicates <- df[duplicated(df$name), ]

# Print the rows containing duplicates
print(df_duplicates)
```

Remove duplicated row, keep unique rows.

```
# Subset the dataframe to remove duplicates
df <- subset(df, !duplicated(df))
```

Check if library is not already installed then install it if not then load it.

```
# Check if package is already installed
if(!require(dplyr)) {
  # Install package if not installed
  install.packages("dplyr")
}

# Load package
library(dplyr)
```

## Functions

### Add column to dataframe.

The next chunck of code allows you to add a categorical columns based on a numerical one in a dataframe 

```
df$age_category <- cut(df$age, breaks = c(-Inf, 30, 60, Inf), 
                       labels = c("under 30", "30-60", "over 60"))
```

### Combinations functions (k by n)

```
# Calculates the number of combinations of x items that can be selected from a set of n distinct items.
comb = function(n, x) {
  factorial(n) / factorial(n-x) / factorial(x)
}
```

```
# The max_com function calculates the maximum number of combinations that can be made from a set of n distinct items.
max_com = function(n){
  sum = 0
  for(i in 1:n){sum = sum + comb(n,i)}
  return(sum)
}
```

### Datetime, time, timezone

Here you can find a bunch of functions to deal with datetime management in R. I tried to use base R if possible to limit call to external packages.

```
convert_to_utc = function(vector){
  return(as.POSIXct(vector, format = "%Y-%m-%d %H:%M:%S", tz = "UTC", usetz = TRUE))
}
```

```
convert_to_timezone <- function(dataframe, timezone_col, datetime_col) {
  
  # Extract the timezone and datetime columns from the dataframe)
  timezone <- dataframe[[timezone_col]]
  
  vec_datetime_col <- dataframe[[datetime_col]]

  # convert the datetime value to POSIXct format
  utc_datetime <- convert_to_utc(vec_datetime_col)
  
  # create an empty vector to store the converted datetime values
  datetime_in_timezone <- rep("", nrow(dataframe))
  
  # loop over the rows of the dataframe and convert each datetime value to the corresponding timezone
  for (i in seq_along(timezone)) {datetime_in_timezone[i] <- format(utc_datetime[i], tz = timezone[i], usetz = TRUE)}

  # return the vector of datetime values in the specified timezone
  return(datetime_in_timezone)
}
```

```
date_diff_seconds <- function(date1, date2) {

  # convert both dates to POSIXct format
  date1_posix <- as.POSIXct(date1)
  date2_posix <- as.POSIXct(date2)
  
  # calculate the difference in seconds
  diff_seconds <- difftime(date2_posix, date1_posix, units = "secs")
  
  # return the result as a numeric value
  return(as.numeric(diff_seconds))
  
}
```

```
convert_minutes_to_hour <- function(minutes) {

  # Convert minutes to hours
  hours <- floor(minutes / 60)
  
  # Extract the remaining minutes
  remaining_minutes <- minutes %% 60
  
  # Create a formatted string for the hour of the day
  hour_string <- sprintf("%02d:%02d", hours, remaining_minutes)
  
  # Return the hour string
  return(hour_string)
}
```

### Install, check and load library

For one library.

```
install_and_load_package <- function(pkg_name) {
  if (!require(pkg_name, character.only = TRUE)) {
    install.packages(pkg_name)
    library(pkg_name, character.only = TRUE)
  } else {
    library(pkg_name, character.only = TRUE)
  }
}
```

For multiple libraries and manage package from bioconductor.

```
install_and_load_packages <- function(pkg_names) {
  for (pkg_name in pkg_names) {
    if (!requireNamespace(pkg_name, quietly = TRUE)) {
      tryCatch({
        if (substr(pkg_name, 1, 2) == "Bi") {
          # Bioconductor package
          if (!requireNamespace("BiocManager", quietly = TRUE)) {
            install.packages("BiocManager")
            library(BiocManager)
          }
          BiocManager::install(pkg_name)
          library(pkg_name, character.only = TRUE)
        } else {
          # CRAN package
          install.packages(pkg_name)
          library(pkg_name, character.only = TRUE)
        }
      }, error = function(e) {
        message("Failed to install and load package: ", pkg_name)
      })
    } else {
      library(pkg_name, character.only = TRUE)
    }
  }
}
```

### Dataframes mangement

```
# Remove duplicated rows
remove_duplicated_rows <- function(df, cols_to_check) {
  # identify the duplicated rows based on the specified columns
  dup_rows <- duplicated(df[, cols_to_check]) | duplicated(df[, cols_to_check], fromLast = TRUE)
  
  # remove the duplicated rows
  df_unique <- df[!dup_rows, ]
  
  # return the updated dataframe
  return(df_unique)
}

# Convert col to character type
convert_cols_to_character <- function(df, colnames_to_convert) {
  # loop through each column name and convert to character type
  for (colname in colnames_to_convert) {
    df[[colname]] <- as.character(df[[colname]])
  }
  
  # return the updated dataframe
  return(df)
}
```

# ![Alt text](src/img/sql_img.svg) <a name="sql"></a>

## Insert variables from R/python into CH query

Use the syntax :

```
select *
from my_table
where my_feature in {tuple(my_list)}
```

## Join

### Wikimedia references

[Link to join explanations](https://commons.wikimedia.org/wiki/File%3ASQL_Joins.svg)

# ![Alt text](src/img/git_img.svg) <a name="git"></a>

## Command line

### Create template

This command will install a template directory from an existing one at the requested url.

```
pip install cookiecutter && cookiecutter git@gitlab.corp.your_compagny.com:Yourdir/yourproject
```

### initialize gitlab repo

- initialisation git : ```git init```
- Create “origin” remote project : ```git remote add origin git@gitlab.corp.withings.com:your_master_dir/your_dir/my_repo_name```
- Change the url for origin remote project : ```git remote set-url origin git@gitlab.corp.withings.com:your_master_dir/your_dir/my_repo_name```
- Track lfs system if needed : ```git lfs track "data/**"```
- add all dir & file at . in the project : ```git add .```
- commit and push : ```git commit -m "Initial commit" && git push -u origin master```

### Initialize github repo

- Check if your take token are up to date or you'll have problem prompt ask name and pwd to connect 
- First create the empty repo on your account without nothint inside then 
- All in one command:  ```git init && git commit -m "first commit" && git branch -M main && git remote add origin https://github.com/YOUR_NAME/YOUR_REPO_NAME.git && git push -u origin main```

### All in one command

- ```git add . && git commit -m "your_commit_comment" && git push```


# ![Alt text](src/img/vpn_img.svg)  <a name="vpn"></a>

## Protonvpn

[Proton VPN](https://protonvpn.com/?) is maybe the more trustable vpn at the moment only because of its country juridiction (swiss) ath the moment (around 2021). It is just a bit more expensive than its counter part. Be aware that a VPN is rarely useful for common people. The agressiv marketing telling you it's dangerous to be unprotected is kind of overkill except if you live in autocratic country.

## openvpn

### Command line

- start : sudo openvpn path/to/conf.ovpn

# ![Alt text](src/img/datascience_img.svg) <a name="datascience"></a>

## Statistical inference with R

This book is nicely written and the open documentation is free. It regroups all what you need to explore data with R and go deeper with linear algebra like regression model: 

- [ModernDive](https://moderndive.com/)

# ![Alt text](src/img/end_img.svg) <a name="end"></a>
