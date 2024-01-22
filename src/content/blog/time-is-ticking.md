---
title: Time is ticking...(Part 1)
author: SmolPeaCat
pubDatetime: 2024-01-19T23:05:45
featured: true
draft: false
tags:
  - blog
  - tech
  - web-dev
description: In this Article I talk about my billion dollar idea. for free !!!
---

## Table of content

## What I learned today

Hey this is SmolPeaCat back with another blog ! youhouu

Today I'm going to talk about..you guessed it Astro !!

Since I finished my exams(finally), I now have two weeks of free time..naturally coding was is the first thing that I'm going to do !

But what did I do ?

Well I had this idea of maeking a web app that would count down the days that i have left..until the last day of my coding expedition.

The 5th february...

So my plan was simple and I divided the app into a few easy steps :

### Step 1 : Create some a way to show how many days are left

**Here is my beatiful web app !**

![image](@assets/images/count-down_site.png)

Splendid right ? haha yeah In know, I'm quiote proud of it..But you have not seen everything !!

You are probably asking yourself _How did you make something so perfect ???_

It is pretty simple **FIRST**

We need to use npm, yarn, etc to create an [Astro](https://astro.build) app :D

You simply have to type (I'm using npm btw)

```bash
npm create astro@latest
```

and you will greeted by this awesome fella named Houston in your terminal !

![image](@assets/images/houston.png)

Then he will simply ask you some question..

**Where should we create your new project ?**

You can choose whatever it does not matter

**How would you like to start your new project ?**

ff..You are not thinking about choosing the sample files right ? we do things the **HARD** way here so choose "Empty"

**Install dependencies ?**

YES

**Do you plan to write TypeScript ?**

I don't know the language so no haha

**Init the git repo ?**

Is that even a question ??

...

If you don't want to use **Astro** it's fine..you can use whatever you want as long as you can have a javascript script running one your website ( should not be that difficult for you right ? No since you are an avid follower of my blog and you probably saw my [course](https://www.youtube.com/watch?v=cvh0nX08nRw) on HTML CSS and JavaScript for only 999$ !!!)

But be aware..you'll be missing Houston wishing you good luck.

![houston](@assets/images/houston2.png)

Okay now we have to write a pretty simple javascript function that will take the current Date and given a specific date, it will ouput the number of days between today and.. well the date

Let's go step by step..

-> **I'm going to assume that you use Astro from now on**

in your `src/` directory create a new directory and a js file.

you will have directory structure like this

```bash
/
├── src/
    ├── pages/
    |    └── index.astro
    ├── scripts/
        └── main.js
```

Now what do we put inside of the file ?

well let's break down what we want to do..

1. get the date of today
2. enter the desired date
3. Using those two calculating the difference between the two
4. Output the number of days..

Since we plan to create a function that we will be able to call in the [frontmatter](https://docs.astro.build/en/core-concepts/astro-syntax/#jsx-like-expressions) of the `.astro` file
we simply need to create a function that needs an input (The date)

I'm going to use the date format **DD-MM-YYYY**, because I'm used to unsing this one.
If you want an other format the change won't be too difficult to do

let's start by defining the function (give it the "export" so that we can use it elsewhere), then we will create a variable to store the current date as an object

more on how `Date()` works [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date).

```javascript
export function calculateDaysDifference(targetDate) {
  const currentDateObj = new Date();
}
```

since we are inputing the date wanted and that it will be a `string` we will need to create an other Date object with the argument.

```javascript
export function calculateDaysDifference(targetDate) {
  // Rest of the code

  const parts = targetDate.split("-");
  const targetDay = parseInt(parts[0], 10);
  const targetMonth = parseInt(parts[1], 10) - 1;
  const targetYear = parseInt(parts[2], 10);

  const targetDateObj = new Date(targetYear, targetMonth, targetDay);
}
```

Here we simply splitted the given arg so that we will have an array with the day, month and Year each as its own string.

Then you can see that we used `parseInt`, it is used to convert the string to a decimal number

the number of `targetMonth` is decremented by one because in Javascript's `Date` objects are zero-indexed (ie., January is 0, February is 1, and so on)

More on `parseInt` [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

We finally create a Date object with the given date.

Now we need to find a way to do the difference between those two objects..is this possible ? hell yeah !

```javascript
export function calculateDaysDifference(targetDate) {
  // Rest of the code
  const timeDiff = targetDateObj - currentDateObj;

  const daysDIff = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));

  return daysDIff;
}
```

we **litteraly** do the difference between those two objects ! from this operation we will get the number of milliseconds between them and to convert those into days we use a simple mathematic operation.

and boom there we have it !! Our way of getting the difference between two date is created.

Good job :D

Final code looks like that

```javascript
export function calculateDaysDifference(targetDate) {
  const currentDateObj = new Date();

  const parts = targetDate.split("-");
  const targetDay = parseInt(parts[0], 10);
  const targetMonth = parseInt(parts[1], 10) - 1;
  const targetYear = parseInt(parts[2], 10);

  const targetDateObj = new Date(targetYear, targetMonth, targetDay);

  const timeDiff = targetDateObj - currentDateObj;

  const daysDIff = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));

  return daysDIff;
}
```

### Step 2 : How to use the script

The most difficult part is done ! Now to link this script to your `.astro` files is pretty simple and quick.

Go find your `index.astro` file we will modify the `h1` tag to display the days remainaing from your chosen date..

Now you may be wondering hmm how are we going to use the function now ?

Well just you wait I'm going to show you something **AMAZING**

put this at the before your `html` tag, below the `body`

```javascript
<script src="../scripts/main.js"></script>
```

and...? do you remember when I talked about the frontmatter ? It's now time to use it !

At the top of your file create a space like this

```javascript
---
// frontmatter stuff here
---
// Your code

```

now simply import the function and use it.

```javascript
---
import {calculateDaysDifference} from '../scripts/main'

const daysLeft = calculateDaysDifference("DD-MM-YYYY")
---
// Your code

```

Now we got a variable..great..You need to use it inside of the `h1` tage like that

```javascript
<h1>{daysLeft}</h1>
```

And _voilà_ !

You should be able to have to correct number of days remaining !! (No need to thank me haha)

...

Part 1 is now DONE.

There are still a LOT of things to do before my web-app is fully done..I still have to style my site to make it look better, improve my Astro skills to make everything better and add a backend for a big idea that I have...

but since it's getting late (currently 1:03 lmao), I'm going to bed !

See you !
