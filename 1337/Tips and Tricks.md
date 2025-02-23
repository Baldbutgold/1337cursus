# Screen Related
## Changing Screen Brightness
Run xrandr in your terminal
`xrandr`

after that you will get your display name see the screenshot below so there DP-2

and then run this command 

`xrandr --output DP-2 --brightness .8`
`xrandar --output display-name --brightness value from 0 - 1` 
## Trigger Dark Mode

`xrandr --output DP-2 --gamma 1.0:0.8:0.7`

and this to reset night mode 

`xrandr --output DP-2 --gamma 1:1:1`

make sure to change your display name ou sf 

you can also setup aliases just like this
alias light='xrandr --output DP-2 --brightness .8'
alias nightmode='xrandr --output DP-2 --gamma 1.0:0.8:0.7'

P.S : There is a github repo that kinda of controls the screen but it seems that whenever you log off the session it uninstalls or something like that I am not 100% sure. here is the link : [https://github.com/lordamit/Brightness](https://github.com/lordamit/Brightness "https://github.com/lordamit/Brightness")
# Startup configuration

This is also very important and it is very simple
Head to Tweaks > Find Startup Applications on the left bar > click on the `+`
After that choose the apps you want to start at the Startup

# Shortcuts to configure 

to setup keyboard shortcuts head to settings > Find keyboard on the left panel > find `view and customize shortcuts` at the bottom of the screen > click on plus > search for `home folder` >  click on it > setup your own shortcut

You might find a lot of useful shortcuts there too, I will add them as I go.

I Have `super + e` configured to open files