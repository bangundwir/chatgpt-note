use python, read index.html file and parse it with beautifulshoup. find all dev with <div class="css-zgsss css-6u0yw2"> for each div from above list, try find img with class="css-1mygogd" try find span with class="prd\_link-product-name css-3um8ox" data-testid="linkProductName"> then title=span. except title='' try find span with class="prd\_link-product-price css-1ksb19c" data-testid="linkProductPrice"> except rating='' try find span with class="prd\_rating-average-text css-t70v7i" then price=span.text except price='' open data.json and write link,title,rating and price in json format.

Here is some sample Python code that demonstrates how you can use the BeautifulSoup library to parse an HTML file and extract the information you specified:

```python
from bs4 import BeautifulSoup
import json

# read the HTML file
with open('index.html', 'r') as file:
    html = file.read()

# parse the HTML with BeautifulSoup
soup = BeautifulSoup(html, 'html.parser')

# find all <div> elements with class "css-zgsss css-6u0yw2"
divs = soup.find_all('div', class_='css-zgsss css-6u0yw2')

# create an empty list to store the extracted information
data = []

# iterate over the divs
for div in divs:
    link = div.find('img', class_='css-1mygogd')
    title = div.find('span', class_='prd_link-product-name css-3um8ox', attrs={'data-testid': 'linkProductName'})
    rating = div.find('span', class_='prd_link-product-price css-1ksb19c', attrs={'data-testid': 'linkProductPrice'})
    price = div.find('span', class_='prd_rating-average-text css-t70v7i')
    title = title.text if title else ''
    rating = rating.text if rating else ''
    price = price.text if price else ''
    data.append({'link': link['src'], 'title': title, 'rating': rating, 'price': price})

# write the data to a JSON file
with open('data.json', 'w') as file:
    json.dump(data, file)
```

In this example, we're using the `find_all()` method to find all the `<div>` elements with the class "css-zgsss css-6u0yw2" and then iterating over them with a for loop. For each `<div>` element, we're using the `find()` method to find the `<img>` and `<span>` elements with the specified classes. If the elements are not found, the title, rating and price will be an empty string. Finally, we're writing the extracted information to a JSON file using the json.dump method.