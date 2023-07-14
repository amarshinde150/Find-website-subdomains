# Finding Active Sub-domains Of A Website In Kali Linux
The "Finding Sub-domains of a Website in Kali Linux" tool is a powerful command-line utility that leverages Kali Linux's robust set of tools to efficiently discover and enumerate sub-domains associated with a target website, providing valuable insights for security assessments and penetration testing.
## Setup
#### Step 1 : Open Terminal And Install Following Tools
```
$ sudo apt install amass
$ sudo apt install massdns
```

#### Step 2 : Configuring Profile File
In terminal to open .profile file
```
$ nano ~/.profile
```
#### Step 3 : Add This Code At The End Of This File
```
function my_amass() {
    if [[ $1 == "--help" ]]; then
        echo "Usage: my_amass <domain name> <location of output file>"
        echo "Arguments:"
        echo "  'domain name' : Enter domain name of website to scan"
        echo "  'output file' : Enter the location of output file"
	      echo "Example: my_amass example.com ./subdomain_list.txt"
    else
        amass enum -d "$1" -passive -o "$2"
    fi
}

#To find active subdomains from list of subdomain
function my_massdns() {
    if [[ $1 == "--help" ]]; then
        echo "Usage: To find all active subdomains from list of domains"
        echo "Syntax: my_massdns <subdomain file> <output file>"
        echo "Arguments:"
        echo "  'subdomain file' : path of subdomain file"
        echo "  'output file' : path to output file"
      	echo "Example: my_massdns subdomain.txt ./active_subdomain.txt"
    else
        # Command implementation goes here
        massdns -r '/home/amar/Public/Tools/massdns/lists/resolvers.txt' -t A -o S $1 -w temp_subdomain_ll1.txt
        sed 's/A.*//' temp_subdomain_ll1.txt | sed 's/CN.*//' | sed 's/\..$//' > $2
        rm temp_subdomain_ll1.txt
    fi
}

#Function to find all sub-domains of a website
function my_findalldomains() {
    if [[ $1 == "--help" ]]; then
        echo "Usage: Function to find all active subdomains of a website"
        echo "Syntax: my_findalldomains example.com" 
        echo "Arguments:"
        echo "  arg1 : domain name"
    else
        # Command implementation goes here
        my_amass $1 ./temp_domain.txt
        my_massdns ./temp_domain.txt "./active_subdomains_"$1
        rm temp_domain.txt
        echo "Files:"
        ls
    fi
}
```
#### Step 4 : Save The Code
> cntrl + x <br/>
> then type 'Y' and then <kbd>Enter</kbd>

#### Step 5 : Source Command
```
$ source ~/.profile
```
#### Step 6 : You Can See Details Of Custom Commands
```
$ my_amass --help
$ my_massdns --help
$ my_findalldomains --help
```
But we will use only ```my_findalldomains``` this command because it will call other two functions internally

#### Step 7 : Now Use The Command To Get All Active Subdomain Of A Website
I am taking example.com as an example
```
$ my_findalldomains example.com
```
## Usage & Conclusion
By running this command you will get a file named ```active_subdomain_example.com```
So in future whenever you want to find subdomains of a website
Run this command
```
$ source ~/.profile
```
Followed by this command
```
$ my_findalldomains example.com
```
