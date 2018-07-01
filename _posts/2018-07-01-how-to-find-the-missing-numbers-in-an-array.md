---
layout: post
title: "How to find the missing numbers in an array"
---
If you just want to know the solution please jump to the end of the post where I post the full solution.

Let me give you a bit of a background story as to how I encountered this particular problem. Although I don't think it's all that uncommon so I'm sure there's lots of other ways to encounter it.

I like to listen to this radio show called **A state of trance** it's hosted by **Armin van Buuren** and they play mostly trance music. I don't really like to listen to the most recent episodes because there's way too much talking but I do like to listen to the old episodes which contain mostly music. This is the reason I went to [archived episodes](https://archive.org/details/Armin_van_Buuren_A_State_of_Trance_001-499) to pick an old episode to listen to.

To my surprise there were only 498 files listed even though the first file was called `filename_000` and the last file was called `filename_499`. Something which would make you expect 500 files instead of 498. I tried to find the missing episodes but I soon gave up because I knew this would be a tedious task.

With this problem in the back of my head I started looking at the rest of the page and I soon found a `M3U` file. A `M3U` file is basically a console file with on each line the location to a media file. In my case all the locations to every state of trance episode in the archive. It looks like this:

```m3u
http://archive.org/download/Armin_van_Buuren_A_State_of_Trance_001-499/Armin_van_Buuren_A_State_of_Trance_Episode_000.mp3
http://archive.org/download/Armin_van_Buuren_A_State_of_Trance_001-499/Armin_van_Buuren_A_State_of_Trance_Episode_001.mp3
http://archive.org/download/Armin_van_Buuren_A_State_of_Trance_001-499/Armin_van_Buuren_A_State_of_Trance_Episode_002.mp3
...
```

