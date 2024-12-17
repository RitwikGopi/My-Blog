---
layout: post
title: "Making sense of python GIL’s impact on threading"
share: true
image:
  thumbnail: "/assets/2024/python-gil.png"
excerpt: ""
date: 2024-12-17 18:50:00.000000000 +05:30
type: post
status: publish
categories:
  - Technical
tags:
  - python
  - GIL
  - multithreading
  - threading
  - parallelism
permalink: "/2024/12/17/1/"
---

# Making sense of python GIL’s impact on threading

I have been working with python for around a decade now. I knew [GIL](https://realpython.com/python-gil/) existed in python, but never really thought about it much since most of the time I was dealing with low load servers or simple scripts. Recently at work I started working on projects with a higher volume of requests and had to explore the performance optimisation aspects. During different topics that I had to explore, GIL was the one that caught my attention. I thought why not explore this with some practical example.

Before diving into its practical impacts, let’s briefly explain what GIL is: The Global Interpreter Lock (GIL) is a mechanism in Python that allows only one thread to execute Python bytecode at a time, even on multi-core systems.

![Source [learn python](https://realpython.com/python-gil/)](https://cdn-images-1.medium.com/max/2808/1*J0nGlF04UyrRAogXtXrM4w.png)

While this made perfect sense theoretically, I felt coming up with an example and demonstrating its effect on CPU bound vs IO bound workload could make it more clear from a practical point of view. (You can find why CPU bound loads and IO bound loads have different effect due to GIL here in this [video](https://www.youtube.com/watch?v=XVcRQ6T9RHo).)

## Setup

* We will be using a program which will check if a number is prime or not in a given list of numbers. 

* We will generate a very large list of numbers(10⁸ random numbers.)

* Then we will try different approach to check the numbers and measuring the time taken using [timeit](https://docs.python.org/3/library/timeit.html) module.

* You can find the entire code [here](https://github.com/RitwikGopi/python-GIL-test).

### **Approach 1 — Sequential processing**

In the approach 1 what we will do is we will simply loop over the list of numbers and check if each number is prime or not. This will be the base line for our performance check.

    def loop(numbers):
        result = {}
        for number in numbers:
            result[number] = is_prime(number)
        return result

**Result** 

`[loop]: Total time for 5 calls: 157.2251 seconds`

This will be our base line performance. And we will see what improvements we can make.

### Approach 2 — Multi threaded processing

This is probably a common approach some one will think of when we need performance improvement. Parallelise the processing using threading. To achieve this we will split the input list in to 4 equal parts and we will check if a numbers are prime using 4 threads. 

    def split_threaded(numbers):
        mid_point = len(numbers) // 4
        split_1 = numbers[:mid_point]
        split_2 = numbers[mid_point:mid_point * 2]
        split_3 = numbers[mid_point * 2:mid_point * 3]
        split_4 = numbers[mid_point * 3:]
        inputs = [split_1, split_2, split_3, split_4]
    
        with concurrent.futures.ThreadPoolExecutor(max_workers=4) as executor:
            futures = [executor.submit(loop, inp) for inp in inputs]
            for future in concurrent.futures.as_completed(futures):
                future.result()

**Result**

`[split_threaded]: Total time for 5 calls: 168.8397 seconds`

Surprisingly what we observe is an even worse performance than the sequential processing. The reason for this is 

* In CPU-bound workloads, Python’s GIL ensures that only one thread runs at a time, preventing true parallelism.

* Context switching between threads introduces overhead, which worsens performance.

This doesn’t mean we should never use threads. Will come to that in approach 4

### Approach 3 — Multiprocessing

So while the threading failed to make the performance improvements, does that mean we can not parallelise the processing? Not at all. This is where **multiprocessing** comes in to picture. Multiprocessing spawns a process instead of threads, which allows it to run independent of each other effectively negating the impact of GIL. Here also we will split the input to 4 and process in parallel but this time with the multiprocessing.

    def split_multiprocessing(numbers):
        mid_point = len(numbers) // 4
        split_1 = numbers[:mid_point]
        split_2 = numbers[mid_point:mid_point * 2]
        split_3 = numbers[mid_point * 2:mid_point * 3]
        split_4 = numbers[mid_point * 3:]
        inputs = [split_1, split_2, split_3, split_4]
        with multiprocessing.Pool(4) as pool:
            pool.map(loop, inputs)

**Result**

`[split_multiprocessing]: Total time for 5 calls: 70.2142 seconds`

Now we can see a clear difference in the performance. It has improved a lot.

### Approach 4 — Multithreading with HTTP call

We can see that multi processing is clearly able to achieve parallelism. But does that mean multi threading is not useful at all? Not at all. But there is a condition. If the workload is IO bound and waiting for IO operations, in such case we can use the threading for parallelisation. 

Here I will run the computation as a server.

    from flask import Flask, jsonify, request
    
    from main import loop
    
    app = Flask(__name__)
    
    
    @app.route("/loop", methods=["POST"])
    def perform():
        numbers = request.get_json()
        return loop(numbers)

Run this with gunicorn. ***Make sure to use workers as 4and threads as 1***

    .venv/bin/gunicorn -b 0.0.0.0:5000 --workers=4 --threads=1  --capture-output --access-logfile - server:app

And I will again split the input to 4 parts and make API calls to the /loop endpoint in threaded fashion. 

    def api(numbers):
        url = "http://0.0.0.0:5000/loop"
        headers = {
            'Content-Type': 'application/json'
        }
        return requests.request("POST", url, headers=headers, json=numbers)
    
    
    def split_threaded_api(numbers):
        mid_point = len(numbers) // 4
        split_1 = numbers[:mid_point]
        split_2 = numbers[mid_point:mid_point * 2]
        split_3 = numbers[mid_point * 2:mid_point * 3]
        split_4 = numbers[mid_point * 3:]
        inputs = [split_1, split_2, split_3, split_4]
    
        with concurrent.futures.ThreadPoolExecutor(max_workers=4) as executor:
            futures = [executor.submit(api, inp) for inp in inputs]
            for future in concurrent.futures.as_completed(futures):
                future.result()

**Result**

`[split_threaded_api]: Total time for 5 calls: 100.1269 seconds`

While not as good as earlier we still have a much better performance. Now since the main script is actually waiting on the IO for results from endpoint the effect of GIL is not becoming a bottleneck.

In IO-bound tasks like network calls or file reads, Python releases the GIL while waiting for IO operations. This allows other threads to make progress, effectively improving performance through concurrency.

## Closing Thoughts

You might be wondering why don’t I use the multiprocessing always and ditch the threading all together? You can but it will come at a cost. Threads are much light weight compared to a process since threads have a shared memory space while multiprocess has separate memory space. This also makes data transfer between processes tougher. 

So basically which one to use will boil down to your specific use case and the nature of the load. For IO bound workload you could get away with threading while for CPU bound you might need multiprocessing.

Also the understating of this helps you configure the HTTP server’s like gunicorn. For CPU heavy servers you would probably need more workers and lesser threads while for IO bound heavy server you could have larger number of threads. Also the multi processing parallelism will be limited by the number of CPU cores your machine have since that is the maximum true parallelism that we can achieve. While the threads when used for IO bound applications we can go beyond CPU cores since these work in concurrent manner waiting for IO operations. 

* **CPU-bound tasks** → Use **multiprocessing**.

* **IO-bound tasks** → Use **multithreading**.

* **Server optimization** → Tailor workers and threads based on workload type.

That’s all what I learned from my little experiment. Feel free to add your thoughts and comments.
 
[**GitHub - RitwikGopi/python-GIL-test**](https://github.com/RitwikGopi/python-GIL-test)


