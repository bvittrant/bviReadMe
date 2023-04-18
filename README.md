# General informations : bviReadMe

Usual functions things for everythings !

# Useful links

- [Data team handbook](https://about.gitlab.com/handbook/business-technology/data-team/)

# IDE

## VCS 

### shortcuts

- collapse all chunks possible : ctrl+k then ctrl+0

## RStudio

## Creating section

*To insert a new code section you can use the Code -> Insert Section command. Alternatively, any comment line which includes at least four trailing dashes (-), equal signs (=), or pound signs (#) automatically creates a code section. For example, all of the following lines create code sections*

 \# Section One ---------------------------------
 
 \# Section Two =================================
 
 \#\#\# Section Three ############################# 

## Shortcuts

### Collapsing sections

- Collapse — Alt+L
- Expand — Shift+Alt+L
- Collapse All — Alt+O
- Expand All — Shift+Alt+O

# Python 

## How to import with direct path

tuto :  https://docs.python.org/3/library/importlib.html#importing-a-source-file-directly

```

file_path_module_functions = "path/to/your/file.py"

module_name = "name"

spec = importlib.util.spec_from_file_location(module_name, file_path_module_functions)

module = importlib.util.module_from_spec(spec)

sys.modules[module_name] = module

spec.loader.exec_module(module)

from name import *
```

# R

## Plot

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

## Actions

Split string and keep specific element

```
sapply(strsplit(colnames(d), "_"), getElement, 1)
```

## Functions


### Combinations functions (k by n)

```
comb = function(n, x) {
  factorial(n) / factorial(n-x) / factorial(x)
 
}
```

Compute the max number of different combination possible for a group of features without repettion and no order

```
max_com = function(n){
  sum = 0
  for(i in 1:n){sum = sum + comb(n,i)}
  return(sum)
}
```

### Dateime, time, timezone

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

# SQL

## Join

### Wikimedia references

[Link to join explanations](https://commons.wikimedia.org/wiki/File%3ASQL_Joins.svg)

# Heatmap

## R

### Complexe heatmap

- [Offcial website](https://jokergoo.github.io/ComplexHeatmap-reference/book/)
- [interactive complexe heatmap !](https://github.com/jokergoo/InteractiveComplexHeatmap)

# Git lab/hub

## Command line

### Create template

pip install cookiecutter
cookiecutter git@gitlab.corp.withings.com:Yourdir/yourproject

### initialize gitlab repo

- initialisation git : ```git init```
- Create “origin” remote project : ```git remote add origin git@gitlab.corp.withings.com:your_master_dir/your_dir/my_repo_name```
- Change the url for origin remote project : ```git remote set-url origin git@gitlab.corp.withings.com:your_master_dir/your_dir/my_repo_name```
- Track lfs system if needed : ```git lfs track "data/**"```
- add all dir & file at . in the project : ```git add .```
- commit and push : ```git commit -m "Initial commit" && git push -u origin master```

### Initializz github repo

- Check if your take token are up to date or you'll have problem prompt ask name and pwd to connect 
- First create the empty repo on your account without nothint inside then 
- All in one command:  ```git init && git commit -m "first commit" && git branch -M main && git remote add origin https://github.com/YOUR_NAME/YOUR_REPO_NAME.git && git push -u origin main```

### All in one command

- ```git add . && git commit -m "your_commit_comment" && git push```

# Pyenv

## Command line

- ```pyenv versions```
- ```pyenv install 3.8.0```
- ```pyenv virtualenv x.x.x my_data_env```
- ```pyenv virtualenv 3.8.0 my_data_env```
- ```pyenv local my_data_env```
- ```pyenv uninstall my_data_env```




# VPN

## openvpn

### Command line

- start : sudo openvpn path/to/conf.ovpn

