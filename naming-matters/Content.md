## Good code is self-describing

Good code is code that is easy to understand and maintain yet do its job well. I feel naming conventions and good practices are severely under-used in software engineering and I attribute this in no little part to schools and online tutorials.

## You shouldn't need comments to understand what the code does

As per my other blog post, [comment should say why your code exists](https://danger-zone.viales.fr/Post/1/how-to-write-better-coding-comments) not what it does. Naming can take care of the that, and the code it self will describe "how" it does it. 

## This variable isn't actually YOURS !

`my`  ... please stop prefixing your symbols with `my`.  This prefix adds no value to your code, only to its volume, making it harder to read. It is a net negative on your code's readability and should be eradicated ! I believe this habit of using `my` as variable name prefix come from school and tutorials, that when demonstrating a coding principle with no real domain meaning use abstract semantically null names such as `myVariable` or `myMethod`.

## Good naming !

Some like to say that naming symbols is the hardest part of coding. I say it shouldn't be so ! 

A symbol's name should describe what the symbol's purpose is in its context. I agree, this sentence is basically canned wisdom served barely heated, so I'll elaborate.

### Variables

A variable's job is to contain data and be passed around. It serves as a label for a particular bit of data. Give it a name that describes what it contains on the local domain-level. If you don't know how to name a variable, then what it contains is unclear. If the person that writes the code doesn't know what variables contain, god help whoever is tasked with debugging their code !

```csharp
var allOrdersForRequestingUser = _dbContext.Orders.Where(order => order.UserId == requestionUserId);
var latestOrderForRequestingUser = allOrdersForRequestingUser.OrderBy(order => order.CreationDate).Last();
return latestOrderForRequestingUser;
```

In the above example, we have two variable with fairly long names by most people's standard. But all you need to understand the flow of the code is read the variable names from top to bottom:

1. A variable "contains" all the recorded "orders" for a given user (what "user" means here should have been made obvious by the context, method name and such)
2. A variable contains the last order, sorted by date. Notice the use of latest, not last, stating that it's the "most recent".

### Functions & methods

A function's job is to perform an operation. It should describe what operation it is expected to perform. If you can't give the function a clear name (notice I didn't write "concise") then you either don't truly know what it does or it does too many things. Break it down, refactor it. Understand your own code !

A function's name should describe the operation it performs and it goes in pair with the surrounding variable name. The core principle is the same as for variables and so I won't give an example. 

### Results

Just keep in mind, if it gives a result, the name should say what this result contains. Basically a `GetVariableName`.

### Side effect

Methods with side effects are often the hardest to name, because their behaviour is not pure it can difficult to formalize their actual work and the nesting of these methods can make the naming of the parent far more difficult. How to chose a name for these kind of situations is up to you.

## Break down your code

In example given above in the "variables" section, the two defined variable could have easily been made into a one-liner and in the context of this example it really wouldn't have changed much. But with longer queries it can become primordial to actually break down the code into more variables and methods as their name allows for plain ol' english to be inserted in the middle of the code, allowing it to self-comment. 

For this very reason, I like to avoid writing expressions directly inside `return` statements and prefer to use a "staging" variable. It will be compiled away and so does not have any impact on performance but it can provide crucial information regarding what is actually being returned. 

Here is a code sample that will (probably, I haven't checked) be compiled *identically* to the one above, but isn't as easy or as fast to read. 

```csharp
return _dbContext.Orders.Where(order => order.UserId == requestionUserId).OrderBy(order => order.CreationDate).Last();
```

Once again, this a very short example so the difference might not be immediately obvious but keep in mind, to understand what is happening here your only option is to actually read the code and with longer statements it could quickly turn into trying to read the future in a spaghetti carcass !

### Reality-check

In this context, I would really have used a sort of middle ground. Probably done the LINQ operations as a one-liner and staged the result in `latestOrderForRequestingUser`, assuming the first variable isn't needed. I over-did things for demonstration purposes.

## Note on Languages

### Not everyone speak Klingon

I'd like to have a word with you regarding languages... Symbols and comments should be written in English. I'm not saying that because "that's my language" and "it's best" (I'm a native French and English is dumb as hell). I say that because it's nearly universal, at least in the western world. If you write your code in English, even broken, I have a chance of understand the intention without the need to read the code itself. If you write it in Spanish, Polish, Slovak or Japanese I'll have a hard time not hating you.

### 'Cause you think everyone speaks English ?

Nope, but there are most likely more people that speak *some* English than your native tongue. Unless you speak one of the Chinese or Indian languages.

### Meh, nobody else will read this code

#### Stack Overflow doesn't speak Klingon !

Yes, and I assume you won't ever post a code sample to stack overflow ? *EVER* ?

#### No, I swear, only me will read this code

Suit yourself. Let's just hope you never try to make it open source because if someone tries to help, let's hope they speak your native tongue. But hey, it's alright. Keep your secret, we'll just refactor them away :-p

## This goes far beyond variables & functions

I have only explicitly talked about variables and functions in this post but the principle extends to all coding-related "things", all symbols, file names, server names or whatever as long as it is *purely technical*. Obviously a blog post's name or a product's name are subject to other constraints, particularly SEO and marketing so the above advice does not apply. 