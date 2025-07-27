


1 - Insert Console cable and login to the switch until you have the login prompt:

<img width="449" height="346" alt="image" src="https://github.com/user-attachments/assets/0f5832e0-ff98-41c1-8e8a-f6e32d696195" />

2 - Turn off Router (keep you console window opened to see the boot process)

<img width="919" height="147" alt="image" src="https://github.com/user-attachments/assets/6773338e-163b-41fb-81fe-7828fdab118b" />

3 - Wait some seconds, and turn on the router:

<img width="932" height="127" alt="image" src="https://github.com/user-attachments/assets/eea25930-fc34-4cc8-b99a-7587c945045d" />

4 - Go to putty and right click top of the window, then click special commands > break. _(You need to click multiple times until you get to the Rommon mode)_

<img width="839" height="567" alt="image" src="https://github.com/user-attachments/assets/3b8ed850-d8cf-450d-80a3-6b8ed6a7e8a9" />

5 - Once in ROMMON mode prompt, add the next command _(Change the default Configuration Register Value to 0x2142)_: 

````py
confreg 0x2142
````

6 - Restart the router:

````
reset
````

7 - Wait until the router boots again and do the following:
- copy the old config intro running config (so I don't lose the old config) tip: this is the inverse command that we usually to save :P)
- change the secret
- save configuration
- restart the router

````
enable

copy startup-config running-config

!
!

configure terminal

enable secret cisco.12345

end

copy running-config startup-config

configure terminal

config-register 0x2102

end

copy running-config startup-config

reload

````

## Resources

https://www.youtube.com/watch?v=WYQMJoIKvvA
