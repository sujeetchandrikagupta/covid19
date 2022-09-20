
import requests
import json 
from tkinter import *

window = Tk()

#window config
window.title('COVID19 Info')
window.geometry('400x100')

def get_country_info():
    url = 'https://api.covid19api.com/summary'   
    response = requests.request("GET",url)

    #get data from APi store in json
    data = json.loads(response.text)

    #get index for country
    searchcountry = txt.get()

    def get_country_index(country):
        for index,item in enumerate(data['Countries']): 
            if item['Country'] == country:
                return index

    countryid = get_country_index(searchcountry)  

    #Get New Confirmed & toatal Confirmed
    newconfirmed = data['Countries'][countryid]['NewConfirmed']
    totalconfirmed = data['Countries'][countryid]['TotalConfirmed']

    covid_msg = f'last number of new confirmed cases in {searchcountry} {newconfirmed}.\nThe total cases are: {totalconfirmed}'

    #return covid msg to gui
    output_text.set(covid_msg)

#create Label
lbl = Label(window, text='Enter Country:')
lbl.grid(column=0,row=0, sticky=E)

#create Enter Filed
txt =Entry(window, width=30)
txt.grid(column=1, row=0, sticky=W)

#create button
btn = Button(window, text='Get Information', command=get_country_info)
btn.grid(column=2, row=0, sticky=W)

#Display output
output_text = StringVar()
lbl_output = Label(window, textvariable=output_text)
lbl_output.grid(column=0,columnspan=2, row=1, sticky=W)

window.mainloop()
