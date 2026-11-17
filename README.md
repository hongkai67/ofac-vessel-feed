The OFAC Data Pipeline - How it works 

The pipeline was built to solve one core problem. The US Treasury updates the OFAC sanctions list whenever a new designation is issued but there is no automatic way for a browser-based tool to pull directly from a 108MB government XML file in real time. Hence, the pipeline processes the raw official data daily and makes it into a web feed that can consume instantly. 

 

The Source 

The US Treasury publishes the official Specially Designated Nationals (SDN) Enhanced XML file at sanctionslistservice.ofac.treas.gov. This file is 108MB and contains 19,254 sanctioned entities as of July 2026, of which 1,517 are vessels. It is published in a structured XML format containing each vessel's IMO number, name, aliases, sanctions programme, flag state, vessel type, call sign, owner, and tonnage. This is the file our pipeline processes every day.  

 

What is GitHub ? 

GitHub is a free online platform where developers store and manage their code and files. It is specifically built for code. It has a feature called GitHub Pages which lets you publish files to a public web address. Hence, anyone with the link can access the file directly from their browser. This is how we serve the vessels.json file to VesselScreen. 

GitHub also has a feature called GitHub Actions, which lets you set up automated tasks that run on a schedule like an alarm clock that triggers a job at a set time every day without you having to do anything manually. 

 

Step 1 - Download the 108MB XML from the US Treasury 

The first step is to go directly to the source. The US Treasury's Office of Foreign Assets Control publishes the SDN Enhanced XML at their official Sanctions List Service portal. This is the only fully authoritative version of the list. It contains third-party databases, aggregators and commercial services all ultimately derive their data from this same file. By downloading directly from the Treasury, we guarantee that our data is as accurate and complete as it is the official list so there is no middleman introducing delays or inaccuracies. 

 

Step 2 - Run the Parser 
 The parse_ofac.py script reads through the XML file, filters out only the vessel entries (entity type 602), extracts all the relevant fields. For example, IMO number, flag, vessel type, owner, sanctions programme and decodes any special characters. Overall, organises everything into a clean structured format. 

 

 

Figure _: parse_ofac.py script 

 

Step 3 - Upload the XML to GitHub 
Upload the downloaded XML file and the parse_ofac.py parser script to the GitHub repository. The repository is where all the files are stored and managed. 

 

Step 4 - vessels.json is Generated 
Once the parser finishes, it writes all the extracted vessel data into a single file called vessels.json. This file contains all 1,517 sanctioned vessels in a format that web browsers can read instantly.  

Figure _: vessels.json file 

 

Step 5 - GitHub Pages Publishes the File 
 The vessels.json file is committed to the GitHub repository. Because GitHub Pages is enabled on the repository, the file is automatically made available at a fixed public web address: 

hongkai67.github.io/ofac-vessel-feed/vessels.json 

Hence, anyone or any app can now access this file directly from that link.  

 

Step 6 - GitHub Actions runs automatically 
Rather than repeating steps 1 to 5 manually every day, a GitHub Actions workflow is set up to do it automatically. Every day at 06:00 UTC (2:00 PM Singapore time), GitHub Actions downloads the latest XML from the US Treasury, runs the parser, generates a fresh vessels.json, and publishes it, all without any human involvement. VesselScreen fetches this address every time it loads, pulling in the latest vessel data within seconds of opening.
