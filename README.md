# Interview-Assignment-2024
Reconciling data sheets for orders, inventory and assets.
Instructions for the Take-Home Test:

Identify data issues between the exports in the different tabs
Outline how you determined these issues
Outline how you would reconcile and fix the issues

--------------------------------------------------------------------------------------------------------------

Solutions to Test DDA

The data issues found between the different tabs were:

•	There were 869 discrepancies in the entries for “Total Amount / MRC” when comparing the order sheet with the order product sheet.
•	There were 60 discrepancies in the entries for “Status” when comparing the order sheet with the asset sheet.
•	There were 511 discrepancies in the entries for “Product” when comparing the order sheet with the asset sheet.
•	There were 922 discrepancies in the entries for “MRC” when comparing the order sheet with the asset sheet.


Outline how you determined these issues. – These issues when found using the code written below 

 -- Check of Status values for Orders to Product orders using the OrderID identifier - 0
SELECT *
FROM orders
JOIN product_orders
ON orders.OrderId = product_orders.OrderId
WHERE orders.Status IS NOT product_orders.Status

-- Check of Total Amount / MRC values for Orders to Product orders using the OrderID identifier-869
SELECT * 
FROM orders
JOIN product_orders
ON orders.OrderId = product_orders.OrderId
WHERE orders.TotalAmount IS NOT product_orders.MRC

-- Check of Status values for assets to Product orders using the OrderProductID identifier - 60
SELECT *
FROM assets
JOIN product_orders
ON assets.OrderProductID = product_orders.OrderProductID
WHERE assets.Status IS NOT product_orders.Status

-- Check of Product values for assets to Product orders using the OrderProductID identifier - 511
SELECT *
FROM assets
JOIN product_orders
ON assets.OrderProductID = product_orders.OrderProductID
WHERE assets.Product IS NOT product_orders.Product

-- Check of MRC for assets values to Product orders using the OrderProductID identifier - 922
SELECT *
FROM assets
JOIN product_orders
ON assets.OrderProductID = product_orders.OrderProductID
WHERE assets.MRC IS NOT product_orders.MRC



Outline how you would reconcile and fix the issues.
-----------------------  RECONCILITATION IS COMPLETED BY THE FOLLOWING    ---------------------- 

----- Updating the Total Amount / MRC values for product_orders using orders
UPDATE product_orders
SET MRC = (
	SELECT TotalAmount
	FROM orders
	WHERE product_orders.OrderId = orders.OrderId);

---- Updating the Status values of assets using updated product_orders
UPDATE assets	
SET Status = (
	SELECT Status
	FROM product_orders
	WHERE assets.OrderProductID = product_orders.OrderProductID); 

---- Updating the Product values of assets using updated product_orders
UPDATE assets	
SET Product = (
	SELECT Product
	FROM product_orders
	WHERE assets.OrderProductID = product_orders.OrderProductID); 

---- Updating the MRC values of assets using updated product_orders
UPDATE assets	
SET MRC = (
	SELECT MRC
	FROM product_orders
	WHERE assets.OrderProductID = product_orders.OrderProductID); 

After completing the updating of the sheets, a retest was run using the code outlined in answer 2. Using the COUNT(*) function I was able to verify that there were now 0 discrepancies between both sets of sheets : {Orders and Product_orders} and {Assets and Product_orders}.


