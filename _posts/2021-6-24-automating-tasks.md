---
layout: post
title: Tutorial 8. Automating tasks in the shell
---
### Learning objectives
* Utilize bash to run `for` loops and `if` conditions
* Edit text with sed


## Introduction
Bash can automate simple tasks in the command line, some of the most commonly used tools here are: **for** loops, **if** statements, and text editing with **sed** commands.

## Try and Learn
Iterating a task in bash can be done with a bash **for** loop. The variable `i` loops through the specified range, here from 1 to 10. Bash commands between the `do` and `done` can utilize the variable i. For illustration we print here the value of the variable i with the command `echo`:
{% highlight plaintext %}
for i in {1..10}
do
	echo $i
done
{% endhighlight %}

Simple logic can be implemented in bash with the **if** statement. If the condition is true, the commands between `then` and `else` are executed. If the condition is wrong, the commands between `else` and `fi` are executed. In this illustration case the condition checked in the if statement is the existence of a `test.txt` file.
{% highlight plaintext %}
if [[ -f test.txt ]]; then
echo "file exists"
else
echo "file does not exist"
fi
{% endhighlight %}

A commonly encountered text editing task is the replacement of specific works in a text file. This can be one with the bash command **sed**. The following `sed` command replaces any occurrence of `oldvalue` in the file `test.txt` with the `newvalue`.

{% highlight plaintext %}
sed -i -e "s/oldvalue/newvalue/g" test.txt
{% endhighlight %}
