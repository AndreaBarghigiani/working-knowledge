A simple application that will help me (and hopefully others) to generate invoices, the main need of this application (the reason why I am doing so), is that I need to generate invoices that are able to display two currencies at the same time.
## Version 0.1
The application must run and solve the base scope with Rails7 and Hotwire.
### Features
- **exchange rates**
- **pdf generation**
- **analytics**

# Details
Have a look at the [[Dev Notes]] 
#### User:
`email`
`password`
`profile` *has_one*
`company` *has_many*
#### Profile
`fist_name`
`last_name`
`description`
`user` *belongs_to*
#### Company
`name`
`description`
`address`
`website`
`vat`
`user` *belongs_to*


# Learning opportunities
- *make API calls and store data in database*
- *generate PDF*
#projects