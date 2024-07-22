 vfu                                                           vvv   vfu  <u>Method 1</u> : After sending the item in the cart, try to *change it's price* in the POST request.

<u>Method 2</u> : If Website Doesn't allow **Negative Total Amount**, Add some *New Items* so that price remains +ve but is below the actual cost of the item.

<u>Method 3</u> : If you regenerate the same/new *Coupon Codes after every newsletter signup*, change your **account Email ID** & try the same coupon code again!

<u>Method 4</u> : Try [[Integer Overflow]] by adding items to the cart!

<u>Method 5</u> : Use **Path Truncation** (Email ID wala)

<u>Method 6</u> : **Weak isolation on dual-use endpoint**
-   Only remove one parameter at a time to ensure all relevant code paths are reached.
-   Try deleting the name of the parameter as well as the value. The server will typically handle both cases differently.
-   Follow multi-stage processes through to completion. Sometimes tampering with a parameter in one step will have an effect on another step further along in the workflow.

<u>Method 7</u> : *Add the Item* to the Cart & then directly go to the **transcation confirmed page**!

<u>Method 8</u> : **Drop Requests** to see what happens, maybe you get access to higher priviliges!

