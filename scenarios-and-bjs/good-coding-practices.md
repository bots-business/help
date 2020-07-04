# Good coding practices

## Use good names

Do you have children.

Two boys and one girl. You do not call them b1, b2 ang g1, are you not ?

So why do you call your variables like that?

**Bad examples:**

* x1
* y2
* d3

**Good:**

* currentLimit
* maxCount
* userName

## Use simular names!

![](../.gitbook/assets/image%20%2856%29.png)

## Use JSON property

**Bad code:**

![](../.gitbook/assets/image%20%2857%29.png)

\*\*\*\*

**Good code:**

![](../.gitbook/assets/image%20%2862%29.png)

## Do not use many methods of getProperty

**Bad code:**

![](../.gitbook/assets/image%20%2855%29.png)

{% hint style="warning" %}
Each getProperty method spent above 10-100 ms for execution. So if you have 10 getProperty methods bot can spent above 1 sec for execution! It is slowly
{% endhint %}

**Use JSON type:**

![](../.gitbook/assets/image%20%2863%29.png)

## Use functions!

Do not repeat youself!

Bad code:![](https://telegra.ph/file/31bc228cf1f6f793ff034.png)

![](../.gitbook/assets/image%20%2861%29.png)



Good code:![](https://telegra.ph/file/aa3021f92fbd73e5c9ede.png)

![](../.gitbook/assets/image%20%2859%29.png)

