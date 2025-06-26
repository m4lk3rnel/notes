- https://www.secureops.com/wp-content/uploads/2021/06/Sony-Breach-Analysis-v4.pdf
- September 2014: "hackers broke into Sony's networks and stole confidential data and planted malware".
- "... when Sony employees tried to log into their computers a picture of a red skeleton appeared on their computer screens with the words `#Hacked by GOP#`"
- The malware also destroyed **70 percent** of Sony's laptops and computers (physical damage).

- Two technical ways to obtain data that belongs to people or organizations.
	- **break in** and **intercept** the data as it is transmitted and then **decrypt** it.
	- **Social Engineering**. The attacker asks for it in a way that tricks the users into believing they should give the information to the attacker.

- **Spear-phishing**. Attackers search online for information about the individuals and then use that information to **specifically** target that person.

- Often, hackers will copy legitimate emails, but then replace the hyperlinks that would re-direct potential victims to infrastructure under the **attacker's control** where then deliver malware to the victim's computer.

### Phishing for Access
- Facebook sends emails to users to alert then when a computer with a new IP address signs into their account, and contain links to follow-up on the sign-in.
	- the attackers replaced the hyperlink with `http://www.fancug.com/link/facebok_en.html` for "logging in" to Facebook. 

- the hackers also created emails that looked like they came from Google.
	- one of the emails: "welcome a recipient to Google's Drive service" with a link to `http://.[DOMAIN REDACTED].com/x/o?u=2cfb0877-eaa9-4061- bf7e-a2ade6a30d32&c=374814` which was also likely to have sent the user to a malicious file.
	- Another email claimed to be from Google, alerting the user that “’malicious activities are detected’” but the Google hyperlinks offering information about how to mitigate malicious activities and Google’s terms of service contained URLs that were not related to Google, and instead were likely malicious.

- some Sony employees also received emails from "Apple" asking users to verify their Apple ID's because of supposed unauthorized activity and provided them with a link that took them to a form that **appeared to be** from Apple but were fake. The credentials entered by the Sony employees fell into the hands of the individuals who sent the fake emails.

### Infrastructure Used in the Hack
- infrastructure components:
	- certain IP addresses
	- compromised hop point computers (**proxy chaining**)
	- proxy IP address services
	- Dynamic Domain Name System (DDNS)

- The FBI determined that:
	- `175.45.176.0/22` is owned by Star Joint Venture, a known North Korean internet provider.
	- `210.52.109.0/24` is owned by a Chinese ISP, but was previously linked to North Korean activity.
	- `WHOIS` can be used to get to the source IP of a chain.
	- Malware often contains **hardcoded C2 IPs** or **domain names** which can be extracted.
	- these can be used for [[Sinkholing (DNS)]].