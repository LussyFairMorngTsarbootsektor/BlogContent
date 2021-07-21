## Write comments, they say

Many articles and many teachers tell their readers and students to "comment their code", but often say very little of *why* we should comment code. For this reason, I often see comments that simply describe what the code does, which as it happens, is not only useless but can sometimes be actively counterproductive. 

## What are comments, a personal definition

> Bits of text within source code that are of no use to the compiler (generally), only for the developers as context, about the code itself.

## What makes a good comment

Let's cut to the chase: what *are* good comments ? Well, good comments are the ones providing information about the code that could not be obtained through context or naming. An example of this, written by a coworker, goes something like this:

```csharp
// Partner X has started forbidding calls with TLS version inferior to 1.2
tlsVersionManager.MinimumVersion = TLS.12
```

This comment line provides information that the code itself could not carry over easily, it provides context as to why this line of code exists in the first place, it is clear and concise. *This* is a useful comment. The code and the comment are complimentary.

- Reading the code you understand precisely what is done: Set the minimum version to TLS 1.2
- Reading the comment line you understand why it is done: We can't call partner X with a TLS version inferior to 1.2

## What makes a bad comment

### Bad, are descriptive comments

Comments that simply describe what the code does are useless. It should be obvious what your code does simply by reading it. Naming matters ! If you name a function `ExecuteTask` and add a comment that says `// Calls partner's API to retrieve customer information`, then you should have named it `GetCustomerInformationFromPartner` or something of this sort. Yes, this will give very long names, but you will know what the method is supposed to do simply by reading its name, in context, where it's actually used.

When you're writing the method itself, it doesn't matter much, but when it's called 73 times across millions of lines of code written and maintained by multiple developers (*you* count as multiple developers), having to read the comments just to get a glimpse of what the method does is a terrible waste of time and focus away from whatever task was at hand.

### Bad, are joke comments

For reason that should be obvious, jokes in comment just as in the code itself should be used very sparingly, if at all. You might be having a blast while writing that joke in a comment line, having just come up with that brand new super-cool coding play-on-words, but you won't laugh much when you are debugging the same code, don't understand any of it because all of your batch files are called `man.bat` and your variables are declared with `var iable = 69;`.

### Bad, are file headers

Comment file headers, a practice born in the olden days pre-2000 that should have died pre-2000. Writing a massive blob of text at the start of each and every one of the files stating your name, nickname, email and other contact information. It often also states the creation date, last edit date, and other such now useless data. 

Since the rise of subversion tools like GIT, these file headers have become entirely redundant. If you wish to know who created a file, just check the git history. You'll find the original commit which should contain the aforementioned data. 

File headers can be even worse: you can sometimes find the complete license text at the start of each file in a repository ! If anyone reading your source code is willing to look at the license, they will find it in the LICENSE.MD file. 

All this text bloats your file with no added value. Get rid of it !

## Special cases

There are cases where writing a "bad" comment line is acceptable.

### Optimization

When altering code for optimization purposes, it can often become harder to read and understand. In this context, it is acceptable to write a comment describing what is being done, but also make sure to include a short straight-to-the-point comment regarding *why* this code is written this way. Otherwise you take the risk of having an other developer undo your optimization for readability's sake !

### Technical restrictions

Very similar to optimization, this bad-comments-allowed arises when you run into a limitation of your tech stack and end up writing counter-intuitive code. It is to be treated the same way as an optimization. 

An example of this I can give is for the usage of the `dynamic` keyword in C# that was used in a library I co-wrote with a co-worker. We were implementing a generic domain-specific wrapper around an other API and were instantiating inherited constrained generic types at compile time. The compiler was instantiating objects using the parent class, to make it instantiate child types we had to declare the variable as `dynamic`. In this particular comment, we gave a short explanation of why the "bad" `dynamic` variable type was used and a link to the relevant stack overflow answer.

## Why you should care

Because code is read more often than it is written. Write a function today, it might be read hundreds of times and left untouched before the next great flood ! Take the extra minute to write this method well, so that you save a hundred times one minute when you revisit your own code later.

## Tips on writing better comments

- Again, keep in mind you're not alone. Write your code assuming someone with little priori knowledge of the specifics of  your project will work on it, and that could very well be you ! 
- Assume others know the language. Never explain the framework, there should already be hundreds of articles on this topic online (and if there isn't, what kind of obscure coboloesque language do you use???). Explaining what a Null Coalescing operator is every time you use one is sheer madness.
- Always ask yourself "Why am I writing this function in this particular way ?". If the answer wouldn't be obvious to a master of your tech stack that is new to your project, explain it.