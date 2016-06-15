R Snippets
====

Send emails from R

```r
library(mailR)
send.mail(from = "xxx@gmail.com",
          to = c("xxx@gmail.com", "xxx@gmail.com"),
          subject = "xxx",
          body = "<html><img src=\"unnamed-chunk-3.png\"><br/><img src=\"unnamed-chunk-4.png\"></html>",
          html = TRUE,
          inline=TRUE,
          smtp = list(host.name = "smtp.gmail.com", port = 465, user.name = "xxx", passwd = "*", ssl = TRUE),
          attach.files = c("unnamed-chunk-3.png", "unnamed-chunk-4.png"),
          authenticate = TRUE,
          send = TRUE)
```


```r
# Change mimetype to PNG
options(jupyter.plot_mimetypes = "image/png") 
#options(jupyter.plot_mimetypes = "image/svg+xml") 

options(repr.plot.width = 8, repr.plot.height = 3)
```

