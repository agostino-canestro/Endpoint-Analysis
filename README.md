<h1>Endpoint Analysis</h1>

<h2>Description</h2>
The objective of this project is to interpret web server logs to understand how the site was compromised. This exercise involves using Linux commands to extract and analyze data from the logs, identifying the time frame of the logs, source IP addresses, user agents, post requests, and other relevant information. The scenario suggests that a plugin installed on the WordPress site was vulnerable to a remote code execution vulnerability, which allowed an attacker to gain access to the server's underlying operating system. This project, which is hosted on BlueteamLabs, offers practical experience in log analysis, incident investigation, and identifying vulnerabilities. It guides users through the process of analyzing web server logs to uncover the methods and tactics used in a cyber attack, enhancing their understanding of security incident response and threat detection. Additionally, the project incorporates the use of AbuseIPDB and VirusTotal for analyzing suspicious IP addresses, and Oracle VM VirtualBox for creating a virtualized environment to safely conduct the analysis.
<br />

<h2>Languages and Utilities Used</h2>

- <b>Blueteamlabs</b> 
- <b>AbuseIPDB</b>
- <b>VirusToal</b>

<h2>Environments Used </h2>

- <b>OracleVM VirtualBox</b> 
- <b>Kali Linux</b>

<h2>Project Walk-through:</h2>

