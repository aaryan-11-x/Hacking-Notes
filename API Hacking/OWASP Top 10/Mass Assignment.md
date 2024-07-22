# Meaning
Mass Assignment is like a scenario where you have a form to update your user profile on a website. Imagine this form has fields for your name, email, and admin status (whether you're an admin or not). You, as a regular user, shouldn't be able to change your admin status to "yes" and give yourself admin powers.

Now, the Mass Assignment vulnerability is when a website allows you to submit all these fields in a single request, and you can sneakily include the admin status field with a value of "yes." Even though you're not supposed to be an admin, the website mistakenly grants you admin privileges because it didn't properly check or filter what data you were allowed to update.

In simpler terms, Mass Assignment is like tricking a website into giving you more access or control than you should have by submitting certain data when you shouldn't be able to.

---
# Example
1) **Negative orders**
- When you order something, the order requests contains the `product_id`, `value`, `quantity`, etc.
- Change the quantity or any of the values to *negative* and check if it works
![[Pasted image 20230928200948.png]]


2) **Submit fields from client side**
- Try to submit some fields from your end in JSON and see if that changes something.
- If you're updating a value, use `PUT` method.

Ex: We want to return an order without the order actually being returned!
%% Check API requests with params as well %%

This is the API request: `/workshop/api/shop/orders/34`
![[Pasted image 20230928201720.png]]

- Add the `quantity` & `status` attributes on request side but *change* their values.
- Use `PUT` method as you're updating the values

![[Pasted image 20230928202153.png]]
- Change the status to `returned` & see the magic!

![[Pasted image 20230928202327.png]]
- Our balance increases to **$240**!



3) **Executing commands by updating attributes**
The `converse_params` attribute contains a part of a command, we can use this for ***RCE***.

%% You can't change `converse_params` in a image/video upload request %%
%% Not sure but this works on `PUT` request and not `POST` %%

![[Pasted image 20230928203740.png]]