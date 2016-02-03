# This is still work in progress

# My C# coding styles guidelines

This coding guideline should help you write cleaner and more expressive C# code. It's my own (and i really emphasis own here) pespective on what makes code better and easier to read.
However, the most important thing about coding guidelines is that you should have one. Even if you work alone and not part of a team, you should strive to make your code uniform and respect some coding guidelines.

1.  [Generic Advices](#generic_advices)
2.  [Language constructs](#language_constructs)
3.  [Code comments](#language_constructs)
4.  [Indentation levels](#indentation)
5.  [Line length](#linelength)
6.  [Line length](#linebreaking)
7.  [Class member structure](#memberstructure)

#### Generic advices

- use long descriptive names. Don't shorten names needlessly. You have Intellisense, you're not typing all characters eachtime.
- don't use abbreviation unless it's a very well know term.
- keep in mind the quality of the variable names usually makes the difference between easely understandable code and incomprehesible one.
- English should be the only language used when writing both code and comments.
- don't use _ (or any other character for that matter) to abbreviate variables based on their location. When reading code the variable location is not important and "Find all references" will "fix" this immediately.
- succint code is overall better as long as you don't overdo it.

#### Language constructs

- Constants
Constants should be declared all in uppercase :
`public int FIXED_WEIGTH = 44;`
Uppecase makes the constant values stand out when reading the code and it makes it very clear that particular value won't be changed anywhere in the code.

- var keyword.
Don't use the var keyboard (unless you write a LINQ query which returns a anonymous object because var is the only thing you can use in this situation). Consider the following piece of code :
`var stuff = GetMyStuff();`
This code can't be understood at first glance. You're writing code in a strong typed language and the actualy type is not visible there.

- Lamda expressions (versus anonymous functions)
Personally i consider anonymous methods to be obsolette and encourage anyone to use lamdas everywhere. This code
`all.Find(delegate(e){ return e.IsAvailable; })`
has to much ceremony over this version
`all.Find(examples.IsAvailable);`
With lambda expression just keep in mind that you should use a descriptive names to the lamda parameters IF you using more than one parameter (so don't use one char variables). If you have a single parameter it's fine to use a single char because you should infer the type from the collection's name.

- LINQ queries Personally i'm not a big fan of the LINQ query syntax
`IEnumerable <int>numQuery1 = from num in numbers
where num % 2 == 0
orderby num select num;</int>`
and i do prefer to use method syntax
`IEnumerable <int>numQuery2 = numbers.Where(num => num % 2 == 0).OrderBy(n => n);</int>`
The reason is both code readability ( i find it a lot easier to read the method names rather than scan the code for the SQL like keywords) and also because you're not actually writing SQL there : just the fact that 'query' has to start with from instead of select (i know the reason for this, it's mostly related to providing intelisense) but it's still something that somehow resembles SQL instead of the real deal.

-

#### Code comments

I'm a firm believer of commenting code JUST when it needs comments. I see no value of littering the code with useless, obvious comments.
Comments should be concise, to the point and most importantly they should explain the WHY is something done in a certain way.

#### Indentation levels

I use tabs for indentation because tabs are condifurable while spaces aren't. The indent size i use is 4 (i find 2 too narrow, specially when you read code on a +24 inch monitor and 8 too wasteful).
`Is Visual Studio open Tools/Options/Text Editor/C# to configure the indentation.
`

#### Line length

This is usually a touchy topic. My advice is not to have a fixed length but "chop" the line where it makes sense. I personally find that having a fixed line length makes it harder to read the code in different scenarios (for instance....having the line length set to 80 chars makes the code look bad on a 24' 1080p monitor because the entire right side of the editor will be unused. Likewise having a 140 line length will be annoying when trying to code on a 14' laptop because you'll have to scroll horizontally all the time).

#### Line breaking

First thing first. Brackets *always* go on separate lines :
`public void DoStuff()
{
}
`Never
`public void DoStuff(){
}`

#### Member structure

Inside a class my structure is always the same ( from top to bottom):
- constants and readonly fields;
- private and public variables.
- delegates/events
- constrctors
- properties
- methods