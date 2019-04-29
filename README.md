Problem

Our client is an online marketplace, here is a sample of some of the products available on our site:
Product code  | Name                   | Price
----------------------------------------------------------
001           | Lavender heart         | £9.25
002           | Personalised cufflinks | £45.00
003           | Kids T-shirt           | £19.95


This is just an example of products, your system should be ready to accept any kind of product.
Our marketing team wants to offer promotions as an incentive for our customers to purchase these items.
If you spend over £60, then you get 10% off of your purchase. If you buy 2 or more lavender hearts then the price drops to £8.50.
Our check-out can scan items in any order, and because our promotions will change, it needs to be flexible regarding our promotional rules.
The interface to our checkout looks like this (shown in Ruby):
co = Checkout.new(promotional_rules)
co.scan(item)
co.scan(item)
price = co.total


Implement a checkout system that fulfills these requirements. Do this outside of any frameworks. We’re looking for candidates to demonstrate their knowledge of TDD.

Test data
---------
Basket: 001,002,003
Total price expected: £66.78

Basket: 001,003,001
Total price expected: £36.95

Basket: 001,002,001,003
Total price expected: £73.76

==========================

Implementation
----
Description of flow

* Here is the checkout system with flexible discounts built in Ruby - developed using TDD methodology.

* Based on the specification, I made a number of key implementation decisions:

  * Modelled domain with independent Checkout and Item objects to allow for list of items to be easily extended

  * Enabled flexible promotional rules through encapsulating the logic for discounts in Proc's that could be stored in a hash constant ```PROMOTIONAL_RULES``` and instantiated with the Checkout object ```Checkout.new(PROMOTIONAL_RULES)```
  * The area that I would be keen to develop further is how to further enhance flexibility of discounts - current version encapsulates the discount logic as an interchangeable validation check based on inputting total cost of items and full array of scanned items. To extend I would continue to explore how to encapsulate entire logic and return variable output as closure (e.g. item_total) in Proc or similar design pattern.


How to run in IRB
----------------

Run the below command in your terminal

```
git clone https://github.com/djadma/promotionApp.git
cd promotionApp
bundle install # for install rspec

irb
require './lib/checkout.rb'
#load up the promotions logic and instantiate checkout
  PROMO1 = Proc.new do |scanned_items, item_total|
    scanned_items.map{|x| x[1]}.select{|x| x == "Lavender heart"}.length > 1
  end

  PROMO2 = Proc.new do |item_total|
    item_total > 60
  end

  PROMOTIONAL_RULES = {"promo1":  PROMO1, "promo2": PROMO2}

  co = Checkout.new(PROMOTIONAL_RULES)

#instantiate the items you want to checkout
  item1 = Item.new "001", "Lavender heart", 9.25
  item2 = Item.new "002", "Personalised cufflinks", 45.00
  item3 = Item.new "003", "Kids T-Shirt", 19.95

#run commands
  co.scan(item1)
  co.scan(item2)
  co.scan(item3)
  price = co.total

```