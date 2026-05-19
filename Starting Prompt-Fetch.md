Create a .md file that will serve as the prompt for this project. 

I would like to build an app that will help me create high quality Poshmark listings using the bulk upload template provided on this spreadsheet, particularly the 'Listing Details' tab. 

1) The app will first read a folder of photos taken of various pieces of clothing. The app will group photos that belong to the same unique piece of clothing and label them in a series, such as 'clothing_upload_<date today>_001_front.' Possible suffixes include 'front', 'back', 'label', 'closeup', 'callout'.

2) After grouping the photos together, the app should use the 'front' image in each folder to search the web for (a) a matching stock photo (image direct URL), (b) the product brand and name (e.g., Free People Carpe Diem Shorts), (c) the MSRP (original retail price in USD), (d) the recommended sale price based on prices of similar products sold in the market. Based on its findings, the app should then generate a Poshmark listing description for the item, complete with hashtags, keywords, measurement placeholders, and the original retail price. The app should then publish and store all these details on a user-friendly table. 

3) Once done, the app should populate all these details on the 'bulk_upload_template_photo_url' file, 'Listing Details' tab, starting at row 3. Each unique clothing piece should be entered into a new row.
- SKU: Automatically generate a SKU ID using the first letter of each word of the brand name followed by a number up to 4 digits, in sequence. For example, Free People Carpe Diem Shorts would be FP0001 if it were the first Free People product listed.
- Title: This should contain the Product Brand and Name. If one of the photos in the product folder has an attached paper tag that shows that it is a brand new item, the Title should start with 'NWT' to indicate that the product is New With Tags.
- Description: This should contain the Poshmark listing description generated earlier.
- Department: Select the appropriate department from the dropdown. 
- Category: Select the appropriate category based on what maps to the department on the 'Category' tab.
- Quantity: Default to 1. 
- Size: Read the 'label' photo of the clothing item and select from the possible options on the dropdown.
- Condition: Unless the item is NWT, evaluate the photo and select the appropriate condition from the possible options.
- Brand: Input the brand of the product.
- Color1: Select one of the colors from the dropdown based on the color(s) on the product.
- Color2: If there is more than one color on the product, select the second color from the dropdown.
- Original Price: Input the MSRP researched earlier.
- Listing Price: Input the recommended sale price from earlier.
- Availability: Default to 'Not for Sale'.
- Primary image: Input the stock image URL discovered earlier. 

4) Once the file is complete, save it as a .xlsx file in a folder with today's date. Allow the user to preview the file. 