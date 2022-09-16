# Python_Live_Project

OVERVIEW

Two week project utilizing Django with Python in order to build a website and app with several linked documents/pages. Using templates we laid out the basic structure we want applied to each document. This was a great way to see the bigger picture of a large project and how all the moving parts relate to each other and work together. In this project we worked with HTML, CSS, Python, Django specific syntax, and learned how to implement Beautiful Soup.


The app I built is a Wishlist Creator/Manager for new guitars. Using the combined languages and syntax outlined above I built various templates and views/functions to work with a database and allow users to make edits or delete previous lists as well as save changes or make new lists from scratch. The app also utilizes Beautiful Soup for web scraping and returns the desired data from other websites with applicable information.

--------In this two week sprint I completed 8 stories each comprised of a specific task to complete.----------

1. Built and registered basic app to create Guitar Wishlists using Django, this included linking the views.py, settings.py, urls,py, and other files as well as views to render the desired page with basic elements/styling.

2. Created a model to base our objects off of with all fields we want to have data for. Also linked our model form to a database in order to save new items.
Model Description/Included Fields for a new Guitar:

```
class GitFiddle(models.Model):
    list_name = models.CharField(max_length=50)
    brand = models.CharField(max_length=30)
    strings = models.IntegerField()
    acoustic = models.CharField(max_length=30)
    frets = models.IntegerField()
    price = models.DecimalField(max_digits=100, decimal_places=2)

    objects = models.Manager()


    def __str__(self):
        return self.list_name
```

3. Created a new HTML page that retrieves all the items from database and sends them to a template. Then displaying those items on the page with some fields treated as labels/headers. 

4. Made a details page showing individual items when selected with all relevant information/data.
Using variables with Django tags to plugin database data.
```
<div class="details_text">
    <h3>LIST NAME: <span class="details_result_text">{{ chosenList.list_name }}</span></h3>
    <h3>BRAND: <span class="details_result_text">{{ chosenList.brand }}</span></h3>
    <h3>AMOUNT OF STRINGS: <span class="details_result_text">{{ chosenList.strings }}</span></h3>
    <h3>ACOUSTIC OR ELECTRIC? <span class="details_result_text">{{ chosenList.acoustic }}</span></h3>
    <h3>AMOUNT OF FRETS: <span class="details_result_text">{{ chosenList.frets }}</span></h3>
    <h3>PRICE (USD): $<span class="details_result_text">{{ chosenList.price }}</span></h3>
</div>
```


5. Added a edit page where ammendments can be applied to previously saved items in our database. Also added functionality to delete selected items with a confirmation page.
Below is how I laid out the functions/views for the EDIT and DELETE pages:
```
def guitar_wishlist_edit(request, pk):
    chosenList = get_object_or_404(GitFiddle, pk=pk)
    form = GuitarForm(data=request.POST or None, instance=chosenList)
    if request.method == 'POST':
        if form.is_valid():
            form.save()
            return redirect('GuitarWishlist_viewall')
    content = {'form': form, 'chosenList': chosenList}
    return render(request, 'GuitarWishlist/guitar_wishlist_edit.html', content)

#view for delete page
def guitar_wishlist_delete(request, pk):
    chosenList = get_object_or_404(GitFiddle, pk=pk)
    if request.method == 'POST':
        chosenList.delete()
        return redirect('GuitarWishlist_viewall')
    content = {'chosenList': chosenList}
    return render(request, 'GuitarWishlist/guitar_wishlist_delete.html', content)
```

6. Setup Beautiful Soup in order to parse through other websites and scrape data from them.

7. Utilized Beautiful Soup to take the data we scraped and then display it on one of our pages. Also ensured data was stripped of unwanted formatting/text. 
Shown below is the way I defined variables to be used in our returned data results from the scraping page.
```
 #PRS
    paged = requests.get("https://www.musiciansfriend.com/guitars/prs-dw-ce24-24-floyd-electric-guitar")
    soupd = BeautifulSoup(paged.content, 'html.parser')

    #description/brand name definitions-----------------------------
    # Format: info = soup.find_all('a', 'ui-link')[0].get_text()    **skip increment(s) between desc's when mult links present otherwise wrong text is returned**
    desc = soup.find_all('h1', 'pdp-section-title_title')[0].get_text()
    desca = soupa.find_all('h1', 'pdp-section-title_title')[0].get_text()
    descb = soupb.find_all('h1', 'pdp-section-title_title')[0].get_text()
    descc = soupc.find_all('h1', 'pdp-section-title_title')[0].get_text()
    descd = soupd.find_all('h1', 'pdp-section-title_title')[0].get_text()
```    

8. Finally I used the remainder of my time to clean up unwanted whitespace, improve aesthetics, and made the syling cohesive across the many pages.

![Python_live_project_css](https://user-images.githubusercontent.com/107223231/190718400-592f06d5-6793-4be9-aaf1-505231d727ab.jpg)
