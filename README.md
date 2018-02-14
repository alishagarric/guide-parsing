# guide-parsing
Parses blog html on Shopify site for guides and provides for loop to display content on each guide slide/section

Place this script in replacment of {{ article.content }}

This script written in Liquid parses a blog written as:
slide 0 - title (formatted as an h4 heading)
slide 0 - image (using insert image tool)
slide 0 - description (formatted in default paragraph tag)

slide 1 - ingredients (as list elements of an unordered list)
  (if an ingredient is a product, include a URL directly after ingredient name)
  
slide x - title (formatted as an h4 heading)
slide x - image (using insert image tool)
slide x - description (formatted in default paragraph tag)

How the parsing script works:
  1. Seperate the html into three chunks (slide 0 html, ingredients html, and all other slide's html)
  2. Join the two chunks of code that aren't the ingredients
  3. Seperate individual ingredients and their link (if they have one) into their own strings by splitting the unordered list at <li> and <http> and joining them with ",,,"
  4. If link does not exist for an ingredient, list link as "nil" in that index of its string.
  3. Seperate ingredients string and links string into an array by splitting at ",,,"
  4. Seperate the "normal" slide's html by title, image and description into their own strings by splitting at </h4> and <p> and joining them with ",,,"
  5. Again, seperate their own strings into an array by splitting at ",,,"
  
  The for loop:
  1. loops through the amount of slides (by looping through the titles)
  2. If its the first slide, display title, image and description
  3. If its the second slide, either display ingredient normally
    or if that ingredient has a corresponses link, display it surrouned by an <a> tag
  4. If its any other slide, display title, image and description
