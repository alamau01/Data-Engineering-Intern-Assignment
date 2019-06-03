# Data-Engineering-Intern-Assignment

#Downloaded and installed the following dependencies required for web scraping:
import csv #For the list to be written out to a comma-demilited file, python's built-in csv module is imported
import requests
from bs4 import BeautifulSoup

#Selected the wikipedia url to be scraped and assigned it to the variable wikipedia:
wikipedia = 'https://en.wikipedia.org/wiki/List_of_United_States_cities_by_population'
wiki_content = requests.get(wikipedia) #to download the contents of the wikipedia url
wiki_content = wiki_content.content

sp = BeautifulSoup(wiki_content, 'html.parser')
#Extracting the table of cities from the wikipedia url:
table_of_cities = sp.find_all('table', attrs = {'class': 'wikitable sortable'})[0]

#The rows in the table are converted into a list which can be looped to extract the necessary data.
#The rows are denoted by the <tr> tags in the table:
rows_lst = []
for row in table_of_cities.find_all('tr'):
    #Next, each cell in the row is selected inside the loop; cells are denoted by the <td> tag:
    cells_lst = []
    for cell in row.find_all('td'):
        cells = cell.text.replace('&nbsp;', ' ') #to replace the non-breaking spaces in the html content
        cells_lst.append(cells)
    rows_lst.append(cells_lst)
    
#A new file has been generated and all the information has been extracted into it using the writerows tool:
out_file = open("./top_cities.csv", "wb")
wr = csv.writer(out_file)
wr.writerow(["2018 rank", "City", "State", "2018 estimate", "2010 census", "Change", "Land area in sq. miles", "Land area in sq. km", "Population density in sq. miles", "Population density in sq. km", "Location"])
wr.writerow(rows_lst)
