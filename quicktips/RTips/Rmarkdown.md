# Rmarkdown

### Boilerplate:

##### YAML header 

```yaml
---
title: "Untitled"
author: ""
date: "2/17/2017"
output:
  html_document:
    toc: true
    toc_depth: 3
---
```

##### CSS styling

```html
<style type="text/css">
h1,h2,h3,h4,h5,h6{color:#00008b}body{font-size:14px}table td{font-size:10px}.dataTables_wrapper,table th{font-size:12px}h1{font-size:40px}h2{font-size:32px}h3{font-size:24px}h4{font-size:20px}h5{font-size:16px}h6{font-size:12px}code.r,pre{font-size:10px}
</style>
```

### DT: Use DataTables (jQuery library) in R

##### Render a table using DataTables

```r
library(DT)
table.df %>% datatable(options=list(paging=TRUE, searching=FALSE, info=FALSE))
```

##### Render 
