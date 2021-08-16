---
layout: post
comments: true
title: Tutorial 10. Multilingual in programming?
---

### Learning objectives
* Get some tips about learning programming as a beginner in computational chemistry.
* Topics:
    1. The first language to learn?
    1. From one to more?
    1. More isn't always better

## Introduction
Computational chemists often get the feeling that they need to work with multiple programming languages. For example:

* To use machine learning packages, you feel you need [Python](https://docs.python.org/3.7/).
* To develop new functionality in a quantum chemistry code, you find that you need to read and modify the historical code written in the 70s, which are unfortunately, [Fortan77](https://web.stanford.edu/class/me200c/tutorial_77/).
* To implement fancy parallel algorithm GPUs, you want to embrace [CUDA](https://developer.nvidia.com/cuda-toolkit), and inevitably, you also need [C++](https://www.cplusplus.com/).
* To implement an advanced graphical interface for your package, you may feel that [Tcl/Tk](https://www.tcl.tk/) is needed.
* To and make your computational database available to the public, you find it necessary to learn some language related to building a website.

I will share some of my opinions about learning multiple programming languages.

<span style="color:red">**Disclaimer**</span> some of the following contents are personal opinions and may not fit your learning needs.

### Which programming language should I learn first?
This may be a common question asked by beginners. I would answer this from two aspects.

1. Many times, it's not like we intentionally choose which language to start with, but we encounter our first programming language and need to use it. Some of us may have a required programming course to take, and the language used in that course automatically becomes our first language. My first language is C, which may seem very odd because nowadays, most colleges are teaching introductory level programming courses with Python or [Java](https://go.java/?intcmp=gojava-banner-java-com). However, C is a required course in my undergraduate curriculum. The first language may also be the language needed for your first research project.

1. If you are free to choose any language to start with and not restricted to curriculum requirements, I would recommend [Python](https://docs.python.org/3.7/) for its **versatility** and **easiness**.

1. However, the main point is that it doesn't matter too much which language you pick first. The concepts are similar among different languages, and algorithmic thinking does not depend on programing language. You should choose one language most needed for your research and try to be proficient with it.

### I have learned one programming language. How do I go for more?
If you ever ask yourself this question, please think about answering another question first:
* Why do I need that new language? Is it necessary?

In many cases, we can find workarounds without learning a new language. Nowadays, many interfaces are bridging different languages. For example, I can make web pages like this tutorial website you are currently reading with [Markdown](https://guides.github.com/features/mastering-markdown/), which I already use in Jupyter notebooks. Although this website is built with [Ruby](https://www.ruby-lang.org/en/) and rendered in [HTML](https://www.w3schools.com/html/), I don't need to code a single line of Ruby or HTML, because the [Jekyll](https://jekyllrb.com/) environment automatically converts my Markdown text into the webpage.

Similarly, many quantum chemistry programs originally written in C/[C++](https://www.cplusplus.com/)/Fortran are now interfaced with Python, and a Python coder can work with the Python interface without digging into C/[C++](https://www.cplusplus.com/)/Fortran.

These workarounds reduce the overhead for learning a new language and help you focus on your research rather than the technical hassles.

### I really have the need to learn a second language. Any tips and tricks?

I can share a few tips.

1. **Python to C/C++.** Nowadays, most students start with Python and later need C/C++ for some research projects. I recommended some intro-level Coursera courses for C/C++, and they found it helpful. One is the [Programming Fundamentals](https://www.coursera.org/learn/programming-fundamentals?action=enroll) by Duke University. The other is the [Object-Oriented Data Structures in C++](https://www.coursera.org/learn/cs-fundamentals-1) by UIUC. Both are short courses that can be completed within 21 hours. Another online course I always recommend is the [CS 106B Programming Abstractions](https://see.stanford.edu/Course/CS106B) offered by Stanford University, which is more advanced than the two courses mentioned above according to the feedbacks of my students.

1. **C to C++.** I can share my personal experience. My first programming language is C, and later learned C++ from [CS 106B Programming Abstractions](https://see.stanford.edu/Course/CS106B). For me, the most valuable part about learning C++ is to learn the Object Orient Programming idea, which is thoroughly discussed in CS106B.

1. **C to Python.** I guess this rarely happens nowadays since most colleges offer high-level languages in the intro-level programming course. If you have a similar experience as me (learning C before learning Python), then here is my experience. The biggest difference is that I can code in C without the Internet, but I cannot code in Python without the Internet. I need to search the [Python online documentation](https://docs.python.org/3.7/) about the usage of different packages all the time. Unlike C, where almost everything needs to be code from scratch, Python has so many packages we can use, which is both good and bad (for me). Good that to achieve the same goal, I can write shorter programs in Python. Bad that I cannot memorize the usage of any function anymore, and I look up the online documentation all the time. That is also how I learned Python: as a C/C++ programmer, I have never taken any formal training on Python, but I learn it while I code by checking the online documentation and examples.

1. **C/C++ to CUDA.** I will talk about that in another tutorial.

### Take home messages
**More Isn’t Always Better.** You don't need to learn more programming languages when there is an alternative solution with the language you know.
