# Frognado in Valdoria 
The Valdorian Times has published another groundbreaking article ahead of the election. This time, it's about the incumbent mayoral candidate. According to the Times, the Mayor took bribes from FramtidX, a development company, to allow them to build a mall on protected land, a special marsh home to endangered frogs. 🐸🌿

Is this another fake story published by the Times, or is there any truth to it? As a cybersecurity analyst for FramtidX, you must dig deep, uncover the cyber side of the scandal, and discover the truth. 🕵️‍♂️💻🔍 Every piece of evidence you find will help reveal the reality behind the headlines. Can you solve the mystery and protect the future of Valdoria? ⚖️🌟

This is a continuation of the Scandal In Valdoria game. But, don't worry, you can still play this without having done that first.

This module was first published in collaboration with Cybrary at Blackhat 2024

## Section 1: Maybe it's just a tadpole? 😢👀
### Who is the Web Administrator? 
``` kql
Employees
| where role == 'Web Administrator'
```
Since its gonna be used multiple times, instead of copying the info one by one, I made it into a variable.

### When did the defacement happen exactly? 
I used the variables to search for any process events related to the 'Shadow Truth' defacement incident.
``` kql
let anita = Employees
| where role == 'Web Administrator'
| distinct hostname;
ProcessEvents
| where hostname in (anita)
| where process_commandline has 'Shadow Truth'
```
