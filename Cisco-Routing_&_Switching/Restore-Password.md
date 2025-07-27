


1 - Insert Console cable and login to the switch until you have the login prompt:

<img width="449" height="346" alt="image" src="https://github.com/user-attachments/assets/0f5832e0-ff98-41c1-8e8a-f6e32d696195" />

2 - Turn off Router (keep you console window opened to see the boot process)

<img width="919" height="147" alt="image" src="https://github.com/user-attachments/assets/6773338e-163b-41fb-81fe-7828fdab118b" />

3 - Wait some seconds, and turn on the router:

<img width="932" height="127" alt="image" src="https://github.com/user-attachments/assets/eea25930-fc34-4cc8-b99a-7587c945045d" />

4 - Go to putty and right click top of the window, then click special commands > break. _(You need to click **MULTIPLE TIMES FAST** TO "CATCH THE BOOT", until you get to the Rommon mode)_

`CTRL + c` like crazy!

<img width="570" height="494" alt="image" src="https://github.com/user-attachments/assets/77abc79a-08e3-43df-a2a8-8aa7901bfbc5" />

5 - Once in ROMMON mode prompt, add the next command _(Change the default Configuration Register Value to 0x2142)_: 

````py
confreg 0x2142
````

6 - Restart the router:

````
reset
````

7 - Wait until the router boots...

<img width="545" height="214" alt="image" src="https://github.com/user-attachments/assets/70938559-41cf-4479-acf2-6bd9d51fc92a" />

8 - Enter to the switch (it will not have any password), and copy the old config intro running config (so I don't lose the old config) tip: this is the inverse command that we usually to save :P), wait and you will see a prompt similar to `s705r2#`

````
enable

copy startup-config running-config


````

9 - Change the enable secret & admin/password + save configuration

````
configure terminal

username admin privilege 15 secret admin.cisco
!
enable secret cisco.12345

end
write memory

!
!


````

10 - Change register to `0x2102` + save configuration

````
configure terminal

config-register 0x2102

end
write memory

!
!


````

<img width="680" height="632" alt="image" src="https://github.com/user-attachments/assets/bc84252f-6f88-4409-970f-dc6a80696f5e" />

11 -  restart the router

````
reload


````

12 - You now can access access with the new admin and enable passwords:




## Resources

- https://www.youtube.com/watch?v=WYQMJoIKvvA
- https://youtu.be/WYQMJoIKvvA?si=eTtJN8e7a4nOzObk
















<img width="915" height="36" alt="image" src="https://github.com/user-attachments/assets/616c16ac-0223-4e4e-a784-29662f7aafbe" />
































