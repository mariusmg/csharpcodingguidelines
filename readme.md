## C# coding guideline

This succint coding guideline should help you write cleaner and more expressive C# code. It's my own (and i really emphasis own here) perspective on what makes code better and easier to read.  
However, the most important thing about coding guidelines is that you should have one. Even if you work alone and not part of a team, you should strive to make your code uniform and respect some coding guidelines.  

1.  Generic advices
2.  Language constructs
3.  Code comments
4.  Formatting
5.  Class member structure

#### Generic advices :

- use long descriptive names. Don't shorten names needlessly. You have Intellisense, you're not typing all characters each time.  
- don't use abbreviation unless it's a very well know term.  
- keep in mind the quality of the variable names usually makes the difference between easily understandable code and incomprehensible one.  
- English should be the only language used when writing both code and comments.  
- don't use _ (or any other character for that matter) to abbreviate variables based on their location. When reading code the variable location is not important and "Find all references" gives this information when needed.  
- succinct code is overall better as long as you don't overdo it (remeber that ultimate goal is always code which is as easy as possible to grok ).  
- declare variables as close as possible to actual usage.  
- a function / method should do 1 thing and 1 thing only. Split the implementation over multiple smaller methods.
- always remove dead and commented code. There's no point in keeping that (you always have source control to get back to old code ).
- strive to make code as unambiguous as possible. Ambiguity ALWAYS creates problems.
- 

#### Language constructs

###### Constants

Constants should be declared all in uppercase :  
    
    public int FIXED_WEIGTH = 44;  
    
Uppercase makes the constant values stand out when reading the code and it makes it very clear that particular value won't be changed anywhere in the code.  

###### var keyword.

Don't use the var keyboard (unless you write a LINQ query which returns a anonymous object because var is the only thing you can use in this situation). Consider the following piece of code :  
    
    var stuff = GetMyStuff();
This code can't be understood at first glance. You're writing code in a strong typed language and the actual type is not visible there.  

###### Lambda expressions (versus anonymous functions)  
Personally i consider anonymous methods to be obsolete and encourage anyone to use lambdas everywhere. This code  

    all.Find(delegate(e){ return e.IsAvailable; })

has too much ceremony over this version :
        
        all.Find(examples.IsAvailable);  
        
With lambda expression just keep in mind that you should use a descriptive names to the lambda parameters IF you using more than one parameter (so don't use one char variables). If you have a single parameter it's fine to use a single char because you should infer the type from the collection's name.  

###### LINQ queries

Personally i'm not a big fan of the LINQ query syntax  
    
    IEnumerable <int>numQuery1 = from num in numbers where num % 2 == 0 orderby num select num;
    
and i do prefer to use method syntax  

    IEnumerable <int>numQuery2 = numbers.Where(num => num % 2 == 0).OrderBy(n => n);

The reason is both code readability ( i find it a lot easier to read the method names rather than scan the code for the SQL like keywords) and also because you're not actually writing SQL there : just the fact that 'query' has to start with from instead of select (i know the reason for this, it's mostly related to providing Intellisense) but it's still something that somehow resembles SQL instead of the real deal.  

###### String interpolation FTW
C#6 added string interpolation . Use it because its a lot cleaner compared with string.Format()
    
    log.Write("This stuff has value {firstName} and it ends with {age}); 
    
    versus
    
    log.Write(string.Format("This stuff has value {0}, and it ends with {1}, firstName, age));
    
###### Use enums to pass statuses instead of strings
    Bad:
    
    public void DoMonkeyStuff(string status)
    {
        if(status == "first")
        {
        }
        
        if(status == "second")
        {
        }
    }
    
    Good :

    public void DoMonkeyStuff(MonkeyStatus status)
    {
        if(status == MonkeyStatus.First)
        {
        }
        
        if(status == MonkeyStatus.Second)
        {
        }
    }
    
    You're writing code in a static typed language, so just let the compiler cacth as many stuff as possible at compile time (instead of runtime).

######   Exceptions 

 - don't rethrow exceptions (unless you really want to reset the stacktrace). So use throw instead of throw ex.

 - don't catch exceptions unless you can deal with them (and no, just logging it doesn't mean you deal with it). The corollary to this is that you don't catch exceptions, log them and rethrow them. That justs messed up the log file without accomplishing anything.
 
- feel free to create your own app specific exceptions ( for instance exceptions specific to different domain models) and use those to "short circuit" the entire call stack. It's both simpler and more clear to throw a exception instead of returning a result whose values needs to be checked on each method invocation.


###### Write proper ifs
Don't write code like this :

    if(condition)
    {
          fuckzillion lines of code
    }
    else
    {
            return stuff;
    }

Putting the else part first makes code more readable :

    if(condition == false)
    {
        return stuff;
    }

    //continue here


#### Static vs instance
Don't overdo it with with static methods. Understand the tradeoffs of static methods and always make sure you are not modifying shared state from static methods (be especially careful with this in the context of web apps). 

#### Deal with nulls 
Use the null coalescing and null traversal operator to deal with null objects / graph of objects with null properties.
Also you can really decrease the number of NullReferenceExceptions in your code by using a C# implementation of Maybe monad (look it up on GitHub, you'll find plenty).  The idea is to never return null from your own methods but always return a instance of the Maybe type.  


#### Code comments

Comment code JUST when it needs comments. I see no value of littering the code with useless, obvious comments (green noise). Comments should be concise, to the point and most importantly they should explain the WHY is something done in a certain way.


#### Formatting


##### Tabs. Always tabs

I use tabs for indentation because tabs are configurable while spaces aren't. The tab + indent size i use is 3 because it strikes a nice balance between usability and compactness. In Visual Studio open Tools/Options/Text Editor/C# to configure the indentation.

##### Line length

This is usually a touchy topic. My advice is not to have a fixed length but "chop" the line where it makes sense. I personally find that having a fixed line length makes it harder to read the code in different scenarios (for instance....having the line length set to 80 chars makes the code look bad on a 24' 1080p monitor because the entire right side of the editor will be unused. Likewise having a 140 line length will be annoying when trying to code on a 14' laptop because you'll have to scroll horizontally all the time).  


#####  Braces

Braces *always* go on separate lines (and braces are NOT optional) :

    public void DoStuff()  
    {  
    }

Don't do :  

    public void DoStuff(){  
    }

And do not :

    if(something)
        Blah();
        
One nice thing about this rule is that, if you respect it, it's much easier to jump between C# and Javascript/TypeScript code in web projects. 

#####  Empty lines ?

Leave 1 empty line between different members, don't format code like this :

        public string Age { get;set;}
        public string Name { get;set;}

It's compact, but writing it like this is easier to read :
 
        public string Age
        {
            get;
            set;
        }

        public string Name
        {
            get;
            set;
        }

Inside a method , keep the same rule :

 This : 
    
       public void DoThings()
       {
            int age = 23;
            
            bool response = InvokeThisAndThat();
            
            if(response)
            {
                 Console.WriteLine("Response is correct");        
            }
       }
   
   No this :
   
       public void DoThings()
       {
            int age = 23;
            bool response = InvokeThisAndThat();
            if(response){  Console.WriteLine("Response is correct");  }
       }
   
Leave the code some room to breathe , don't mush everything together.   
   

#### Member structure

Try to have a fixed memeber structure inside a class. Overall it just makes code easier to read/follow. My structure is always the same ( from top to bottom):  
- constants and readonly fields;  
- private and public variables.  
- delegates/events  
- constructors  
- properties  
- methods (in order  :  public , protected and private ).