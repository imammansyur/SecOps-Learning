# Valdoria Votes
## Section 4
### What is Anderson Snooper’s email address?
``` kql
Employees
| where name == "Anderson Snooper"
```

### What is the email address of the person he was conversing with?
To do that, I need to get his email, set his email as the sender, and set it to the time around the successful login attempt.
``` kql
let ansnooper = Employees
| where name == "Anderson Snooper"
| distinct email_addr;
Email
| where sender in (ansnooper)
| where timestamp between(datetime(2024-10-05T10:46:47Z) .. datetime(2024-10-11T10:46:47Z))
```

### "Snooper" was observed asking Schmelly how one might gain access to what devices?
Since we already knew that "Snooper" was conversing with Schmelly, we can make the timespan shorter. From the time "Snooper" first sent the email to Schmelly until the first email unrelated to the conversation. We can also show Schmelly's reply by setting the sender to his email address too using OR.
``` kql
let ansnooper = Employees
| where username == "ansnooper"
| distinct email_addr;
Email
| where sender in (ansnooper) or sender == "barry_schmelly@valdoriavotes.gov"
| where timestamp between(datetime(2024-10-08T12:52:34.000Z) .. datetime(2024-10-08T14:31:34.000Z))
```

### Using Snooper's IP address, let's check his activity in InboundNetworkEvents.
As always, I made Snooper's Employee details into variable then take what i needed, in this case his IP address. Then I use that to get the InboundNetworkEvents.
``` kql
let ansnooper = Employees
| where username == "ansnooper"
| distinct ip_addr;
InboundNetworkEvents
| where src_ip  in (ansnooper)
```

### According to the bot, the voting machines are not actually connected to the internet.
Since there will be keywords that could be in the prompt or the response, I grouped it in a variable.
``` kql
let var = "internet";
AIPrompts
| where prompt has var or response has var
```

## Section 5
### When did they log in to Bobama's account?
According to the transcript, the phone call happened on 2024-10-15 at 11:59 PM. I just need to show the auth events after that time.
``` kql
let bobama = Employees
| where name == "Arrack Bobama"
| distinct username;
AuthenticationEvents
| where username in (bobama)
| where timestamp >= todatetime("2024-10-16T00:00:00Z")
```
