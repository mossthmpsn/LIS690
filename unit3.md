# Learning the CLI
- three reasons to prefer CLI over GUI
  - GUI entails extra software
  - GUI exposes systems to extra security risks
  - GUI not great for automation
# Searching with grep
- grep is a command-line tool that searches for text
- works line by line
- case-sensitive
- can do inverse searching using "-v" and ignore case using "-i"
  - will still return string within a larger word
- can use regex for start of line (rejecting string from start of line) using ^
- regex for end of line: $ after term
- count matches: -c
- Boolean "OR" (AKA infix operator): |
  - can include more than two strings (i.e. bsd|gpl|apache)
- use "\<"string"\> to limit only the string of characters as its own term
  - can also use -w
- context matches
  - include the lines after results: "-A [number]"
  - include lines before results: "-B [number]"
- halt matching (stop returning results after a certain number of hits): -m[number]
- returning line numbers: -n
- character class matching
  - character classes: [a-z]
  - repetition: {number}
  - for four letter numbers: "[0-9]{number}"
  - for results that start with a specific letter: "letter.{number of letters in between}letter"
- practice
  - search on Scopus, select docs, click "Export", click "BibTex", select all Citation information and Bibliographic information, click Export
  - UPLOAD FILE
# Managing Software
- package manager: used to handle installation, upgrades, and uninstalls (Ubuntu uses dpkg and apt on the front end)
- use package manager: sudo
- sudo syntax
  - create data directory: cd /usr/local/bin
  sudo mkdir data
  - create file in directory outside of home: cd /srv
  sudo touch data.csv
- apt commands
  - update list of software and compare to installed: sudo apt update
  - upgrade software: sudo apt upgrade
  - help pages for commands using tldr
  - search for tldr: apt search tldr
  - specific info on package: apt show tldr
  - install package: sudo apt install tldr
  - remove package: sudo apt --purge remove tldr
  - remove dependencies to keep system clean: sudo apt autoremove
  - remove extra files: sudo apt clean
- dpkg
  - to install .deb files: sudo dpkg -i <file_name.deb>`
  - to uninstall applications installed with dpkg: sudo dpkg --purge <application_name>
- stick with apt generally
# Library Search
- yaz-client: info retrieval client to query bibliographic databases
  - also a search/retrieve via url (SRU) and search/retrieve web service (SRW)
  - allow us to interact with protocols directly from command line
- installing yaz
  - search for software: apt search yaz
  - info on program: apt show yaz
  - install: sudo apt install yaz
- documentation
  - access manual page: man yaz-client
  - search attributes available to yaz: man bib1-attr
- using yaz
  - start yaz program: yaz-client
  - connect UK library catalog: Z> open saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY
- queries: uses prefix query notation (PQN)
  - example: Z> find @and @attr 1=4 "information" @attr 1=21 "library science"
  - find: search request; can be abbreviated to "f"
  - @and: Boolean AND
  - @attr 1=4: search in Title field
  - "information": term for Title search
  - @attr 1=21: search in subject heading field
  - "library science": term for subject heading search
  - look into results: show [insert number]
  - @attr 1=1: search in personal name field
  - @attr 1=1016: search in any field
  - quit: quit
- advanced usage
  - append bibliographic records to a file: yaz-client -m [filename].marc
  - examine records retrieved: exit yaz, head [filename].marc
  - determine file type: file [filename].marc
  - convert file to JSON: yaz-marcdump -o json [filename].marc > [filename].json
  - format JSON for readability: jq . [filename].json > [files-formatted].json
  - select all fields: jq '.fields[]' [files-formatted].json
  - select 650 fields: jq '.fields[] | select(has("650"))' [files-formatted].json
  - select a number from the 650 fields: jq '.fields[] | select(has("650")) | .["650"].subfields[] | select(has("x")) | .x' [files-formatted].json
  - select geographic subdivision (z) from the 650 fields: jq '.fields[] | select(has("650")) | .["650"].subfields[] | select(has("z")) | .z' [files-formatted].json
  - to XML: yaz-marcdump -o marcxml [filename].marc > [filename].xml
- downloading all results (specifically from UK libraries here)
  - locate 120 records and save to file the records
  yaz-client
  > set_marcdump [filename].new
  > open saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY
  > find @and @attr 1=4 "technology" @attr 1=21 "library science"
  > show 1 +120
  > quit
  - convert to JSON and examine file
  - yaz-client
  > set_marcdump records.new
  > open saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY
  > find @and @attr 1=4 "technology" @attr 1=21 "library science"
  > show 1 +120
  > quit
  - delete most common subject heading and sum number from count
  jq '.fields[] | select(has("650")) | .["650"].subfields[] | select(has("a")) | .a' records-formatted.json |\
  sort | \
  sed 's/\.//g' | \
  awk '{ print tolower($0) }' | \
  sort | \
  uniq -c | \
  sort -n | \
  sed '$d' | \
  awk '{ sum+=$1 } END{print sum}'





