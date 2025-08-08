# Valdoria Votes
The Valdoria Board of Elections is gearing up for the most critical election in recent memory. To ensure a smooth voting experience, the board has hired additional poll workers in the past month, preparing them to support operations and assist voters. Officials have made it clear that the voting machines are highly secure, working tirelessly to reassure the public about the integrity of the election process.

However, malicious actors are actively working to sow doubt, hoping to make citizens question the validity of their vote. As Election Day approaches, Valdoria's citizens anxiously watch, wondering if democracy will withstand these challenges.
## Section 4: Snooping around
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
let keyword = "internet";
AIPrompts
| where prompt has keyword or response has keyword
```

## Section 5 : Look on my [Security], ye Mighty, and despair!
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
### What email address did they send this email to?
We knew from previous query that the sucessful login attempt happened at 10/16/2024, 12:00:00 AM. I just need to show what happened after that time with Bobama's email activity.
``` kql
let bobama = Employees
| where name == "Arrack Bobama"
| distinct email_addr;
Email
| where sender in (bobama)
| where timestamp >= todatetime("2024-10-16T00:00:00Z")
```
Now that we know there's a conversation between Bobama's email and help@dominosvotingsystems.com, I want to know what were they talking about. I included the email in the query.
``` kql
let bobama = Employees
| where name == "Arrack Bobama"
| distinct email_addr;
Email
| where sender in (bobama) or sender == "help@dominosvotingsystems.com"
| where timestamp >= todatetime("2024-10-16T00:00:00Z")
```
