# Making Memes with R and the Magick Package

## My Meme I Made With R and Magick
![My meme](my_meme.png)

## How I Made That Meme
```r
library(magick)


# from https://imgflip.com/memetemplate/240650289/I-need-spongebob
i_need_meme <- image_read("https://i.imgflip.com/3z9yy9.png") %>% 
    image_crop("1200x2000+0+200") %>%
    image_scale("500")

# from https://knowyourmeme.com/photos/1842748-go-to-horny-jail
no_dog <- image_read("https://i.kym-cdn.com/photos/images/newsfeed/001/842/748/23b") %>%
    image_scale("743") %>%
    image_extent("743x776", color="white")

# from https://imgflip.com/memegenerator/139971723/Spongebob-Burning-Paper
burning_paper_meme <- image_read("https://i.imgflip.com/2bc2vf.jpg?a457635") %>% 
    image_scale("500")

# from https://knowyourmeme.com/photos/1908788-wholesome-memes
yes_dog <- image_read("https://i.kym-cdn.com/photos/images/newsfeed/001/908/788/652.png") %>%
    image_crop("550x350+50+100") %>%
    image_scale("743")


large_r_logo <- image_blank(width = 800, height=620, color="white") %>% 
    image_composite(image_read("images/R_logo.png")) %>%
    image_scale("80")

small_r_logo <- large_r_logo %>% image_scale("40")

edited_i_need_meme <- i_need_meme %>% 
    image_annotate("I need to impute the missing values\nbefore using machine learning", 
                   size = 28, 
                   gravity="south", 
                   location = "+0+10", 
                   color="black") %>%
    image_composite(large_r_logo, offset="+260+15") %>% 
    image_composite(small_r_logo, offset="+130+340") %>%
    image_composite(small_r_logo, offset="+380+340")


top_level <- c(edited_i_need_meme, no_dog) %>% 
    image_append()

edited_burning_paper_meme <- burning_paper_meme %>%
    image_annotate("Data with\nmissing\nvalues", 
                   size=30, 
                   location="+50+120", 
                   color="black") %>%
    image_annotate("df %>% dropna()", 
                   size=20, 
                   color="black",
                   degrees = 30,
                   font = "mono",
                   weight = 700,
                   location = "+5+475")

edited_yes_dog <- yes_dog %>% 
    image_annotate("gud", size=60, color = "black", weight=800, location = "+325+500")


bottom_level <- c(edited_burning_paper_meme, edited_yes_dog) %>% 
    image_append()

combined_meme <- c(top_level, bottom_level) %>%
    image_append(stack=TRUE)

final_meme <- image_draw(combined_meme)
abline(h=777, col="black", lwd=10)

final_meme %>% image_write(path="images/my_meme.png", format="png")
```
