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
  1. Seperate the html into three chunks by splitting at <ul and </ul (slide 0 html, ingredients html, and all other slide's html)
  2. Join the two chunks of code that aren't the ingredients into one string (slide 0 html, and all other slide's html)
  3. Taking ingredients html, seperate individual ingredients into an array, where the arrays elements are each ingredient and their link (if they have one) 
  4. Do this by splitting at <li
  5. For each element (string) in that array, test to see if it has a link by seeing if "http" exists in the string
  6. If it has a link, split the string between the ingredient name and the URL, add the link to the end of a string titled links, and add the ingredient title to the end of a string titled ingredients
  7. If it does not have a link, add the string (ingredient title) to the end of a string titled ingredients, add "nil" to the end of a string titled links.
  8. Seperate ingredients string and links string into arrays by splitting at ",,,"
  9. Seperate the "normal" slide's html into an array, where the arrays elements are the content of the slides by splitting at </h4>
  10. For each element in that array, seperate into a second array where its elements are strings containing the content for title, image and description 
  11. Do this by splitting at <p>
  12. Test within each element for <h4 or <img (else if string doesn't contain <h4 or <img it is a desciption). 
  13. If it contains <h4 add it to the end of a string titled titles, if it contains <img add it to the end of a string titled images, and if it contains neither add it to the end of a string titled descriptions
  14. Seperate these strings into arrays by splitting at ",,,"
  
 How the for loop works:
  1. Loops through each slide (by looping through the titles array)
  2. Display slide's title, image and description
  3. If its the second slide, display ingredients before the rest of the content mentioned in step 2.
  4. Display ingredients by looping through ingredients and links array. If a ingredient has a corresponding link, display it surrounded by an <a tag

