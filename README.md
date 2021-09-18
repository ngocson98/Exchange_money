# Exchange_money
## Prerequisites
The currency converter project in python requires you to have basic knowledge of python programming and the pygame library.
##### Install on terminal:
+ tkinter – For User Interface (UI):  ***pip install tkinter***
+ requests – to get url            :  ***pip install requests***
## Steps to Build the Python Project on Currency Converter
1. Real-time Exchange rates
2. Import required Libraries
3. CurrencyConverter Class
4. UI for CurrencyConverter
5. Main Function
### 1. Real-time Exchange rates
To get real-time exchange rates, we will use: *https://api.exchangerate-api.com/v4/latest/USD*
![exchange-rate-json-data](https://user-images.githubusercontent.com/87347502/133896743-67fc8d7b-74c7-4465-bbba-8fa5a6cfa41d.png)
Here, we can see the data in JSON format, with the following details:
+ ***Base – USD***: It means we have our base currency USD. which means to convert any currency we have to first convert it to USD then from USD, we will convert it in whichever currency we want.
+ ***Date and time***: It shows the last updated date and time.
+ ***Rates***: It is the exchange rate of currencies with base currency USD.
### 2. Import the libraries:
    import requests
    from tkinter import *
    import tkinter as tk
    from tkinter import ttk
### 3. Create the CurrencyConverter class:
#### 3.1. Let’s create the constructor of class.
    class chuyen_doi():
        def __init__(self,url):
                self.data = requests.get(url).json()
                self.currencies = self.data['rates']

requests.get(url) load the page in our python program and then .json() will convert the page into the json file. We store it in a data variable.
#### 3.2. Convert() method:
        def convert(self, from_currency, to_currency, amount): 
            initial_amount = amount 
            if from_currency != 'USD' : 
                amount = amount / self.currencies[from_currency]
            #limiting the precision to 4 decimal places 
            amount = round(amount * self.currencies[to_currency], 4) 
            return amount
+ ***From_currency***: currency from which you want to convert.
+ ***to _currency***: currency in which you want to convert.
+ ***Amount***: how much amount you want to convert. 
### 4. Now let’s create a UI 
    class App(tk.Tk):
        def __init__(self, converter):
            tk.Tk.__init__(self)
            self.title = 'Currency Converter'
            self.currency_converter = converter
***Converter***: Currency Converter object which we will use to convert currencies. Above code will create a Frame.
+ Let’s Create the ***Converter***
        #self.configure(background = 'blue')
        self.geometry("500x200")
        #Label
        self.intro_label = Label(self, text='Exchange money', fg='red', relief=tk.RAISED, borderwidth=2)
        self.intro_label.config(font=('Time New Romand',20,'bold'))

        self.date_label = Label(self, text=f"1 USD = {self.currency_converter.convert('USD','VND',1)} VND \n Date : {self.currency_converter.data['date']}", relief=tk.GROOVE, borderwidth=2)

        self.intro_label.place(x=112, y=3)
        self.date_label.place(x=160, y=50)
![project-window](https://user-images.githubusercontent.com/87347502/133896769-22fbb141-b31c-4747-8521-436e255c7aec.jpg)
Now let’s create the entry box for the amount and options of currency in the frame. So That users can enter the amount and choose among currencies.
        # Entry box
        valid = (self.register(self.restrictNumberOnly), '%d', '%P')
        self.amount_field = Entry(self, bd=3, relief=tk.RIDGE, justify=tk.CENTER, validate='key', validatecommand=valid, width=16)
        self.converted_amount_field_label = Label(self, text='', fg='black', bg='white', relief=tk.RIDGE, justify=tk.CENTER, width=16, borderwidth=3)

        # Dropdown
        self.from_currency_variable = StringVar(self)
        self.from_currency_variable.set("USD") # default value
        self.to_currency_variable = StringVar(self)
        self.to_currency_variable.set("VND") # default value

        font = ("Courier", 12, "bold")
        self.option_add('*TCombobox*Listbox.font', font)
        self.from_currency_dropdown = ttk.Combobox(self, textvariable=self.from_currency_variable, values=list(self.currency_converter.currencies.keys()), font=font, state='readonly', width=12, justify=tk.CENTER)
        self.to_currency_dropdown = ttk.Combobox(self, textvariable=self.to_currency_variable, values=list(self.currency_converter.currencies.keys()), font=font, state='readonly', width=12, justify=tk.CENTER)

        # placing
        self.from_currency_dropdown.place(x = 30, y= 120)
        self.amount_field.place(x = 28, y = 150)
        self.to_currency_dropdown.place(x = 340, y= 120)
        #self.converted_amount_field.place(x = 346, y = 150)
        self.converted_amount_field_label.place(x = 338, y = 150)
Now Let’s add the **CONVERT** button which will call the perform function.
        # Convert button
        self.convert_button = Button(self, text="Convert", fg="black", command=self.perform)
        self.convert_button.config(font=('Courier', 10, 'bold'))
        self.convert_button.place(x = 220, y = 135)
Command = self.perform – It means on click it will call perform().
#### perform() method:
    def perform(self):
        amount = float(self.amount_field.get())
        from_curr = self.from_currency_variable.get()
        to_curr = self.to_currency_variable.get()

        converted_amount = self.currency_converter.convert(from_curr, to_curr, amount)
        converted_amount = round(converted_amount, 2)

        self.converted_amount_field_label.config(text=str(converted_amount))
#### RestrictNumberOnly() method:
Now let’s create a *restriction* in our entry box. So that user can enter only a number in Amount Field. We have discussed earlier that this will be done by our *RrestricNumberOnly method.*
    def restrictNumberOnly(self, action, string):
        regex = re.compile(r"[0-9,]*?(\.)?[0-9,]*$")
        result = regex.match(string)
        return (string == "" or (string.count('.') <= 1 and result is not None))
### 5. Let’s create the main function.
if __name__ == '__main__':
    url = 'https://api.exchangerate-api.com/v4/latest/USD'
    converter = chuyen_doi(url)

    App(converter)
    mainloop()
## Output
![Screenshot from 2021-09-19 00-13-51](https://user-images.githubusercontent.com/87347502/133896934-17159494-519d-4bf5-847b-dc32235f9df4.png)
## Finish
# Thank you!