The first thing I had to do was extract all the numbers from this file. Something which should be fairly trivial but as you will see later I still ran into some problems. I went to [regexr.com](https://regexr.com/), pasted in the contents of the file and came up with the following pattern:

```regexp
/(\d+)\.mp3/g
```

*Capture every digit followed by `.mp3`.*

[regexr.com](https://regexr.com/) has a handy list feature which allowed me to create a comma separated list of numbers by writing the following pattern:

```regexp
$1, 
```

*Output the first capture group followed by a comma.*

After which we end up with:

```text
000, 001, 002, ...
```

[Try it yourself](https://regexr.com/3rqnr)

Alright with that out of the way we can finally start writing some code to figure out which numbers are missing. I started with something like this:

```javascript
const numbers = [000, 001, 002, 003, 004, 005, 006, 007, 008, 009, 010, 011, 012, 013, 015, 016, 017, 019];

for (let numberIndex = 0; numberIndex < numbers.length; numberIndex++) {
    const number = numbers[numberIndex];

    if (number !== numberIndex) {
        console.log(numberIndex);
        break;
    }
}
```

*For this example I've used a smaller array of numbers and used different missing numbers.*

Now if you are coding in a good editor you'll probably already notice something is off but I was coding on [codepen.io](https://codepen.io) and the warning wasn't that clear. You should note that the missing numbers here are `14` and `18` and that's what I expected as my output. But what I got instead was:

```console
10
```

This took me by surprise so I decided to also log the `number` to the console to find out why they were not equal. Now my code looks like this:

```javascript
const numbers = [000, 001, 002, 003, 004, 005, 006, 007, 008, 009, 010, 011, 012, 013, 015, 016, 017, 019];

for (let numberIndex = 0; numberIndex < numbers.length; numberIndex++) {
    const number = numbers[numberIndex];

    if (number !== numberIndex) {
        console.log(number);
        console.log(numberIndex);
        break;
    }
}
```

After which the console outputs:

```console
8
10
```

I'm asking myself why `010` equals `8` and know something fishy is up with the notation of my numbers. I google **javascript 010 equals 8** and find out that by adding a `0` in front, the number is parsed as base 8 (octal). I've got 2 options now. I can turn all the numbers into strings and parse them as decimals or I can remove the leading `0`'s. I pick the latter.

I return to my regex pattern and after some struggles this is what I come up with:

```regexp
/(0|[1-9]\d*)\.mp3/g
```

*A `0` or a digit between `1` and `9` followed by any number of  digits and `.mp3`.*

This removed all the leading `0`'s and turned my numbers into this:

```text
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 15, 16, 17, 19, 
```

[Try it yourself](https://regexr.com/3rqom)

Okay let's try the following code:

```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 15, 16, 17, 19];

for (let numberIndex = 0; numberIndex < numbers.length; numberIndex++) {
    const number = numbers[numberIndex];

    if (number !== numberIndex) {
        console.log(numberIndex);
        break;
    }
}
```

Success! It outputs:

```console
14
```

I open the `M3U` file in my editor and jump to line `14` (or the equivalent line in my file) and low and behold it shows the number `015`. But I think to myself I've only found the first number how do I find the second number. This is a piece of cake! Just add 1 to the numberIndex for every number you're missing.

```javascript
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 15, 16, 17, 19];
let addition = 0;

for (let numberIndex = 0; numberIndex < numbers.length; numberIndex++) {
    const number = numbers[numberIndex];

    if (number !== (numberIndex + addition)) {
        console.log(numberIndex + addition);
        addition++;
    }
}
```

Output:

```console
14
18
```

Alright nice that's exactly what I wanted. I've now found the 2 missing episode numbers. I guess I could stop here and everything would be right in the world.

But something was telling me that I couldn't stop there because what if there was more than 1 missing number in a row or if the order was wrong? Let's play the devil and see how my code performs.

```javascript
const numbers = [0, 1, 2, 3, 20, 8, 9, 10, 11, 12, 13, 17, 18, 19];
let addition = 0;

for (let numberIndex = 0; numberIndex < numbers.length; numberIndex++) {
    const number = numbers[numberIndex];

    if (number !== (numberIndex + addition)) {
        console.log(numberIndex);
        addition++;
    }
}
```

Output:

```console
4
5
6
11
12
13
```

It kind of works but it soon starts falling apart.

The sorting part is trivial just call the sort method on the array like so:

```javascript
numbers.sort((a, b) => a - b);
```

Which turns the array into:

```javascript
[0, 1, 2, 3, 8, 9, 10, 11, 12, 13, 17, 18, 19, 20]
```

Don't try to call it without your own comparison function because that will sort the numbers alphabetically and turn your array into this:

```javascript
[0, 1, 10, 11, 12, 13, 17, 18, 19, 2, 20, 3, 8, 9]
```

Something you probably don't want.

Okay let's focus our attention on finding the missing numbers again. This is what our code looks like now:

```javascript
const numbers = [0, 1, 2, 3, 20, 8, 9, 10, 11, 12, 13, 17, 18, 19];
let addition = 0;

numbers.sort((a, b) => a - b);

for (let numberIndex = 0; numberIndex < numbers.length; numberIndex++) {
    const number = numbers[numberIndex];

    if (number !== (numberIndex + addition)) {
        console.log(numberIndex);
        addition++;
    }
}
```

My first thought was to figure out the next number in the array and adding the difference between the next number and my current number to the addition but this seemed to be getting messy. I already wasn't very happy by adding an addition to my numberIndex before comparing it and I would also have to check if we were already at the end of the array.

I realized I didn't really have to check the first number because it doesn't have any numbers before it. I also realized that I could compare the previous number to the current number to figure out if a number was missing by subtracting them from each other. After all if the number was bigger than 1 that would mean we had a missing number. And last but not least it would avoid me from using the index to perform checks, something which was nagging me too.

A few alterations later my code looked like the following:

```javascript
const numbers = [0, 1, 2, 3, 20, 8, 9, 10, 11, 12, 13, 17, 18, 19];
let addition = 0;

numbers.sort((a, b) => a - b);

for (let numberIndex = 1; numberIndex < numbers.length; numberIndex++) {
    let currentNumber = numbers[numberIndex];
    let previousNumber = numbers[numberIndex - 1];
    let difference = currentNumber - previousNumber;

    if (difference !== 1) {
        for (let addition = 1; addition < difference; addition++) {
            console.log(previousNumber + addition);
        }
    }
}
```

Output:

```console
4
5
6
7
14
15
16
```

Finally I can sleep in peace!

[Original code pen for my answer.](https://codepen.io/321jurgen/pen/MXLobK)

What if you would have to find the missing numbers in base 8 (octal)?