<p align="center">
Step 1: Set up a virtual machine configured with a Linux-based operating system: <br/>
<img src="https://i.imgur.com/C7f2K2l.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 2: Navigate to BlueteamLabs which is where the exercise is hosted:  <br/>
<img src="https://i.imgur.com/od4bUzy.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 3: Using the search bar, navigate to the exercise “Log Analysis - Compromised WordPress” and click “Start Challenge: <br/>
<img src="https://i.imgur.com/yx0aySK.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 4: Download the zip file which contains the web server logs that we will be analyzing:  <br/>
<img src="https://i.imgur.com/mBPJTMG.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 5: Enter the password provided on BlueteamLabs to unzip the zip file:  <br/>
<img src="https://i.imgur.com/2Z00LqB.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 6: The log that we will be analyzing is access.log. The commands "head access.log" and "tail access.log" are used to quickly examine the web server log file. The "head" command displays the first few lines to identify the starting point of the events logged, while the "tail" command shows the last few lines to determine the endpoint of the timeframe covered. This initial overview helps set the context for the analysis by understanding the time range and identifying any significant changes or patterns:  <br/>
<img src="https://i.imgur.com/bWhCHUT.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/4Y5yydu.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 7: The command "cat access.log | cut -d ' ' -f 1 | sort | uniq -c | sort -nr" is utilized to analyze the web server log file, extracting and counting unique source IP addresses. It identifies the most active or potentially suspicious IP addresses by sorting them based on their frequency of occurrence. This analysis aids in understanding the traffic pattern and identifying potential security threats in the log data:  <br/>
<img src="https://i.imgur.com/RcIBLEv.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 8: We then re-run the following command and save it to the file IPs.txt for further analysis:  <br/>
<img src="https://i.imgur.com/7wRKqOV.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/Iv4qDn3.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 9: We will need to extract the user agent field shown below:  <br/>
<img src="https://i.imgur.com/Xyanl1p.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 9: The commands cat access.log | cut -d '"' -f 1 and subsequent iterations with increasing field numbers are used to extract specific fields from the web server log, where fields are delimited by double quotation marks ("). Incrementing the field number allows for the analysis of different components of the log entries, such as the IP address, HTTP method, requested URL, and notably, the user agent, which is typically found in the sixth field (-f 6):  <br/>
<img src="https://i.imgur.com/G0QGUEf.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/0BfAFMt.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/1HYuRZQ.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/L8UCkrs.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 10: We then save this data to a file called useragents.txt:  <br/>
<img src="https://i.imgur.com/BFH40Uj.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/GyzQZ0i.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 11: The command grep 'POST' access.log | grep -v '403' | cut -d ' ' -f 1 is used in the project to filter web server logs for successful HTTP POST requests, excluding those with a '403' status code indicating forbidden access. It then extracts the source IP addresses of these requests, focusing the analysis on potentially unauthorized actions. This step is crucial for identifying and investigating security incidents related to file uploads or data modification on the server:  <br/>
<img src="https://i.imgur.com/3u0SGut.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 12: The command grep 'POST' access.log | grep -v '403' | cut -d ' ' -f 1 | sort | uniq -c | sort -nr is used to further refine the analysis of HTTP POST requests from the web server logs. It excludes requests with a '403' status code, extracts the source IP addresses, and then sorts and counts the unique IP addresses to identify the frequency of POST requests from each IP. This helps to prioritize the investigation by focusing on IPs with a higher number of successful POST requests, which could indicate potential malicious activity or security breaches:  <br/>
<img src="https://i.imgur.com/nO62zuU.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 13: We then save the following data to the file POSTIP.txt:  <br/>
<img src="https://i.imgur.com/ZWjYn1J.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 14: Taking a lot at the data, and referencing the prior logs, we notice that the IP address 103.69.55.212 is making a POST request with a suspicious php file “fr34k.php":  <br/>
<img src="https://i.imgur.com/OX4bYsN.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/s2UE5YI.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 15: Using the website AbuseIPDB, which is a platform for reporting and checking the reputation of IP addresses, and VirusTotal, which is a service that analyzes files, IP addresses, and URLs for viruses, malware, and other threats, we analyze the IP address to assess its potential security risks and malicious activities:  <br/>
<img src="https://i.imgur.com/2D7DeEh.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/4nFtLdO.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 16: We notice that a GET request is made to a plug called “contact-form-7” and another called “simple-file-list” so we will perform additional research on these:  <br/>
<img src="https://i.imgur.com/q1POUUb.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/xIttOEC.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/zvLCao3.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/qy2CV8A.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/cH0NvJ3.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 17: Taking a look at the code for WordPress Plugin Simple File List 4.2.2, we can note that it performs pre-authenticated RCE:  <br/>
<img src="https://i.imgur.com/MfYwTnR.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 18: We then perform searches for “contact” and “simple” on access.log to see if we can uncover any additional info:  <br/>
<img src="https://i.imgur.com/8mb71Kc.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/aUmDV1G.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 19: We notice when we search for “simple” we see a POST request with fr3ak.php:  <br/>
<img src="https://i.imgur.com/YGhZe2Y.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 20: We also notice the IP address 119.241.22.121 using “python-requests” in the above screenshot so we will attempt to uncover any additional information regarding this IP:  <br/>
<img src="https://i.imgur.com/YI45ET5.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 21: We notice a POST request for a login hb-token=adminLogin and also mention of “WPScan v3.8.0” so we will see if we can uncover any additional information from AbuseIDP or VirusTotal:  <br/>
<img src="https://i.imgur.com/xabXNAi.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/bB0u4Gd.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/lC4HvoD.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/3QuBxs2.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 22: Search for “contact-form-7” within access.log we can see mention of “action=activate” and make note of the time and do the same now for “simple-file-list:  <br/>
<img src="https://i.imgur.com/DRmJBdk.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/uyQXGa0.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 23: Now that we have uncovered enough information, we can move on to the challenge questions:  <br/>
<img src="https://i.imgur.com/QpsUqKk.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 24: To answer the first question we research for “adminlogin” and look for the POST request:  <br/>
<img src="https://i.imgur.com/du2t2pb.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/eoPCT8G.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 25: To see the tools that the attacker used, we look through the useragents file:  <br/>
<img src="https://i.imgur.com/EJ6qk2L.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 26: To see the CVE of contact-form-7, we simply search the name of the vulnerability on Google:  <br/>
<img src="https://i.imgur.com/NWXvf8T.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/uXZOTrB.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 27: There were file uploads for the simple-file-list which indicates to us that this plugin is what was exploited to gain access:  <br/>
<img src="https://i.imgur.com/6VZhP21.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 28: The name of the PHP web shell file is fr34k.php which we noticed earlier when we were searching through access.log:  <br/>
<img src="https://i.imgur.com/bmOJ759.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<img src="https://i.imgur.com/k7k6Pii.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 29: Taking a look at the last entry of “grep ‘fr3ak.php’ access.log” we can see the response code that was provided when the web shell was accessed for the final time was 404:  <br/>
<img src="https://i.imgur.com/34yge6u.png" height="80%" width="80%" alt="Endpoint Analysis"/>
<br />
<br />
Step 30: Here is the complete list of answers to each challenge question:  <br/>
<img src="https://i.imgur.com/nn3cdyX.png" height="80%" width="80%" alt="Endpoint Analysis"/>
</p>

