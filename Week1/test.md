
# Assignment 1
For this assignment you are welcomed to use other regex resources such a regex "cheat sheets" you find on the web.



Before start working on the problems, here is a small example to help you understand how to write your own answers. In short, the solution should be written within the function body given, and the final result should be returned. Then the autograder will try to call the function and validate your returned result accordingly. 


```python
def example_word_count():
    # This example question requires counting words in the example_string below.
    example_string = "Amy is 5 years old"
    
    # YOUR CODE HERE.
    # You should write your solution here, and return your result, you can comment out or delete the
    # NotImplementedError below.
    result = example_string.split(" ")
    return len(result)

    #raise NotImplementedError()
```

## Part A

Find a list of all of the names in the following string using regex.


```python
import re
def names():
    simple_string = """Amy is 5 years old, and her sister Mary is 2 years old. 
    Ruth and Peter, their parents, have 3 kids."""
    
    pattern = "([A-Z]{1}[a-z]+)"
    return re.findall(pattern, simple_string)

len(names())==4
```




    True




```python
assert len(names()) == 4, "There are four names in the simple_string"

```

## Part B

The dataset file in [assets/grades.txt](assets/grades.txt) contains a line separated list of people with their grade in 
a class. Create a regex to generate a list of just those students who received a B in the course.


```python
import re
Blist = []
def grades():
    with open ("assets/grades.txt", "r") as file:
        grades = file.read()
        grades = grades.split("\n")
        pattern = "(?P<names>[\w ]*)\:\s{1}(?P<grade>[\w]{1})"
        
        for item in re.finditer(pattern, str(grades)):
            if item.group(2)== "B":
                Blist.append(str(item.group(1)))
                #print(Blist)
        #for i in Blist:
            #print(i)
        
    return Blist

```


```python
assert len(grades()) == 16

```

## Part C

Consider the standard web log file in [assets/logdata.txt](assets/logdata.txt). This file records the access a user makes when visiting a web page (like this one!). Each line of the log has the following items:
* a host (e.g., '146.204.224.152') 
* a user_name (e.g., 'feest6811' **note: sometimes the user name is missing! In this case, use '-' as the value for the username.**)
* the time a request was made (e.g., '21/Jun/2019:15:45:24 -0700')
* the post request type (e.g., 'POST /incentivize HTTP/1.1' **note: not everything is a POST!**)

Your task is to convert this into a list of dictionaries, where each dictionary looks like the following:
```
example_dict = {"host":"146.204.224.152", 
                "user_name":"feest6811", 
                "time":"21/Jun/2019:15:45:24 -0700",
                "request":"POST /incentivize HTTP/1.1"}
```


```python
import re
def logs():
    dict = []
    with open("assets/logdata.txt", "r") as file:
        logdata = file.read()
        pattern = r""" 
                        (?P<host>\d+(?:\.\d+){3})
                        \s+\S+\s+ 
                        (?P<user_name>\S+)\s+\[
                        (?P<time>[^\]\[]*)\]\s+   
                        "(?P<request>[^"]*)" 
                        """
    for item in re.finditer(pattern, logdata, re.VERBOSE):
        dict.append(item.groupdict())
        
    return dict
print(len(logs()))
```

    979



```python
assert len(logs()) == 979

one_item={'host': '146.204.224.152',
  'user_name': 'feest6811',
  'time': '21/Jun/2019:15:45:24 -0700',
  'request': 'POST /incentivize HTTP/1.1'}
assert one_item in logs(), "Sorry, this item should be in the log results, check your formating"

```


```python

```
