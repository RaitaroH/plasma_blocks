# plasma_blocks

Install the actual applet first from ![here](https://github.com/Zren/plasma-applet-commandoutput). You are also gonna need a ![nerd font](https://github.com/ryanoasis/nerd-fonts).

Now, add some command output. Here are a few ideas you can do:

![](https://i.imgur.com/iVHAOL0.png)

## Updates (no discover needed)

Command: `echo $(/home/raitaro/bin/pkcon_check)`, every 60000ms, with a Nerd Font.

```
#!/bin/bash

# Check for updates
state=$(pkcon get-updates | grep "There are no")

case $state in
  "") printf "﮻"  ;;
  *) printf "" ;;
esac

```


## Weather (wttr.in)

Thanks to Luke Smith for the ![temp code](https://github.com/LukeSmithxyz/voidrice/blob/master/.scripts/statusbar/weather).

Command: `echo $(/home/raitaro/bin/weather -t)` for temperature and `-c` for the cast, every 180000ms, with a Nerd Font.

```
#!/bin/bash

params="$(getopt -o ct -l cast,temperature --name "$0" -- "$@")"
eval set -- "$params"

case "$1" in
  # Open a directory in audacious
  -c|cast)
    state=$(curl wttr.in -s | sed -n 3p | sed -e "s/[^ a-zA-Z']//g" -e 's/ \+/ /' -e 's/\ //g' -e 's/mmm//g' -e 's/mm//g')
    # echo $state
    [[ $state == *Fog*      || $state == *Mist* ]]     && printf ''  && exit 1
    [[ $state == *Sun*      || $state == *Clear* ]]    && printf '盛' && exit 1
    [[ $state == *Freezing* || $state == *SnowMist* ]] && printf 'ﭽ'  && exit 1
    [[ $state == *Rain*     || $state == *Drizzle* ]]  && printf ''  && exit 1
    [[ $state == *torm* ]] && printf '' && exit 1
    [[ $state == *loud* ]] && printf '' && exit 1
    [[ $state == *cast* ]] && printf '' && exit 1
    [[ $state == *Snow* ]] && printf '' && exit 1

    ;;
    -t|temperature)
      # curl wttr.in -s | sed '13q;d' | grep -o "m\\(-\\)*[0-9]\\+" | sort -n -t 'm' -k 2n | sed -e 1b -e '$!d' | tr '\n|m' ' ' | awk '{print "",$1 "°C","",$2 "°C"}'
      curl wttr.in -s | sed '13q;d' | grep -o "m\\(-\\)*[0-9]\\+" | sort -n -t 'm' -k 2n | sed -e 1b -e '$!d' | tr '\n|m' ' ' | awk '{print "",$1,"",$2}'
	  ;;
esac
```