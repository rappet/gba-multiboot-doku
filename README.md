# gba-multiboot-doku

_used [gbatek](https://problemkaputt.de/gbatek.htm#gbawirelessadapter) as reference_

## GBA Wireless Adapter Commands

### Wireless Command/Parameter Transmission

```
  GBA       Adapter
  9966ppcch 80000000h   ;-send command (cc), and num param_words (pp)
  <param01> 80000000h   ;\
  <param02> 80000000h   ; send "pp" parameter word(s), if any
  ...       ...         ;/
  80000000h 9966rraah   ;-recv ack (aa=cc+80h), and num response_words (rr)
  80000000? <reply01>   ;\
  80000000? <reply02>   ; recv "rr" response word(s), if any
  ...       ...         ;/
```

Wireless 32bit Transfers
```
  wait until [4000128h].Bit2=0  ;want SI=0
  set [4000128h].Bit3=1         ;set SO=1
  wait until [4000128h].Bit2=1  ;want SI=1
  set [4000128h].Bit3=0,Bit7=1  ;set SO=0 and start 32bit transfer
```

All command/param/reply transfers should be done at Internal Clock (except, Response Words for command 25h,27h,35h,37h should use External Clock).

### Wireless Commands

```
  Cmd Para Reply Name
  10h -    -     Hello (send immediately after login)
  11h -    1     Good/Bad response to cmd 16h ?
  12h
  13h -    1
  14h
  15h
  16h 6    -     Introduce (send game/user name)
  17h 1    -     Config (send after Hello) (eg. param=003C0420h or 003C043Ch)
  18h
  19h
  1Ah
  1Bh
  1Ch -    -
  1Dh -    NN    Get Directory? (receive list of game/user names?)
  1Eh -    NN    Get Directory? (receive list of game/user names?)
  1Fh 1    -     Select Game for Download (send 16bit Game_ID)


  20h -    1
  21h -    1     Good/Bad response to cmd 1Fh ?
  22h
  23h
  24h -    -
  25h                                       ;use EXT clock!
  26h -    -
  27h -    -     Begin Download ?           ;use EXT clock!
  28h
  29h
  2Ah
  2Bh
  2Ch
  2Dh
  2Eh
  2Fh


  30h 1    -
  31h
  32h
  33h
  34h
  35h                                       ;use EXT clock!
  36h
  37h                                       ;use EXT clock!
  38h
  39h
  3Ah
  3Bh
  3Ch
  3Dh -    -     Bye (return to language select)
  3Eh
  3Fh
```

Special Response 996601EEh for error or so? (only at software side?)
