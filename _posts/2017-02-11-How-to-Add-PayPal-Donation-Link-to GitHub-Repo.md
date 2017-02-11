---
layout: post
title:  "How to Add PayPal Donation Link to GitHub Repo"
date:   2017-02-11 17:38:20 
---


>"Most people never ask, that is just what separates sometimes the people who do things and the people who just dream about them"
- Steve Jobs


## Introduction 

Ever create software that other people love using? Ever wanted to ask for donations but did not know an easy way to do it?  Well, today I am going to show you how to simply add a PayPal donation button to your GitHub README.md in order to get people sending you donations! After all, it never hurts to just ask.

The first step here is to have a PayPal account and have your bank accounts linked in order to actually be able to receive money.  Here is a link to signup for [paypal][payPalPage] and here you can add your bank [accounts][addAccounts].

## Create a Button

In order to generate the code for the button you must head over to [this][createButton] page.  

Once on that page, select Donations from the drop down menu.  In the *Organization Name/Service* field, please type in the purpose for the donation, or the organization field.  In this case it would be your software that you created or your company name.  The *Donation ID* field is optional, here you may enter an ID in order to track where the donations come from. Next, chose the currency you would like to receive payments in ( I chose USD for US dollars).  Then, you may select a fixed amount that a donor may donate or leave it up to them.  To leave it up to them, select *Donors enter their contribution amount*.  After, select *Use secure merchant account ID* option to help keep your email safe.  Then scroll below and go click *Create Button*.  This generates the HTML that you will be using.  If you need more help with this part, check out this [link][donationTutorial].

## GitHub

Now that you have the HTML generated for your button, you are ready to add it to your GitHub repository.  Here is what the final markdown will look like:

```
[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=8U849S663ZGTN&lc=US&item_name=Edit%20Docs%20Amazon%20Echo%20Skill&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted)
```
The first part of the link `![Donate]` is what is displayed when the image cannot be found. 

In order to get the first link, within the first set of parentheses, head over to the webpage where the HTML code was generated for the button.  In the code, under the *Website* Tab, go to the third `<input>` tag.  Go to the `src` attribute of this tag, and copy everything within the quotes.  Here is where to find this:

![WebsitePage](/assets/PayPalTut/Website.png)

This link should start with `https://www.paypalobjects.com`.This goes right in between the parentheses  here: 
`[![Donate](https://www.paypalobjects.com...)]`.  This is what actually generates the image for the Donate Button.  

Next, head over to the generated button code, but click on the *Email* tab.  Copy the entire link displayed in that tab.  It should look like this:

![EmailTab](/assets/PayPalTut/Email.png)

Then add this right after the closing bracket in parentheses.  For example:
`[![Donate](https://www.paypalobjects.com...)](paypalEmaillink.com)`
This allows the user to send you money.

Save the README, commit and push it up to your repository! If you click on the donate button it should bring you to a page like this:

![PayPage](/assets/PayPalTut/PayPage.png)

## Conclusion

I hope this helps anyone who was struggling to get the donation button on their repo!  If you have any questions, feel free to reach out to me by email: antonio.cucciniello16@gmail.com or on twitter @antocucciniello.  If you would like to donate please donate here:

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=8U849S663ZGTN&lc=US&item_name=Edit%20Docs%20Amazon%20Echo%20Skill&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted)  

[payPalPage]: https://www.paypal.com/
[addAccounts]: https://www.paypal.com/us/selfhelp/article/how-do-i-link-a-bank-account-to-my-paypal-account-faq686
[createButton]: https://www.paypal.com/us/cgi-bin/?cmd=_web-tools&fli=true&fli=true
[donationTutorial]: https://www.paypal.com/webapps/mpp/get-started/donate-button

 

