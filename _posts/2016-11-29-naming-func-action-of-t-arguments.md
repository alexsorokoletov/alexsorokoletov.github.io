---
layout: post
title: Named parameters in Func<T>/Action<T> type in C#?
comments: True
---
If you ever wondered, why Visual Studio (and Visual Studio for Mac) suggest you to name your lambda arguments with names like arg1, arg2 ... argN and so on...

Here is a common situation where somewhere in your library or helper class you have a method taking in `Func<T>` or `Action<T>`.
![Definition of your method using Func/Action classes](/assets/naming_func_of_t_declaration.png)
and then when you want to use this method VS suggests you following names: arg1 and arg2.
Like there is nothing better than that :)
![Definition of your method using Func/Action classes](/assets/naming_func_of_t_default_behavior.png)

There is a cheap and easy **solution for that**.

- Define your own delegate with same signature as `Func<T>` or `Func<T1,T2,T3>` you are using already.
- Specify intended names for these arguments (in my case arg1 could be better described as service, where arg2 is actually a token).
- In original method, replace Func<T> with this new delegate.

![Defining custom delegate with correct names and using it in the original method](/assets/naming_func_of_t_custom_named_delegate.png)
After these changes when you are going to use the method, you will see intellisense suggesting correct names and not generic ones.
![Now you get better names from VS as a suggestion](/assets/naming_func_of_t_improved_names.png)

<!--more-->

#### Pros and Cons
*Pros*

- You, your team and other people using the API that you expose (whether internally in the app or in the library or NuGet) will see better names and probably will use these names.
- So code will be easier to read and maintain.
- If you decide to extend the delegate, you will have to do it in one place only.
- It's fun to have smart code suggestions!

*Cons*

- Bit of extra work and extra code


#### Bonus
You can have optional `Func<T>` arguments!
In the example below you can omit `token` argument completely.

![Optional arguments in Func of T csharp](/assets/naming_func_t_bonus.png)


{% include social.html %}
